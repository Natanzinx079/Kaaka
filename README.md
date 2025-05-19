-- Natan Dead Rails - Nathub Style (com scroll e altura maior)
local ScreenGui = Instance.new("ScreenGui", game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "NatanHub"
ScreenGui.ResetOnSpawn = false

-- Main Frame
local main = Instance.new("Frame", ScreenGui)
main.Size = UDim2.new(0, 420, 0, 400)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.BorderSizePixel = 0
main.Visible = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

-- Top bar
local top = Instance.new("Frame", main)
top.Size = UDim2.new(1, 0, 0, 30)
top.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
top.BorderSizePixel = 0
Instance.new("UICorner", top).CornerRadius = UDim.new(0, 10)

-- Title
local title = Instance.new("TextLabel", top)
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "NatanHub | Dead Rails (0.3.1)"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 16
title.Font = Enum.Font.GothamBold
title.BackgroundTransparency = 1
title.TextXAlignment = Enum.TextXAlignment.Left

-- Close & Minimize Buttons
local close = Instance.new("TextButton", top)
close.Size = UDim2.new(0, 30, 1, 0)
close.Position = UDim2.new(1, -35, 0, 0)
close.Text = "X"
close.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
close.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", close).CornerRadius = UDim.new(0, 6)

local minimize = Instance.new("TextButton", top)
minimize.Size = UDim2.new(0, 30, 1, 0)
minimize.Position = UDim2.new(1, -70, 0, 0)
minimize.Text = "-"
minimize.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
minimize.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", minimize).CornerRadius = UDim.new(0, 6)

-- Side Tabs
local sideTabs = Instance.new("Frame", main)
sideTabs.Size = UDim2.new(0, 100, 1, -30)
sideTabs.Position = UDim2.new(0, 0, 0, 30)
sideTabs.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
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
	page.ScrollBarThickness = 4
	page.CanvasSize = UDim2.new(0, 0, 2, 0)
	page.Visible = (i == 1)
	Instance.new("UICorner", page).CornerRadius = UDim.new(0, 6)
	pages[name] = page

	button.MouseButton1Click:Connect(function()
		for _, pg in pairs(pages) do pg.Visible = false end
		page.Visible = true
	end)
end

-- Sliders na aba Character
local charPage = pages["Character"]

local function createSlider(parent, name, min, max, default, callback)
	local y = #parent:GetChildren() * 30

	local label = Instance.new("TextLabel", parent)
	label.Size = UDim2.new(0, 200, 0, 20)
	label.Position = UDim2.new(0, 10, 0, y)
	label.Text = name .. ": " .. default
	label.TextColor3 = Color3.new(1,1,1)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.Gotham
	label.TextSize = 14

	local box = Instance.new("TextBox", parent)
	box.Size = UDim2.new(0, 60, 0, 20)
	box.Position = UDim2.new(0, 220, 0, y)
	box.Text = tostring(default)
	box.TextColor3 = Color3.new(1,1,1)
	box.BackgroundColor3 = Color3.fromRGB(50,50,50)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)

	box.FocusLost:Connect(function()
		local num = tonumber(box.Text)
		if num then
			num = math.clamp(num, min, max)
			callback(num)
			label.Text = name .. ": " .. num
		end
	end)
end

createSlider(charPage, "WalkSpeed", 16, 100, 16, function(value)
	game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)

createSlider(charPage, "JumpPower", 50, 200, 50, function(value)
	game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
end)

-- Minimizar/Restaurar
local miniBtn = Instance.new("TextButton", ScreenGui)
miniBtn.Size = UDim2.new(0, 100, 0, 30)
miniBtn.Position = UDim2.new(0, 10, 0.4, 0)
miniBtn.Text = "NatanHub"
miniBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
miniBtn.TextColor3 = Color3.new(1,1,1)
miniBtn.Visible = false
Instance.new("UICorner", miniBtn).CornerRadius = UDim.new(0, 6)

minimize.MouseButton1Click:Connect(function()
	main.Visible = false
	miniBtn.Visible = true
end)

miniBtn.MouseButton1Click:Connect(function()
	main.Visible = true
	miniBtn.Visible = false
end)

close.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)
