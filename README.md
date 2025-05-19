-- NATAN DEAD HUB - Interface Profissional estilo NatHub
-- Compatível com Delta Executor e Android
-- Desenvolvido para o jogo Dead Rails

-- Variáveis principais
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera

-- Função para criar notificações
local function Notify(msg)
    game.StarterGui:SetCore("SendNotification", {
        Title = "Natan Dead",
        Text = msg,
        Duration = 4
    })
end

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "NatanDeadGUI"

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 420, 0, 290)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 0
MainFrame.BackgroundTransparency = 0.05
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 10)

-- Título
local Title = Instance.new("TextLabel")
Title.Text = "NATAN DEAD"
Title.Font = Enum.Font.GothamBlack
Title.TextColor3 = Color3.fromRGB(255, 70, 70)
Title.TextSize = 22
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Parent = MainFrame

-- Botões Minimizar e Fechar
local Close = Instance.new("TextButton")
Close.Size = UDim2.new(0, 30, 0, 30)
Close.Position = UDim2.new(1, -35, 0, 0)
Close.Text = "X"
Close.TextColor3 = Color3.fromRGB(255, 60, 60)
Close.TextSize = 18
Close.Font = Enum.Font.GothamBold
Close.BackgroundTransparency = 1
Close.Parent = MainFrame
Close.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local Minimize = Instance.new("TextButton")
Minimize.Size = UDim2.new(0, 30, 0, 30)
Minimize.Position = UDim2.new(1, -70, 0, 0)
Minimize.Text = "-"
Minimize.TextColor3 = Color3.fromRGB(180, 180, 180)
Minimize.TextSize = 18
Minimize.Font = Enum.Font.GothamBold
Minimize.BackgroundTransparency = 1
Minimize.Parent = MainFrame

local contentVisible = true
Minimize.MouseButton1Click:Connect(function()
    contentVisible = not contentVisible
    for _, v in pairs(MainFrame:GetChildren()) do
        if v:IsA("Frame") and v.Name ~= "Tabs" then
            v.Visible = contentVisible
        end
    end
end)

-- Tabs laterais
local Tabs = Instance.new("Frame")
Tabs.Name = "Tabs"
Tabs.Size = UDim2.new(0, 100, 1, -30)
Tabs.Position = UDim2.new(0, 0, 0, 30)
Tabs.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Tabs.BorderSizePixel = 0
Tabs.Parent = MainFrame

local TabsCorner = Instance.new("UICorner", Tabs)
TabsCorner.CornerRadius = UDim.new(0, 6)

-- Área de conteúdo
local Content = Instance.new("Frame")
Content.Name = "Content"
Content.Size = UDim2.new(1, -100, 1, -30)
Content.Position = UDim2.new(0, 100, 0, 30)
Content.BackgroundTransparency = 1
Content.Parent = MainFrame

-- Criar função para gerar abas e conteúdo
local function CreateTab(name)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 0, 30)
    Button.BackgroundTransparency = 1
    Button.Text = name
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 14
    Button.Font = Enum.Font.Gotham
    Button.Parent = Tabs

    local Page = Instance.new("Frame")
    Page.Name = name
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.BackgroundTransparency = 1
    Page.Visible = false
    Page.Parent = Content

    Button.MouseButton1Click:Connect(function()
        for _, v in pairs(Content:GetChildren()) do
            if v:IsA("Frame") then
                v.Visible = false
            end
        end
        Page.Visible = true
    end)

    return Page
end

-- Criar abas
local MainTab = CreateTab("Main")
local TeleportsTab = CreateTab("Teleports")
local ESPTab = CreateTab("ESP")
local MiscTab = CreateTab("Extras")

-- Ativar a primeira aba por padrão
Content.Main.Visible = true

-- Continuação virá com os botões e funcionalidades reais de teleporte, esp e autofarm...
