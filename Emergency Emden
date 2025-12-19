--[[
everything here is by KTN93
]]

local OrionLib = loadstring(game:HttpGet(('https://gist.githubusercontent.com/therealnefex/e359e7d405917df77ff4fdb1374c68f2/raw/f4fb8b03520ef9bb44712ac49ca81ac0462f07b7/nova-hub-orion')))()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")

local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChildOfClass("Humanoid")
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local SpeedMultiplier = 1
local JumpMultiplier = 1
local NoClipEnabled = false
local FlyEnabled = false
local FlySpeed = 50
local Flying = false
local FlyConnection = nil
local NoclipLoop = nil
local NameTagEnabled = false
local NameTag = nil
local antiFallDamageEnabled = false
local infinityJumpEnabled = false
local changingColors = false
local isForceField = false
local godModeEnabled = false

local SpeedConnection
local function SetupSpeedHack()
    if SpeedConnection then
        SpeedConnection:Disconnect()
    end
    
    SpeedConnection = RunService.Heartbeat:Connect(function()
        if Character and HumanoidRootPart and Humanoid then
            local moveDirection = Humanoid.MoveDirection
            
            if moveDirection.Magnitude > 0 then
                local newVelocity = moveDirection * (16 * SpeedMultiplier)
                HumanoidRootPart.Velocity = Vector3.new(
                    newVelocity.X,
                    HumanoidRootPart.Velocity.Y,
                    newVelocity.Z
                )
            end
        end
    end)
end

local function ToggleFly()
    FlyEnabled = not FlyEnabled
    
    if FlyEnabled then
        Flying = true
        local BodyVelocity = Instance.new("BodyVelocity")
        BodyVelocity.Velocity = Vector3.new(0, 0, 0)
        BodyVelocity.MaxForce = Vector3.new(40000, 40000, 40000)
        BodyVelocity.Parent = HumanoidRootPart
        
        Humanoid.PlatformStand = true
        
        FlyConnection = RunService.Heartbeat:Connect(function()
            if not FlyEnabled or not Character then
                if FlyConnection then
                    FlyConnection:Disconnect()
                end
                return
            end
            
            local camera = workspace.CurrentCamera
            local moveDirection = Vector3.new(0, 0, 0)
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                moveDirection = moveDirection + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                moveDirection = moveDirection - Vector3.new(0, 1, 0)
            end
            
            if moveDirection.Magnitude > 0 then
                BodyVelocity.Velocity = moveDirection.Unit * FlySpeed
            else
                BodyVelocity.Velocity = Vector3.new(0, 0, 0)
            end
        end)
        
        OrionLib:MakeNotification({
            Name = "Fly Enabled",
            Content = "Fly mode has been activated!",
            Time = 3
        })
    else
        Flying = false
        Humanoid.PlatformStand = false
        
        if FlyConnection then
            FlyConnection:Disconnect()
            FlyConnection = nil
        end
        
        for _, obj in pairs(HumanoidRootPart:GetChildren()) do
            if obj:IsA("BodyVelocity") then
                obj:Destroy()
            end
        end
        
        OrionLib:MakeNotification({
            Name = "Fly Disabled",
            Content = "Fly mode has been deactivated!",
            Time = 3
        })
    end
end

