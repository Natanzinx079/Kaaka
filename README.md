local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatHub"
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false

-- Draggable main frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 500, 0, 320)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

-- TopBar
local TopBar = Instance.new("Frame", MainFrame)
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TopBar.BorderSizePixel = 0

local Title = Instance.new("TextLabel", TopBar)
Title.Text = "NatHub | DBacon"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -60, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Botões
local CloseBtn = Instance.new("TextButton", TopBar)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 16
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Size = UDim2.new(0, 30, 0, 35)
CloseBtn.Position = UDim2.new(1, -60, 0, 0)
CloseBtn.BackgroundTransparency = 1

local MinBtn = Instance.new("TextButton", TopBar)
MinBtn.Text = "-"
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 18
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Size = UDim2.new(0, 30, 0, 35)
MinBtn.Position = UDim2.new(1, -30, 0, 0)
MinBtn.BackgroundTransparency = 1

-- Side Menu com abas
local SideMenu = Instance.new("Frame", MainFrame)
SideMenu.Size = UDim2.new(0, 110, 1, -35)
SideMenu.Position = UDim2.new(0, 0, 0, 35)
SideMenu.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
SideMenu.BorderSizePixel = 0

local UICornerMenu = Instance.new("UICorner", SideMenu)
UICornerMenu.CornerRadius = UDim.new(0, 8)

local tabNames = {"Main", "Character", "Teleport", "Combat", "Visual", "Config"}
for i, name in ipairs(tabNames) do
	local tab = Instance.new("TextButton", SideMenu)
	tab.Size = UDim2.new(1, 0, 0, 35)
	tab.Position = UDim2.new(0, 0, 0, (i - 1) * 37)
	tab.Text = name
	tab.Font = Enum.Font.GothamBold
	tab.TextSize = 14
	tab.TextColor3 = Color3.fromRGB(255, 255, 255)
	tab.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	tab.BorderSizePixel = 0
end

-- Área de conteúdo
local MainContent = Instance.new("Frame", MainFrame)
MainContent.Name = "MainContent"
MainContent.Size = UDim2.new(1, -110, 1, -35)
MainContent.Position = UDim2.new(0, 110, 0, 35)
MainContent.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainContent.BorderSizePixel = 0

local UICornerContent = Instance.new("UICorner", MainContent)
UICornerContent.CornerRadius = UDim.new(0, 8)

-- Funcionalidade X
CloseBtn.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

-- Funcionalidade Minimizar
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
	if minimized then
		MainFrame:TweenSize(UDim2.new(0, 500, 0, 320), "Out", "Quad", 0.3, true)
		wait(0.3)
		SideMenu.Visible = true
		MainContent.Visible = true
	else
		SideMenu.Visible = false
		MainContent.Visible = false
		MainFrame:TweenSize(UDim2.new(0, 500, 0, 35), "Out", "Quad", 0.3, true)
	end
	minimized = not minimized
end)
