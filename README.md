-- 🔥 Carregando Fluent UI e Addons
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- 🎨 Criando a Interface do Hack
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡",
    SubTitle = "by (👾 NexusPrime Hub 💠)",
    TabWidth = 160,
    Size = UDim2.fromOffset(480, 310),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Criando as abas do menu
local Tabs = {
    Avatar = Window:AddTab({ Title = "Avatar Tab", Icon = "shirt" }),
    Troll = Window:AddTab({ Title = "Troll Tab", Icon = "skull" }),
}

-- 🧑‍🎨 **Sistema de Avatar**
local function fireAvatarChange(id, notificationTitle)
    local argsTable = type(id) == "table" and id or {1, 1, 1, 1, 1, id}
    local args = {"CharacterChange", argsTable, "👾 NexusPrime Hub 💠"}
    local replicatedStorage = game:GetService("ReplicatedStorage")
    local starterGui = game:GetService("StarterGui")

    if replicatedStorage and starterGui then
        local remote = replicatedStorage.RE:FindFirstChild("1Avata1rOrigina1l")
        if remote then
            remote:FireServer(unpack(args))
            starterGui:SetCore("SendNotification", {Title = notificationTitle, Text = "Wait Please 1-10 Seconds", Duration = 5})
        end
    end
end

Tabs.Avatar:AddButton({ Title = "Headless Horseman (👤)", Callback = function() fireAvatarChange(134082579, "Headless Horseman") end })
Tabs.Avatar:AddButton({ Title = "Korblox DeathSpeaker (👤)", Callback = function() fireAvatarChange(16580493236, "Korblox DeathSpeaker") end })

-- 🚀 **Sistema de ESP**
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

_G.ESPVisible = true
_G.TextColor = Color3.fromRGB(255, 0, 0) -- Vermelho
_G.TextSize = 14

local function CreateESP()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= Players.LocalPlayer then
            local ESP = Drawing.new("Text")
            RunService.RenderStepped:Connect(function()
                if v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                    local Vector, OnScreen = Camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
                    ESP.Size = _G.TextSize
                    ESP.Color = _G.TextColor
                    ESP.Position = Vector2.new(Vector.X, Vector.Y - 25)
                    ESP.Text = v.Name
                    ESP.Visible = _G.ESPVisible and OnScreen
                else
                    ESP.Visible = false
                end
            end)
        end
    end
end

Tabs.Troll:AddButton({ Title = "Ativar ESP 🔍", Callback = CreateESP })

-- 💀 **KillBrick (mata jogadores que tocarem nele)**
Tabs.Troll:AddButton({ Title = "Spawn KillBrick ☠️", Callback = function()
    local KillBrick = Instance.new("Part")
    KillBrick.Size = Vector3.new(5, 1, 5)
    KillBrick.Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, -3, 0)
    KillBrick.Anchored = true
    KillBrick.Color = Color3.fromRGB(255, 0, 0)
    KillBrick.Parent = workspace

    KillBrick.Touched:Connect(function(touch)
        local humanoid = touch.Parent:FindFirstChild("Humanoid")
        if humanoid and touch.Parent ~= game.Players.LocalPlayer.Character then
            humanoid.Health = 0
        end
    end)
end })

-- 🏃‍♂️ **Velocidade Infinita**
Tabs.Troll:AddButton({ Title = "Velocidade Infinita ⚡", Callback = function()
    local player = game.Players.LocalPlayer
    player.Character.Humanoid.WalkSpeed = 100
end })

-- 🚀 **Pulo Infinito**
Tabs.Troll:AddButton({ Title = "Pulo Infinito 🦘", Callback = function()
    local player = game.Players.LocalPlayer
    player.Character.Humanoid.JumpPower = 200
end })
