-- Function to create the sword
local function createSword()
    -- Give the player an unanchored sword with damage in their backpack
    local sword = Instance.new("Tool")
    sword.Name = "Sword"
    
    -- Add a LocalScript to the sword to handle damage
    local damageScript = Instance.new("LocalScript")
    damageScript.Parent = sword
    damageScript.Source = [[
        local tool = script.Parent
        local handle = tool:WaitForChild("Handle")
        local debounce = false
        local damageAmount = 10 -- Adjust damage as needed

        handle.Touched:Connect(function(hit)
            if not debounce and hit.Parent:FindFirstChild("Human) 
