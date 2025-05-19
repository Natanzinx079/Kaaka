-- Natan Hub Nathub Style Atualizado

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("NatanHub", "DarkTheme")

-- Aba Principal
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Automations")

MainSection:NewButton("Auto Bond", "Coleta automática de obrigações", function()
    -- Texto de status na tela
    local AutoBondText = Instance.new("TextLabel")
    AutoBondText.Size = UDim2.new(0, 400, 0, 120)
    AutoBondText.Position = UDim2.new(0.5, -200, 0.5, -60)
    AutoBondText.AnchorPoint = Vector2.new(0.5, 0.5)
    AutoBondText.BackgroundTransparency = 1
    AutoBondText.TextStrokeTransparency = 0.8
    AutoBondText.Text = "Auto Bond <font color='rgb(0,170,255)'>is working</font>, do not touch anything\n<font color='rgb(0,170,255)'>get.nathub.xyz</font>\nCurrent Task: <font color='rgb(0,170,255)'>Finding</font> Chair"
    AutoBondText.TextColor3 = Color3.new(1, 1, 1)
    AutoBondText.TextSize = 22
    AutoBondText.Font = Enum.Font.GothamBold
    AutoBondText.TextWrapped = true
    AutoBondText.RichText = true
    AutoBondText.ZIndex = 9999
    AutoBondText.Parent = game:GetService("CoreGui")

    -- Executa ação similar ao Auto Bond
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("ProximityPrompt") and obj.Parent.Name == "Bond" then
            fireproximityprompt(obj)
            wait(0.2)
        end
    end

    wait(5)
    AutoBondText:Destroy()
end)

-- Seções adicionais com rolagem
local Character = Window:NewTab("Character")
local Teleport = Window:NewTab("Teleport")
local Visual = Window:NewTab("Visual")
local Combat = Window:NewTab("Combat")
local Configuration = Window:NewTab("Configuration")

-- Função de rolagem automática (não aplicável diretamente com Kavo UI padrão)
-- Suporte visual apenas: para grande número de botões, a UI já inclui scroll

-- Botões fixos para minimizar e fechar
local CoreGui = game:GetService("CoreGui")
local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "NatanHubFloatingButtons"
ScreenGui.ResetOnSpawn = false

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Text = "-"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(0, 10, 0, 10)
MinimizeButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
MinimizeButton.TextColor3 = Color3.new(1, 1, 1)
MinimizeButton.ZIndex = 10
MinimizeButton.Parent = ScreenGui

local CloseButton = Instance.new("TextButton")
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(0, 50, 0, 10)
CloseButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.ZIndex = 10
CloseButton.Parent = ScreenGui

MinimizeButton.MouseButton1Click:Connect(function()
    for _, ui in pairs(CoreGui:GetChildren()) do
        if ui.Name == "KavoUI" then
            ui.Enabled = not ui.Enabled
        end
    end
end)

CloseButton.MouseButton1Click:Connect(function()
    for _, ui in pairs(CoreGui:GetChildren()) do
        if ui.Name == "KavoUI" or ui.Name == "NatanHubFloatingButtons" then
            ui:Destroy()
        end
    end
end)
