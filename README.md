local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ========== Auto Farm Function ==========
local isRunning = false

local function runAutoFarm()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")
    local speed = 400

    local function teleportTo(targetCFrame)
        if not isRunning then return end
        local distance = (root.Position - targetCFrame.Position).Magnitude
        local info = TweenInfo.new(distance / speed, Enum.EasingStyle.Linear)

        root.Anchored = true
        for _, part in pairs(char:GetChildren()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end

        local tween = TweenService:Create(root, info, {CFrame = targetCFrame})
        tween:Play()
        tween.Completed:Wait()
    end

    while isRunning do
        char = player.Character or player.CharacterAdded:Wait()
        root = char:WaitForChild("HumanoidRootPart")

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

    -- Cleanup เมื่อหยุด
    if player.Character then
        local r = player.Character:FindFirstChild("HumanoidRootPart")
        if r then
            r.Anchored = false
            for _, part in pairs(player.Character:GetChildren()) do
                if part:IsA("BasePart") then part.CanCollide = true end
            end
        end
    end
end

-- ========== UI ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SYLASSHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Main Frame
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 340, 0, 110)
main.Position = UDim2.new(0.5, -170, 0.5, -55)
main.BackgroundColor3 = Color3.fromRGB(28, 26, 35)
main.BorderSizePixel = 0
main.Parent = screenGui
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)

-- เส้นขอบสีม่วง
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(160, 60, 200)
stroke.Thickness = 1.5
stroke.Parent = main

-- ========== Title Bar ==========
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 36)
titleBar.BackgroundColor3 = Color3.fromRGB(22, 20, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = main
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 8)

-- ไอคอน ⚡
local iconLabel = Instance.new("TextLabel")
iconLabel.Size = UDim2.new(0, 28, 0, 28)
iconLabel.Position = UDim2.new(0, 8, 0.5, -14)
iconLabel.BackgroundTransparency = 1
iconLabel.Text = "⚡"
iconLabel.TextScaled = true
iconLabel.Parent = titleBar

-- ชื่อ Hub
local hubName = Instance.new("TextLabel")
hubName.Size = UDim2.new(0, 80, 1, 0)
hubName.Position = UDim2.new(0, 38, 0, 0)
hubName.BackgroundTransparency = 1
hubName.Text = "SYLASS"
hubName.TextColor3 = Color3.fromRGB(220, 80, 255)
hubName.Font = Enum.Font.GothamBold
hubName.TextSize = 14
hubName.TextXAlignment = Enum.TextXAlignment.Left
hubName.Parent = titleBar

local hubSub = Instance.new("TextLabel")
hubSub.Size = UDim2.new(0, 50, 1, 0)
hubSub.Position = UDim2.new(0, 118, 0, 0)
hubSub.BackgroundTransparency = 1
hubSub.Text = "Hub"
hubSub.TextColor3 = Color3.fromRGB(220, 220, 220)
hubSub.Font = Enum.Font.GothamBold
hubSub.TextSize = 14
hubSub.TextXAlignment = Enum.TextXAlignment.Left
hubSub.Parent = titleBar

-- เส้นแบ่งใต้ titlebar
local line = Instance.new("Frame")
line.Size = UDim2.new(1, 0, 0, 1)
line.Position = UDim2.new(0, 0, 1, -1)
line.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
line.BorderSizePixel = 0
line.Parent = titleBar

-- ปุ่ม — (minimize)
local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 28, 0, 28)
minBtn.Position = UDim2.new(1, -62, 0.5, -14)
minBtn.BackgroundTransparency = 1
minBtn.Text = "—"
minBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
minBtn.Font = Enum.Font.GothamBold
minBtn.TextSize = 14
minBtn.Parent = titleBar

-- ปุ่ม X
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 28, 0, 28)
closeBtn.Position = UDim2.new(1, -30, 0.5, -14)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(200, 80, 80)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
closeBtn.Parent = titleBar

-- ========== Checkbox Button ==========
local function createCheckbox(parent, labelText, posY, onEnable, onDisable)
    local row = Instance.new("Frame")
    row.Size = UDim2.new(1, -16, 0, 36)
    row.Position = UDim2.new(0, 8, 0, posY)
    row.BackgroundTransparency = 1
    row.Parent = parent

    -- Checkbox box
    local box = Instance.new("Frame")
    box.Size = UDim2.new(0, 24, 0, 24)
    box.Position = UDim2.new(0, 0, 0.5, -12)
    box.BackgroundColor3 = Color3.fromRGB(70, 65, 85)
    box.BorderSizePixel = 0
    box.Parent = row
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 4)

    local checkMark = Instance.new("TextLabel")
    checkMark.Size = UDim2.new(1, 0, 1, 0)
    checkMark.BackgroundTransparency = 1
    checkMark.Text = ""
    checkMark.TextColor3 = Color3.fromRGB(255, 255, 255)
    checkMark.Font = Enum.Font.GothamBold
    checkMark.TextSize = 14
    checkMark.Parent = box

    -- Label
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -34, 1, 0)
    label.Position = UDim2.new(0, 34, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = labelText
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.Font = Enum.Font.Gotham
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = row

    local enabled = false
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    btn.Parent = row

    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            box.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
            checkMark.Text = "✓"
            label.Text = "[ Disable ] " .. labelText:gsub("%[ Enable %] ", "")
            label.TextColor3 = Color3.fromRGB(220, 80, 255)
            if onEnable then onEnable() end
        else
            box.BackgroundColor3 = Color3.fromRGB(70, 65, 85)
            checkMark.Text = ""
            label.Text = "[ Enable ] " .. labelText:gsub("%[ Enable %] ", "")
            label.TextColor3 = Color3.fromRGB(200, 200, 200)
            if onDisable then onDisable() end
        end
    end)
end

-- ========== เพิ่ม Checkbox ==========
createCheckbox(main, "[ Enable ] Auto Farm Gold & Gold Block", 38,
    function()
        isRunning = true
        task.spawn(runAutoFarm)
    end,
    function()
        isRunning = false
    end
)

-- ========== ปุ่ม Minimize / Close ==========
local isMinimized = false
minBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    main.Size = isMinimized
        and UDim2.new(0, 340, 0, 36)
        or  UDim2.new(0, 340, 0, 110)
end)

closeBtn.MouseButton1Click:Connect(function()
    isRunning = false
    main.Visible = false
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
