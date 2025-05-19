--[[
  NatanHub (Dead Rails) – Estilo Nathub
  ---------------------------------------------------------
  • Compatível com Delta Executor (Android) – GUI vai para PlayerGui
  • Visual idêntico ao Nathub (Imagem 2)
  • Abas laterais: Main, Character, Teleport, Visual, Combat, Config
  • Aba Main com Webhook Link, Auto Bond (toggle) e Auto Win (botão com ícone)
  ---------------------------------------------------------
]]

-->> CONFIG ----------------------------------------------
local HUB_NAME      = "NatanHub"
local HUB_VERSION   = "0.3.1"
local ICON_SPARKLE  = 6034509999   -- Ícone usado no botão Auto Win
-----------------------------------------------------------

-- Serviços ------------------------------------------------
local Players = game:FindService("Players")
local LocalPlayer = Players.LocalPlayer

-- ScreenGui ------------------------------------------------
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = HUB_NAME
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Main frame ----------------------------------------------
local main = Instance.new("Frame", ScreenGui)
main.Size = UDim2.new(0, 420, 0, 280)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.BorderSizePixel = 0
main.Active, main.Draggable = true, true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", main).Color = Color3.fromRGB(45,45,45)

-- Top bar --------------------------------------------------
local top = Instance.new("Frame", main)
top.Size = UDim2.new(1, 0, 0, 32)
top.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
top.BorderSizePixel = 0
Instance.new("UICorner", top).CornerRadius = UDim.new(0, 10)

-- Título ---------------------------------------------------
local title = Instance.new("TextLabel", top)
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = (HUB_NAME.." | Dead Rails ("..HUB_VERSION..")")
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 16
title.Font = Enum.Font.GothamBold
title.BackgroundTransparency = 1
title.TextXAlignment = Enum.TextXAlignment.Left

-- Botão Fechar --------------------------------------------
local closeBtn = Instance.new("TextButton", top)
closeBtn.Size = UDim2.new(0, 28, 0, 22)
closeBtn.Position = UDim2.new(1, -34, 0.5, -11)
closeBtn.Text = "✕"
closeBtn.Font = Enum.Font.Gotham
closeBtn.TextSize = 16
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.BackgroundColor3 = Color3.fromRGB(180,30,30)
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 6)
closeBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão Minimizar -----------------------------------------
local miniBtnTop = Instance.new("TextButton", top)
miniBtnTop.Size = UDim2.new(0, 28, 0, 22)
miniBtnTop.Position = UDim2.new(1, -68, 0.5, -11)
miniBtnTop.Text = "–"
miniBtnTop.Font = Enum.Font.Gotham
miniBtnTop.TextSize = 22
miniBtnTop.TextColor3 = Color3.new(1,1,1)
miniBtnTop.BackgroundColor3 = Color3.fromRGB(60,60,60)
Instance.new("UICorner", miniBtnTop).CornerRadius = UDim.new(0, 6)

-- Abas Laterais -------------------------------------------
local side = Instance.new("Frame", main)
side.Size = UDim2.new(0, 110, 1, -32)
side.Position = UDim2.new(0, 0, 0, 32)
side.BackgroundColor3 = Color3.fromRGB(25,25,25)
side.BorderSizePixel = 0
Instance.new("UICorner", side).CornerRadius = UDim.new(0, 6)

local tabNames = {"Main","Character","Teleport","Visual","Combat","Config"}
local pages = {}

local list = Instance.new("UIListLayout", side)
list.Padding = UDim.new(0,4)
list.FillDirection = Enum.FillDirection.Vertical
list.SortOrder = Enum.SortOrder.LayoutOrder

for i,name in ipairs(tabNames) do
    local tab = Instance.new("TextButton", side)
    tab.Size = UDim2.new(1, -8, 0, 30)
    tab.Position = UDim2.new(0, 4, 0, 0)
    tab.Text = name
    tab.Font = Enum.Font.GothamMedium
    tab.TextSize = 14
    tab.TextColor3 = Color3.fromRGB(240,240,240)
    tab.BackgroundColor3 = Color3.fromRGB(35,35,35)
    Instance.new("UICorner", tab).CornerRadius = UDim.new(0,6)

    local page = Instance.new("Frame", main)
    page.Size = UDim2.new(1, -120, 1, -42)
    page.Position = UDim2.new(0, 120, 0, 42)
    page.BackgroundColor3 = Color3.fromRGB(30,30,30)
    page.Visible = (i==1)
    Instance.new("UICorner", page).CornerRadius = UDim.new(0,6)

    pages[name] = page
    tab.MouseButton1Click:Connect(function()
        for _,p in pairs(pages) do p.Visible = false end
        page.Visible = true
    end)
end

