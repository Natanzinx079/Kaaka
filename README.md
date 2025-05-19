-- NatanHub | Dead Rails (Nathub Style)
-- Compatível com Delta Executor para Android

if not game:IsLoaded() then game.Loaded:Wait() end

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "NatanHub"

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 480, 0, 320)
main.Position = UDim2.new(0.5, -240, 0.5, -160)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)

local topbar = Instance.new("Frame", main)
topbar.Size = UDim2.new(1, 0, 0, 30)
topbar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
topbar.BorderSizePixel = 0
Instance.new("UICorner", topbar).CornerRadius = UDim.new(0, 8)

local title = Instance.new("TextLabel", topbar)
title.Size = UDim2.new(1, 0, 1, 0)
title.BackgroundTransparency = 1
title.Text = "  NatanHub | Dead Rails (0.3.1)"
title.TextColor3 = Color3.new(1,1,1)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local closeBtn = Instance.new("TextButton", topbar)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.Text = "🟥"
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.BackgroundTransparency = 1
closeBtn.Font = Enum.Font.Gotham
closeBtn.TextSize = 16

closeBtn.MouseButton1Click:Connect(function()
	gui:Destroy()
end)

local sideTabs = Instance.new("Frame", main)
sideTabs.Size = UDim2.new(0, 100, 1, -30)
sideTabs.Position = UDim2.new(0, 0, 0, 30)
sideTabs.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
sideTabs.BorderSizePixel = 0
Instance.new("UICorner", sideTabs).CornerRadius = UDim.new(0, 6)

local tabNames = {"Main", "Character", "Teleport", "Visual", "Combat", "Config"}
local pages = {}

for i, name in ipairs(tabNames) do
	local button = Instance.new("TextButton", sideTabs)
	button.Size = UDim2.new(1, 0, 0, 30)
	button.Position = UDim2.new(0, 0, 0, (i - 1) * 32)
	button.Text = name
	button.TextColor3 = Color3.new(1,1,1)
	button.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	button.Font = Enum.Font.Gotham
	button.TextSize = 14
	Instance.new("UICorner", button).CornerRadius = UDim.new(0, 6)

	local page = Instance.new("ScrollingFrame", main)
	page.Size = UDim2.new(1, -100, 1, -30)
	page.Position = UDim2.new(0, 100, 0, 30)
	page.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	page.Visible = (i == 1)
	page.BorderSizePixel = 0
	page.ScrollBarThickness = 6
	page.CanvasSize = UDim2.new(0, 0, 2, 0)
	page.AutomaticCanvasSize = Enum.AutomaticSize.Y
	page.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
	page.ClipsDescendants = true
	Instance.new("UICorner", page).CornerRadius = UDim.new(0, 6)

	local uiListLayout = Instance.new("UIListLayout", page)
	uiListLayout.Padding = UDim.new(0, 6)
	uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder

	pages[name] = page

	button.MouseButton1Click:Connect(function()
		for _, pg in pairs(pages) do pg.Visible = false end
		page.Visible = true
	end)
end

-- Exemplo de conteúdo em uma aba (Main)
local webhookLabel = Instance.new("TextLabel", pages["Main"])
webhookLabel.Size = UDim2.new(1, -20, 0, 20)
webhookLabel.Position = UDim2.new(0, 10, 0, 10)
webhookLabel.BackgroundTransparency = 1
webhookLabel.Text = "Webhook Link"
webhookLabel.TextColor3 = Color3.new(1,1,1)
webhookLabel.Font = Enum.Font.Gotham
webhookLabel.TextSize = 14
webhookLabel.TextXAlignment = Enum.TextXAlignment.Left

local webhookBox = Instance.new("TextBox", pages["Main"])
webhookBox.Size = UDim2.new(1, -20, 0, 30)
webhookBox.Position = UDim2.new(0, 10, 0, 36)
webhookBox.Text = "Your Webhook Link"
webhookBox.TextColor3 = Color3.new(1,1,1)
webhookBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
webhookBox.ClearTextOnFocus = false
webhookBox.Font = Enum.Font.Gotham
webhookBox.TextSize = 14
Instance.new("UICorner", webhookBox).CornerRadius = UDim.new(0, 6)

local autoBondLabel = Instance.new("TextLabel", pages["Main"])
autoBondLabel.Size = UDim2.new(1, -20, 0, 20)
autoBondLabel.Position = UDim2.new(0, 10, 0, 72)
autoBondLabel.BackgroundTransparency = 1
autoBondLabel.Text = "Auto Obrigação"
autoBondLabel.TextColor3 = Color3.new(1,1,1)
autoBondLabel.Font = Enum.Font.Gotham
autoBondLabel.TextSize = 14
autoBondLabel.TextXAlignment = Enum.TextXAlignment.Left

local autoBondToggle = Instance.new("TextButton", pages["Main"])
autoBondToggle.Size = UDim2.new(0, 100, 0, 30)
autoBondToggle.Position = UDim2.new(0, 10, 0, 98)
autoBondToggle.Text = "OFF"
autoBondToggle.TextColor3 = Color3.new(1,1,1)
autoBondToggle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
autoBondToggle.Font = Enum.Font.Gotham
autoBondToggle.TextSize = 14
Instance.new("UICorner", autoBondToggle).CornerRadius = UDim.new(0, 6)

local auto = false
autoBondToggle.MouseButton1Click:Connect(function()
	auto = not auto
	autoBondToggle.Text = auto and "ON" or "OFF"
end)

local winBtn = Instance.new("TextButton", pages["Main"])
winBtn.Size = UDim2.new(1, -20, 0, 30)
winBtn.Position = UDim2.new(0, 10, 0, 134)
winBtn.Text = "Auto Win"
winBtn.TextColor3 = Color3.new(1,1,1)
winBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
winBtn.Font = Enum.Font.GothamBold
winBtn.TextSize = 14
Instance.new("UICorner", winBtn).CornerRadius = UDim.new(0, 6)

winBtn.MouseButton1Click:Connect(function()
	-- lógica de vitória aqui
end)
