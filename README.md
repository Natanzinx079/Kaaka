-- NATAN DEAD HUB - Interface Profissional Estilo NatHub
-- Script Completo, Avançado, Compatível com Delta e Android
-- Interface flutuante com animações, abas, minimizar, fechar e cores vibrantes

-- Inicialização da UI
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local Tabs = Instance.new("Frame")
local TeleportTab = Instance.new("ScrollingFrame")
local CombatTab = Instance.new("ScrollingFrame")
local VisualTab = Instance.new("ScrollingFrame")
local AutoTab = Instance.new("ScrollingFrame")
local MiscTab = Instance.new("ScrollingFrame")
local MinimizeButton = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")

-- Configuração geral
ScreenGui.Name = "NatanDeadHub"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 500, 0, 300)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "NATAN DEAD - HUB"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBlack

Tabs.Name = "Tabs"
Tabs.Parent = MainFrame
Tabs.BackgroundTransparency = 1
Tabs.Position = UDim2.new(0, 0, 0, 40)
Tabs.Size = UDim2.new(1, 0, 1, -40)

-- Botões de controle
MinimizeButton.Parent = MainFrame
MinimizeButton.Text = "-"
MinimizeButton.Size = UDim2.new(0, 40, 0, 40)
MinimizeButton.Position = UDim2.new(1, -80, 0, 0)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.Font = Enum.Font.GothamBlack
MinimizeButton.TextScaled = true

CloseButton.Parent = MainFrame
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 40, 0, 40)
CloseButton.Position = UDim2.new(1, -40, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBlack
CloseButton.TextScaled = true

-- Função Minimizar
local minimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    for _, v in pairs(Tabs:GetChildren()) do
        if v:IsA("ScrollingFrame") then
            v.Visible = not minimized
        end
    end
    MainFrame.Size = minimized and UDim2.new(0, 500, 0, 50) or UDim2.new(0, 500, 0, 300)
end)

-- Função Fechar
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Criação de Abas
local function createTab(name, parent)
    local tab = Instance.new("ScrollingFrame")
    tab.Name = name
    tab.Parent = parent
    tab.Size = UDim2.new(1, 0, 1, 0)
    tab.CanvasSize = UDim2.new(0, 0, 3, 0)
    tab.ScrollBarThickness = 6
    tab.BackgroundTransparency = 1
    tab.Visible = false
    return tab
end

TeleportTab = createTab("TeleportTab", Tabs)
CombatTab = createTab("CombatTab", Tabs)
VisualTab = createTab("VisualTab", Tabs)
AutoTab = createTab("AutoTab", Tabs)
MiscTab = createTab("MiscTab", Tabs)

-- Ativar a primeira aba por padrão
TeleportTab.Visible = true

-- Criação de Botões de Abas
local buttonNames = {"Teleport", "Combat", "Visual", "Auto", "Misc"}
local tabsRef = {TeleportTab, CombatTab, VisualTab, AutoTab, MiscTab}

for i, name in ipairs(buttonNames) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Parent = MainFrame
    tabBtn.Size = UDim2.new(0, 100, 0, 30)
    tabBtn.Position = UDim2.new(0, (i - 1) * 100, 0, 270)
    tabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    tabBtn.Text = name
    tabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    tabBtn.Font = Enum.Font.Gotham
    tabBtn.TextScaled = true

    tabBtn.MouseButton1Click:Connect(function()
        for _, tab in ipairs(tabsRef) do tab.Visible = false end
        tabsRef[i].Visible = true
    end)
end

-- Continuação com funções reais virá a seguir...

-- Deseja agora que eu adicione os botões e scripts funcionais nas abas (teleporte, autofarm, esp, etc)?
