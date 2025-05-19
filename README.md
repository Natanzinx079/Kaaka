-- Dead Realm Auto Bond Script
-- Versão 3.0 - Compatível com Delta e Android
-- Baseado no Nathub com melhorias

local Settings = {
    AutoBond = {
        Enabled = true,
        Key = "e",
        Distance = 25,
        Cooldown = 2,
        Priority = "Closest", -- Closest, LowestHealth, Random
        CheckInterval = 0.2,
        Notify = true
    },
    UI = {
        Enabled = true,
        Style = "Modern", -- Simple, Modern, Pro
        Position = {x = 10, y = 10},
        Theme = "Dark" -- Dark, Light, Blue
    }
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local BondedPlayers = {}
local LastBondTime = 0
local UIShowing = true

-- Funções principais
function IsValidTarget(player)
    if not player then return false end
    if player == LocalPlayer then return false end
    if not player.Character then return false end
    if not player.Character:FindFirstChild("Humanoid") then return false end
    if player.Character.Humanoid.Health <= 0 then return false end
    return true
end

function GetDistance(player1, player2)
    if not player1.Character or not player2.Character then return math.huge end
    if not player1.Character:FindFirstChild("HumanoidRootPart") then return math.huge end
    if not player2.Character:FindFirstChild("HumanoidRootPart") then return math.huge end
    
    return (player1.Character.HumanoidRootPart.Position - player2.Character.HumanoidRootPart.Position).Magnitude
end

function CanBondWith(player)
    if not IsValidTarget(player) then return false end
    if GetDistance(LocalPlayer, player) > Settings.AutoBond.Distance then return false end
    
    -- Verificar se já está bondado (lógica específica do Dead Realm)
    -- Esta parte precisa ser adaptada conforme a mecânica exata do jogo
    return true
end

function PerformBond(target)
    if not target then return end
    
    -- Simular pressionamento da tecla
    keypress(Settings.AutoBond.Key)
    wait(0.05)
    keyrelease(Settings.AutoBond.Key)
    
    LastBondTime = tick()
    
    if Settings.AutoBond.Notify then
        Notify("Bonded with "..target.Name)
    end
    
    table.insert(BondedPlayers, target)
end

function FindBestTarget()
    local potentialTargets = {}
    local players = Players:GetPlayers()
    
    for _, player in ipairs(players) do
        if CanBondWith(player) then
            table.insert(potentialTargets, player)
        end
    end
    
    if #potentialTargets == 0 then return nil end
    if #potentialTargets == 1 then return potentialTargets[1] end
    
    -- Lógica de prioridade
    if Settings.AutoBond.Priority == "Closest" then
        table.sort(potentialTargets, function(a, b)
            return GetDistance(LocalPlayer, a) < GetDistance(LocalPlayer, b)
        end)
    elseif Settings.AutoBond.Priority == "LowestHealth" then
        table.sort(potentialTargets, function(a, b)
            return a.Character.Humanoid.Health < b.Character.Humanoid.Health
        end)
    end
    
    return potentialTargets[1]
end

-- Interface do usuário
function DrawUI()
    if not Settings.UI.Enabled or not UIShowing then return end
    
    local theme = {
        Dark = {
            Background = Color3.fromRGB(20, 20, 20),
            Text = Color3.fromRGB(255, 255, 255),
            Accent = Color3.fromRGB(0, 150, 255)
        },
        Light = {
            Background = Color3.fromRGB(240, 240, 240),
            Text = Color3.fromRGB(0, 0, 0),
            Accent = Color3.fromRGB(0, 100, 255)
        },
        Blue = {
            Background = Color3.fromRGB(10, 30, 50),
            Text = Color3.fromRGB(200, 230, 255),
            Accent = Color3.fromRGB(0, 200, 255)
        }
    }[Settings.UI.Theme]
    
    -- Desenhar fundo
    drawSquare(Settings.UI.Position.x, Settings.UI.Position.y, 200, 80, theme.Background, 0.7)
    
    -- Título
    drawText("Auto Bond Pro", Settings.UI.Position.x + 10, Settings.UI.Position.y + 5, theme.Accent, 14, true)
    
    -- Status
    local statusText = Settings.AutoBond.Enabled and "ACTIVE" or "INACTIVE"
    local statusColor = Settings.AutoBond.Enabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 50, 50)
    drawText("Status: "..statusText, Settings.UI.Position.x + 10, Settings.UI.Position.y + 30, statusColor, 12, false)
    
    -- Info
    drawText("Bonds: "..#BondedPlayers, Settings.UI.Position.x + 10, Settings.UI.Position.y + 50, theme.Text, 12, false)
end

-- Loop principal
function AutoBondLoop()
    while Settings.AutoBond.Enabled do
        if tick() - LastBondTime >= Settings.AutoBond.Cooldown then
            local target = FindBestTarget()
            if target then
                PerformBond(target)
            end
        end
        wait(Settings.AutoBond.CheckInterval)
    end
end

-- Controles
function HandleKeyPress(input)
    if input.KeyCode == Enum.KeyCode.F1 then
        Settings.AutoBond.Enabled = not Settings.AutoBond.Enabled
        Notify("Auto Bond "..(Settings.AutoBond.Enabled and "enabled" or "disabled"))
        
        if Settings.AutoBond.Enabled then
            coroutine.wrap(AutoBondLoop)()
        end
    elseif input.KeyCode == Enum.KeyCode.F2 then
        UIShowing = not UIShowing
        Notify("UI "..(UIShowing and "shown" or "hidden"))
    end
end

-- Inicialização
function Init()
    -- Verificar ambiente
    if not drawing or not keypress then
        warn("Este script requer funções específicas do executor")
        return
    end
    
    -- Configurar listeners
    game:GetService("UserInputService").InputBegan:Connect(HandleKeyPress)
    
    -- Iniciar loops
    if Settings.AutoBond.Enabled then
        coroutine.wrap(AutoBondLoop)()
    end
    
    -- Loop da UI
    while true do
        DrawUI()
        wait(0.1)
    end
end

-- Notificações
function Notify(message)
    if Settings.UI.Enabled then
        -- Implementar sistema de notificação visual
        print("[AutoBond] "..message)
    end
end

-- Iniciar script
Init()
