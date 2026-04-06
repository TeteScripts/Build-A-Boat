local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ========== Auto Farm Function ==========
local isRunning = false

local function runAutoFarm()
    while isRunning do
        local char = player.Character
        if not char then task.wait(1) continue end
        local root = char:FindFirstChild("HumanoidRootPart")
        local humanoid = char:FindFirstChild("Humanoid")
        if not root or not humanoid then task.wait(1) continue end

        local speed = 400

        local function teleportTo(targetCFrame)
            if not isRunning then return end
            local distance = (root.Position - targetCFrame.Position).Magnitude
            local info = TweenInfo.new(distance / speed, Enum.EasingStyle.Linear)

            local ff = Instance.new("ForceField")
            ff.Visible = false
            ff.Parent = char

            root.Anchored = true
            for _, part in pairs(char:GetChildren()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end

            local tween = TweenService:Create(root, info, {CFrame = targetCFrame})
            tween:Play()
            tween.Completed:Wait()
            ff:Destroy()
        end

        for i = 1, 10 do
            if not isRunning then break end
            local stage = workspace.BoatStages.NormalStages:FindFirstChild("CaveStage" .. i)
            if stage then
                teleportTo(stage.DarknessPart.CFrame * CFrame.new(0, 60, 0))
            end
        end

        if isRunning then
            local chestTrigger = workspace.BoatStages.NormalStages.TheEnd.GoldenChest.Trigger
            if chestTrigger then
                teleportTo(chestTrigger.CFrame * CFrame.new(0, 3, 0))
                task.wait(0.5)
                root.Anchored = false
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then part.CanCollide = true end
                end
            end
        end
        task.wait(1)
    end

    local char = player.Character
    if char then
        local r = char:FindFirstChild("HumanoidRootPart")
        if r then
            r.Anchored = false
            for _, part in pairs(char:GetChildren()) do
                if part:IsA("BasePart") then part.CanCollide = true end
            end
        end
    end
end

-- ========== UI ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Tezxz0Hub"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- ========== ปุ่มลอย ==========
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 38, 0, 38)
toggleBtn.Position = UDim2.new(0, 10, 0.5, -19)
toggleBtn.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
toggleBtn.Text = "T"
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.TextScaled = true
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.BorderSizePixel = 0
toggleBtn.ZIndex = 10
toggleBtn.Parent = screenGui
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)

-- ========== Main Frame ==========
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 340, 0, 95)
main.Position = UDim2.new(0.5, -170, 0.5, -47)
main.BackgroundColor3 = Color3.fromRGB(28, 26, 35)
main.BorderSizePixel = 0
main.Visible = false
main.ZIndex = 5
main.Parent = screenGui
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(160, 60, 200)
stroke.Thickness = 1.5
stroke.Parent = main

-- ========== Title Bar ==========
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 32)
titleBar.BackgroundColor3 = Color3.fromRGB(22, 20, 30)
titleBar.BorderSizePixel = 0
titleBar.ZIndex = 5
titleBar.Parent = main
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 8)

local iconLabel = Instance.new("TextLabel")
iconLabel.Size = UDim2.new(0, 24, 0, 24)
iconLabel.Position = UDim2.new(0, 8, 0.5, -12)
iconLabel.BackgroundTransparency = 1
iconLabel.Text = "⚡"
iconLabel.TextScaled = true
iconLabel.ZIndex = 5
iconLabel.Parent = titleBar

local hubName = Instance.new("TextLabel")
hubName.Size = UDim2.new(0, 80, 1, 0)
hubName.Position = UDim2.new(0, 36, 0, 0)
hubName.BackgroundTransparency = 1
hubName.Text = "Tezxz0"
hubName.TextColor3 = Color3.fromRGB(200, 80, 255)
hubName.Font = Enum.Font.GothamBold
hubName.TextSize = 13
hubName.TextXAlignment = Enum.TextXAlignment.Left
hubName.ZIndex = 5
hubName.Parent = titleBar

local hubSub = Instance.new("TextLabel")
hubSub.Size = UDim2.new(0, 40, 1, 0)
hubSub.Position = UDim2.new(0, 116, 0, 0)
hubSub.BackgroundTransparency = 1
hubSub.Text = "Hub"
hubSub.TextColor3 = Color3.fromRGB(220, 220, 220)
hubSub.Font = Enum.Font.GothamBold
hubSub.TextSize = 13
hubSub.TextXAlignment = Enum.TextXAlignment.Left
hubSub.ZIndex = 5
hubSub.Parent = titleBar

