-- Natan Dead Hub - Versão 1.0
-- Script super completo para Dead Rails (Roblox)
-- Compatível com Delta Executor e Android
-- Design moderno, animações, abas, notificações e funções FULL

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

-- Interface Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatanDeadHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

-- Janela Principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 420, 0, 320)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

-- Sombra leve para a janela (UICorner + UIStroke para dar charme)
local UICornerMain = Instance.new("UICorner", MainFrame)
UICornerMain.CornerRadius = UDim.new(0, 10)
local UIStrokeMain = Instance.new("UIStroke", MainFrame)
UIStrokeMain.Color = Color3.fromRGB(200, 30, 30)
UIStrokeMain.Thickness = 2
UIStrokeMain.Transparency = 0.3

-- Cabeçalho (Título e botões)
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Header.Parent = MainFrame

local UICornerHeader = Instance.new("UICorner", Header)
UICornerHeader.CornerRadius = UDim.new(0, 10)

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Text = "Natan Dead"
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 20
TitleLabel.TextColor3 = Color3.fromRGB(230, 40, 40)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Size = UDim2.new(1, -80, 1, 0)
TitleLabel.Position = UDim2.new(0, 10, 0, 0)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = Header

-- Botão fechar (X)
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -40, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 18
CloseBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
CloseBtn.AutoButtonColor = false
CloseBtn.Parent = Header

local CloseBtnCorner = Instance.new("UICorner", CloseBtn)
CloseBtnCorner.CornerRadius = UDim.new(0, 5)

CloseBtn.MouseEnter:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(200, 40, 40)}):Play()
end)
CloseBtn.MouseLeave:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 50)}):Play()
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão minimizar (-)
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -80, 0, 5)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
MinimizeBtn.Text = "-"
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 26
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
MinimizeBtn.AutoButtonColor = false
MinimizeBtn.Parent = Header

local MinimizeBtnCorner = Instance.new("UICorner", MinimizeBtn)
MinimizeBtnCorner.CornerRadius = UDim.new(0, 5)

MinimizeBtn.MouseEnter:Connect(function()
    TweenService:Create(MinimizeBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(200, 40, 40)}):Play()
end)
MinimizeBtn.MouseLeave:Connect(function()
    TweenService:Create(MinimizeBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 50)}):Play()
end)

local isMinimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    if not isMinimized then
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 420, 0, 40)}):Play()
        for _,v in pairs(MainFrame:GetChildren()) do
            if v ~= Header then
                v.Visible = false
            end
        end
        isMinimized = true
    else
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 420, 0, 320)}):Play()
        for _,v in pairs(MainFrame:GetChildren()) do
            v.Visible = true
        end
        isMinimized = false
    end
end)

-- Barra lateral (menu de abas)
local SideBar = Instance.new("Frame")
SideBar.Name = "SideBar"
SideBar.Size = UDim2.new(0, 130, 1, -40)
SideBar.Position = UDim2.new(0, 0, 0, 40)
SideBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
SideBar.Parent = MainFrame

local UICornerSideBar = Instance.new("UICorner", SideBar)
UICornerSideBar.CornerRadius = UDim.new(0, 10)

-- Container do conteúdo das abas
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -130, 1, -40)
ContentFrame.Position = UDim2.new(0, 130, 0, 40)
ContentFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ContentFrame.Parent = MainFrame

local UICornerContent = Instance.new("UICorner", ContentFrame)
UICornerContent.CornerRadius = UDim.new(0, 10)

-- TweenService para animações
local TweenService = game:GetService("TweenService")

-- Criar botões de abas na barra lateral
local Tabs = {"Teleports", "Combat", "World", "Misc"}

local tabButtons = {}
local tabFrames = {}

local function CreateTabButton(text, parent)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.BorderSizePixel = 0
    btn.Position = UDim2.new(0, 10, 0, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Text = text
    btn.Parent = parent
    btn.AutoButtonColor = false
    
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 70, 70)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
    end)
    
    return btn
end

for i, tabName in ipairs(Tabs) do
    local btn = CreateTabButton(tabName, SideBar)
    btn.Position = UDim2.new(0, 10, 0, 10 + (45 * (i - 1)))
    tabButtons[tabName] = btn
    
    local frame = Instance.new("ScrollingFrame")
    frame.Name = tabName .. "Frame"
    frame.BackgroundTransparency = 1
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.Position = UDim2.new(0, 0, 0, 0)
    frame.ScrollBarThickness = 4
    frame.Visible = (i == 1)
    frame.Parent = ContentFrame
    tabFrames[tabName] = frame
    
    btn.MouseButton1Click:Connect(function()
        for _, f in pairs(tabFrames) do
            f.Visible = false
        end
        frame.Visible = true
        
        for _, b in pairs(tabButtons) do
            TweenService:Create(b, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
            b.TextColor3 = Color3.new(1, 1, 1)
        end
        TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(230, 40, 40)}):Play()
        btn.TextColor3 = Color3.new(1, 1, 1)
    end)
end