-->> FUNÇÕES DE CONSTRUÇÃO DE COMPONENTES ------------------
local function newSection(parent, titleText)
    local y = (#parent:GetChildren())*36
    local title = Instance.new("TextLabel", parent)
    title.Size = UDim2.new(1, -20, 0, 18)
    title.Position = UDim2.new(0, 10, 0, y)
    title.Text = titleText
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Font = Enum.Font.GothamBold
    title.TextSize = 14
    title.TextColor3 = Color3.fromRGB(255,255,255)
    title.BackgroundTransparency = 1
    return y+22
end

local function newInput(parent, placeholder)
    local y = (#parent:GetChildren())*36
    local box = Instance.new("TextBox", parent)
    box.Size = UDim2.new(1, -20, 0, 30)
    box.Position = UDim2.new(0, 10, 0, y)
    box.PlaceholderText = placeholder
    box.ClearTextOnFocus = false
    box.Text = ""
    box.TextColor3 = Color3.new(1,1,1)
    box.TextSize = 14
    box.Font = Enum.Font.Gotham
    box.BackgroundColor3 = Color3.fromRGB(40,40,40)
    Instance.new("UICorner", box).CornerRadius = UDim.new(0,6)
    return box
end

local function newToggle(parent,labelText,callback)
    local y = (#parent:GetChildren())*36
    local holder = Instance.new("Frame", parent)
    holder.Size = UDim2.new(1,-20,0,30)
    holder.Position = UDim2.new(0,10,0,y)
    holder.BackgroundTransparency = 1

    local lbl = Instance.new("TextLabel", holder)
    lbl.Size = UDim2.new(1,-50,1,0)
    lbl.Text = labelText
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 14
    lbl.TextColor3 = Color3.fromRGB(220,220,220)
    lbl.BackgroundTransparency = 1

    local tog = Instance.new("TextButton", holder)
    tog.Size = UDim2.new(0,30,0,18)
    tog.Position = UDim2.new(1,-30,0.5,-9)
    tog.BackgroundColor3 = Color3.fromRGB(60,60,60)
    tog.Text = ""
    tog.AutoButtonColor = false
    Instance.new("UICorner", tog).CornerRadius = UDim.new(0,6)

    local state=false
    local function setState(on)
        state = on
        tog.BackgroundColor3 = on and Color3.fromRGB(0,170,255) or Color3.fromRGB(60,60,60)
        if callback then callback(on) end
    end
    tog.MouseButton1Click:Connect(function()
        setState(not state)
    end)
    return setState
end

local function newButton(parent,labelText,callback,iconId)
    local y = (#parent:GetChildren())*36
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(1,-20,0,30)
    btn.Position = UDim2.new(0,10,0,y)
    btn.Text = ""
    btn.AutoButtonColor = false
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)

    local lbl = Instance.new("TextLabel", btn)
    lbl.Size = UDim2.new(1,-40,1,0)
    lbl.Text = labelText
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 14
    lbl.TextColor3 = Color3.fromRGB(240,240,240)
    lbl.BackgroundTransparency = 1

    if iconId then
        local ic = Instance.new("ImageLabel", btn)
        ic.Size = UDim2.new(0,20,0,20)
        ic.Position = UDim2.new(1,-25,0.5,-10)
        ic.BackgroundTransparency = 1
        ic.Image = "rbxassetid://"..iconId
    end

    btn.MouseButton1Click:Connect(function()
        if callback then callback() end
    end)
    return btn
end

-->> CONTEÚDO DA ABA MAIN ----------------------------------
local mainPage = pages["Main"]

newSection(mainPage,"Webhook Link")
local webhookBox = newInput(mainPage,"Your Webhook Link")

newSection(mainPage,"Auto Bond")
local toggleBond = newToggle(mainPage,"Automatically collect bond fast",function(on)
    -- TODO: implemente lógica Auto Bond
end)

newSection(mainPage,"Win")
newButton(mainPage,"Auto Win",function()
    -- TODO: implemente lógica Auto Win
end,ICON_SPARKLE)

-->> SLIDERS DE CHARACTER (aba Character) ------------------
local charPage = pages["Character"]

local function makeSlider(labelText,minVal,maxVal,defaultVal,onChange)
    local y = (#charPage:GetChildren())*36
    local lbl = Instance.new("TextLabel", charPage)
    lbl.Size = UDim2.new(0,200,0,20)
    lbl.Position = UDim2.new(0,10,0,y)
    lbl.Text = labelText..": "..defaultVal
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 14
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.BackgroundTransparency = 1

    local box = Instance.new("TextBox", charPage)
    box.Size = UDim2.new(0,60,0,20)
    box.Position = UDim2.new(0,220,0,y)
    box.Text = tostring(defaultVal)
    box.TextColor3 = Color3.new(1,1,1)
    box.BackgroundColor3 = Color3.fromRGB(50,50,50)
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    Instance.new("UICorner", box).CornerRadius = UDim.new(0,6)

    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then
            val = math.clamp(val,minVal,maxVal)
            lbl.Text = labelText..": "..val
            onChange(val)
        end
    end)
end

makeSlider("WalkSpeed",16,100,16,function(v)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = v
    end
end)

makeSlider("JumpPower",50,200,50,function(v)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = v
    end
end)

-->> MINIMIZAR --------------------------------------------
local dockBtn = Instance.new("TextButton", ScreenGui)
dockBtn.Size = UDim2.new(0,110,0,32)
dockBtn.Position = UDim2.new(0,10,0.45,0)
dockBtn.Text = HUB_NAME

dockBtn.Font = Enum.Font.GothamBold
dockBtn.TextSize = 14
dockBtn.TextColor3 = Color3.new(1,1,1)
dockBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
Instance.new("UICorner", dockBtn).CornerRadius = UDim.new(0,6)
dockBtn.Visible = false

afterMinimized = false
miniBtnTop.MouseButton1Click:Connect(function()
    main.Visible = false
    dockBtn.Visible = true
end)

dockBtn.MouseButton1Click:Connect(function()
    main.Visible = true
    dockBtn.Visible = false
end)
