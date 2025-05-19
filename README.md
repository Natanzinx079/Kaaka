-- Dead Realm Ultimate Script
-- Versão 4.0 - Nathub Style Plus
-- Compatível com Delta e Android

local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()
local Window = Rayfield:CreateWindow({
   Name = "Dead Realm Ultimate",
   LoadingTitle = "Nathub Style Plus",
   LoadingSubtitle = "by github/seuscréditos",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DeadRealmConfig",
      FileName = "NathubStylePlus"
   },
   Discord = {
      Enabled = true,
      Invite = "discordlink",
      RememberJoins = true
   }
})

-- Configurações principais
local Settings = {
    AutoBond = {
        Enabled = false,
        Key = "E",
        Distance = 30,
        Cooldown = 1.5,
        Priority = "Closest",
        Notify = true,
        Sound = true,
        AntiDetection = true
    },
    Visuals = {
        ESP = {
            Enabled = false,
            TeamCheck = true,
            Boxes = true,
            Names = true,
            Health = true,
            Distance = true,
            Color = Color3.fromRGB(0, 255, 0)
        },
        Crosshair = {
            Enabled = true,
            Style = "Plus",
            Color = Color3.fromRGB(255, 0, 0),
            Size = 12,
            Gap = 3
        }
    },
    Combat = {
        AutoAim = {
            Enabled = false,
            Key = "Q",
            FOV = 50,
            Smoothness = 0.2,
            HitChance = 95,
            Priority = "Closest"
        },
        TriggerBot = {
            Enabled = false,
            Delay = 0.1,
            Range = 20
        }
    },
    Movement = {
        Speed = {
            Enabled = false,
            Speed = 22,
            Mode = "CFrame"
        },
        BHop = {
            Enabled = false,
            Power = 45
        }
    },
    Misc = {
        AntiAFK = true,
        FullBright = false,
        FPSBoost = true,
        Rejoin = {
            Enabled = false,
            Delay = 300
        }
    }
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local BondedPlayers = {}
local LastBondTime = 0
local Target = nil
local Connections = {}

function Notify(Title, Text, Duration)
    Rayfield:Notify({
        Title = Title,
        Content = Text,
        Duration = Duration or 5,
        Image = 4483362458,
        Actions = {
            Ignore = {
                Name = "Ok",
                Callback = function() end
            },
        },
    })
end

function PlaySound(ID)
    if not Settings.AutoBond.Sound then return end
    local Sound = Instance.new("Sound")
    Sound.SoundId = "rbxassetid://"..ID
    Sound.Parent = workspace
    Sound:Play()
    game:Debris:AddItem(Sound, 2)
end

function GetCharacter(Player)
    return Player and Player.Character or nil
end

function GetHumanoid(Player)
    local Char = GetCharacter(Player)
    return Char and Char:FindFirstChildOfClass("Humanoid") or nil
end

function IsAlive(Player)
    local Humanoid = GetHumanoid(Player)
    return Humanoid and Humanoid.Health > 0 or false
end

function GetTeam(Player)
    return Player and Player.Team or nil
end

function IsEnemy(Player)
    return GetTeam(Player) ~= GetTeam(LocalPlayer)
end

function GetDistance(Player)
    local Char1, Char2 = GetCharacter(LocalPlayer), GetCharacter(Player)
    if not Char1 or not Char2 then return math.huge end
    local Root1, Root2 = Char1:FindFirstChild("HumanoidRootPart"), Char2:FindFirstChild("HumanoidRootPart")
    if not Root1 or not Root2 then return math.huge end
    return (Root1.Position - Root2.Position).Magnitude
end

function CanBond(Player)
    if not IsAlive(Player) then return false end
    if not IsEnemy(Player) then return false end
    if table.find(BondedPlayers, Player) then return false end
    if GetDistance(Player) > Settings.AutoBond.Distance then return false end
    return true
end

function FindBestBondTarget()
    local PotentialTargets = {}

    for _, Player in ipairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and CanBond(Player) then
            table.insert(PotentialTargets, Player)
        end
    end

    if #PotentialTargets == 0 then return nil end
    if #PotentialTargets == 1 then return PotentialTargets[1] end

    table.sort(PotentialTargets, function(a, b)
        if Settings.AutoBond.Priority == "Closest" then
            return GetDistance(a) < GetDistance(b)
        elseif Settings.AutoBond.Priority == "LowestHP" then
            return GetHumanoid(a).Health < GetHumanoid(b).Health
        else
            return math.random() > 0.5
        end
    end)

    return PotentialTargets[1]
end

function PerformBond()
    if not Settings.AutoBond.Enabled then return end
    if tick() - LastBondTime < Settings.AutoBond.Cooldown then return end

    local Target = FindBestBondTarget()
    if not Target then return end

    -- >>> REMOVIDO virtualInput para compatibilidade com Delta/Android <<<
    -- Simular ação: aqui você pode substituir por firetouchinterest ou outro método

    LastBondTime = tick()
    table.insert(BondedPlayers, Target)

    if Settings.AutoBond.Notify then
        Notify("Auto Bond", "Bonded with "..Target.Name)
        PlaySound(6537351034)
    end
end

-- Interface Nathub Style
local AutoBondTab = Window:CreateTab("Auto Bond", 4483362458)
local CombatTab = Window:CreateTab("Combat", 4483362458)
local VisualsTab = Window:CreateTab("Visuals", 4483362458)
local MovementTab = Window:CreateTab("Movement", 4483362458)
local MiscTab = Window:CreateTab("Misc", 4483362458)

AutoBondTab:CreateToggle({
    Name = "Auto Bond",
    CurrentValue = Settings.AutoBond.Enabled,
    Flag = "AutoBondToggle",
    Callback = function(Value)
        Settings.AutoBond.Enabled = Value
        if Value then
            Notify("Auto Bond", "Auto Bond ativado!")
        end
    end
})

AutoBondTab:CreateDropdown({
    Name = "Prioridade",
    Options = {"Closest", "LowestHP", "Random"},
    CurrentOption = Settings.AutoBond.Priority,
    Flag = "PriorityDropdown",
    Callback = function(Option)
        Settings.AutoBond.Priority = Option
    end
})

AutoBondTab:CreateSlider({
    Name = "Distância",
    Range = {10, 50},
    Increment = 1,
    Suffix = "studs",
    CurrentValue = Settings.AutoBond.Distance,
    Flag = "DistanceSlider",
    Callback = function(Value)
        Settings.AutoBond.Distance = Value
    end
})

CombatTab:CreateToggle({
    Name = "Auto Aim",
    CurrentValue = Settings.Combat.AutoAim.Enabled,
    Flag = "AutoAimToggle",
    Callback = function(Value)
        Settings.Combat.AutoAim.Enabled = Value
    end
})

VisualsTab:CreateToggle({
    Name = "ESP",
    CurrentValue = Settings.Visuals.ESP.Enabled,
    Flag = "ESPToggle",
    Callback = function(Value)
        Settings.Visuals.ESP.Enabled = Value
    end
})

MovementTab:CreateToggle({
    Name = "Speed Hack",
    CurrentValue = Settings.Movement.Speed.Enabled,
    Flag = "SpeedToggle",
    Callback = function(Value)
        Settings.Movement.Speed.Enabled = Value
    end
})

MiscTab:CreateToggle({
    Name = "Anti-AFK",
    CurrentValue = Settings.Misc.AntiAFK,
    Flag = "AntiAFKToggle",
    Callback = function(Value)
        Settings.Misc.AntiAFK = Value
    end
})

table.insert(Connections, RunService.Heartbeat:Connect(function()
    if Settings.AutoBond.Enabled then
        PerformBond()
    end

    if Settings.Movement.Speed.Enabled and IsAlive(LocalPlayer) then
        local Char = GetCharacter(LocalPlayer)
        local Humanoid = GetHumanoid(LocalPlayer)
        if Humanoid then
            if Settings.Movement.Speed.Mode == "CFrame" then
                -- Código para CFrame Speed
            else
                -- Código para Velocity Speed
            end
        end
    end
end))

if Settings.Misc.AntiAFK then
    table.insert(Connections, game:GetService("Players").LocalPlayer.Idled:Connect(function()
        game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
        task.wait(1)
        game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
    end))
end

Notify("Dead Realm Ultimate", "Script carregado com sucesso!", 8)

game:GetService("Players").PlayerRemoving:Connect(function(Player)
    if Player == LocalPlayer then
        for _, Connection in ipairs(Connections) do
            Connection:Disconnect()
        end
        Rayfield:Destroy()
    end
end)
