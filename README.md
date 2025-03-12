local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()

-- Criando a Interface
local Window = OrionLib:MakeWindow({
    Name = "👾ZenithCore👾",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "TrollHub"
})

-- Criando um botão grande para abrir/fechar o menu
local toggleKey = Enum.KeyCode.RightShift -- Tecla para abrir/fechar
local isMenuOpen = true

local OpenCloseButton = Instance.new("ImageButton")
OpenCloseButton.Size = UDim2.new(0, 100, 0, 100) -- Tamanho grande
OpenCloseButton.Position = UDim2.new(0.9, 0, 0.05, 0) -- Posição no canto
OpenCloseButton.BackgroundTransparency = 1
OpenCloseButton.Image = "rbxassetid://14265681361"
OpenCloseButton.Parent = game:GetService("CoreGui")

OpenCloseButton.MouseButton1Click:Connect(function()
    isMenuOpen = not isMenuOpen
    OrionLib:Toggle(isMenuOpen)
end)

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == toggleKey then
        isMenuOpen = not isMenuOpen
        OrionLib:Toggle(isMenuOpen)
    end
end)


-- Criando as Abas (Tabs)
local TrollTab = Window:MakeTab({ Name = "Troll", Icon = "rbxassetid://4483362458", PremiumOnly = false })
local MusicTab = Window:MakeTab({ Name = "Música", Icon = "rbxassetid://6034509993", PremiumOnly = false })
local HacksTab = Window:MakeTab({ Name = "Hacks", Icon = "rbxassetid://6034509993", PremiumOnly = false })
local ScriptsTab = Window:MakeTab({ Name = "Scripts", Icon = "rbxassetid://6034509973", PremiumOnly = false })
local AboutTab = Window:MakeTab({ Name = "Sobre", Icon = "rbxassetid://6034509992", PremiumOnly = false })

-----------------------------------------------------------
-- 🤡 TROLL (Teleportar, Espectar, Matar)
-----------------------------------------------------------
TrollTab:AddSection({ Name = "Controle de Jogadores" })

local selectedPlayer = ""

TrollTab:AddTextbox({
    Name = "Nome do Jogador",
    Default = "",
    TextDisappear = true,
    Callback = function(value)
        selectedPlayer = value
    end
})

TrollTab:AddButton({
    Name = "Teleportar Todos para Mim",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer
        local root = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")

        if root then
            for _, target in pairs(players:GetPlayers()) do
                if target.Character and target ~= localPlayer then
                    local targetRoot = target.Character:FindFirstChild("HumanoidRootPart")
                    if targetRoot then
                        targetRoot.CFrame = root.CFrame
                    end
                end
            end
        end
    end
})

TrollTab:AddButton({
    Name = "Espectar Jogador",
    Callback = function()
        local players = game:GetService("Players")
        local target = players:FindFirstChild(selectedPlayer)

        if target and target.Character then
            game.Workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChildOfClass("Humanoid")
        end
    end
})

TrollTab:AddButton({
    Name = "Parar de Espectar",
    Callback = function()
        game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
})

--------------------------------------
-- 🎶 Aba Música (Tocar para Todos)
--------------------------------------

MusicTab:AddSection({ Name = "Escolha sua Música" })

local globalMusicId = ""
local globalSound
local isLoopEnabled = false

MusicTab:AddToggle({
    Name = "Tocar em Loop 🔁",
    Default = false,
    Callback = function(value)
        isLoopEnabled = value
    end
})

MusicTab:AddTextbox({
    Name = "ID da Música Global",
    Default = "",
    TextDisappear = false,
    Callback = function(value)
        globalMusicId = value
    end
})

local musicIds = {
    ["🎵 Música 1"] = "6454199333",
    ["🎵 Música 2"] = "6427245762",
    ["🎵 Música 3"] = "6489326185",
    ["🎵 Música 4"] = "6433157341",
    ["🎵 Música 5"] = "6436089393",
    ["🎵 Música 6"] = "18841894272",
    ["🎵 Música 7"] = "16190784547"
}

for name, id in pairs(musicIds) do
    MusicTab:AddButton({
        Name = name,
        Callback = function()
            if globalSound then globalSound:Destroy() end
            globalSound = Instance.new("Sound", game.Workspace)
            globalSound.SoundId = "rbxassetid://" .. id
            globalSound.Volume = 10
            globalSound.Looped = isLoopEnabled
            globalSound:Play()
        end
    })
end

MusicTab:AddButton({
    Name = "Tocar ID Personalizado 📢",
    Callback = function()
        if globalMusicId ~= "" then
            if globalSound then globalSound:Destroy() end
            globalSound = Instance.new("Sound", game.Workspace)
            globalSound.SoundId = "rbxassetid://" .. globalMusicId
            globalSound.Volume = 10
            globalSound.Looped = isLoopEnabled
            globalSound:Play()
        end
    end
})

MusicTab:AddButton({
    Name = "Parar Música Global ⛔",
    Callback = function()
        if globalSound then
            globalSound:Stop()
            globalSound:Destroy()
            globalSound = nil
        end
    end
})

--------------------------------------
-- 💻 Aba Hacker (Anti Sit)
--------------------------------------

HacksTab:AddSection({ Name = "Anti Sit (Desativar/Sentado)" })

local antiSitEnabled = false

HacksTab:AddToggle({
    Name = "Ativar/Desativar Anti Sit 🚫",
    Default = false,
    Callback = function(value)
        antiSitEnabled = value
        if antiSitEnabled then
            game.Players.LocalPlayer.Character.Humanoid.Sit = false
            game.Players.LocalPlayer.Character.Humanoid.Seated:Connect(function()
                game.Players.LocalPlayer.Character.Humanoid.Sit = false
            end)
        else
            game.Players.LocalPlayer.Character.Humanoid.Sit = false
        end
    end
})

-----------------------------------------------------------
-- 🧑‍💻 SCRIPTS (Executar Scripts Extras)
-----------------------------------------------------------
ScriptsTab:AddSection({ Name = "Executar Scripts" })

ScriptsTab:AddButton({
    Name = "Fly Script ✈️",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end
})

ScriptsTab:AddButton({
    Name = "RAEL Hub 🔧",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Laelmano24/Rael-Hub/main/main.txt"))()
    end
})

ScriptsTab:AddButton({
    Name = "Sander X 🛸",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/sXPiterXs1111/Sanderxv3.30/main/sanderx3.30'))()
    end
})

-----------------------------------------------------------
-- ℹ️ SOBRE
-----------------------------------------------------------
AboutTab:AddSection({ Name = "Criado por Shelby, user discord: snobodj" })

AboutTab:AddParagraph({
    Title = "Troll Hub 🤡",
    Content = "Criado para trollar no Brookhaven RP! Divirta-se!"
})

OrionLib:Init()
