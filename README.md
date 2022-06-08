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

## Day 5 - Treasure Hunt Game

### Part 1 - HidingSpots Script:

```lua
local Locations = {} -- variable named Locations set to empty list
local Model = script.Parent -- variable named Model set to whatever the script is in

for i, P in pairs(Model:children()) do -- for every object in model
	if( P:IsA('BasePart') ) then -- if its a part of any kind, do the following code
		P.Anchored = true -- anchor the part so it wont be affected by physics
		P.Transparency = 1 -- make the part invisible
		P.CanCollide = false -- disable the part colliding with other parts
		table.insert(Locations, P) -- add the part to the Locations list
	end
end

if #Locations <= 1 then -- if the amount of things in the list is less than or equal to 1
	return error("You need more than one Part in the model!") -- don't continue, we need atleast 2 spots
end

local Orb = Instance.new('Part', workspace) -- create a part in the workspace
Orb.Anchored = true -- anchor it so it doesnt fall down
Orb.Material = 'Neon' -- make it glow
Orb.Shape = 'Ball' -- make it a sphere
Orb.Size = Vector3.new(3,3,3) -- make it 3x3x3
Orb.CanCollide = false -- disable part colliding with other parts

local Light = Instance.new('PointLight', Orb)

local Spot -- make an empty variable called Spot

-- chooseSpot will choose the new random spot to hide the Orb
function chooseSpot() -- define a function called chooseSpot
	local chosenSpot = Spot -- set variable called chosenSpot to the current Spot
	while chosenSpot == Spot do -- while the chosenSpot is the current Spot, do the next line
		chosenSpot = Locations[math.random(1, #Locations)] -- choose a random spot from the list
	end
	return chosenSpot -- return the chosen spot
end

-- spawnOrb will move the orb somewhere else
function spawnOrb() -- define a function called spawnOrb
	Spot = chooseSpot() -- set Spot equal to the value of running chooseSpot function
	Orb.CFrame = Spot.CFrame -- move the orb to the new spot
	Orb.Transparency = 0.1 -- make the orb visible
end

-- fadeOrbOut will make the orb slowly disappear
function fadeOrbOut() -- define a new function called fadeOrbOut
	for i=.1, 1, .025 do -- from the number .1 to 1 in incrememts of 0.025, do the following
		Orb.Transparency = i -- set the orb's transparency to the number
		wait() -- wait 1/30th of a second
	end
	Orb.Transparency = 1 -- make the orb invisible
end

local Debounce = false -- define a variable called debounce to false

Orb.Touched:connect(function() -- when the Orb is touched, run this code
	if(Debounce)then -- if the Debounce variable is true
		return -- stop immediately, don't do anything
	end
	Debounce = true -- set debounce to true
	fadeOrbOut() -- Call the fadeOrbOut function
	wait(2) -- wait 2 seconds after it fades out
	spawnOrb() -- move it somewhere else
	Debounce = false -- set debounce to false
end)

spawnOrb() -- run the spawnOrb function
```

> The spheres that they place should have disappeared except for one. They are looking for the glowing sphere. When it is touched, it will hide somewhere else.

> REMEMBER: They can add more hiding spots even after making the script, just add more spheres to the folder!

---

## Part 2 - Hazard/Damage Script:

```lua
local poisonTimers = {}

local function findPlayerFromCharacter(char)
	for i,v in pairs(game.Players:players())do
		if(v.Character == char)then return v;end;
	end
end

script.Parent.Touched:Connect(function(part)
	local h = part.Parent:findFirstChild('Humanoid');
	if(h)then
		local player = findPlayerFromCharacter(h.Parent);
		if(player)then
			if(poisonTimers[player])then
				poisonTimers[player] = os.time() + 5
			else
				poisonTimers[player] = os.time() + 5
				while(true)do
					local t = poisonTimers[player]
					if(t)then
						if(os.time() < t)then
							h:TakeDamage(8)
						else poisonTimers[player] = nil end;
					else break end
					wait(1)
				end
			end
		end
	end
end)
```

> Add this script to the chosen hazard object, the same way that we have done it for the whole week.
