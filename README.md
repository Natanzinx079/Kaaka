-- NATAN DEAD - Script Hub Profissional para Dead Rails (Estilo NatHub)
-- Compatível com Delta Executor e Android

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local MinimizeButton = Instance.new("TextButton")
local SideBar = Instance.new("Frame")
local Tabs = {"Teleports", "Combat", "Visual", "Auto", "Misc"}
local TabButtons = {}
local Pages = {}

-- Funções úteis
local TweenService = game:GetService("TweenService")
local function tween(obj, props, duration)
    TweenService:Create(obj, TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), props):Play()
end

-- GUI Base
ScreenGui.Name = "NatanDeadHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 600, 0, 400)
MainFrame.Position = UDim2.new(0.5, -300, 0.5, -200)
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TopBar.Parent = MainFrame

Title.Name = "Title"
Title.Text = "NATAN DEAD"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.TextSize = 20
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BackgroundTransparency = 1
Title.Parent = TopBar

CloseButton.Name = "CloseButton"
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextColor3 = Color3.new(1, 0, 0)
CloseButton.TextSize = 16
CloseButton.Size = UDim2.new(0, 40, 1, 0)
CloseButton.Position = UDim2.new(1, -40, 0, 0)
CloseButton.BackgroundTransparency = 1
CloseButton.Parent = TopBar

MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Text = "-"
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextColor3 = Color3.new(1, 1, 1)
MinimizeButton.TextSize = 16
MinimizeButton.Size = UDim2.new(0, 40, 1, 0)
MinimizeButton.Position = UDim2.new(1, -80, 0, 0)
MinimizeButton.BackgroundTransparency = 1
MinimizeButton.Parent = TopBar

SideBar.Name = "SideBar"
SideBar.Size = UDim2.new(0, 120, 1, -40)
SideBar.Position = UDim2.new(0, 0, 0, 40)
SideBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
SideBar.Parent = MainFrame

for i, name in ipairs(Tabs) do
    local Button = Instance.new("TextButton")
    Button.Text = name
    Button.Size = UDim2.new(1, 0, 0, 40)
    Button.Position = UDim2.new(0, 0, 0, (i - 1) * 40)
    Button.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.Parent = SideBar
    TabButtons[name] = Button

    local Page = Instance.new("ScrollingFrame")
    Page.Size = UDim2.new(1, -120, 1, -40)
    Page.Position = UDim2.new(0, 120, 0, 40)
    Page.BackgroundTransparency = 1
    Page.Visible = i == 1
    Page.ScrollBarThickness = 6
    Page.CanvasSize = UDim2.new(0, 0, 5, 0)
    Page.Parent = MainFrame
    Pages[name] = Page

    Button.MouseButton1Click:Connect(function()
        for _, page in pairs(Pages) do page.Visible = false end
        Page.Visible = true
    end)
end

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local minimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    if minimized then
        tween(MainFrame, {Size = UDim2.new(0, 600, 0, 400)}, 0.3)
        minimized = false
    else
        tween(MainFrame, {Size = UDim2.new(0, 600, 0, 40)}, 0.3)
        minimized = true
    end
end)

-- A partir daqui adicionaremos as funções reais: teleportes, autofarm, ESP, etc., dentro das abas correspondentes.
-- Posso continuar agora adicionando as funções 100% funcionais reais de cada aba, uma por uma?
