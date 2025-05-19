-- Natan Dead - Hub Profissional Inspirado no NatHub
-- Script 100% funcional para Delta Executor e Android
-- Interface flutuante com animações, botões de minimizar e fechar

-- Inicialização de serviços e jogadores
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local TweenService = game:GetService("TweenService")

-- Criação da interface principal (flutuante)
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatanDeadHub"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 500, 0, 320)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Name = "MainFrame"
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.ClipsDescendants = true

-- Cantos arredondados
local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "Natan Dead Hub"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22

-- Botão de fechar (X)
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.BackgroundTransparency = 1
CloseBtn.MouseButton1Click:Connect(function()
    MainFrame:Destroy()
end)

-- Botão de minimizar (-)
local MinBtn = Instance.new("TextButton", MainFrame)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -60, 0, 0)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 20
MinBtn.BackgroundTransparency = 1

local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    local goalSize = minimized and UDim2.new(0, 500, 0, 30) or UDim2.new(0, 500, 0, 320)
    TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = goalSize}):Play()
end)

-- Abas laterais (Teleports, Combat, Visual, Auto, Misc)
local Tabs = {"Teleports", "Combat", "Visual", "Auto", "Misc"}
local Buttons = {}
local SelectedTab = nil
local TabFrames = {}

for i, tab in ipairs(Tabs) do
    local Button = Instance.new("TextButton", MainFrame)
    Button.Size = UDim2.new(0, 90, 0, 30)
    Button.Position = UDim2.new(0, 5, 0, 35 + (i - 1) * 35)
    Button.Text = tab
    Button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.Name = tab .. "Button"

    local TabFrame = Instance.new("Frame", MainFrame)
    TabFrame.Name = tab .. "Frame"
    TabFrame.Position = UDim2.new(0, 100, 0, 35)
    TabFrame.Size = UDim2.new(1, -110, 1, -45)
    TabFrame.BackgroundTransparency = 1
    TabFrame.Visible = false
    TabFrames[tab] = TabFrame

    Button.MouseButton1Click:Connect(function()
        if SelectedTab then
            TabFrames[SelectedTab].Visible = false
        end
        TabFrame.Visible = true
        SelectedTab = tab
    end)

    Buttons[tab] = Button
end

-- Pronto para adicionar funções visuais, teleportes, autofarm, ESP etc.
-- Próximo passo: adicionar os botões e scripts reais nas abas acima.

-- Confirma se posso continuar com as funções reais agora.
