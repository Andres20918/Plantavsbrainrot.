-- Hub: andrew41
-- Funciona en KRNL y Delta

-- Servicios
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Crear ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "andrew41_Hub"
screenGui.Parent = game:GetService("CoreGui")

-- Función para crear marco movible
local function createMovableFrame()
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 200)
    frame.Position = UDim2.new(0.5, -150, 0.5, -100)
    frame.BackgroundColor3 = Color3.new(0,0,0)
    frame.BackgroundTransparency = 0.5
    frame.BorderSizePixel = 0
    frame.Active = true
    frame.Draggable = true
    frame.Parent = screenGui

    -- Minimizar botón
    local minimizeBtn = Instance.new("TextButton")
    minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
    minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
    minimizeBtn.Text = "_"
    minimizeBtn.TextColor3 = Color3.new(1,1,1)
    minimizeBtn.BackgroundColor3 = Color3.new(1,0,0)
    minimizeBtn.Parent = frame

    local minimized = false
    minimizeBtn.MouseButton1Click:Connect(function()
        minimized = not minimized
        for _, v in pairs(frame:GetChildren()) do
            if v:IsA("TextButton") and v ~= minimizeBtn then
                v.Visible = not minimized
            end
        end
        frame.Size = minimized and UDim2.new(0, 150, 0, 40) or UDim2.new(0, 300, 0, 200)
    end)

    return frame
end

local hubFrame = createMovableFrame()

-- Función para crear botones
local function createButton(parent, text, position, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 120, 0, 40)
    button.Position = position
    button.Text = text
    button.TextColor3 = Color3.new(1,1,1)
    button.BackgroundColor3 = Color3.new(1,0,0)
    button.BorderSizePixel = 2
    button.BorderColor3 = Color3.fromRGB(255, 0, 255) -- Raindow borde inicial
    button.Parent = parent

    -- Animar borde arcoíris
    spawn(function()
        while true do
            for i = 0, 1, 0.01 do
                local color = Color3.fromHSV(i, 1, 1)
                button.BorderColor3 = color
                wait(0.03)
            end
        end
    end)

    button.MouseButton1Click:Connect(callback)
    return button
end

-- Función duplicar plantas
local function duplicatePlant()
    local tool = player.Character and player.Character:FindFirstChildOfClass("Tool")
    if tool then
        local clone = tool:Clone()
        clone.Parent = workspace
        clone.Handle.CFrame = player.Character.HumanoidRootPart.CFrame + Vector3.new(3,0,0)
        print("Planta duplicada lista para tradear/colocar.")
    else
        print("No tienes ninguna planta en la mano.")
    end
end

-- Función duplicar cerebro
local function duplicateBrain()
    local tool = player.Character and player.Character:FindFirstChild("Cerebro") or player.Backpack:FindFirstChild("Cerebro")
    if tool then
        local clone = tool:Clone()
        clone.Parent = workspace
        clone.Handle.CFrame = player.Character.HumanoidRootPart.CFrame + Vector3.new(3,0,0)
        print("Cerebro duplicado listo para tradear.")
    else
        print("No tienes el Cerebro en la mano o mochila.")
    end
end

-- Crear botones
createButton(hubFrame, "Duplique Plants", UDim2.new(0, 20, 0, 50), duplicatePlant)
createButton(hubFrame, "Duplique Brain", UDim2.new(0, 160, 0, 50), duplicateBrain)
