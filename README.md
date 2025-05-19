-- NATAN DEAD HUB - PROFISSIONAL SCRIPT PARA DEAD RAILS
-- Interface estilo NatHub, com animações, teleporte, ESP, AutoFarm e mais

-- [Inicialização da Interface]
local NatanUI = Instance.new("ScreenGui")
NatanUI.Name = "NatanDead"
NatanUI.ResetOnSpawn = false
NatanUI.IgnoreGuiInset = true
NatanUI.ZIndexBehavior = Enum.ZIndexBehavior.Global
NatanUI.Parent = game.CoreGui

local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 600, 0, 400)
Main.Position = UDim2.new(0.5, -300, 0.5, -200)
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Main.BorderSizePixel = 0
Main.AnchorPoint = Vector2.new(0.5, 0.5)
Main.Draggable = true
Main.Active = true
Main.Parent = NatanUI

local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TopBar.Parent = Main

local Title = Instance.new("TextLabel")
Title.Text = "NATAN DEAD"
Title.Font = Enum.Font.GothamBlack
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.TextSize = 22
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Parent = TopBar

local Close = Instance.new("TextButton")
Close.Text = "X"
Close.Font = Enum.Font.GothamBold
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.TextSize = 18
Close.Size = UDim2.new(0, 40, 0, 40)
Close.Position = UDim2.new(1, -40, 0, 0)
Close.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
Close.Parent = TopBar
Close.MouseButton1Click:Connect(function()
    NatanUI:Destroy()
end)

local Minimize = Instance.new("TextButton")
Minimize.Text = "-"
Minimize.Font = Enum.Font.GothamBold
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.TextSize = 18
Minimize.Size = UDim2.new(0, 40, 0, 40)
Minimize.Position = UDim2.new(1, -80, 0, 0)
Minimize.BackgroundColor3 = Color3.fromRGB(50, 50, 0)
Minimize.Parent = TopBar

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, 0, 1, -40)
Content.Position = UDim2.new(0, 0, 0, 40)
Content.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Content.Parent = Main

Minimize.MouseButton1Click:Connect(function()
    Content.Visible = not Content.Visible
end)

-- [Menu Lateral - Abas]
local Sidebar = Instance.new("Frame")
Sidebar.Size = UDim2.new(0, 120, 1, 0)
Sidebar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Sidebar.Parent = Content

local Tabs = {}
local TabContent = Instance.new("Frame")
TabContent.Size = UDim2.new(1, -120, 1, 0)
TabContent.Position = UDim2.new(0, 120, 0, 0)
TabContent.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TabContent.Parent = Content

local function createTab(name)
    local Button = Instance.new("TextButton")
    Button.Text = name
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 16
    Button.Size = UDim2.new(1, 0, 0, 40)
    Button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Parent = Sidebar
    
    local Page = Instance.new("ScrollingFrame")
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.CanvasSize = UDim2.new(0, 0, 2, 0)
    Page.ScrollBarThickness = 6
    Page.Visible = false
    Page.Parent = TabContent

    Button.MouseButton1Click:Connect(function()
        for _, tab in pairs(Tabs) do
            tab.Page.Visible = false
        end
        Page.Visible = true
    end)

    table.insert(Tabs, {Button = Button, Page = Page})
    return Page
end

-- [Criando Abas]
local HomeTab = createTab("Home")
local TeleportTab = createTab("Teleports")
local CombatTab = createTab("Combat")
local VisualTab = createTab("Visual")
local AutoTab = createTab("Auto")
local MiscTab = createTab("Misc")

Tabs[1].Page.Visible = true

-- A seguir serão adicionadas as funções reais: TP, ESP, AutoFarm, etc...
-- Continuar?
