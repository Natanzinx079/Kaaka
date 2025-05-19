-- NATAN DEAD HUB 1.0 - Estilo NatHub - Completo e Profissional
-- Compatível Delta Executor e Android

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local TweenService = game:GetService("TweenService")

-- Criar ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatanDeadHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Função para facilitar criação de elementos
local function Create(class, properties)
    local obj = Instance.new(class)
    for prop, val in pairs(properties) do
        obj[prop] = val
    end
    return obj
end

-- Main Frame
local Main = Create("Frame", {
    Name = "MainFrame",
    Parent = ScreenGui,
    BackgroundColor3 = Color3.fromRGB(30, 30, 30),
    Size = UDim2.new(0, 480, 0, 350),
    Position = UDim2.new(0.3, 0, 0.3, 0),
    Active = true,
    Draggable = true,
    BorderSizePixel = 0,
})

-- Título
local Title = Create("TextLabel", {
    Parent = Main,
    Size = UDim2.new(1, 0, 0, 50),
    BackgroundColor3 = Color3.fromRGB(20, 20, 20),
    Text = "NATAN DEAD HUB 1.0",
    TextColor3 = Color3.fromRGB(255, 0, 0),
    Font = Enum.Font.GothamBlack,
    TextSize = 28,
    TextScaled = true,
})

-- Botões Minimize e Close
local MinimizeBtn = Create("TextButton", {
    Parent = Main,
    Text = "-",
    Size = UDim2.new(0, 40, 0, 50),
    Position = UDim2.new(1, -80, 0, 0),
    BackgroundColor3 = Color3.fromRGB(40, 40, 40),
    TextColor3 = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.GothamBlack,
    TextSize = 24,
})

local CloseBtn = Create("TextButton", {
    Parent = Main,
    Text = "X",
    Size = UDim2.new(0, 40, 0, 50),
    Position = UDim2.new(1, -40, 0, 0),
    BackgroundColor3 = Color3.fromRGB(40, 40, 40),
    TextColor3 = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.GothamBlack,
    TextSize = 24,
})

-- Função Minimize/Maximize
local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        for _, child in pairs(Main:GetChildren()) do
            if child ~= Title and child ~= MinimizeBtn and child ~= CloseBtn then
                child.Visible = false
            end
        end
        Main.Size = UDim2.new(0, 480, 0, 50)
    else
        for _, child in pairs(Main:GetChildren()) do
            child.Visible = true
        end
        Main.Size = UDim2.new(0, 480, 0, 350)
    end
end)

-- Função fechar GUI
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Frame para os botões das abas
local TabButtonsFrame = Create("Frame", {
    Parent = Main,
    Size = UDim2.new(1, 0, 0, 40),
    Position = UDim2.new(0, 0, 0, 50),
    BackgroundColor3 = Color3.fromRGB(20, 20, 20),
})

-- Frame para conteúdo das abas
local ContentFrame = Create("Frame", {
    Parent = Main,
    Size = UDim2.new(1, -10, 1, -100),
    Position = UDim2.new(0, 5, 0, 95),
    BackgroundColor3 = Color3.fromRGB(15, 15, 15),
    BorderSizePixel = 0,
})

-- Tabela pra guardar as abas
local tabs = {}
local activeTab = nil

