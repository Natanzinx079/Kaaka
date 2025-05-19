--[[
   Natan Dead - Hub Profissional para Dead Rails (Roblox)
   Inspirado visualmente em KiciaHook, NatHub e Lunor
   Compatível com Delta Executor e Android
   Desenvolvido com foco em funcionalidades reais e sem bugs
--]]

-- Proteção inicial contra execução duplicada
if getgenv().NatanDeadLoaded then return end
getgenv().NatanDeadLoaded = true

-- Serviços
local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- GUI Principal
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatanDeadHub"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 500, 0, 320)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "Natan Dead - Hub Profissional"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 18

-- Minimize e Fechar
local CloseButton = Instance.new("TextButton", MainFrame)
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local MinimizeButton = Instance.new("TextButton", MainFrame)
MinimizeButton.Text = "-"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -70, 0, 5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 14

local isMinimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    for _, v in pairs(MainFrame:GetChildren()) do
        if v:IsA("TextButton") or v:IsA("TextLabel") then
            if v ~= Title and v ~= MinimizeButton and v ~= CloseButton then
                v.Visible = not isMinimized
            end
        end
    end
end)

-- Funções úteis
local function teleportTo(position)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(position)
    end
end

local function fullBright()
    Lighting.Brightness = 5
    Lighting.FogEnd = 100000
    Lighting.GlobalShadows = false
    Lighting.ClockTime = 12
end

local function removeFog()
    Lighting.FogEnd = 100000
end

local function unlockMouse()
    local userInputService = game:GetService("UserInputService")
    userInputService.MouseBehavior = Enum.MouseBehavior.Default
end

local function unlockCamera()
    LocalPlayer.CameraMaxZoomDistance = 500
end

local function toggleNoclip()
    local noclip = false
    RunService.Stepped:Connect(function()
        if noclip and LocalPlayer.Character then
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide then
                    v.CanCollide = false
                end
            end
        end
    end)
    return function(state)
        noclip = state
    end
end

local setNoclip = toggleNoclip()

-- Base Locations (em cima das torretas)
local Bases = {
    ["Trem"] = Vector3.new(100, 20, 300),
    ["Base 1"] = Vector3.new(342, 110, -721),
    ["Base 2"] = Vector3.new(914, 128, -148),
    ["Base 3"] = Vector3.new(1274, 122, -835),
    ["Base Final"] = Vector3.new(2223, 138, -552)
}

-- AutoFarm simples
local autoFarm = false
spawn(function()
    while task.wait(0.5) do
        if autoFarm then
            teleportTo(Bases["Trem"])
        end
    end
end)

-- Criar Botões
local Y = 50
local function createButton(text, callback)
    local button = Instance.new("TextButton", MainFrame)
    button.Size = UDim2.new(0, 480, 0, 30)
    button.Position = UDim2.new(0, 10, 0, Y)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 14
    button.MouseButton1Click:Connect(callback)
    Y = Y + 35
end

-- Botões principais
createButton("Teleportar para o Trem", function()
    teleportTo(Bases["Trem"])
end)

for base, pos in pairs(Bases) do
    if base ~= "Trem" then
        createButton("Teleporte para " .. base, function()
            teleportTo(pos)
        end)
    end
end

createButton("Ativar AutoFarm", function()
    autoFarm = not autoFarm
end)

createButton("Toggle Noclip", function()
    setNoclip(true)
end)

createButton("Unlock Mouse", unlockMouse)
createButton("Unlock Camera", unlockCamera)
createButton("Full Bright", fullBright)
createButton("Remove Fog", removeFog)

-- ESP básico (apenas nome no topo de players)
local function createESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and not player.Character:FindFirstChild("ESP") then
            local bill = Instance.new("BillboardGui", player.Character)
            bill.Name = "ESP"
            bill.Adornee = player.Character:WaitForChild("Head")
            bill.Size = UDim2.new(0, 100, 0, 40)
            bill.StudsOffset = Vector3.new(0, 2, 0)
            bill.AlwaysOnTop = true
            local text = Instance.new("TextLabel", bill)
            text.Size = UDim2.new(1, 0, 1, 0)
            text.Text = player.Name
            text.TextColor3 = Color3.fromRGB(255, 0, 0)
            text.BackgroundTransparency = 1
            text.TextScaled = true
        end
    end
end

createButton("Ativar ESP de Jogadores", createESP)

-- Fim do Script
print("[Natan Dead Hub] Script carregado com sucesso!")
