-- Natan Dead - Script super completo para Dead Rails (Roblox)
-- Design inspirado no NatHub, com menu flutuante, minimizar animado e funções completas

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

local TweenInfoDefault = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

-- Configurações visuais
local Colors = {
    MainColor = Color3.fromRGB(255, 0, 0), -- Vermelho NatHub
    Background = Color3.fromRGB(25, 25, 25),
    TabInactive = Color3.fromRGB(40, 40, 40),
    Text = Color3.fromRGB(255, 255, 255),
    ToggleOn = Color3.fromRGB(0, 200, 0),
    ToggleOff = Color3.fromRGB(150, 0, 0),
}

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatanDeadGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Janela principal flutuante
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 450, 0, 350)
MainFrame.Position = UDim2.new(0.5, -225, 0.5, -175)
MainFrame.BackgroundColor3 = Colors.Background
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

-- Sombra para dar profundidade
local Shadow = Instance.new("UIStroke")
Shadow.Thickness = 2
Shadow.Color = Colors.MainColor
Shadow.Parent = MainFrame

-- Barra superior (título + botões)
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Colors.MainColor
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Text = "Natan Dead"
Title.TextColor3 = Colors.Text
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(0, 150, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Parent = TopBar

-- Botão minimizar com animação
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Text = "-"
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 24
MinimizeBtn.TextColor3 = Colors.Text
MinimizeBtn.BackgroundColor3 = Colors.TabInactive
MinimizeBtn.Size = UDim2.new(0, 40, 1, 0)
MinimizeBtn.Position = UDim2.new(1, -90, 0, 0)
MinimizeBtn.Parent = TopBar
MinimizeBtn.AutoButtonColor = false
MinimizeBtn.MouseEnter:Connect(function()
    MinimizeBtn.BackgroundColor3 = Colors.MainColor
end)
MinimizeBtn.MouseLeave:Connect(function()
    MinimizeBtn.BackgroundColor3 = Colors.TabInactive
end)

-- Botão fechar
local CloseBtn = Instance.new("TextButton")
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.TextColor3 = Colors.Text
CloseBtn.BackgroundColor3 = Colors.TabInactive
CloseBtn.Size = UDim2.new(0, 40, 1, 0)
CloseBtn.Position = UDim2.new(1, -45, 0, 0)
CloseBtn.Parent = TopBar
CloseBtn.AutoButtonColor = false
CloseBtn.MouseEnter:Connect(function()
    CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
end)
CloseBtn.MouseLeave:Connect(function()
    CloseBtn.BackgroundColor3 = Colors.TabInactive
end)

-- Container para abas e conteúdo
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, 0, 1, -30)
ContentFrame.Position = UDim2.new(0, 0, 0, 30)
ContentFrame.BackgroundColor3 = Colors.TabInactive
ContentFrame.BorderSizePixel = 0
ContentFrame.Parent = MainFrame

-- Barra de abas lateral
local TabsFrame = Instance.new("Frame")
TabsFrame.Size = UDim2.new(0, 130, 1, 0)
TabsFrame.BackgroundColor3 = Colors.Background
TabsFrame.BorderSizePixel = 0
TabsFrame.Parent = ContentFrame

-- Container do conteúdo da aba
local TabContent = Instance.new("Frame")
TabContent.Size = UDim2.new(1, -130, 1, 0)
TabContent.Position = UDim2.new(0, 130, 0, 0)
TabContent.BackgroundColor3 = Colors.Background
TabContent.BorderSizePixel = 0
TabContent.Parent = ContentFrame