local function CreateTab(name)
    local tab = Create("ScrollingFrame", {
        Parent = ContentFrame,
        Size = UDim2.new(1, 0, 1, 0),
        BackgroundColor3 = Color3.fromRGB(15, 15, 15),
        ScrollBarThickness = 6,
        CanvasSize = UDim2.new(0, 0, 2, 0),
        Visible = false,
    })
    tabs[name] = tab

    -- Botão da aba
    local btn = Create("TextButton", {
        Parent = TabButtonsFrame,
        Text = name,
        BackgroundColor3 = Color3.fromRGB(40, 40, 40),
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Size = UDim2.new(0, 96, 1, -4),
        Position = UDim2.new(0, (#TabButtonsFrame:GetChildren()-1) * 98, 0, 2),
        Font = Enum.Font.GothamBold,
        TextSize = 18,
    })
    btn.MouseButton1Click:Connect(function()
        if activeTab then
            tabs[activeTab].Visible = false
        end
        tab.Visible = true
        activeTab = name
        for _, button in pairs(TabButtonsFrame:GetChildren()) do
            if button:IsA("TextButton") then
                button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            end
        end
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end)

    return tab
end

-- Criar as abas principais
local teleportTab = CreateTab("Teleport")
local combatTab = CreateTab("Combat")
local visualTab = CreateTab("Visual")
local autoTab = CreateTab("Auto")
local miscTab = CreateTab("Misc")

-- Ativar aba Teleport por padrão
tabs[activeTab or "Teleport"].Visible = true
activeTab = "Teleport"
-- Highlight botão aba Teleport
for _, btn in pairs(TabButtonsFrame:GetChildren()) do
    if btn:IsA("TextButton") and btn.Text == "Teleport" then
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end

-- FUNÇÕES PRA BOTÕES E FEATURES ---

-- Função utilitária pra criar botão nas abas
local function CreateButton(tab, text, callback)
    local button = Create("TextButton", {
        Parent = tab,
        Text = text,
        Size = UDim2.new(0, 200, 0, 35),
        Position = UDim2.new(0, 20, 0, (#tab:GetChildren() - 1) * 40 + 10),
        BackgroundColor3 = Color3.fromRGB(50, 50, 50),
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Font = Enum.Font.GothamBold,
        TextSize = 20,
        AutoButtonColor = true,
    })
    button.MouseButton1Click:Connect(callback)
    return button
end

-- TELEPORT FUNCTIONS
local TeleportService = game:GetService("TeleportService")

-- Exemplo: Teleport para o trem (ajuste o nome do objeto do trem conforme o jogo)
local function TeleportToTrain()
    local train = workspace:FindFirstChild("Train") or workspace:FindFirstChild("train") or workspace:FindFirstChild("Trem")
    if train and train.PrimaryPart then
        LocalPlayer.Character.HumanoidRootPart.CFrame = train.PrimaryPart.CFrame + Vector3.new(0, 5, 0)
    else
        warn("Trem não encontrado para teleportar.")
    end
end

-- Teleport para as bases (exemplo, ajuste nomes conforme mapa)
local bases = {
    "Base1",
    "Base2",
    "Base3",
    "Base4",
}

local function TeleportToBase(baseName)
    local base = workspace:FindFirstChild(baseName)
    if base and base.PrimaryPart then
        LocalPlayer.Character.HumanoidRootPart.CFrame = base.PrimaryPart.CFrame + Vector3.new(0, 5, 0)
    else
        warn("Base "..baseName.." não encontrada.")
    end
end

-- Criar botões de teleporte para bases
CreateButton(teleportTab, "Teleport to Train", TeleportToTrain)

for _, baseName in pairs(bases) do
    CreateButton(teleportTab, "Teleport to "..baseName, function()
        TeleportToBase(baseName)
    end)
end

-- COMBAT TAB - Kill Aura Exemplo (Básico)
local killAuraEnabled = false
local killAuraRange = 20

local function ToggleKillAura()
    killAuraEnabled = not killAuraEnabled
    if killAuraEnabled then
        print("Kill Aura ativado.")
        spawn(function()
            while killAuraEnabled do
                for _, enemy in pairs(workspace.Enemies:GetChildren()) do
                    if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                        local dist = (enemy.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                        if dist <= killAuraRange then
                            enemy.Humanoid:TakeDamage(10)
                        end
                    end
                end
                wait(0.3)
            end
        end)
    else
        print("Kill Aura desativado.")
    end
end

CreateButton(combatTab, "Toggle Kill Aura", ToggleKillAura)

-- VISUAL TAB - ESP Simples
local espEnabled = false
local espBoxes = {}

local function CreateESP()
    for _, npc in pairs(workspace.Enemies:GetChildren()) do
        if not espBoxes[npc] and npc:FindFirstChild("HumanoidRootPart") then
            local box = Drawing.new("Square")
            box.Visible = true
            box.Color = Color3.fromRGB(255, 0, 0)
            box.Thickness = 2
            box.Transparency = 1
            box.Filled = false
            espBoxes[npc] = box
        end
    end
end

local function UpdateESP()
    for npc, box in pairs(espBoxes) do
        if npc and npc:FindFirstChild("HumanoidRootPart") and npc.Humanoid.Health > 0 then
            local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(npc.HumanoidRootPart.Position)
            if onScreen then
                box.Position = Vector2.new(pos.X - 20, pos.Y - 20)
                box.Size = Vector2.new(40, 40)
                box.Visible = true
            else
                box.Visible = false
            end
        else
            box:Remove()
            espBoxes[npc] = nil
        end
    end
end

local espConnection

local function ToggleESP()
    espEnabled = not espEnabled
    if espEnabled then
        CreateESP()
        espConnection = RunService.RenderStepped:Connect(UpdateESP)
        print("ESP ativado.")
    else
        if espConnection then
            espConnection:Disconnect()
            espConnection = nil
        end
        for _, box in pairs(espBoxes) do
            box:Remove()
        end
        espBoxes = {}
        print("ESP desativado.")
    end
end

CreateButton(visualTab, "Toggle ESP", ToggleESP)

-- AUTO TAB - AutoFarm (exemplo)
local autoFarmEnabled = false

local function ToggleAutoFarm()
    autoFarmEnabled = not autoFarmEnabled
    if autoFarmEnabled then
        print("AutoFarm ativado.")
        spawn(function()
            while autoFarmEnabled do
                -- Exemplo: coletar moedas ou atacar inimigos
                wait(1)
            end
        end)
    else
        print("AutoFarm desativado.")
    end
end

CreateButton(autoTab, "Toggle AutoFarm", ToggleAutoFarm)

-- MISC TAB - Noclip (exemplo básico)
local noclipEnabled = false

local function ToggleNoclip()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        print("Noclip ativado.")
        local function noclipLoop()
            while noclipEnabled do
                for _, part in pairs(LocalPlayer.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
                wait()
            end
            for _, part in pairs(LocalPlayer.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
        spawn(noclipLoop)
    else
        print("Noclip desativado.")
    end
end

CreateButton(miscTab, "Toggle Noclip", ToggleNoclip)

-- Fim do script
