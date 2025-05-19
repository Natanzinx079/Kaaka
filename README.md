e desejar

-- Fim do Script
-- NatHub | Dead Rails (0.3.1) - Estilo da imagem com abas funcionais

local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatHub"
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame", ScreenGui)
Frame.AnchorPoint = Vector2.new(0.5, 0.5)
Frame.Position = UDim2.new(0.5, 0, 0.5, 0)
Frame.Size = UDim2.new(0, 500, 0, 300)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.BackgroundTransparency = 0.1
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Name = "MainFrame"

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", Frame)
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", Frame)
CloseBtn.Text = "X"
CloseBtn.Size = UDim2.new(0, 20, 0, 20)
CloseBtn.Position = UDim2.new(1, -25, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.Gotham
CloseBtn.TextSize = 14
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local SideMenu = Instance.new("Frame", Frame)
SideMenu.Position = UDim2.new(0, 0, 0, 40)
SideMenu.Size = UDim2.new(0, 120, 1, -40)
SideMenu.BackgroundTransparency = 1

local Content = Instance.new("Frame", Frame)
Content.Position = UDim2.new(0, 130, 0, 40)
Content.Size = UDim2.new(1, -140, 1, -50)
Content.BackgroundTransparency = 1
Content.Name = "ContentFrame"

local UIPageLayout = Instance.new("UIPageLayout", Content)
UIPageLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIPageLayout.EasingStyle = Enum.EasingStyle.Quad
UIPageLayout.EasingDirection = Enum.EasingDirection.Out
UIPageLayout.Padding = UDim.new(0, 0)

local Tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}

for _, name in pairs(Tabs) do
    local Button = Instance.new("TextButton", SideMenu)
    Button.Text = name
    Button.Size = UDim2.new(1, -10, 0, 30)
    Button.Position = UDim2.new(0, 5, 0, (_ - 1) * 35)
    Button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.AutoButtonColor = true

    local TabPage = Instance.new("Frame", Content)
    TabPage.Size = UDim2.new(1, 0, 1, 0)
    TabPage.BackgroundTransparency = 1
    TabPage.Name = name

    local Label = Instance.new("TextLabel", TabPage)
    Label.Size = UDim2.new(1, 0, 1, 0)
    Label.Text = "Conteúdo da aba: " .. name
    Label.BackgroundTransparency = 1
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 18
    Label.TextWrapped = true

    Button.MouseButton1Click:Connect(function()
        UIPageLayout:JumpTo(TabPage)
    end)
end
