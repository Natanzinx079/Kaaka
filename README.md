local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.IgnoreGuiInset = true
ScreenGui.Name = "NatHub"

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 400, 0, 250)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -125)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Parent = ScreenGui
MainFrame.Visible = true

-- UICorner
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame

-- TopBar
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local UICornerBar = Instance.new("UICorner", TopBar)
UICornerBar.CornerRadius = UDim.new(0, 12)

-- Title
local Title = Instance.new("TextLabel")
Title.Text = "NatHub | DBacon"
Title.Font = Enum.Font.GothamSemibold
Title.TextSize = 14
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -60, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

-- Close Button (X)
local CloseBtn = Instance.new("TextButton")
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.Gotham
CloseBtn.TextSize = 14
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -60, 0, 0)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Parent = TopBar

-- Minimize Button (-)
local MinBtn = Instance.new("TextButton")
MinBtn.Text = "-"
MinBtn.Font = Enum.Font.Gotham
MinBtn.TextSize = 20
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -30, 0, 0)
MinBtn.BackgroundTransparency = 1
MinBtn.Parent = TopBar

-- Side Menu
local SideMenu = Instance.new("Frame")
SideMenu.Name = "SideMenu"
SideMenu.Size = UDim2.new(0, 100, 1, -30)
SideMenu.Position = UDim2.new(0, 0, 0, 30)
SideMenu.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
SideMenu.BorderSizePixel = 0
SideMenu.Parent = MainFrame

local UICornerMenu = Instance.new("UICorner", SideMenu)
UICornerMenu.CornerRadius = UDim.new(0, 8)

-- Main Button (aba)
local MainButton = Instance.new("TextButton")
MainButton.Text = "Main"
MainButton.Size = UDim2.new(1, 0, 0, 40)
MainButton.Position = UDim2.new(0, 0, 0, 0)
MainButton.Font = Enum.Font.Gotham
MainButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MainButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainButton.BorderSizePixel = 0
MainButton.Parent = SideMenu

-- Main Content Area
local MainContent = Instance.new("Frame")
MainContent.Name = "MainContent"
MainContent.Size = UDim2.new(1, -100, 1, -30)
MainContent.Position = UDim2.new(0, 100, 0, 30)
MainContent.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainContent.BorderSizePixel = 0
MainContent.Parent = MainFrame

local UICornerContent = Instance.new("UICorner", MainContent)
UICornerContent.CornerRadius = UDim.new(0, 8)

-- Funcionalidade do botão X
CloseBtn.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

-- Funcionalidade do botão -
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
	if minimized then
		MainFrame:TweenSize(UDim2.new(0, 400, 0, 250), "Out", "Quad", 0.3, true)
		wait(0.3)
		SideMenu.Visible = true
		MainContent.Visible = true
	else
		SideMenu.Visible = false
		MainContent.Visible = false
		MainFrame:TweenSize(UDim2.new(0, 400, 0, 30), "Out", "Quad", 0.3, true)
	end
	minimized = not minimized
end)
