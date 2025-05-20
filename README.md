local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local Gui = Instance.new("ScreenGui", game.CoreGui)
Gui.Name = "NatHubUI"
Gui.ResetOnSpawn = false

local Main = Instance.new("Frame", Gui)
Main.Size = UDim2.new(0, 400, 0, 300)
Main.Position = UDim2.new(0.5, -200, 0.5, -150)
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 12)

local Header = Instance.new("TextLabel", Main)
Header.Size = UDim2.new(1, 0, 0, 36)
Header.BackgroundTransparency = 1
Header.Text = "NatHub | Dead Reais"
Header.Font = Enum.Font.GothamBold
Header.TextSize = 16
Header.TextColor3 = Color3.fromRGB(255, 255, 255)

local MinBtn = Instance.new("TextButton", Main)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -65, 0, 3)
MinBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
MinBtn.Text = "-"
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.TextSize = 20
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(1, 0)

local CloseBtn = Instance.new("TextButton", Main)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 3)
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(1, 0)

local ContentGroup = Instance.new("Frame", Main)
ContentGroup.Size = UDim2.new(1, 0, 1, -36)
ContentGroup.Position = UDim2.new(0, 0, 0, 36)
ContentGroup.BackgroundTransparency = 1

CloseBtn.MouseButton1Click:Connect(function()
	Gui:Destroy()
end)

-- Botão de restaurar
local RestoreBtn = Instance.new("TextButton", Gui)
RestoreBtn.Size = UDim2.new(0, 80, 0, 30)
RestoreBtn.Position = UDim2.new(0.5, -40, 0.5, -15)
RestoreBtn.Text = "NatHub"
RestoreBtn.Visible = false
RestoreBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
RestoreBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
RestoreBtn.Font = Enum.Font.GothamBold
RestoreBtn.TextSize = 14
Instance.new("UICorner", RestoreBtn).CornerRadius = UDim.new(0, 8)

-- Corrigido: Animação de minimizar com botão de restaurar
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
	if not minimized then
		minimized = true
		local tweenOut = TweenService:Create(Main, TweenInfo.new(0.3), {
			Size = UDim2.new(0, 0, 0, 0),
			Position = UDim2.new(0.5, 0, 0.5, 0)
		})
		tweenOut:Play()
		tweenOut.Completed:Once(function()
			Main.Visible = false
			RestoreBtn.Visible = true
		end)
	end
end)

RestoreBtn.MouseButton1Click:Connect(function()
	RestoreBtn.Visible = false
	Main.Visible = true
	Main.Size = UDim2.new(0, 0, 0, 0)
	Main.Position = UDim2.new(0.5, 0, 0.5, 0)
	local tweenIn = TweenService:Create(Main, TweenInfo.new(0.3), {
		Size = UDim2.new(0, 400, 0, 300),
		Position = UDim2.new(0.5, -200, 0.5, -150)
	})
	tweenIn:Play()
	minimized = false
end)

-- Tabs
local SideBar = Instance.new("Frame", ContentGroup)
SideBar.Size = UDim2.new(0, 50, 1, 0)
SideBar.Position = UDim2.new(0, 0, 0, 0)
SideBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Instance.new("UICorner", SideBar).CornerRadius = UDim.new(0, 10)

local UIList = Instance.new("UIListLayout", SideBar)
UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIList.SortOrder = Enum.SortOrder.LayoutOrder
UIList.Padding = UDim.new(0, 5)

local Tabs = {}
local CurrentTab = nil
local tabList = {
	{Name = "Misc", Icon = "rbxassetid://6031075938"},
	{Name = "Player", Icon = "rbxassetid://6031265976"},
}

for _, tab in ipairs(tabList) do
	local Btn = Instance.new("ImageButton", SideBar)
	Btn.Size = UDim2.new(0, 40, 0, 40)
	Btn.Image = tab.Icon
	Btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	Instance.new("UICorner", Btn).CornerRadius = UDim.new(1, 0)

	local Page = Instance.new("Frame", ContentGroup)
	Page.Name = tab.Name.."Page"
	Page.Size = UDim2.new(1, -60, 1, 0)
	Page.Position = UDim2.new(0, 60, 0, 0)
	Page.BackgroundTransparency = 1
	Page.Visible = false

	Tabs[tab.Name] = Page

	Btn.MouseButton1Click:Connect(function()
		if CurrentTab then Tabs[CurrentTab].Visible = false end
		Page.Visible = true
		CurrentTab = tab.Name
	end)
end

Tabs["Misc"].Visible = true
CurrentTab = "Misc"

