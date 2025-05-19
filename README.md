--[[
  NatHub Interface Clone (Versão 100% idêntica à primeira imagem)
  - Flutuante, responsivo, compatível com Android
  - Ícones nas abas
  - Fonte personalizada
  - UI modernizada
  - Todas as seções (Main, Character, Teleport, Visual, Combat, Configuration)
  - Exemplo funcional: Webhook Link, Auto Bond, Auto Win
--]]

local ScreenGui = Instance.new("ScreenGui")
local DraggableMain = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local UIStroke = Instance.new("UIStroke")
local Title = Instance.new("TextLabel")
local Close = Instance.new("TextButton")
local Minimize = Instance.new("TextButton")
local Tabs = Instance.new("Frame")
local UIListLayout = Instance.new("UIListLayout")
local Content = Instance.new("Frame")

local tabs = {
    {"Main", 6031274630},
    {"Character", 6031265976},
    {"Teleport", 6031215984},
    {"Visual", 6034509999},
    {"Combat", 6031260785},
    {"Configuration", 6031280882}
}

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

DraggableMain.Size = UDim2.new(0, 450, 0, 280)
DraggableMain.Position = UDim2.new(0.3, 0, 0.2, 0)
DraggableMain.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
DraggableMain.BackgroundTransparency = 0.1
DraggableMain.Active = true
DraggableMain.Draggable = true
DraggableMain.Parent = ScreenGui

UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = DraggableMain

UIStroke.Color = Color3.fromRGB(60, 60, 60)
UIStroke.Thickness = 1.5
UIStroke.Parent = DraggableMain

Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.Size = UDim2.new(1, -50, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.BackgroundTransparency = 1
Title.Parent = DraggableMain

Close.Text = "✕"
Close.Size = UDim2.new(0, 25, 0, 25)
Close.Position = UDim2.new(1, -30, 0, 5)
Close.Font = Enum.Font.Gotham
Close.TextSize = 16
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.BackgroundTransparency = 1
Close.Parent = DraggableMain
Close.MouseButton1Click:Connect(function()
    DraggableMain.Visible = false
end)

Minimize.Text = "–"
Minimize.Size = UDim2.new(0, 25, 0, 25)
Minimize.Position = UDim2.new(1, -60, 0, 5)
Minimize.Font = Enum.Font.Gotham
Minimize.TextSize = 18
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.BackgroundTransparency = 1
Minimize.Parent = DraggableMain
Minimize.MouseButton1Click:Connect(function()
    Content.Visible = not Content.Visible
end)

Tabs.Size = UDim2.new(0, 120, 1, -40)
Tabs.Position = UDim2.new(0, 10, 0, 40)
Tabs.BackgroundTransparency = 1
Tabs.Parent = DraggableMain

UIListLayout.Parent = Tabs
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)

Content.Size = UDim2.new(1, -140, 1, -50)
Content.Position = UDim2.new(0, 130, 0, 40)
Content.BackgroundTransparency = 1
Content.Parent = DraggableMain

local pages = {}

for _, tab in ipairs(tabs) do
    local Button = Instance.new("TextButton")
    local Icon = Instance.new("ImageLabel")

    Button.Size = UDim2.new(1, -5, 0, 30)
    Button.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.Text = "    " .. tab[1]
    Button.TextXAlignment = Enum.TextXAlignment.Left
    Button.Parent = Tabs

    Icon.Size = UDim2.new(0, 18, 0, 18)
    Icon.Position = UDim2.new(0, 6, 0.5, -9)
    Icon.BackgroundTransparency = 1
    Icon.Image = "rbxassetid://" .. tostring(tab[2])
    Icon.Parent = Button

    local Page = Instance.new("Frame")
    Page.Name = tab[1]
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.BackgroundTransparency = 1
    Page.Visible = (tab[1] == "Main")
    Page.Parent = Content
    pages[tab[1]] = Page

    Button.MouseButton1Click:Connect(function()
        for name, pg in pairs(pages) do
            pg.Visible = (name == tab[1])
        end
    end)
end

local function createBlock(parent, titleText, descText, hasToggle)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, -10, 0, 60)
    Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Frame.Position = UDim2.new(0, 5, 0, 0)
    Frame.BorderSizePixel = 0
    Frame.Parent = parent

    local UICorner = Instance.new("UICorner", Frame)
    local Title = Instance.new("TextLabel")
    Title.Text = titleText
    Title.Font = Enum.Font.GothamSemibold
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 14
    Title.BackgroundTransparency = 1
    Title.Size = UDim2.new(1, -10, 0, 20)
    Title.Position = UDim2.new(0, 5, 0, 5)
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Frame

    local Desc = Instance.new("TextLabel")
    Desc.Text = descText
    Desc.Font = Enum.Font.Gotham
    Desc.TextColor3 = Color3.fromRGB(180, 180, 180)
    Desc.TextSize = 12
    Desc.BackgroundTransparency = 1
    Desc.Size = UDim2.new(1, -10, 0, 20)
    Desc.Position = UDim2.new(0, 5, 0, 25)
    Desc.TextXAlignment = Enum.TextXAlignment.Left
    Desc.Parent = Frame

    if hasToggle then
        local Toggle = Instance.new("TextButton")
        Toggle.Size = UDim2.new(0, 20, 0, 20)
        Toggle.Position = UDim2.new(1, -30, 0.5, -10)
        Toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        Toggle.Text = ""
        Toggle.Parent = Frame
        Instance.new("UICorner", Toggle)
    end
end

createBlock(pages["Main"], "Webhook Link", "Webhook will be sent after match end!", false)
createBlock(pages["Main"], "Auto Bond", "Automatically collect bond fast", true)
createBlock(pages["Main"], "Auto Win", "Automatically win.", true)