local line = Instance.new("Frame")
line.Size = UDim2.new(1, 0, 0, 1)
line.Position = UDim2.new(0, 0, 1, -1)
line.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
line.BorderSizePixel = 0
line.ZIndex = 5
line.Parent = titleBar

-- ปุ่ม —
local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 26, 0, 26)
minBtn.Position = UDim2.new(1, -58, 0.5, -13)
minBtn.BackgroundTransparency = 1
minBtn.Text = "—"
minBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
minBtn.Font = Enum.Font.GothamBold
minBtn.TextSize = 13
minBtn.ZIndex = 6
minBtn.Parent = titleBar

-- ปุ่ม X
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 26, 0, 26)
closeBtn.Position = UDim2.new(1, -28, 0.5, -13)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(200, 80, 80)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 13
closeBtn.ZIndex = 6
closeBtn.Parent = titleBar

-- ========== Checkbox ==========
local function createCheckbox(labelText, posY, onEnable, onDisable)
    local row = Instance.new("Frame")
    row.Size = UDim2.new(1, -16, 0, 40)
    row.Position = UDim2.new(0, 8, 0, posY)
    row.BackgroundTransparency = 1
    row.ZIndex = 5
    row.Parent = main

    local box = Instance.new("Frame")
    box.Size = UDim2.new(0, 22, 0, 22)
    box.Position = UDim2.new(0, 0, 0.5, -11)
    box.BackgroundColor3 = Color3.fromRGB(70, 65, 85)
    box.BorderSizePixel = 0
    box.ZIndex = 5
    box.Parent = row
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 4)

    local checkMark = Instance.new("TextLabel")
    checkMark.Size = UDim2.new(1, 0, 1, 0)
    checkMark.BackgroundTransparency = 1
    checkMark.Text = ""
    checkMark.TextColor3 = Color3.fromRGB(255, 255, 255)
    checkMark.Font = Enum.Font.GothamBold
    checkMark.TextSize = 13
    checkMark.ZIndex = 6
    checkMark.Parent = box

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -32, 1, 0)
    label.Position = UDim2.new(0, 30, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = "[ Enable ] " .. labelText
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.Font = Enum.Font.Gotham
    label.TextSize = 11
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.ZIndex = 5
    label.Parent = row

    local enabled = false
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    btn.ZIndex = 7
    btn.Parent = row

    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            box.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
            checkMark.Text = "✓"
            label.Text = "[ Disable ] " .. labelText
            label.TextColor3 = Color3.fromRGB(200, 80, 255)
            if onEnable then onEnable() end
        else
            box.BackgroundColor3 = Color3.fromRGB(70, 65, 85)
            checkMark.Text = ""
            label.Text = "[ Enable ] " .. labelText
            label.TextColor3 = Color3.fromRGB(200, 200, 200)
            if onDisable then onDisable() end
        end
    end)
end

createCheckbox("Auto Farm Gold & Gold Block", 34,
    function()
        isRunning = true
        task.spawn(runAutoFarm)
    end,
    function()
        isRunning = false
    end
)

-- ========== เปิด/ปิด ==========
local isOpen = false
local isMinimized = false

toggleBtn.MouseButton1Click:Connect(function()
    isOpen = not isOpen
    main.Visible = isOpen
    -- ซ่อนปุ่มลอยเมื่อเปิด UI
    toggleBtn.Visible = not isOpen
end)

minBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        main.Size = UDim2.new(0, 340, 0, 32)
    else
        main.Size = UDim2.new(0, 340, 0, 95)
    end
end)

closeBtn.MouseButton1Click:Connect(function()
    isRunning = false
    main.Visible = false
    toggleBtn.Visible = true
    isOpen = false
    isMinimized = false
    main.Size = UDim2.new(0, 340, 0, 95)
end)

-- ========== Drag ==========
local dragging, dragStart, startPos
titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = main.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
    or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        main.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + delta.X,
            startPos.Y.Scale, startPos.Y.Offset + delta.Y
        )
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)
