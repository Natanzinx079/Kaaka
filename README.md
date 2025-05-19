-- Aba "Main" estilo Nathub
local mainPage = pages["Main"]

local function createSection(parent, titleText)
	local sectionTitle = Instance.new("TextLabel", parent)
	sectionTitle.Size = UDim2.new(1, -20, 0, 20)
	sectionTitle.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	sectionTitle.Text = titleText
	sectionTitle.Font = Enum.Font.GothamBold
	sectionTitle.TextSize = 14
	sectionTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
	sectionTitle.BackgroundTransparency = 1
	sectionTitle.TextXAlignment = Enum.TextXAlignment.Left
	return sectionTitle.Position.Y.Offset + 25
end

local function createInputBox(parent, placeholder)
	local box = Instance.new("TextBox", parent)
	box.Size = UDim2.new(1, -20, 0, 30)
	box.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	box.PlaceholderText = placeholder
	box.Text = ""
	box.TextColor3 = Color3.new(1,1,1)
	box.BackgroundColor3 = Color3.fromRGB(40,40,40)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	box.ClearTextOnFocus = false
	Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
	return box
end

local function createToggle(parent, labelText, callback)
	local frame = Instance.new("Frame", parent)
	frame.Size = UDim2.new(1, -20, 0, 30)
	frame.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	frame.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", frame)
	label.Size = UDim2.new(1, -40, 1, 0)
	label.Position = UDim2.new(0, 0, 0, 0)
	label.Text = labelText
	label.Font = Enum.Font.Gotham
	label.TextSize = 14
	label.TextColor3 = Color3.fromRGB(255,255,255)
	label.BackgroundTransparency = 1
	label.TextXAlignment = Enum.TextXAlignment.Left

	local toggle = Instance.new("TextButton", frame)
	toggle.Size = UDim2.new(0, 30, 0, 20)
	toggle.Position = UDim2.new(1, -30, 0.5, -10)
	toggle.BackgroundColor3 = Color3.fromRGB(60,60,60)
	toggle.Text = ""
	toggle.AutoButtonColor = false
	Instance.new("UICorner", toggle).CornerRadius = UDim.new(0, 6)

	local enabled = false
	toggle.MouseButton1Click:Connect(function()
		enabled = not enabled
		toggle.BackgroundColor3 = enabled and Color3.fromRGB(0, 170, 255) or Color3.fromRGB(60,60,60)
		if callback then callback(enabled) end
	end)
end

local function createButton(parent, labelText, callback)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	btn.Text = labelText
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.TextColor3 = Color3.fromRGB(255,255,255)
	btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
	btn.MouseButton1Click:Connect(function()
		if callback then callback() end
	end)
end

-- Adicionando as seções como no Nathub
createSection(mainPage, "Webhook Link")
createInputBox(mainPage, "Your Webhook Link")

createSection(mainPage, "Auto Bond")
createToggle(mainPage, "Automatically collect bond fast", function(state)
	if state then
		-- Código para ativar Auto Bond
	else
		-- Código para desativar Auto Bond
	end
end)

createSection(mainPage, "Win")
createButton(mainPage, "Auto Win", function()
	-- Código para Auto Win aqui
end)
