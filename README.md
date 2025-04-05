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

-- Função do Fluent
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

    local PoderesTab = Window:AddTab({ Title = "Poderes" })
    local Options = Fluent.Options

    local speedValue = "16"
    local jumpValue = "50"

    -- Speed Input
    PoderesTab:AddInput("SpeedInput", {
        Title = "Velocidade (Speed)",
        Placeholder = "Digite a velocidade...",
        Default = "16",
        OnChanged = function(value)
            speedValue = value
        end
    })

    PoderesTab:AddButton({
        Title = "Aplicar Speed",
        Callback = function()
            local humanoid = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = tonumber(speedValue) or 16
            end
        end
    })

    -- Jump Input
    PoderesTab:AddInput("JumpInput", {
        Title = "Altura do Pulo (Jump)",
        Placeholder = "Digite a altura do pulo...",
        Default = "50",
        OnChanged = function(value)
            jumpValue = value
        end
    })

    PoderesTab:AddButton({
        Title = "Aplicar Jump",
        Callback = function()
            local humanoid = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.JumpPower = tonumber(jumpValue) or 50
            end
        end
    })

    -- Infinite Jump Toggle
    local InfiniteJumpEnabled = false
    PoderesTab:AddToggle("InfiniteJump", { Title = "Infinito Jump", Default = false }):OnChanged(function(state)
        InfiniteJumpEnabled = state
    end)

    game:GetService("UserInputService").JumpRequest:Connect(function()
        if InfiniteJumpEnabled then
            local character = game.Players.LocalPlayer.Character
            if character and character:FindFirstChildOfClass("Humanoid") then
                character:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end)

    -- Botão de Comando (Infinite Yield)
    PoderesTab:AddButton({
        Title = "Command",
        Callback = function()
            loadstring(game:HttpGet("https://rawscripts.net/raw/Infinite-Yield_500"))()
        end
    })

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

local UserInputService = game:GetService("UserInputService")