-- Função para criar botões nas abas
local function CreateButton(text, parent, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, 0)
    btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    btn.BorderSizePixel = 0
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Text = text
    btn.AutoButtonColor = false
    btn.Parent = parent
    
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(100, 40, 40)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
    end)
    
    btn.MouseButton1Click:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(200, 30, 30)}):Play()
        wait(0.15)
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(100, 40, 40)}):Play()
        callback()
    end)
    
    return btn
end

-- Teleportes
local teleportsFrame = tabFrames["Teleports"]
local teleportsY = 10

local Bases = {
    ["Base 1"] = Vector3.new(100, 50, 200),
    ["Base 2"] = Vector3.new(350, 50, 600),
    ["Base 3"] = Vector3.new(700, 50, 400),
    ["Base Final"] = Vector3.new(1000, 50, 900),
    ["Trem"] = Vector3.new(500, 10, 500)
}

for baseName, pos in pairs(Bases) do
    local btn = CreateButton("Ir para " .. baseName, teleportsFrame, function()
        local character = LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = CFrame.new(pos + Vector3.new(0, 5, 0))
        end
    end)
    btn.Position = UDim2.new(0, 10, 0, teleportsY)
    teleportsY = teleportsY + 50
end

-- Combat Tab
local combatFrame = tabFrames["Combat"]
local combatY = 10

local KillAuraEnabled = false

local KillAuraBtn = CreateButton("Toggle Kill Aura", combatFrame, function()
    KillAuraEnabled = not KillAuraEnabled
    if KillAuraEnabled then
        Notify("Kill Aura ativado")
    else
        Notify("Kill Aura desativado")
    end
end)
KillAuraBtn.Position = UDim2.new(0, 10, 0, combatY)
combatY = combatY + 50

-- Função Kill Aura simples (ataca inimigos perto)
RunService.Heartbeat:Connect(function()
    if KillAuraEnabled then
        local character = LocalPlayer.Character
        if not character then return end
        local hrp = character:FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        for _, enemy in pairs(Workspace.Enemies:GetChildren()) do
            if enemy:FindFirstChild("HumanoidRootPart") and enemy:FindFirstChildOfClass("Humanoid") and enemy.Humanoid.Health > 0 then
                local dist = (hrp.Position - enemy.HumanoidRootPart.Position).Magnitude
                if dist < 15 then
                    -- Ataque (exemplo, dependendo do jogo pode variar)
                    -- Usar evento remoto para atacar
                    pcall(function()
                        game:GetService("ReplicatedStorage").Events.Attack:FireServer(enemy)
                    end)
                end
            end
        end
    end
end)

-- World Tab
local worldFrame = tabFrames["World"]
local worldY = 10

local NoclipEnabled = false
local noclipConnection

local function noclipOn()
    noclipConnection = RunService.Stepped:Connect(function()
        if LocalPlayer.Character then
            for _, part in pairs(LocalPlayer.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end

local function noclipOff()
    if noclipConnection then
        noclipConnection:Disconnect()
    end
    if LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end

local NoclipBtn = CreateButton("Toggle Noclip", worldFrame, function()
    NoclipEnabled = not NoclipEnabled
    if NoclipEnabled then
        noclipOn()
        Notify("Noclip ativado")
    else
        noclipOff()
        Notify("Noclip desativado")
    end
end)
NoclipBtn.Position = UDim2.new(0, 10, 0, worldY)
worldY = worldY + 50

local FullBrightBtn = CreateButton("Toggle Full Bright", worldFrame, function()
    local Lighting = game:GetService("Lighting")
    if Lighting.Brightness < 2 then
        Lighting.Brightness = 2
        Lighting.Ambient = Color3.fromRGB(255, 255, 255)
        Notify("Full Bright ativado")
    else
        Lighting.Brightness = 1
        Lighting.Ambient = Color3.fromRGB(127, 127, 127)
        Notify("Full Bright desativado")
    end
end)
FullBrightBtn.Position = UDim2.new(0, 10, 0, worldY)
worldY = worldY + 50

local RemoveFogBtn = CreateButton("Toggle Remove Fog", worldFrame, function()
    local Lighting = game:GetService("Lighting")
    if Lighting.FogEnd < 100000 then
        Lighting.FogEnd = 100000
        Notify("Névoa removida")
    else
        Lighting.FogEnd = 1000
        Notify("Névoa restaurada")
    end
end)
RemoveFogBtn.Position = UDim2.new(0, 10, 0, worldY)
worldY = worldY + 50

-- Misc Tab
local miscFrame = tabFrames["Misc"]
local miscY = 10

local AutoFarmEnabled = false
local AutoFarmBtn = CreateButton("Toggle Auto Farm", miscFrame, function()
    AutoFarmEnabled = not AutoFarmEnabled
    if AutoFarmEnabled then
        Notify("Auto Farm ativado")
    else
        Notify("Auto Farm desativado")
    end
end)
AutoFarmBtn.Position = UDim2.new(0, 10, 0, miscY)
miscY = miscY + 50

local UnlockMouseBtn = CreateButton("Unlock Mouse", miscFrame, function()
