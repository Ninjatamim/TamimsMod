-- All-In-One Character GUI with Q Toggle and Custom Speeds
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hum = char:FindFirstChildOfClass("Humanoid")
local cam = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")

-- GUI
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "AllInOneGUI"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 260, 0, 400)
frame.Position = UDim2.new(0.05, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame)

-- Function to make buttons
local function makeButton(posY, text)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 240, 0, 35)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(70,70,70)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 18
    Instance.new("UICorner", btn)
    return btn
end

-- Buttons
local flyBtn = makeButton(10, "Toggle Fly")
local noclipBtn = makeButton(55, "Toggle Noclip")
local infJumpBtn = makeButton(100, "Infinite Jump")
local sitBtn = makeButton(145, "Sit")
local resetBtn = makeButton(190, "Reset Character")
local rejoinBtn = makeButton(235, "Rejoin Game")

-- Input Boxes for Speeds
local function makeTextBox(posY, placeholder)
    local tb = Instance.new("TextBox", frame)
    tb.Size = UDim2.new(0, 240, 0, 35)
    tb.Position = UDim2.new(0, 10, 0, posY)
    tb.PlaceholderText = placeholder
    tb.ClearTextOnFocus = true
    tb.Text = ""
    tb.BackgroundColor3 = Color3.fromRGB(70,70,70)
    tb.TextColor3 = Color3.fromRGB(255,255,255)
    tb.Font = Enum.Font.SourceSansBold
    tb.TextSize = 18
    Instance.new("UICorner", tb)
    return tb
end

local flySpeedInput = makeTextBox(280, "Fly Speed (default 60)")
local walkSpeedInput = makeTextBox(325, "WalkSpeed (default 16)")
local jumpPowerInput = makeTextBox(370, "JumpPower (default 50)")

-- Hide GUI initially
frame.Visible = false

-- Toggle GUI with Q
UIS.InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.Q and not gameProcessed then
        frame.Visible = not frame.Visible
    end
end)

-- Fly
local flying = false
local flyVel
flyBtn.MouseButton1Click:Connect(function()
    flying = not flying
    local flySpeed = tonumber(flySpeedInput.Text) or 60
    if flying then
        hum.PlatformStand = true
        flyVel = Instance.new("BodyVelocity", char.PrimaryPart)
        flyVel.MaxForce = Vector3.new(9e9,9e9,9e9)
        flyVel.Velocity = Vector3.new(0,0,0)
        game:GetService("RunService").RenderStepped:Connect(function()
            if flying then
                flyVel.Velocity = cam.CFrame.LookVector * flySpeed
            else
                if flyVel then flyVel:Destroy() end
                hum.PlatformStand = false
            end
        end)
    else
        if flyVel then flyVel:Destroy() end
        hum.PlatformStand = false
    end
end)

-- Noclip
local noclip = false
noclipBtn.MouseButton1Click:Connect(function()
    noclip = not noclip
    game:GetService("RunService").Stepped:Connect(function()
        if noclip then
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end)

-- Infinite Jump
infJumpBtn.MouseButton1Click:Connect(function()
    local conn
    conn = game:GetService("UserInputService").JumpRequest:Connect(function()
        hum:ChangeState("Jumping")
    end)
end)

-- Sit / Reset / Rejoin
sitBtn.MouseButton1Click:Connect(function()
    hum.Sit = true
end)

resetBtn.MouseButton1Click:Connect(function()
    player.Character:BreakJoints()
end)

rejoinBtn.MouseButton1Click:Connect(function()
    game:GetService("TeleportService"):Teleport(game.PlaceId, player)
end)

-- WalkSpeed & JumpPower
walkSpeedInput.FocusLost:Connect(function()
    local val = tonumber(walkSpeedInput.Text)
    if val then hum.WalkSpeed = val end
end)

jumpPowerInput.FocusLost:Connect(function()
    local val = tonumber(jumpPowerInput.Text)
    if val then hum.JumpPower = val end
end)