-- Slider funcional
local function CreateSlider(tab, name, min, max, default, callback)
	local Frame = Instance.new("Frame", tab)
	Frame.Size = UDim2.new(1, -20, 0, 60)
	Frame.Position = UDim2.new(0, 10, 0, 10)
	Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 8)

	local Title = Instance.new("TextLabel", Frame)
	Title.Text = name.." ("..default..")"
	Title.Font = Enum.Font.GothamBold
	Title.TextSize = 14
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.BackgroundTransparency = 1
	Title.Size = UDim2.new(1, -10, 0, 20)
	Title.Position = UDim2.new(0, 5, 0, 0)

	local Bar = Instance.new("Frame", Frame)
	Bar.Size = UDim2.new(1, -20, 0, 10)
	Bar.Position = UDim2.new(0, 10, 0, 30)
	Bar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Instance.new("UICorner", Bar).CornerRadius = UDim.new(0, 4)

	local Fill = Instance.new("Frame", Bar)
	Fill.Name = "Fill"
	Fill.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
	Fill.Size = UDim2.new((default - min)/(max - min), 0, 1, 0)
	Fill.Position = UDim2.new(0, 0, 0, 0)
	Fill.BorderSizePixel = 0
	Fill.ZIndex = 2
	Instance.new("UICorner", Fill).CornerRadius = UDim.new(0, 4)

	local dragging = false

	local function update(input)
		local pct = math.clamp((input.Position.X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
		Fill.Size = UDim2.new(pct, 0, 1, 0)
		local value = math.floor(min + (max - min) * pct)
		Title.Text = name.." ("..value..")"
		callback(value)
	end

	Bar.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = true
			Main.Active = false
			update(input)
		end
	end)

	UIS.InputChanged:Connect(function(input)
		if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
			update(input)
		end
	end)

	UIS.InputEnded:Connect(function(input)
		if dragging then
			dragging = false
			Main.Active = true
		end
	end)
end

CreateSlider(Tabs["Misc"], "WallSpeed", 1, 100, 25, function(val)
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		char.Humanoid.WalkSpeed = val
	end
end)

-- Função: ESP de NPCs (ativar/desativar com ícone)
local npcESPEnabled = false
local npcESPConnections = {}

local function createNPCESP(npc)
	if not npcESPEnabled then return end
	local billboard = Instance.new("BillboardGui", npc:FindFirstChild("Head") or npc)
	billboard.Name = "NPC_ESP"
	billboard.Size = UDim2.new(0, 100, 0, 30)
	billboard.AlwaysOnTop = true
	local label = Instance.new("TextLabel", billboard)
	label.Size = UDim2.new(1, 0, 1, 0)
	label.BackgroundTransparency = 1
	label.Text = "NPC"
	label.Font = Enum.Font.GothamBold
	label.TextColor3 = Color3.fromRGB(255, 50, 50)
	label.TextStrokeTransparency = 0.7
	label.TextSize = 14
end

local function toggleNPCESP()
	npcESPEnabled = not npcESPEnabled

	for _, conn in ipairs(npcESPConnections) do
		conn:Disconnect()
	end
	npcESPConnections = {}

	for _, npc in ipairs(workspace:GetDescendants()) do
		if npc:IsA("Model") and npc:FindFirstChild("Humanoid") and npc:FindFirstChild("Head") and not Players:GetPlayerFromCharacter(npc) then
			if npcESPEnabled then
				if not npc:FindFirstChild("NPC_ESP") then
					createNPCESP(npc)
				end
			else
				local gui = npc:FindFirstChild("NPC_ESP")
				if gui then gui:Destroy() end
			end
		end
	end

	if npcESPEnabled then
		table.insert(npcESPConnections, workspace.DescendantAdded:Connect(function(obj)
			if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj:FindFirstChild("Head") and not Players:GetPlayerFromCharacter(obj) then
				task.wait(0.2)
				createNPCESP(obj)
			end
		end))
	end
end

-- Botão com ícone (estilo NatHub)
local function CreateIconButton(tab, tooltip, iconId, callback)
	local Btn = Instance.new("ImageButton", tab)
	Btn.Size = UDim2.new(0, 40, 0, 40)
	Btn.Image = iconId
	Btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	Instance.new("UICorner", Btn).CornerRadius = UDim.new(1, 0)

	local tip = Instance.new("TextLabel", Btn)
	tip.Text = tooltip
	tip.TextColor3 = Color3.fromRGB(255, 255, 255)
	tip.TextSize = 12
	tip.Font = Enum.Font.Gotham
	tip.BackgroundTransparency = 1
	tip.Size = UDim2.new(1, 0, 0, 14)
	tip.Position = UDim2.new(0, 0, 1, 0)

	Btn.MouseButton1Click:Connect(callback)
end

-- Adiciona botão ESP NPC na aba Misc
CreateIconButton(Tabs["Misc"], "ESP NPC", "rbxassetid://6031068426", toggleNPCESP)
