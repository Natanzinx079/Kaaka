-- NatHub Interface | Compatível com Delta e Android

local player = game.Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local DraggableMain = Instance.new("Frame")
DraggableMain.Size = UDim2.new(0, 450, 0, 280)
DraggableMain.Position = UDim2.new(0.3, 0, 0.2, 0)
DraggableMain.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
DraggableMain.BackgroundTransparency = 0.1
DraggableMain.Active = true
DraggableMain.Parent = ScreenGui

local dragToggle, dragInput, dragStart, startPos
DraggableMain.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragToggle = true
		dragStart = input.Position
		startPos = DraggableMain.Position
	end
end)

DraggableMain.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragToggle = false
	end
end)

DraggableMain.InputChanged:Connect(function(input)
	if dragToggle and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		DraggableMain.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

local UICorner = Instance.new("UICorner", DraggableMain)
UICorner.CornerRadius = UDim.new(0, 8)

local UIStroke = Instance.new("UIStroke", DraggableMain)
UIStroke.Color = Color3.fromRGB(60, 60, 60)
UIStroke.Thickness = 1.5

local Title = Instance.new("TextLabel", DraggableMain)
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.Size = UDim2.new(1, -50, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.BackgroundTransparency = 1

local Close = Instance.new("TextButton", DraggableMain)
Close.Text = "✕"
Close.Size = UDim2.new(0, 25, 0, 25)
Close.Position = UDim2.new(1, -30, 0, 5)
Close.Font = Enum.Font.Gotham
Close.TextSize = 16
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.BackgroundTransparency = 1
Close.MouseButton1Click:Connect(function()
	DraggableMain.Visible = false
end)

local Minimize = Instance.new("TextButton", DraggableMain)
Minimize.Text = "–"
Minimize.Size = UDim2.new(0, 25, 0, 25)
Minimize.Position = UDim2.new(1, -60, 0, 5)
Minimize.Font = Enum.Font.Gotham
Minimize.TextSize = 18
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.BackgroundTransparency = 1

local Tabs = Instance.new("Frame", DraggableMain)
Tabs.Size = UDim2.new(0, 120, 1, -40)
Tabs.Position = UDim2.new(0, 10, 0, 40)
Tabs.BackgroundTransparency = 1

local UIListLayout = Instance.new("UIListLayout", Tabs)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)

local Content = Instance.new("Frame", DraggableMain)
Content.Size = UDim2.new(1, -140, 1, -50)
Content.Position = UDim2.new(0, 130, 0, 40)
Content.BackgroundTransparency = 1

Minimize.MouseButton1Click:Connect(function()
	Content.Visible = not Content.Visible
end)

local tabs = {
	{"Main", 6031274630},
	{"Character", 6031265976},
	{"Teleport", 6031215984},
	{"Visual", 6034509999},
	{"Combat", 6031260785},
	{"Configuration", 6031280882}
}

for _, tab in ipairs(tabs) do
	local Button = Instance.new("TextButton", Tabs)
	Button.Size = UDim2.new(1, -5, 0, 30)
	Button.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	Button.TextColor3 = Color3.fromRGB(255, 255, 255)
	Button.Font = Enum.Font.Gotham
	Button.TextSize = 14
	Button.Text = "    " .. tab[1]
	Button.TextXAlignment = Enum.TextXAlignment.Left

	local Icon = Instance.new("ImageLabel", Button)
	Icon.Size = UDim2.new(0, 18, 0, 18)
	Icon.Position = UDim2.new(0, 6, 0.5, -9)
	Icon.BackgroundTransparency = 1
	Icon.Image = "rbxassetid://" .. tostring(tab[2])
end

local MainContent = Instance.new("Frame", Content)
MainContent.Size = UDim2.new(1, 0, 1, 0)
MainContent.BackgroundTransparency = 1
MainContent.Visible = true

local function createBlock(titleText, descText, hasToggle)
	local Frame = Instance.new("Frame", MainContent)
	Frame.Size = UDim2.new(1, -10, 0, 60)
	Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	Frame.Position = UDim2.new(0, 5, 0, #MainContent:GetChildren() * 65)
	Frame.BorderSizePixel = 0

	Instance.new("UICorner", Frame)

	local Title = Instance.new("TextLabel", Frame)
	Title.Text = titleText
	Title.Font = Enum.Font.GothamSemibold
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextSize = 14
	Title.BackgroundTransparency = 1
	Title.Size = UDim2.new(1, -10, 0, 20)
	Title.Position = UDim2.new(0, 5, 0, 5)
	Title.TextXAlignment = Enum.TextXAlignment.Left

	local Desc = Instance.new("TextLabel", Frame)
	Desc.Text = descText
	Desc.Font = Enum.Font.Gotham
	Desc.TextColor3 = Color3.fromRGB(180, 180, 180)
	Desc.TextSize = 12
	Desc.BackgroundTransparency = 1
	Desc.Size = UDim2.new(1, -10, 0, 20)
	Desc.Position = UDim2.new(0, 5, 0, 25)
	Desc.TextXAlignment = Enum.TextXAlignment.Left

	if hasToggle then
		local Toggle = Instance.new("TextButton", Frame)
		Toggle.Size = UDim2.new(0, 20, 0, 20)
		Toggle.Position = UDim2.new(1, -30, 0.5, -10)
		Toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		Toggle.Text = ""
		Instance.new("UICorner", Toggle)
	end
end

createBlock("Webhook Link", "Webhook will be sent after match end!", false)
createBlock("Auto Bond", "Automatically collect bond fast", true)
createBlock("Auto Win", "Automatically win.", true)
