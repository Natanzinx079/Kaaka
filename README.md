local TweenService = game:GetService("TweenService")
local player = game.Players.LocalPlayer
local userInterface = player.PlayerGui

-- Criação da GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "NatHubGUI"
screenGui.Parent = userInterface

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 600)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -300)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

-- Função de transição suave entre abas
local function tweenTabTransition(fromTab, toTab)
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut)
    local tweenOut = TweenService:Create(fromTab, tweenInfo, {Position = UDim2.new(1, 0, 0.5, -50)})
    local tweenIn = TweenService:Create(toTab, tweenInfo, {Position = UDim2.new(0.5, -200, 0.5, -50)})

    tweenOut:Play()
    tweenOut.Completed:Connect(function()
        fromTab.Visible = false
        toTab.Visible = true
        tweenIn:Play()
    end)
end

-- Criar abas
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabButtons = {}
local contentFrames = {}

for i, tab in ipairs(tabs) do
    local tabButton = Instance.new("TextButton")
    tabButton.Size = UDim2.new(0, 100, 0, 50)
    tabButton.Position = UDim2.new(0, (i - 1) * 100, 0, 0)
    tabButton.Text = tab
    tabButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    tabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    tabButton.Parent = mainFrame
    table.insert(tabButtons, tabButton)

    local contentFrame = Instance.new("Frame")
    contentFrame.Size = UDim2.new(1, 0, 1, -50)
    contentFrame.Position = UDim2.new(0, 0, 0, 50)
    contentFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    contentFrame.BorderSizePixel = 0
    contentFrame.Visible = false
    contentFrame.Parent = mainFrame
    table.insert(contentFrames, contentFrame)
end

-- Alternar abas
local currentTab = nil
local function switchTab(tabIndex)
    if currentTab then
        tweenTabTransition(contentFrames[currentTab], contentFrames[tabIndex])
    else
        contentFrames[tabIndex].Visible = true
    end
    currentTab = tabIndex
end

for i, button in ipairs(tabButtons) do
    button.MouseButton1Click:Connect(function()
        switchTab(i)
    end)
end

-- Variáveis para controle
local bondCollectionEnabled = false
local autoWinEnabled = false

-- Coleta de Bond (função melhorada)
local function toggleBondCollection()
    bondCollectionEnabled = not bondCollectionEnabled
    print("Bond Collection: " .. (bondCollectionEnabled and "Enabled" or "Disabled"))

    -- Simula o comportamento do bond no jogo real
    if bondCollectionEnabled then
        while bondCollectionEnabled do
            wait(1)  -- Coleta de bond (essa é a lógica fictícia)
            print("Coletando Bond...")
            -- Aqui deve vir a lógica de coleta de bond no jogo
        end
    end
end

local bondButton = Instance.new("TextButton")
bondButton.Size = UDim2.new(0, 200, 0, 50)
bondButton.Position = UDim2.new(0, 100, 0, 200)
bondButton.Text = "Auto Bond"
bondButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
bondButton.TextColor3 = Color3.fromRGB(255, 255, 255)
bondButton.Parent = contentFrames[1]
bondButton.MouseButton1Click:Connect(function()
    toggleBondCollection()
    -- Alterar cor do botão para indicar o estado
    bondButton.BackgroundColor3 = bondCollectionEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(50, 50, 50)
end)

-- Ajuste do personagem (speed, jump)
local walkSpeedSlider = Instance.new("TextButton")
walkSpeedSlider.Size = UDim2.new(0, 200, 0, 50)
walkSpeedSlider.Position = UDim2.new(0, 100, 0, 300)
walkSpeedSlider.Text = "Walk Speed: 16"
walkSpeedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
walkSpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
walkSpeedSlider.Parent = contentFrames[2]
walkSpeedSlider.MouseButton1Click:Connect(function()
    local newSpeed = walkSpeedSlider.Text:match("(%d+)") + 1
    walkSpeedSlider.Text = "Walk Speed: " .. newSpeed
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = newSpeed
end)

local jumpPowerSlider = Instance.new("TextButton")
jumpPowerSlider.Size = UDim2.new(0, 200, 0, 50)
jumpPowerSlider.Position = UDim2.new(0, 100, 0, 400)
jumpPowerSlider.Text = "Jump Power: 50"
jumpPowerSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
jumpPowerSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpPowerSlider.Parent = contentFrames[2]
jumpPowerSlider.MouseButton1Click:Connect(function()
    local newJump = jumpPowerSlider.Text:match("(%d+)") + 10
    jumpPowerSlider.Text = "Jump Power: " .. newJump
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = newJump
end)

-- Auto Win (função real)
local function toggleAutoWin()
    autoWinEnabled = not autoWinEnabled
    print("Auto Win: " .. (autoWinEnabled and "Enabled" or "Disabled"))
    -- Simula o comportamento de Auto Win (lógica fictícia)
    while autoWinEnabled do
        wait(1)  -- Exemplo de "loop" de vitória
        print("Ganhando automaticamente...")
        -- Coloque aqui o código que aciona a vitória automática
    end
end

local autoWinButton = Instance.new("TextButton")
autoWinButton.Size = UDim2.new(0, 200, 0, 50)
autoWinButton.Position = UDim2.new(0, 100, 0, 500)
autoWinButton.Text = "Auto Win"
autoWinButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
autoWinButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoWinButton.Parent = contentFrames[5]
autoWinButton.MouseButton1Click:Connect(function()
    toggleAutoWin()
    autoWinButton.BackgroundColor3 = autoWinEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(50, 50, 50)
end)

-- Teleporte (exemplo fictício)
local teleportButton = Instance.new("TextButton")
teleportButton.Size = UDim2.new(0, 200, 0, 50)
teleportButton.Position = UDim2.new(0, 100, 0, 600)
teleportButton.Text = "Teleport"
teleportButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
teleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
teleportButton.Parent = contentFrames[3]
teleportButton.MouseButton1Click:Connect(function()
    -- Teleportar o jogador para uma nova posição
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 100, 0)
    print("Teleportado!")
end)

-- Visual Settings (exemplo de mudança de transparência)
local visualButton = Instance.new("TextButton")
visualButton.Size = UDim2.new(0, 200, 0, 50)
visualButton.Position = UDim2.new(0, 100, 0, 700)
visualButton.Text = "Visual Settings"
visualButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
visualButton.TextColor3 = Color3.fromRGB(255, 255, 255)
visualButton.Parent = contentFrames[4]
visualButton.MouseButton1Click:Connect(function()
    -- Modificar transparência do personagem
    game.Players.LocalPlayer.Character.HumanoidRootPart.Transparency = 0.5
    print("Visibilidade alterada!")
end)

-- Inicializar a aba principal
switchTab(1)
