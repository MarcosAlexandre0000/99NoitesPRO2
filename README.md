local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Variáveis de controle
local autoFarm = false
local autoCollect = false
local autoStats = false
local farmDelay = 1

-- Criando GUI
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 300, 0, 350)
Frame.Position = UDim2.new(0, 50, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
Frame.Active = true
Frame.Draggable = true

-- Título
local title = Instance.new("TextLabel", Frame)
title.Size = UDim2.new(1,0,0,40)
title.Position = UDim2.new(0,0,0,0)
title.Text = "99 Noites Delta GUI"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1

-- Função criar botão toggle simples
local function createToggleButton(name, yPos, variable)
    local btn = Instance.new("TextButton", Frame)
    btn.Size = UDim2.new(0, 260, 0, 35)
    btn.Position = UDim2.new(0,20,0,yPos)
    btn.BackgroundColor3 = Color3.fromRGB(60,60,60)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.TextSize = 16
    btn.Text = name.." : "..(variable() and "ON" or "OFF")
    
    btn.MouseButton1Click:Connect(function()
        variable(not variable())
        btn.Text = name.." : "..(variable() and "ON" or "OFF")
    end)
end

-- Função criar slider simples
local function createSlider(name, yPos, variable, min, max)
    local label = Instance.new("TextLabel", Frame)
    label.Size = UDim2.new(0,260,0,25)
    label.Position = UDim2.new(0,20,0,yPos)
    label.Text = name..": "..variable()
    label.TextColor3 = Color3.new(1,1,1)
    label.BackgroundTransparency = 1
    label.TextSize = 14
    
    local slider = Instance.new("Frame", Frame)
    slider.Size = UDim2.new(0,260,0,15)
    slider.Position = UDim2.new(0,20,0,yPos+25)
    slider.BackgroundColor3 = Color3.fromRGB(80,80,80)
    
    local fill = Instance.new("Frame", slider)
    fill.Size = UDim2.new((variable()-min)/(max-min),0,1,0)
    fill.BackgroundColor3 = Color3.fromRGB(0,200,255)
    
    slider.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            local dragging = true
            local function move(input)
                local pos = math.clamp(input.Position.X - slider.AbsolutePosition.X,0,slider.AbsoluteSize.X)
                local val = min + (pos/slider.AbsoluteSize.X)*(max-min)
                variable(val)
                fill.Size = UDim2.new((val-min)/(max-min),0,1,0)
                label.Text = name..": "..string.format("%.2f",val)
            end
            local connection1
            local connection2
            connection1 = mouse.Move:Connect(move)
            connection2 = mouse.Button1Up:Connect(function()
                connection1:Disconnect()
                connection2:Disconnect()
            end)
        end
    end)
end
