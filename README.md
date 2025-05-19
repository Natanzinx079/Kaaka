--// NatHub Dead Rails 0.3.1 Estilo Visual Igual ao da Imagem - Parte 1: Interface Básica //

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local MinimizeButton = Instance.new("TextButton")
local TabsHolder = Instance.new("Frame")
local UIListLayout = Instance.new("UIListLayout")

-- Abas
local Tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local TabButtons = {}

ScreenGui.Name = "NatHub_GUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.Size = UDim2.new(0, 500, 0, 300)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Draggable = true

TopBar.Name = "TopBar"
TopBar.Parent = MainFrame
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
TopBar.BorderSizePixel = 0

Title.Name = "Title"
Title.Parent = TopBar
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.Size = UDim2.new(1, -60, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Font = Enum.Font.GothamSemibold
Title.TextSize = 14

CloseButton.Name = "CloseButton"
CloseButton.Parent = TopBar
CloseButton.Size = UDim2.new(0, 30, 1, 0)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundTransparency = 1
CloseButton.Font = Enum.Font.Gotham
CloseButton.TextSize = 14

MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Parent = TopBar
MinimizeButton.Size = UDim2.new(0, 30, 1, 0)
MinimizeButton.Position = UDim2.new(1, -60, 0, 0)
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.BackgroundTransparency = 1
MinimizeButton.Font = Enum.Font.Gotham
MinimizeButton.TextSize = 18

TabsHolder.Name = "TabsHolder"
TabsHolder.Parent = MainFrame
TabsHolder.Position = UDim2.new(0, 0, 0, 30)
TabsHolder.Size = UDim2.new(0, 100, 1, -30)
TabsHolder.BackgroundColor3 = Color3.fromRGB(23, 23, 23)
TabsHolder.BorderSizePixel = 0

UIListLayout.Parent = TabsHolder
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Criação dos botões das abas
for _, tabName in ipairs(Tabs) do
    local Button = Instance.new("TextButton")
    Button.Name = tabName .. "Tab"
    Button.Parent = TabsHolder
    Button.Size = UDim2.new(1, 0, 0, 40)
    Button.Text = tabName
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Button.BorderSizePixel = 0
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    table.insert(TabButtons, Button)
end

-- Botão de fechar
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão de minimizar (animação de ocultar)
local minimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    MainFrame:TweenSize(
        minimized and UDim2.new(0, 100, 0, 60) or UDim2.new(0, 500, 0, 300),
        Enum.EasingDirection.Out,
        Enum.EasingStyle.Sine,
        0.3,
        true
    )
end)

-- Continuação: Abas com conteúdo, funções de Auto Win, WalkSpeed, etc. virão na Parte 2