local function ToggleNoClip()
    NoClipEnabled = not NoClipEnabled
    
    if NoClipEnabled then
        NoclipLoop = RunService.Stepped:Connect(function()
            if Character and NoClipEnabled then
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
        
        OrionLib:MakeNotification({
            Name = "NoClip Enabled",
            Content = "NoClip mode has been activated!",
            Time = 3
        })
    else
        if NoclipLoop then
            NoclipLoop:Disconnect()
            NoclipLoop = nil
        end

        if Character then
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
        
        OrionLib:MakeNotification({
            Name = "NoClip Disabled",
            Content = "NoClip mode has been deactivated!",
            Time = 3
        })
    end
end

local function ToggleForceField(state)
    isForceField = state
    
    if isForceField then
        local forceField = Instance.new("ForceField")
        forceField.Parent = Character
        forceField.Name = "KTN_ForceField"

        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Material = Enum.Material.ForceField
                part.Transparency = 0.3
                part.Color = Color3.fromRGB(0, 150, 255)
            end
        end
        
        OrionLib:MakeNotification({
            Name = "ForceField Enabled",
            Content = "ForceField has been activated!",
            Time = 3
        })
    else
        local forceField = Character:FindFirstChild("KTN_ForceField")
        if forceField then
            forceField:Destroy()
        end
        
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Material = Enum.Material.Plastic
                part.Transparency = 0
                part.Color = Color3.fromRGB(255, 255, 255)
            end
        end
        
        OrionLib:MakeNotification({
            Name = "ForceField Disabled",
            Content = "ForceField has been deactivated!",
            Time = 3
        })
    end
end

local function ToggleGodMode(state)
    godModeEnabled = state
    
    if godModeEnabled then
        if Character then
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanTouch = false
                    part.CanQuery = false
                end
            end
            
            if Humanoid then
                Humanoid.MaxHealth = math.huge
                Humanoid.Health = math.huge
            end
        end
        
        OrionLib:MakeNotification({
            Name = "God Mode Enabled",
            Content = "God Mode has been activated!",
            Time = 3
        })
    else
        if Character then
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanTouch = true
                    part.CanQuery = true
                end
            end
            
            if Humanoid then
                Humanoid.MaxHealth = 100
                Humanoid.Health = 100
            end
        end
        
        OrionLib:MakeNotification({
            Name = "God Mode Disabled",
            Content = "God Mode has been deactivated!",
            Time = 3
        })
    end
end

local function SetupInfiniteJump()
    UserInputService.JumpRequest:Connect(function()
        if infinityJumpEnabled and Character and Humanoid then
            Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end)
end

local function SetupAntiFallDamage()
    RunService.Heartbeat:Connect(function()
        if antiFallDamageEnabled and Character and HumanoidRootPart then
            local velocity = HumanoidRootPart.Velocity
            if velocity.Y < -50 then
                HumanoidRootPart.Velocity = Vector3.new(velocity.X, -5, velocity.Z)
            end
        end
    end)
end

local rainbowColors = {
    Color3.fromRGB(255, 0, 0),
    Color3.fromRGB(255, 127, 0),
    Color3.fromRGB(255, 255, 0),
    Color3.fromRGB(0, 255, 0),
    Color3.fromRGB(0, 0, 255),
    Color3.fromRGB(75, 0, 130),
    Color3.fromRGB(148, 0, 211)
}

local function ToggleRainbowCharacter(state)
    changingColors = state
    
    if changingColors then
        coroutine.wrap(function()
            local currentIndex = 1
            while changingColors and Character do
                local newColor = rainbowColors[currentIndex]
                
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        pcall(function()
                            part.Color = newColor
                        end)
                    end
                end
                
                currentIndex = (currentIndex % #rainbowColors) + 1
                task.wait(0.3)
            end
        end)()
        
        OrionLib:MakeNotification({
            Name = "Rainbow Mode Enabled",
            Content = "Rainbow Character has been activated!",
            Time = 3
        })
    else
        OrionLib:MakeNotification({
            Name = "Rainbow Mode Disabled",
            Content = "Rainbow Character has been deactivated!",
            Time = 3
        })
    end
end

local function toggleNameTag(state)
    NameTagEnabled = state

    if NameTagEnabled then
        if NameTag then 
            NameTag:Destroy()
            NameTag = nil
        end
        
        local head = Character:FindFirstChild("Head")
        if head then
            NameTag = Instance.new("BillboardGui")
            NameTag.Name = "KTN_NameTag"
            NameTag.Parent = head
            NameTag.Size = UDim2.new(0, 200, 0, 50)
            NameTag.StudsOffset = Vector3.new(0, 2.5, 0)
            NameTag.AlwaysOnTop = true
            NameTag.Enabled = true
            NameTag.Adornee = head

            local TextLabel = Instance.new("TextLabel")
            TextLabel.Parent = NameTag
            TextLabel.Size = UDim2.new(1, 0, 1, 0)
            TextLabel.BackgroundTransparency = 1
            TextLabel.Text = "KTN 93 CHEAT"
            TextLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
            TextLabel.TextScaled = true
            TextLabel.Font = Enum.Font.SourceSansBold
            TextLabel.ZIndex = 10
        end
    else
        if NameTag then
            NameTag:Destroy()
            NameTag = nil
        end
    end
end

local function valdTeleport(targetPosition)
    if Character and HumanoidRootPart then
        HumanoidRootPart.CFrame = CFrame.new(targetPosition)
        
        OrionLib:MakeNotification({
            Name = "Teleport Successful",
            Content = "Teleported to destination!",
            Time = 3
        })
    end
end

-- Création de la fenêtre
local Window = OrionLib:MakeWindow({
    Name = "By KTN93 | Best cheat",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "KTN_Config",
    IntroEnabled = true,
    IntroText = "By KTN93"
})

-- Tabs
local MainTab = Window:MakeTab({ Name = "Main", Icon = "rbxassetid://4483345998", PremiumOnly = false })
local PlayerTab = Window:MakeTab({ Name = "Player", Icon = "rbxassetid://4483345998", PremiumOnly = false })
local VisualTab = Window:MakeTab({ Name = "Visual", Icon = "rbxassetid://4483345998", PremiumOnly = false })
local TeleportTab = Window:MakeTab({ Name = "Teleport", Icon = "rbxassetid://4483345998", PremiumOnly = false })

-- Main Tab
MainTab:AddSection({ Name = "KTN Settings" })
MainTab:AddSlider({
    Name = "Speed Multiplier",
    Min = 1,
    Max = 10,
    Default = 1,
    Color = Color3.fromRGB(255, 0, 0),
    Increment = 0.1,
    ValueName = "x",
    Callback = function(Value) SpeedMultiplier = Value end
})
MainTab:AddSlider({
    Name = "Jump Multiplier",
    Min = 1,
    Max = 5,
    Default = 1,
    Color = Color3.fromRGB(0, 255, 0),
    Increment = 0.1,
    ValueName = "x",
    Callback = function(Value) JumpMultiplier = Value end
})
MainTab:AddButton({ Name = "Toggle Fly (X)", Callback = ToggleFly })
MainTab:AddButton({ Name = "Toggle NoClip (N)", Callback = ToggleNoClip })

-- Player Tab
PlayerTab:AddSection({ Name = "Player Modifications" })
PlayerTab:AddToggle({ Name = "God Mode", Default = false, Callback = ToggleGodMode })
PlayerTab:AddToggle({ Name = "ForceField", Default = false, Callback = ToggleForceField })
PlayerTab:AddToggle({
    Name = "Anti Fall Damage",
    Default = false,
    Callback = function(Value) antiFallDamageEnabled = Value end
})
PlayerTab:AddToggle({
    Name = "Infinity Jump",
    Default = false,
    Callback = function(Value) infinityJumpEnabled = Value end
})
PlayerTab:AddToggle({ Name = "Nametag", Default = false, Callback = toggleNameTag })

-- Visual Tab
VisualTab:AddSection({ Name = "Character Visuals" })
VisualTab:AddToggle({ Name = "Rainbow Character", Default = false, Callback = ToggleRainbowCharacter })

-- Teleport Tab
TeleportTab:AddSection({ Name = "Safe Locations" })
TeleportTab:AddButton({ Name = "Safe Zone", Callback = function() valdTeleport(Vector3.new(419.1429443359375, 95.88671112060547, -1813.3375244140625)) end })

TeleportTab:AddSection({ Name = "Shops & Services" })
local teleportLocations = {
    ["UG Corner Shop"] = Vector3.new(-301.88, 44.21, -76.12),
    ["Drone Store"] = Vector3.new(-339.94, 44.21, -517.48),
    ["General Store"] = Vector3.new(-102.43, 44.21, -757.93),
    ["Ship Spawner"] = Vector3.new(-581.32, 35.00, -1707.60),
    ["Car Shop"] = Vector3.new(85.44, 44.21, -2184.96),
    ["Tuner Shop"] = Vector3.new(280.90, 44.21, -2105.23),
    ["Hospital"] = Vector3.new(586.84, 44.21, -1762.20),
    ["Fire Department"] = Vector3.new(-1822.48, 44.21, 437.79),
    ["Prison"] = Vector3.new(-3290.83, 44.21, -193.20),
    ["Police Station"] = Vector3.new(-75.49, 71.66, 739.14)
}
for name, position in pairs(teleportLocations) do
    TeleportTab:AddButton({ Name = "Teleport " .. name, Callback = function() valdTeleport(position) end })
end

TeleportTab:AddSection({ Name = "Robberies" })
local robberyLocations = {
    ["Gas Station 1"] = Vector3.new(-1276.97, 44.06, 2134.93),
    ["Gas Station 2"] = Vector3.new(-641.21, 44.06, 2366.95),
    ["Gas Station 3"] = Vector3.new(510.29, 44.42, -2235.49),
    ["Gas Station 4"] = Vector3.new(-1377.2569580078125, 44.16484069824219, -1119.9266357421875),
    ["Bank"] = Vector3.new(-505.53, 44.21, -1368.42),
    ["Jewelry Store"] = Vector3.new(-455.72, 45.41, 345.04)
}
for name, position in pairs(robberyLocations) do
    TeleportTab:AddButton({ Name = "Teleport " .. name, Callback = function() valdTeleport(position) end })
end

-- Nouveau bouton pour entrer dans la banque (juste en dessous de "Teleport Bank")
TeleportTab:AddButton({
    Name = "Teleport into Bank",
    Callback = function()
        valdTeleport(Vector3.new(-452.70330810546875, 26.308591842651367, -1376.3243408203125))
    end
})

-- Dealer section inside Teleport Tab
TeleportTab:AddSection({ Name = "Dealer" })
local dealerLocations = {
    ["Dealer 1"] = Vector3.new(264.577880859375, 55.843746185302734, 207.3284149169922)
}
for name, position in pairs(dealerLocations) do
    TeleportTab:AddButton({ Name = "Teleport " .. name, Callback = function() valdTeleport(position) end })
end
-- Keybinds
local MenuToggleKey = Enum.KeyCode.Insert

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.KeyCode == MenuToggleKey then
        Window:Toggle()
    end

    if input.KeyCode == Enum.KeyCode.X then
        ToggleFly()
    end

    if input.KeyCode == Enum.KeyCode.N then
        ToggleNoClip()
    end
end)

-- Initial setup
SetupSpeedHack()
SetupInfiniteJump()
SetupAntiFallDamage()

OrionLib:Init()