-- Função para criar botão de aba
local function CreateTabButton(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.BackgroundColor3 = Colors.TabInactive
    btn.Text = name
    btn.Font = Enum.Font.GothamBold
    btn.TextColor3 = Colors.Text
    btn.TextSize = 18
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    btn.Parent = TabsFrame
    btn.MouseEnter:Connect(function()
        if btn.BackgroundColor3 ~= Colors.MainColor then
            btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        end
    end)
    btn.MouseLeave:Connect(function()
        if btn.BackgroundColor3 ~= Colors.MainColor then
            btn.BackgroundColor3 = Colors.TabInactive
        end
    end)
    return btn
end

-- Gerenciar abas (visibilidade e estilo)
local Tabs = {}
local CurrentTab = nil
local function SwitchTab(tabName)
    for name, frame in pairs(Tabs) do
        if name == tabName then
            frame.Visible = true
            -- Ajusta botão aba selecionada
            for _, btn in pairs(TabsFrame:GetChildren()) do
                if btn:IsA("TextButton") then
                    btn.BackgroundColor3 = (btn.Text == tabName) and Colors.MainColor or Colors.TabInactive
                end
            end
        else
            frame.Visible = false
        end
    end
    CurrentTab = tabName
end

-- Criar abas e frames
local TeleportTab = Instance.new("ScrollingFrame")
TeleportTab.Size = UDim2.new(1, 0, 1, 0)
TeleportTab.BackgroundTransparency = 1
TeleportTab.CanvasSize = UDim2.new(0,0,0,0)
TeleportTab.ScrollBarThickness = 5
TeleportTab.Parent = TabContent
TeleportTab.Visible = false
TeleportTab.AutomaticCanvasSize = Enum.AutomaticSize.Y

local ESPTab = Instance.new("ScrollingFrame")
ESPTab.Size = UDim2.new(1, 0, 1, 0)
ESPTab.BackgroundTransparency = 1
ESPTab.CanvasSize = UDim2.new(0,0,0,0)
ESPTab.ScrollBarThickness = 5
ESPTab.Parent = TabContent
ESPTab.Visible = false
ESPTab.AutomaticCanvasSize = Enum.AutomaticSize.Y

local AutoFarmTab = Instance.new("ScrollingFrame")
AutoFarmTab.Size = UDim2.new(1, 0, 1, 0)
AutoFarmTab.BackgroundTransparency = 1
AutoFarmTab.CanvasSize = UDim2.new(0,0,0,0)
AutoFarmTab.ScrollBarThickness = 5
AutoFarmTab.Parent = TabContent
AutoFarmTab.Visible = false
AutoFarmTab.AutomaticCanvasSize = Enum.AutomaticSize.Y

Tabs["Teleport"] = TeleportTab
Tabs["ESP"] = ESPTab
Tabs["AutoFarm"] = AutoFarmTab

local TabButtons = {}
TabButtons["Teleport"] = CreateTabButton("Teleport")
TabButtons["ESP"] = CreateTabButton("ESP")
TabButtons["AutoFarm"] = CreateTabButton("AutoFarm")

for name, btn in pairs(TabButtons) do
    btn.MouseButton1Click:Connect(function()
        SwitchTab(name)
    end)
end

-- Inicializa aba padrão
SwitchTab("Teleport")

-- Função auxiliar para criar botões nas abas
local function CreateButton(parent, text)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 35)
    btn.BackgroundColor3 = Colors.MainColor
    btn.TextColor3 = Colors.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = text
    btn.BorderSizePixel = 0
    btn.AnchorPoint = Vector2.new(0, 0)
    btn.Position = UDim2.new(0, 10, 0, (#parent:GetChildren())*40)
    btn.AutoButtonColor = false
    btn.Parent = parent
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Colors.MainColor
    end)
    return btn
end

-- Função auxiliar para criar toggle (checkbox)
local function CreateToggle(parent, text, default)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -20, 0, 35)
    frame.BackgroundTransparency = 1
    frame.Parent = parent

    local label = Instance.new("TextLabel")
    label.Text = text
    label.Size = UDim2.new(0.8, 0, 1, 0)
    label.TextColor3 = Colors.Text
    label.Font = Enum.Font.GothamBold
    label.TextSize = 16
    label.BackgroundTransparency = 1
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame

    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.new(0.2, -10, 1, 0)
    toggle.Position = UDim2.new(0.8, 10, 0, 0)
    toggle.BackgroundColor3 = default and Colors.ToggleOn or Colors.ToggleOff
    toggle.Text = default and "ON" or "OFF"
    toggle.Font = Enum.Font.GothamBold
    toggle.TextSize = 14
    toggle.TextColor3 = Colors.Text
    toggle.BorderSizePixel = 0
    toggle.Parent = frame
    toggle.AutoButtonColor = false

    local state = default

    toggle.MouseButton1Click:Connect(function()
        state = not state
        toggle.BackgroundColor3 = state and Colors.ToggleOn or Colors.ToggleOff
        toggle.Text = state and "ON" or "OFF"
        if frame.ToggleCallback then
            frame.ToggleCallback(state)
        end
    end)

    function frame:SetCallback(cb)
        self.ToggleCallback = cb
    end

    return frame
end

-- Teleport - Exemplo (precisa ajustar para o mapa do jogo Dead Rails)
local bases = {
    ["Base1"] = Vector3.new(100, 10, 100),
    ["Base2"] = Vector3.new(300, 10, 300),
    ["Base3"] = Vector3.new(500, 10, 500),
    ["Final"] = Vector3.new(700, 10, 700),
    ["Train"] = Vector3.new(200, 10, 200),
}

