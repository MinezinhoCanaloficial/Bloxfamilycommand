-- Botão que executa a função CriarInterfaceFluent
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AbrirFluent"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local AbrirBotao = Instance.new("TextButton")
AbrirBotao.Name = "BotaoFluent"
AbrirBotao.Size = UDim2.new(0, 50, 0, 50)
AbrirBotao.Position = UDim2.new(0, 10, 0, 10)
AbrirBotao.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
AbrirBotao.Text = "☯️"
AbrirBotao.TextColor3 = Color3.new(1, 1, 1)
AbrirBotao.Parent = ScreenGui
AbrirBotao.Draggable = true
AbrirBotao.Active = true

-- Função da Interface Fluent
function CriarInterfaceFluent()
    local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
    local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
    local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

    local Window = Fluent:CreateWindow({
        Title = "PowerPoint " .. Fluent.Version,
        SubTitle = "by GuizinhoGamers0",
        TabWidth = 160,
        Size = UDim2.fromOffset(500, 270),
        Acrylic = true,
        Theme = "Dark",
        MinimizeKey = Enum.KeyCode.LeftControl
    })

    local FarmTab = Window:AddTab({ Title = "Auto Farm" })
    local Options = Fluent.Options

    local AutoFarmAtivo = false

    FarmTab:AddToggle("AutoFarmToggle", { Title = "Auto farm nível", Default = false }):OnChanged(function(state)
        AutoFarmAtivo = state

        if AutoFarmAtivo then
            task.spawn(function()
                while AutoFarmAtivo and task.wait(0.5) do
                    pcall(function()
                        local player = game.Players.LocalPlayer
                        local character = player.Character or player.CharacterAdded:Wait()
                        local humanoid = character:FindFirstChildOfClass("Humanoid")
                        local rootPart = character:FindFirstChild("HumanoidRootPart")

                        -- Encontrar inimigo mais próximo
                        local closestEnemy, minDistance = nil, math.huge
                        for _, v in pairs(workspace.Enemies:GetChildren()) do
                            if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChildOfClass("Humanoid") and v:FindFirstChildOfClass("Humanoid").Health > 0 then
                                local distance = (v.HumanoidRootPart.Position - rootPart.Position).Magnitude
                                if distance < minDistance then
                                    closestEnemy = v
                                    minDistance = distance
                                end
                            end
                        end

                        if closestEnemy then
                            -- Mover até o inimigo
                            rootPart.CFrame = closestEnemy.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
                            -- Atacar com clique (emula apertar botão de ataque)
                            local VirtualInputManager = game:GetService("VirtualInputManager")
                            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                            task.wait()
                            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                        end
                    end)
                end
            end)
        end
    end)

    -- Addons
    SaveManager:SetLibrary(Fluent)
    InterfaceManager:SetLibrary(Fluent)
    SaveManager:IgnoreThemeSettings()
    SaveManager:SetIgnoreIndexes({})
    InterfaceManager:SetFolder("FluentScriptHub")
    SaveManager:SetFolder("FluentScriptHub/specific-game")
    SaveManager:LoadAutoloadConfig()
end

-- Botão para abrir interface
AbrirBotao.MouseButton1Click:Connect(function()
    CriarInterfaceFluent()
end)
