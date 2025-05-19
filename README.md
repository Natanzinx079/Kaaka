--[[
  Natan Dead - Script Profissional para Dead Rails (Roblox)
  Compatível com Delta Executor e Android
  Desenvolvido com funções completas e interface moderna
--]]

-- Proteção e Serviços
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

-- Anti-duplo
pcall(function()
    if CoreGui:FindFirstChild("NatanDead") then
        CoreGui:FindFirstChild("NatanDead"):Destroy()
    end
end)

-- Interface
local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "NatanDead"
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 500, 0, 350)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", MainFrame)
Title.Text = "Natan Dead"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextScaled = true

local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Text = "X"
CloseBtn.Size = UDim2.new(0, 40, 0, 40)
CloseBtn.Position = UDim2.new(1, -40, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextScaled = true
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local MinBtn = Instance.new("TextButton", MainFrame)
MinBtn.Text = "-"
MinBtn.Size = UDim2.new(0, 40, 0, 40)
MinBtn.Position = UDim2.new(1, -80, 0, 0)
MinBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextScaled = true
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    MainFrame.Size = minimized and UDim2.new(0, 500, 0, 50) or UDim2.new(0, 500, 0, 350)
end)

-- Botões
local function createButton(name, yPos, callback)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Text = name
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, yPos)
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Gotham
    btn.TextScaled = true
    btn.MouseButton1Click:Connect(callback)
end

-- Funções
createButton("TP: Trem", 50, function()
    LocalPlayer.Character:PivotTo(workspace.Train.PrimaryPart.CFrame * CFrame.new(0, 5, 0))
end)

createButton("TP: Base 1", 100, function()
    LocalPlayer.Character:PivotTo(CFrame.new(235, 90, -1965))
end)

createButton("TP: Base 2", 150, function()
    LocalPlayer.Character:PivotTo(CFrame.new(-2305, 90, -1812))
end)

createButton("TP: Base 3", 200, function()
    LocalPlayer.Character:PivotTo(CFrame.new(-430, 90, 1900))
end)

createButton("AutoFarm", 250, function()
    if not _G.farming then
        _G.farming = true
        while _G.farming and task.wait(0.2) do
            pcall(function()
                LocalPlayer.Character:PivotTo(workspace.Train.PrimaryPart.CFrame * CFrame.new(0, 5, 0))
                fireproximityprompt(workspace.Train:FindFirstChildOfClass("ProximityPrompt"), 1)
            end)
        end
    else
        _G.farming = false
    end
end)

createButton("ESP Trem", 300, function()
    for _,v in pairs(workspace:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("Humanoid") and not v:FindFirstChild("ESP") then
            local bill = Instance.new("BillboardGui", v)
            bill.Name = "ESP"
            bill.Size = UDim2.new(0, 100, 0, 40)
            bill.AlwaysOnTop = true
            bill.Adornee = v:FindFirstChild("Head")
            local txt = Instance.new("TextLabel", bill)
            txt.Size = UDim2.new(1, 0, 1, 0)
            txt.Text = v.Name
            txt.TextColor3 = Color3.new(1, 0, 0)
            txt.BackgroundTransparency = 1
        end
    end
end)

createButton("Noclip (Toggle N)", 350, function()
    local noclip = false
    Mouse.KeyDown:Connect(function(key)
        if key == "n" then
            noclip = not noclip
        end
    end)
    RunService.Stepped:Connect(function()
        if noclip and LocalPlayer.Character then
            for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide == true then
                    v.CanCollide = false
                end
            end
        end
    end)
end)

-- Extras
createButton("Full Bright", 400, function()
    game:GetService("Lighting").Brightness = 3
    game:GetService("Lighting").TimeOfDay = "14:00:00"
end)

createButton("Remove Fog", 450, function()
    game:GetService("Lighting").FogEnd = 1e10
end)

createButton("Unlock Mouse", 500, function()
    game:GetService("UserInputService”).MouseBehavior = Enum.MouseBehavior.Default
end)

createButton("Unlock Camera", 550, function()
    LocalPlayer.CameraMaxZoomDistance = 1000
end)
