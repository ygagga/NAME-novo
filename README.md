local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()

-- Criando a Janela
local Window = OrionLib:MakeWindow({
    Name = "Troll Hub Brookhaven",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "TrollHubSettings"
})

-- Criando uma Aba Principal
local MainTab = Window:MakeTab({
    Name = "Troll",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Criando um botão para matar jogadores
MainTab:AddButton({
    Name = "Matar Jogador",
    Callback = function()
        -- Código para matar jogador aqui
        print("Jogador eliminado!")
    end
})

-- Criando um toggle para lagar o servidor
MainTab:AddToggle({
    Name = "Lagar Servidor",
    Default = false,
    Callback = function(Value)
        if Value then
            print("Lag ativado")
            -- Código para lagar o servidor
        else
            print("Lag desativado")
        end
    end
})

-- Criando um dropdown para selecionar um jogador
local PlayersDropdown = MainTab:AddDropdown({
    Name = "Selecionar Jogador",
    Default = "Nenhum",
    Options = {"Jogador1", "Jogador2"}, -- Aqui você pode adicionar um código para pegar os jogadores do servidor
    Callback = function(Value)
        print("Selecionado: " .. Value)
    end
})

-- Criando um botão para teleportar jogador selecionado
MainTab:AddButton({
    Name = "Teleportar Jogador",
    Callback = function()
        local selectedPlayer = PlayersDropdown.Value
        if selectedPlayer then
            print("Teleportando " .. selectedPlayer)
            -- Código para teleportar o jogador aqui
        end
    end
})

-- Criando um slider para aumentar a velocidade
MainTab:AddSlider({
    Name = "Velocidade",
    Min = 16,
    Max = 100,
    Default = 16,
    Increment = 1,
    ValueName = "Speed",
    Callback = function(Value)
        print("Velocidade definida para " .. Value)
        -- Código para alterar velocidade do player
    end
})

-- Criando uma notificação inicial
OrionLib:MakeNotification({
    Name = "Bem-vindo!",
    Content = "Troll Hub Brookhaven ativado!",
    Image = "rbxassetid://4483345998",
    Time = 5
})

-- Finalizando o script
OrionLib:Init()
