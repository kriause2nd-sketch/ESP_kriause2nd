-- // Configuration & State
local SETTINGS = {
    Aimbot = {Enabled = true, Smoothness = 0.2, FOV = 150, Sensitivity = 0.45},
    Fly = {Enabled = false, Speed = 1.3, Smoothness = 0.12},
    ESP = {Enabled = true}
}

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- // --- Neon Sidebar UI (Dynamic Update) ---
local UI_Labels = {}
local function createUI()
    local sg = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
    sg.Name = "RivalsMaster"
    
    local container = Instance.new("Frame", sg)
    container.Size = UDim2.new(0, 180, 0, 140)
    container.Position = UDim2.new(0, 20, 0.5, -70)
    container.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    container.BackgroundTransparency = 0.2
    container.BorderSizePixel = 0
    Instance.new("UICorner", container).CornerRadius = UDim.new(0, 10)
    
    local grad = Instance.new("UIGradient", container)
    grad.Color = ColorSequence.new(Color3.fromRGB(0, 255, 150), Color3.fromRGB(0, 150, 255))

    local function addLabel(name, key, pos)
        local l = Instance.new("TextLabel", container)
        l.Size = UDim2.new(1, -20, 0, 30)
        l.Position = UDim2.new(0, 10, 0, pos)
        l.BackgroundTransparency = 1
        l.TextColor3 = Color3.new(1, 1, 1)
        l.Font = Enum.Font.GothamBold
        l.TextSize = 12
        l.TextXAlignment = Enum.TextXAlignment.Left
        UI_Labels[name] = {Label = l, Key = key}
    end

    addLabel("Fly", "F", 15)
    addLabel("Aimbot", "G", 45)
    addLabel("ESP", "H", 75)
    
    local tip = Instance.new("TextLabel", container)
    tip.Size = UDim2.new(1, 0, 0, 20)
    tip.Position = UDim2.new(0, 0, 0, 110)
    tip.Text = "Vertical Fly: Q & E"
    tip.TextColor3 = Color3.fromRGB(150, 150, 150)
    tip.TextSize = 10
    tip.BackgroundTransparency = 1
    tip.Parent = container
end

local function updateUI()
    for name, data in pairs(UI_Labels) do
        local state = SETTINGS[name].Enabled and "ON" or "OFF"
        local color = SETTINGS[name].Enabled and "0, 255, 150" or "255, 50, 50"
        data.Label.Text = string.format("â— %s [%s]: %s", name:upper(), data.Key, state)
    end
end

-- // --- Functions ---
local function getTarget()
    if not SETTINGS.Aimbot.Enabled then return nil end
    local mouse = UserInputService:GetMouseLocation()
    local closest, dist = nil, SETTINGS.Aimbot.FOV
    
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
            local head = p.Character.Head
            local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
            if onScreen then
                local mag = (Vector2.new(screenPos.X, screenPos.Y) - mouse).Magnitude
                if mag < dist then
                    dist = mag
                    closest = screenPos
                end
            end
        end
    end
    return closest
end

local function cleanESP()
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Character then
            local hl = p.Character:FindFirstChild("Highlight")
            if hl then hl:Destroy() end
        end
    end
end

-- // --- Main Loop ---
local flyVel = Vector3.zero

RunService.RenderStepped:Connect(function()
    -- ESP Management
    if SETTINGS.ESP.Enabled then
        if tick() % 0.5 < 0.02 then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and not p.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", p.Character)
                    hl.FillColor = Color3.fromRGB(0, 200, 255)
                    hl.OutlineColor = Color3.new(1, 1, 1)
                end
            end
        end
    else
        if tick() % 0.5 < 0.02 then cleanESP() end
    end

    -- Mouse-Rel Aimbot (Hold RMB)
    if SETTINGS.Aimbot.Enabled and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local target = getTarget()
        if target then
            local mousePos = UserInputService:GetMouseLocation()
            local dx = (target.X - mousePos.X) * SETTINGS.Aimbot.Sensitivity
            local dy = (target.Y - mousePos.Y) * SETTINGS.Aimbot.Sensitivity
            if mousemoverel then
                mousemoverel(dx / (SETTINGS.Aimbot.Smoothness * 10), dy / (SETTINGS.Aimbot.Smoothness * 10))
            end
        end
    end

    -- Q/E Smooth Fly
    if SETTINGS.Fly.Enabled and LocalPlayer.Character then
        local root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root then
            local dir = Vector3.zero
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir += Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir -= Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir -= Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir += Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.E) then dir += Vector3.new(0, 1, 0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.Q) then dir -= Vector3.new(0, 1, 0) end

            flyVel = flyVel:Lerp(dir * SETTINGS.Fly.Speed, SETTINGS.Fly.Smoothness)
            root.CFrame = root.CFrame + flyVel
            root.AssemblyLinearVelocity = Vector3.zero
        end
    end
end)

-- // --- Keybind Logic ---
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    
    if input.KeyCode == Enum.KeyCode.F then
        SETTINGS.Fly.Enabled = not SETTINGS.Fly.Enabled
        if LocalPlayer.Character then
            LocalPlayer.Character.Humanoid.PlatformStand = SETTINGS.Fly.Enabled
        end
    elseif input.KeyCode == Enum.KeyCode.G then
        SETTINGS.Aimbot.Enabled = not SETTINGS.Aimbot.Enabled
    elseif input.KeyCode == Enum.KeyCode.H then
        SETTINGS.ESP.Enabled = not SETTINGS.ESP.Enabled
    end
    updateUI()
end)

createUI()
updateUI()
