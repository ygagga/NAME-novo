local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()

-- Criando a Interface
local Window = OrionLib:MakeWindow({
    Name = "👾ZenithCore👾",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "TrollHub"
})

-- Criando um botão flutuante grande (como o do Delta Executor)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local OpenCloseButton = Instance.new("ImageButton")
OpenCloseButton.Size = UDim2.new(0, 150, 0, 150) -- Tamanho grande igual ao do Delta
OpenCloseButton.Position = UDim2.new(0.02, 0, 0.4, 0) -- Posição ajustável (canto esquerdo)
OpenCloseButton.BackgroundTransparency = 1
OpenCloseButton.Image = "rbxassetid://14265681361" -- Ícone personalizado
OpenCloseButton.Draggable = true -- Permite mover o botão
OpenCloseButton.Parent = ScreenGui

-- Variável para armazenar o estado do menu
local isMenuOpen = true

-- Função para abrir/fechar o menu ao clicar no botão
OpenCloseButton.MouseButton1Click:Connect(function()
    isMenuOpen = not isMenuOpen
    OrionLib:Toggle(isMenuOpen)
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
