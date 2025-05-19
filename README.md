local gui = Instance.new("ScreenGui")
gui.Name = "NatHub"
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = game:FindService("CoreGui")

local main = Instance.new("Frame")
main.Size = UDim2.new(0, 450, 0, 280)
main.Position = UDim2.new(0.5, -225, 0.5, -140)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
main.Active = true
main.Draggable = true
main.Parent = gui

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", main).Color = Color3.fromRGB(60, 60, 60)

local title = Instance.new("TextLabel", main)
title.Text = "NatHub | Dead Rails (0.3.1)"
title.Size = UDim2.new(1, -60, 0, 30)
title.Position = UDim2.new(0, 10, 0, 5)
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.TextColor3 = Color3.new(1, 1, 1)
title.TextXAlignment = Enum.TextXAlignment.Left
title.BackgroundTransparency = 1

local close = Instance.new("TextButton", main)
close.Text = "✕"
close.Size = UDim2.new(0, 25, 0, 25)
close.Position = UDim2.new(1, -30, 0, 5)
close.Font = Enum.Font.Gotham
close.TextSize = 16
close.TextColor3 = Color3.new(1, 1, 1)
close.BackgroundTransparency = 1
close.MouseButton1Click:Connect(function()
	main:Destroy()
end)

local minimize = Instance.new("TextButton", main)
minimize.Text = "–"
minimize.Size = UDim2.new(0, 25, 0, 25)
minimize.Position = UDim2.new(1, -60, 0, 5)
minimize.Font = Enum.Font.Gotham
minimize.TextSize = 18
minimize.TextColor3 = Color3.new(1, 1, 1)
minimize.BackgroundTransparency = 1

local contentFrame = Instance.new("Frame", main)
contentFrame.Position = UDim2.new(0, 130, 0, 40)
contentFrame.Size = UDim2.new(1, -140, 1, -50)
contentFrame.BackgroundTransparency = 1

local tabs = {
	{"Main", 6031274630},
	{"Character", 6031265976},
	{"Teleport", 6031215984},
	{"Visual", 6034509999},
	{"Combat", 6031260785},
	{"Configuration", 6031280882}
}

local tabsFrame = Instance.new("Frame", main)
tabsFrame.Size = UDim2.new(0, 120, 1, -40)
tabsFrame.Position = UDim2.new(0, 10, 0, 40)
tabsFrame.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", tabsFrame)
layout.Padding = UDim.new(0, 6)
layout.SortOrder = Enum.SortOrder.LayoutOrder

local tabContents = {}

local function createTabContent(name)
	local frame = Instance.new("Frame", contentFrame)
	frame.Size = UDim2.new(1, 0, 1, 0)
	frame.BackgroundTransparency = 1
	frame.Visible = false
	tabContents[name] = frame
	return frame
end

local function switchTab(name)
	for tabName, frame in pairs(tabContents) do
		frame.Visible = (tabName == name)
	end
end

for _, tabData in pairs(tabs) do
	local tabName, iconId = unpack(tabData)
	local button = Instance.new("TextButton", tabsFrame)
	button.Size = UDim2.new(1, -5, 0, 30)
	button.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.Font = Enum.Font.Gotham
	button.TextSize = 14
	button.Text = "    " .. tabName
	button.TextXAlignment = Enum.TextXAlignment.Left

	local icon = Instance.new("ImageLabel", button)
	icon.Size = UDim2.new(0, 18, 0, 18)
	icon.Position = UDim2.new(0, 6, 0.5, -9)
	icon.BackgroundTransparency = 1
	icon.Image = "rbxassetid://" .. tostring(iconId)

	local content = createTabContent(tabName)
	button.MouseButton1Click:Connect(function()
		switchTab(tabName)
	end)
end

switchTab("Main")

-- Conteúdo de exemplo para aba Main
local function addExampleContent(parent, text)
	local label = Instance.new("TextLabel", parent)
	label.Text = text
	label.Font = Enum.Font.Gotham
	label.TextColor3 = Color3.new(1, 1, 1)
	label.TextSize = 14
	label.BackgroundTransparency = 1
	label.Size = UDim2.new(1, -10, 0, 25)
	label.Position = UDim2.new(0, 5, 0, 5 + (#parent:GetChildren()-1)*30)
end

addExampleContent(tabContents["Main"], "Auto Bond: ON")
addExampleContent(tabContents["Main"], "Auto Win: ON")
addExampleContent(tabContents["Main"], "Webhook: https://...")

-- Minimizar tudo com bolinha
local minimized = false
local restoreButton = Instance.new("TextButton")
restoreButton.Text = "+"
restoreButton.Size = UDim2.new(0, 30, 0, 30)
restoreButton.Position = UDim2.new(0, 10, 0, 10)
restoreButton.TextSize = 20
restoreButton.Font = Enum.Font.GothamBold
restoreButton.TextColor3 = Color3.new(1, 1, 1)
restoreButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
restoreButton.Visible = false
restoreButton.ZIndex = 9999
Instance.new("UICorner", restoreButton).CornerRadius = UDim.new(1, 0)
restoreButton.Parent = gui

minimize.MouseButton1Click:Connect(function()
	minimized = true
	main.Visible = false
	restoreButton.Visible = true
end)

restoreButton.MouseButton1Click:Connect(function()
	minimized = false
	main.Visible = true
	restoreButton.Visible = false
end)
