local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.ResetOnSpawn = false

-- Estado do menu
local isMinimized = false

-- Container principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 420, 0, 260)
mainFrame.Position = UDim2.new(0.5, -210, 0.5, -130)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Parent = ScreenGui
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.BackgroundTransparency = 0.1

-- UI extras
local uiCorner = Instance.new("UICorner", mainFrame)
uiCorner.CornerRadius = UDim.new(0, 12)
local uiStroke = Instance.new("UIStroke", mainFrame)
uiStroke.Color = Color3.fromRGB(60, 60, 60)
uiStroke.Thickness = 1.2

-- Botão minimizar
local minimize = Instance.new("TextButton")
minimize.Size = UDim2.new(0, 25, 0, 25)
minimize.Position = UDim2.new(1, -30, 0, 5)
minimize.Text = "-"
minimize.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 18
minimize.Parent = mainFrame

Instance.new("UICorner", minimize).CornerRadius = UDim.new(0, 6)

-- Label superior
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -40, 0, 30)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.Text = "NatHub | DBacon"
titleLabel.Font = Enum.Font.GothamSemibold
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.BackgroundTransparency = 1
titleLabel.TextSize = 16
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = mainFrame

-- Lado esquerdo (menu de abas)
local sideMenu = Instance.new("Frame")
sideMenu.Size = UDim2.new(0, 80, 1, -40)
sideMenu.Position = UDim2.new(0, 0, 0, 40)
sideMenu.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
sideMenu.Parent = mainFrame

Instance.new("UICorner", sideMenu).CornerRadius = UDim.new(0, 8)

-- Botão "Main"
local mainBtn = Instance.new("TextButton")
mainBtn.Size = UDim2.new(1, 0, 0, 40)
mainBtn.Position = UDim2.new(0, 0, 0, 0)
mainBtn.Text = "Main"
mainBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
mainBtn.Font = Enum.Font.Gotham
mainBtn.TextSize = 14
mainBtn.Parent = sideMenu

Instance.new("UICorner", mainBtn).CornerRadius = UDim.new(0, 6)

-- Função para animar visibilidade
local function toggleMenu()
	isMinimized = not isMinimized
	for _, v in ipairs(mainFrame:GetChildren()) do
		if v ~= minimize then
			if isMinimized then
				game.TweenService:Create(v, TweenInfo.new(0.25), {Transparency = 1, Position = v.Position + UDim2.new(0, 0, 0, 10)}):Play()
				wait(0.25)
				v.Visible = false
			else
				v.Visible = true
				game.TweenService:Create(v, TweenInfo.new(0.25), {Transparency = 0, Position = v.Position - UDim2.new(0, 0, 0, 10)}):Play()
			end
		end
	end
end

-- Conectar botão de minimizar
minimize.MouseButton1Click:Connect(toggleMenu)
