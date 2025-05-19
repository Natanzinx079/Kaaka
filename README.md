local TweenService = game:GetService("TweenService")
local player = game.Players.LocalPlayer
local userInterface = player.PlayerGui

-- Criação do ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CustomGUI"
screenGui.Parent = userInterface

-- Função de animação para mover e redimensionar a interface
local function animateUI(frame, position, size, duration)
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quart, Enum.EasingDirection.InOut)
    local tween = TweenService:Create(frame, tweenInfo, {Position = position, Size = size})
    tween:Play()
end

-- Função para minimizar a interface
local isMinimized = false
local function minimizeUI()
    if not isMinimized then
        animateUI(mainFrame, UDim2.new(0.5, -50, 0.5, 50), UDim2.new(0, 100, 0, 50), 0.5)
        minimizeButton.Text = "+"
        isMinimized = true
    else
        animateUI(mainFrame, UDim2.new(0.5, -200, 0.5, -300), UDim2.new(0, 400, 0, 600), 0.5)
        minimizeButton.Text = "-"
        isMinimized = false
    end
end

-- Função para fechar a interface
local function closeUI()
    animateUI(mainFrame, UDim2.new(0.5, -200, 0.5, 200), UDim2.new(0, 0, 0, 0), 0.5)
    wait(0.5)
    screenGui:Destroy()  -- Fecha a interface completamente
end

-- Função para mostrar bolinho ao minimizar
local bolinho = Instance.new("ImageLabel")
bolinho.Size = UDim2.new(0, 100, 0, 100)
bolinho.Position = UDim2.new(0.5, -50, 0.5, -50)
bolinho.Image = "rbxassetid://YOUR_IMAGE_ID"  -- Substitua com o ID da sua imagem
bolinho.Parent = screenGui
bolinho.Visible = false

local function showBolinho()
    bolinho.Visible = true
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out)
    local tween = TweenService:Create(bolinho, tweenInfo, {Position = UDim2.new(0.5, -50, 0.5, -50), Size = UDim2.new(0, 100, 0, 100)})
    tween:Play()
    wait(0.3)
    bolinho.Visible = false
end

-- Criação do Frame principal com bordas arredondadas
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 600)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -300)
mainFrame.BackgroundColor3 = Color3.fromRGB(33, 33, 33)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundTransparency = 0.1
mainFrame.UIStroke.Color = Color3.fromRGB(255, 255, 255)
mainFrame.UIStroke.Thickness = 2
mainFrame.BorderRadius = UDim.new(0, 15)  -- Borda arredondada

-- Função para criar botões com animações
local function createButton(text, size, position, parent, callback, color)
    local button = Instance.new("TextButton")
    button.Size = size
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = color or Color3.fromRGB(0, 255, 0)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Parent = parent
    button.AutoButtonColor = true
    button.BorderSizePixel = 0
    button.BorderRadius = UDim.new(0, 12)  -- Bordas arredondadas

    -- Efeitos de animação nos botões
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end)
    
    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = color or Color3.fromRGB(0, 255, 0)
    end)

    -- Conectar o clique do botão
    button.MouseButton1Click:Connect(callback)
    return button
end

-- Botão de Fechar
local closeButton = createButton("X", UDim2.new(0, 50, 0, 50), UDim2.new(1, -55, 0, -25), mainFrame, closeUI, Color3.fromRGB(255, 0, 0))

-- Botão de Minimizar
local minimizeButton = createButton("-", UDim2.new(0, 50, 0, 50), UDim2.new(1, -110, 0, -25), mainFrame, minimizeUI, Color3.fromRGB(255, 255, 0))

-- Funções para controle do jogador (WalkSpeed, JumpPower)
local function changeWalkSpeed(amount)
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = humanoid.WalkSpeed + amount
    end
end

local function changeJumpPower(amount)
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.JumpPower = humanoid.JumpPower + amount
    end
end

-- Função Fly (do KiciaHook)
local isFlying = false
local function toggleFly()
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChild("Humanoid")
        if humanoid and not isFlying then
            isFlying = true
            humanoid.PlatformStand = true
            local bodyGyro = Instance.new("BodyGyro")
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
            bodyGyro.CFrame = character.HumanoidRootPart.CFrame
            bodyGyro.P = 3000
            bodyGyro.Parent = character.HumanoidRootPart
            bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
            bodyVelocity.Velocity = Vector3.new(0, 50, 0)  -- Velocidade para "levitar"
            bodyVelocity.Parent = character.HumanoidRootPart

            wait(0.1)  -- Espera para aplicar a velocidade

            -- Fica voando
            while isFlying do
                bodyGyro.CFrame = character.HumanoidRootPart.CFrame
                wait(0.1)
            end
        elseif humanoid then
            isFlying = false
            humanoid.PlatformStand = false
            -- Remover as forças aplicadas para o voo
            character.HumanoidRootPart:FindFirstChild("BodyGyro"):Destroy()
            character.HumanoidRootPart:FindFirstChild("BodyVelocity"):Destroy()
        end
    end
end

-- Botão Fly
local flyButton = createButton("Fly", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 300), mainFrame, toggleFly, Color3.fromRGB(0, 0, 255))

-- Adicionando controles de velocidade e pulo
local walkSpeedSlider = createButton("Walk Speed: 16", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 400), mainFrame, function()
    changeWalkSpeed(5)  -- Aumenta a velocidade de caminhada
    walkSpeedSlider.Text = "Walk Speed: " .. player.Character.Humanoid.WalkSpeed
end)

local jumpPowerSlider = createButton("Jump Power: 50", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 500), mainFrame, function()
    changeJumpPower(10)  -- Aumenta o poder de pulo
    jumpPowerSlider.Text = "Jump Power: " .. player.Character.Humanoid.JumpPower
end)

-- Função de teleporte para um ponto específico
local teleportButton = createButton("Teleport", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 600), mainFrame, function()
    player.Character:MoveTo(Vector3.new(100, 50, 100))  -- Teleporta para a posição (100, 50, 100)
end)

-- Função de Auto Bond (simulação)
local bondCollectionEnabled = false
local function toggleBondCollection()
    bondCollectionEnabled = not bondCollectionEnabled
    print("Auto Bond: " .. (bondCollectionEnabled and "Enabled" or "Disabled"))

    if bondCollectionEnabled then
        while bondCollectionEnabled do
            wait(1)  -- Coleta de bond (simulação)
            print("Coletando Bond...")
            -- Lógica de coleta de bond aqui
        end
    end
end

local autoBondButton = createButton("Auto Bond", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 700), mainFrame, toggleBondCollection, Color3.fromRGB(255, 165, 0))

-- Inicializa a interface
animateUI(mainFrame, UDim2.new(0.5, -200, 0.5, -300), UDim2.new(0, 400, 0, 600), 0.5)
