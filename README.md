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

-- Função para verificar se a posição de destino está segura
local function isPositionSafe(position)
    local part, position = workspace:FindPartOnRay(Ray.new(position, Vector3.new(0, -1, 0)), player.Character)
    if part then
        return false
    end
    return true
end

-- Função de teleporte para uma posição segura
local function teleportToPositionSafe(position)
    if isPositionSafe(position) then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(position)
    else
        print("Posição não segura para o teleporte.")
    end
end

-- Posições de Tesla e Cidade Abandonada
local teslaPosition = Vector3.new(150, 50, -200)  -- Substitua com as coordenadas corretas do Tesla
local cidadeAbandonadaPosition = Vector3.new(-300, 50, 400)  -- Substitua com as coordenadas corretas da Cidade Abandonada

-- Função de Teleporte para o Tesla
local function teleportToTesla()
    teleportToPositionSafe(teslaPosition)
end

-- Função de Teleporte para a Cidade Abandonada
local function teleportToCidadeAbandonada()
    teleportToPositionSafe(cidadeAbandonadaPosition)
end

-- Botões de Teleporte
local teslaButton = createButton("Teleport to Tesla", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 300), mainFrame, teleportToTesla, Color3.fromRGB(0, 255, 255))
local cidadeAbandonadaButton = createButton("Teleport to Cidade Abandonada", UDim2.new(0, 200, 0, 50), UDim2.new(0, 100, 0, 400), mainFrame, teleportToCidadeAbandonada, Color3.fromRGB(255, 165, 0))

-- Inicializa a interface
animateUI(mainFrame, UDim2.new(0.5, -200, 0.5, -300), UDim2.new(0, 400, 0, 600), 0.5)
