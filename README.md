# Roblox Summer Camp Scripts 

## Collection of pre-made scripts for use in Roblox Studio by ninjas at Code Ninjas summer camp 2022

---

### Jump Boost Script:

```lua
local jumpBoost = script.Parent

local function steppedOn(part)
  local parent = part.Parent
  if game.Players:GetPlayerFromCharacter(parent) then
    parent.Humanoid.JumpPower = 150
    wait(2)
    parent.Humanoid.JumpPower = 50
  end
end
  
  jumpBoost.Touched:connect(steppedOn)
```

> Parent: Any part
> 
> Trigger: When a player touches the parent part
> 
> Result: The player can jump higher for a short time
> 
> Customization: Change the value of JumpHeight on line 6

---

### Speed Boost Script:

```lua
local speedBoost = script.Parent

local function steppedOn(part)
  local parent = part.Parent
  if game.Players:GetPlayerFromCharacter(parent) then
    parent.Humanoid.WalkSpeed = 30
    wait(4)
    parent.Humanoid.WalkSpeed = 16
  end
end
  
  speedBoost.Touched:connect(steppedOn)
```

> Parent: Any part
>
> Trigger: When a player touches the parent part
>
> Result: The player's speed is increased
>
> Customization: Change the value of WalkSpeed on line 6

---

### Kill/Heal Script:

```lua
script.Parent.Touched:connect(function(hit)
  if hit and hit.Parent and hit.Parent:FindFirstChild("Humanoid") then
    hit.Parent.Humanoid.Health = 0
  end
end)
```

> Parent: Any part
>
> Trigger: When a player touches the parent part
>
> Result: The player's health is set to 0 and is killed
>
> Customization: Change the value of Health on line 3 to a number other than 0 to hurt or heal the player

---

### Checkpoint Script:

```lua
local spawn = script.Parent
spawn.Touched:connect(function(hit)
  if hit and hit.Parent and hit.Parent:FindFirstChild("Humanoid") then
    local player = game.Players:GetPlayerFromCharacter(hit.Parent)
    local checkpointData = game.ServerStorage:FindFirstChild("CheckpointData")
    if not checkpointData then
      checkpointData = Instance.new("Model", game.ServerStorage)
      checkpointData.Name = "checkpointData"
    end
    
    local checkpoint = checkpointData:FindFirstChild(tostring(player.userId))
    if not checkpoint then
      checkpoint = Instance.new("ObjectValue", checkpointData)
      checkpoint.Name = tostring(player.userId)
      
      player.CharacterAdded:connect(function(character)
        wait()
        local newUser = tostring(player.userId)
        local newData = game.ServerStorage.CheckpointData[newUser].Value.CFrame
        local newCheckpoint = newData + Vector3.new(0, 4, 0)
        character:WaitForChild("HumanoidRootPart").CFrame = newcheckpoint
      end)
    end
    
    checkpoint.Value = spawn
  end
end)
```

> Parent: Any part
>
> Trigger: When a player touches the parent part
>
> Result: The player will respawn on the parent part instead of the original SpawnLocation
>
> Customization: None

---

### Moving Platform Script:

```lua
local axis = "x"
local distance = 50
local speed = 1

local factor = 0
local initialPosition = script.Parent.CFrame.Position
local moveDirection = Vector3.new(
  axis == "x" and distance or 0,
  axis == "y" and distance or 0,
  axis == "z" and distance or 0
)
local destinationPosition = initialPosition + moveDirection
local RunService = game:GetService("RunService")
function movePlatform (time, step)
  factor = 0.5 * math.sin(time * speed) + 0.5
  local finalPosition = initialPosition:Lerp(destinationPosition, factor)
  script.Parent.CFrame = CFrame.new(finalPosition)
end
RunService.Stepped:Connect(movePlatform)
```

> Parent: Any part
>
> Trigger: None, the script constantly runs
>
> Result: The parent part moves back and forth between two positions
>
> Customization: Set axis on line 1 to "x", "y", or "z" to change the direction
>                Change the value of distance on line 2 to set how far it moves
>                Change the value of speed to alter how fast or slow it moves

---