-- Função para teleporte do personagem
local function TeleportTo(pos)
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = CFrame.new(pos)
    end
end

-- Criar botões de teleporte
for name, pos in pairs(bases) do
    local btn = CreateButton(TeleportTab, "Teleport to "..name)
    btn.MouseButton1Click:Connect(function()
        TeleportTo(pos + Vector3.new(0, 5, 0)) -- Leve ajuste em Y para cima da torreta
    end)
end

-- ESP simples: linhas para inimigos, trem e bases
local ESPFolder = Instance.new("Folder", Workspace)
ESPFolder.Name = "NatanDeadESPFolder"

local ESPEnabled = {
    Enemies = false,
    Train = false,
    Bases = false,
}

local espConnections = {}

-- Função para criar linha de ESP entre player e objeto
local function CreateESPLine(adornParent, color)
    local line = Drawing.new("Line")
    line.Color = color
    line.Thickness = 1.5
    line.Transparency = 1
    line.Visible = false
    return line
end

-- Toggler para ESP inimigos
local espEnemiesToggle = CreateToggle(ESPTab, "ESP Enemies", false)
espEnemiesToggle:SetCallback(function(state)
    ESPEnabled.Enemies = state
    if not state then
        for _, conn in pairs(espConnections) do
            conn:Disconnect()
        end
        espConnections = {}
    else
        -- Começar esp enemies
        -- Exemplo básico: pega inimigos no workspace - ajustar conforme o jogo
        -- Pega inimigos (pode precisar adaptar)
        local function updateESP()
            -- Limpando linhas existentes
            for _, conn in pairs(espConnections) do
                conn:Disconnect()
            end
            espConnections = {}

            -- Criar linhas para inimigos (exemplo)
            for _, enemy in pairs(Workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("HumanoidRootPart") then
                    local line = CreateESPLine(enemy, Color3.fromRGB(255,0,0))
                    line.Visible = true
                    table.insert(espConnections, RunService.RenderStepped:Connect(function()
                        local camera = workspace.CurrentCamera
                        local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if root and enemy and enemy.Parent and enemy.HumanoidRootPart then
                            local screenRoot = camera:WorldToViewportPoint(root.Position)
                            local screenEnemy = camera:WorldToViewportPoint(enemy.HumanoidRootPart.Position)
                            line.From = Vector2.new(screenRoot.X, screenRoot.Y)
                            line.To = Vector2.new(screenEnemy.X, screenEnemy.Y)
                            line.Visible = true
                        else
                            line.Visible = false
                        end
                    end))
                end
            end
        end
        RunService.RenderStepped:Connect(updateESP)
    end
end)

-- ESP do trem (exemplo básico)
local espTrainToggle = CreateToggle(ESPTab, "ESP Train", false)
espTrainToggle:SetCallback(function(state)
    ESPEnabled.Train = state
    -- Pode implementar ESP para trem (similar ao inimigos)
end)

-- ESP das bases
local espBasesToggle = CreateToggle(ESPTab, "ESP Bases", false)
espBasesToggle:SetCallback(function(state)
    ESPEnabled.Bases = state
    -- Pode implementar ESP para bases (similar)
end)

-- AutoFarm básico
local AutoFarmEnabled = false

local autoFarmToggle = CreateToggle(AutoFarmTab, "AutoFarm", false)
autoFarmToggle:SetCallback(function(state)
    AutoFarmEnabled = state
end)

-- AutoFarm loop
spawn(function()
    while true do
        if AutoFarmEnabled then
            local character = LocalPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                -- Exemplo: teleporta para base 1, espera, teleporta para base 2, etc (simplificado)
                for _, pos in pairs(bases) do
                    TeleportTo(pos + Vector3.new(0,5,0))
                    wait(2) -- espera para "farmar"
                    if not AutoFarmEnabled then break end
                end
            end
        end
        wait(0.5)
    end
end)

-- Função minimizar com animação
local minimized = false
local function Minimize()
    if minimized then
        -- Restaurar janela
        local tween = TweenService:Create(MainFrame, TweenInfoDefault, {Size = UDim2.new(0,450,0,350)})
        tween:Play()
        tween.Completed:Wait()
        ContentFrame.Visible = true
        minimized = false
    else
        -- Minimizar para barra fina
        local tween = TweenService:Create(MainFrame, TweenInfoDefault, {Size = UDim2.new(0,450,0,30)})
        tween:Play()
        tween.Completed:Wait()
        ContentFrame.Visible = false
        minimized = true
    end
end

MinimizeBtn.MouseButton1Click:Connect(Minimize)

-- Fechar script
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Fim do script principal
