local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local UserInputService = game:GetService("UserInputService")

-- Criando a ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "NatanHub"
screenGui.Parent = playerGui

-- Criando o Frame principal do hub
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 450)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -225)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

-- Criando o cabeçalho do hub
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.Text = "Natan Hub - Dead Rails"
titleLabel.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 24
titleLabel.Parent = mainFrame

-- Função para fechar o hub
local function closeGui()
    screenGui:Destroy()
end

-- Criando um botão para fechar o hub
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 100, 0, 30)
closeButton.Position = UDim2.new(1, -110, 0, 10)
closeButton.Text = "Fechar"
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.Parent = mainFrame
closeButton.MouseButton1Click:Connect(closeGui)

-- Funções de Teletransporte
local function teleport(locationName)
    local location = game.Workspace:FindFirstChild(locationName)
    if location and player.Character then
        player.Character.HumanoidRootPart.CFrame = location.CFrame
    else
        warn("Localização não encontrada: " .. locationName)
    end
end

-- Criando botões de Teletransporte
local teleportLocations = {
    {"TP Banco", "Banco"},
    {"TP Base Final", "BaseFinal"},
    {"TP Tesla", "Tesla"},
    {"TP Castelo", "Castelo"},
}

for i, loc in ipairs(teleportLocations) do
    local tpButton = Instance.new("TextButton")
    tpButton.Size = UDim2.new(0, 250, 0, 40)
    tpButton.Position = UDim2.new(0.5, -125, 0, 50 + (i - 1) * 50)
    tpButton.Text = loc[1]
    tpButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    tpButton.Parent = mainFrame
    tpButton.MouseButton1Click:Connect(function() teleport(loc[2]) end)
end

-- Função NoClip
local noclipEnabled = false

local function toggleNoClip()
    noclipEnabled = not noclipEnabled
    local character = player.Character
    if character then
        for _, part in ipairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = not noclipEnabled
            end
        end
    end
end

-- Criando o botão NoClip
local noclipButton = Instance.new("TextButton")
noclipButton.Size = UDim2.new(0, 250, 0, 40)
noclipButton.Position = UDim2.new(0.5, -125, 0, 300)
noclipButton.Text = "Toggle NoClip"
noclipButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
noclipButton.Parent = mainFrame
noclipButton.MouseButton1Click:Connect(toggleNoClip)

-- Botão para dar velocidade ao jogador
local speedButton = Instance.new("TextButton")
speedButton.Size = UDim2.new(0, 250, 0, 40)
speedButton.Position = UDim2.new(0.5, -125, 0, 350)
speedButton.Text = "Aumentar Velocidade"
speedButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
speedButton.Parent = mainFrame

speedButton.MouseButton1Click:Connect(function()
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.WalkSpeed = 100 -- Altera a velocidade
    end
end)

-- Estilizando os botões
for _, button in ipairs(mainFrame:GetChildren()) do
    if button:IsA("TextButton") then
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        end)
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
        end)
    end
end
