-- Variables
local frame = script.Parent -- The Frame
local invincibilityButton = frame:WaitForChild("InvincibilityButton") -- Button for invincibility
local teleportButton = frame:WaitForChild("TeleportButton") -- Button for teleportation
local spawnItemButton = frame:WaitForChild("SpawnItemButton") -- Button for spawning items
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Draggable functionality
local userInputService = game:GetService("UserInputService")
local gui = frame

local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

gui.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = gui.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

gui.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

userInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        update(input)
    end
end)

-- Function to toggle invincibility
local function toggleInvincibility()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        if humanoid:FindFirstChild("Invincible") then
            humanoid:FindFirstChild("Invincible"):Destroy()
            invincibilityButton.Text = "Enable Invincibility"
        else
            local invincible = Instance.new("BoolValue")
            invincible.Name = "Invincible"
            invincible.Parent = humanoid
            humanoid:GetPropertyChangedSignal("Health"):Connect(function()
                if invincible.Parent and humanoid.Health < humanoid.MaxHealth then
                    humanoid.Health = humanoid.MaxHealth
                end
            end)
            invincibilityButton.Text = "Disable Invincibility"
        end
    end
end

-- Function to teleport to a safe spot
local function teleportToSafeSpot()
    local safeSpot = Vector3.new(0, 50, 0) -- Adjust this position to your safe spot
    if character then
        character:SetPrimaryPartCFrame(CFrame.new(safeSpot))
    end
end

-- Function to spawn an item
local function spawnItem()
    local tool = Instance.new("Tool")
    tool.Name = "Cheat Tool"
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1, 1, 1)
    handle.Parent = tool
    tool.Parent = player.Backpack
end

-- Connect buttons to functions
invincibilityButton.MouseButton1Click:Connect(toggleInvincibility)
teleportButton.MouseButton1Click:Connect(teleportToSafeSpot)
spawnItemButton.MouseButton1Click:Connect(spawnItem)
