local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Rayfield Example Window",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "Rayfield Interface Suite",
   LoadingSubtitle = "by Sirius",
   ShowText = "Rayfield", -- for mobile users to unhide rayfield, change if you'd like
   Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   ToggleUIKeybind = "K", -- The keybind to toggle the UI visibility (string like "K" or Enum.KeyCode)

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local PlayerTab = Window:CreateTab("Player🙎‍", nil) -- Title, Image

local Toggle = PlayerTab:CreateToggle({
   Name = "Toggle Speed",
   CurrentValue = false,
   Flag = "ToggleSpeed", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
end,
})

local Slider = PlayerTab:CreateSlider({
   Name = "Speed",
   Range = {0, 600},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "SliderSpeed", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		game.Player.LocalPlayer.Character.Humanoid.WalkSpeed = Value
   end,
})

local Toggle = PlayerTab:CreateToggle({
   Name = "Toggle Jumppower",
   CurrentValue = false,
   Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
   end,
})

local Slider = PlayerTab:CreateSlider({
   Name = "Toggle Jumppower",
   Range = {0, 600},
   Increment = 1,
   Suffix = "Jumppower",
   CurrentValue = 50,
   Flag = "SliderJump", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		game.Player.LocalPlayer.Character.Humanoid.JumpPower = Value
   end,
})

local VisualTab = Window:CreateTab("Visual👀", nil) -- Title, Image

local Toggle = VisualTab:CreateToggle({
   Name = "Toggle ESP",
   CurrentValue = false,
   Flag = "ToggleESP", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		local Players = game:GetService("Players")

-- Configuration
local HIGHLIGHT_COLOR = Color3.fromRGB(255, 0, 0) -- Bright Red
local FILL_TRANSPARENCY = 0.5 -- 0 is solid, 1 is invisible
local OUTLINE_TRANSPARENCY = 0 -- 0 is solid, 1 is invisible

local function addHighlightToCharacter(character)
	-- Prevent adding multiple highlights to the same character
	if character:FindFirstChild("WallhackEffect") then 
		return 
	end

	-- Create the Highlight instance
	local highlight = Instance.new("Highlight")
	highlight.Name = "WallhackEffect"
	highlight.Adornee = character
	
	-- Visual Properties
	highlight.FillColor = HIGHLIGHT_COLOR
	highlight.OutlineColor = Color3.fromRGB(255, 255, 255) -- White outline for contrast
	highlight.FillTransparency = FILL_TRANSPARENCY
	highlight.OutlineTransparency = OUTLINE_TRANSPARENCY
	
	-- CRITICAL: This makes it visible through walls
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	
	-- Parent it to the character model
	highlight.Parent = character
end

local function onPlayerAdded(player)
	-- Listen for when the player's character spawns or respawns
	player.CharacterAdded:Connect(addHighlightToCharacter)
	
	-- Handle case where character might already exist when script loads
	if player.Character then
		addHighlightToCharacter(player.Character)
	end
end

-- 1. Apply to all existing players (if script starts mid-game)
for _, player in ipairs(Players:GetPlayers()) do
	onPlayerAdded(player)
end

-- 2. Listen for new players joining
Players.PlayerAdded:Connect(onPlayerAdded)
   end,
})

local Toggle = VisualTab:CreateToggle({
   Name = "Toggle Health Bar ESP",
   CurrentValue = false,
   Flag = "ToggleHealthESP", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		local Players = game:GetService("Players")

-- Configuration
local HEALTH_DISPLAY_NAME = "ThroughWallsHealthDisplay"
local BILLBOARD_SIZE = UDim2.new(0, 100, 0, 20) -- Width: 100 pixels, Height: 20 pixels
local VERTICAL_OFFSET = 2 -- Studs above the player's head

local function addHealthDisplayToCharacter(character)
	-- Check if the character is valid and necessary parts exist
	local humanoid = character:WaitForChild("Humanoid", 10)
	local head = character:WaitForChild("Head", 10)
	
	if not humanoid or not head then return end
	
	-- Prevent adding duplicate health displays
	if character:FindFirstChild(HEALTH_DISPLAY_NAME) then
		return
	end

	-- 1. Create the BillboardGui
	local billboard = Instance.new("BillboardGui")
	billboard.Name = HEALTH_DISPLAY_NAME
	billboard.Adornee = head
	billboard.Size = BILLBOARD_SIZE
	billboard.StudsOffset = Vector3.new(0, VERTICAL_OFFSET, 0)
	
	-- CRITICAL: This makes the UI element visible through walls
	billboard.AlwaysOnTop = true 
	
	-- 2. Create the TextLabel inside the BillboardGui
	local textLabel = Instance.new("TextLabel")
	textLabel.Parent = billboard
	textLabel.Size = UDim2.new(1, 0, 1, 0) -- Fill the BillboardGui
	textLabel.BackgroundTransparency = 1 
	textLabel.TextStrokeTransparency = 0 -- Gives the text a dark outline
	textLabel.Font = Enum.Font.FredokaOne
	textLabel.TextScaled = true
	
	local function updateHealth()
		local hp = math.floor(humanoid.Health)
		
		-- Set the text (e.g., "HP: 100")
		textLabel.Text = "HP: " .. hp
		
		-- Change color based on health percentage for easy readability
		if hp > 70 then
			textLabel.TextColor3 = Color3.fromRGB(0, 200, 0) -- Green (High Health)
		elseif hp > 30 then
			textLabel.TextColor3 = Color3.fromRGB(255, 200, 0) -- Orange (Medium Health)
		else
			textLabel.TextColor3 = Color3.fromRGB(255, 0, 0) -- Red (Low Health)
		end
	end
	
	-- Connect the function to the HealthChanged event for real-time updates
	humanoid.HealthChanged:Connect(updateHealth)
	updateHealth() -- Initial set
	
	billboard.Parent = character
end

local function onPlayerAdded(player)
	-- Connect the handler to the CharacterAdded event to handle respawns
	player.CharacterAdded:Connect(addHealthDisplayToCharacter)
	
	-- If the character is already loaded, apply the display immediately
	if player.Character then
		addHealthDisplayToCharacter(player.Character)
	end
end

-- Connect the handler for new players
Players.PlayerAdded:Connect(onPlayerAdded)

-- Apply to any players already in the game when the script starts
for _, player in ipairs(Players:GetPlayers()) do
	onPlayerAdded(player)
end

local Toggle = VisualTab:CreateToggle({
   Name = "Toggle Tracers",
   CurrentValue = false,
   Flag = "ToggleTracers", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Configuration
local UPDATE_INTERVAL = 0.1 -- Update frequency in seconds
local TRACER_COLOR = Color3.fromRGB(255, 255, 0) -- Yellow tracer lines
local TRACER_WIDTH = 0.2 -- Thickness of the line in studs

-- Storage for the active beams, keyed by Player
local activeBeams = {}
local lastUpdate = 0

-- Function to create and link a tracer beam between two players
local function createTracer(targetPlayer)
	-- Skip if the tracer already exists or the target is invalid
	if activeBeams[targetPlayer] or not targetPlayer.Character then
		return
	end

	local localCharacter = LocalPlayer.Character
	local targetCharacter = targetPlayer.Character
	
	-- We need the Head parts for the attachments
	local localHead = localCharacter and localCharacter:FindFirstChild("Head")
	local targetHead = targetCharacter and targetCharacter:FindFirstChild("Head")
	
	if not localHead or not targetHead then
		return
	end
	
	-- Create the attachments (points where the beam connects)
	local attach0 = Instance.new("Attachment", localHead)
	attach0.Name = "LocalTracerAttach"
	
	local attach1 = Instance.new("Attachment", targetHead)
	attach1.Name = "TargetTracerAttach"
	
	-- Create the Beam
	local beam = Instance.new("Beam", localHead)
	beam.Name = "TracerLine"
	
	-- Link attachments to the beam
	beam.Attachment0 = attach0
	beam.Attachment1 = attach1
	
	-- Visual Configuration
	beam.Color = TRACER_COLOR
	beam.Width0 = TRACER_WIDTH
	beam.Width1 = TRACER_WIDTH
	beam.LightEmission = 1 -- Make it bright
	beam.Segments = 1
	
	-- CRITICAL: Use DepthMode.AlwaysOnTop to make it visible through walls.
	beam.FaceCamera = true 
	beam.LightInfluence = 0
	
	activeBeams[targetPlayer] = {
		Beam = beam,
		Attachment0 = attach0,
		Attachment1 = attach1,
		Connection = targetPlayer.CharacterAdded:Connect(function(newCharacter)
			-- Re-link the tracer if the target player respawns
			local newTargetHead = newCharacter:WaitForChild("Head")
			if newTargetHead then
				attach1.Parent = newTargetHead
			end
		end)
	}
end

-- Function to clean up the tracer when a player leaves
local function removeTracer(targetPlayer)
	local tracerData = activeBeams[targetPlayer]
	if tracerData then
		tracerData.Beam:Destroy()
		tracerData.Attachment0:Destroy()
		tracerData.Attachment1:Destroy()
		tracerData.Connection:Disconnect()
		activeBeams[targetPlayer] = nil
	end
end

-- Main update loop for beam positions
-- Heartbeat runs after physics, which is good for frequent updates
RunService.Heartbeat:Connect(function(deltaTime)
	local currentTime = tick()
	
	-- Throttle the update to respect the requested 0.1 second interval
	if currentTime - lastUpdate < UPDATE_INTERVAL then
		return
	end
	lastUpdate = currentTime

	local localCharacter = LocalPlayer.Character
	if not localCharacter then return end
	
	-- Ensure the local player's attachments are correctly parented (e.g., after respawn)
	local localHead = localCharacter:FindFirstChild("Head")
	if not localHead then return end

	for _, targetPlayer in ipairs(Players:GetPlayers()) do
		if targetPlayer ~= LocalPlayer and targetPlayer.Character then
			-- Check if the target player is still alive and has a character/head
			local targetHead = targetPlayer.Character:FindFirstChild("Head")
			
			if targetHead then
				-- If the tracer doesn't exist, create it
				if not activeBeams[targetPlayer] then
					createTracer(targetPlayer)
				else
					-- If the tracer exists, make sure attachments are correctly parented in case of a quick respawn
					local tracerData = activeBeams[targetPlayer]
					if tracerData.Attachment0.Parent ~= localHead then
						tracerData.Attachment0.Parent = localHead
					end
					if tracerData.Attachment1.Parent ~= targetHead then
						tracerData.Attachment1.Parent = targetHead
					end
				end
			else
				-- If the target player's head is gone (e.g., they just died), remove the beam
				removeTracer(targetPlayer)
			end
		end
	end
end)

-- Handle players leaving the game
Players.PlayerRemoving:Connect(removeTracer)

-- Initialize tracers for players already in the game when the script loads
for _, player in ipairs(Players:GetPlayers()) do
	if player ~= LocalPlayer then
		-- Wait for the character to spawn
		player.CharacterAdded:Wait()
		createTracer(player)
	end
end
   end,
})

local TransportationTab = Window:CreateTab("Transportation🏎", nil) -- Title, Image

local Toggle = TransportationTab:CreateToggle({
   Name = "Toggle Fly",
   CurrentValue = false,
   Flag = "ToggleFly", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
		--// Universal Fly (R6 + R15) — WASD + Q/E vertical — No toggle

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

repeat task.wait() until player.Character

local char = player.Character
local root = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso")
local hum = char:FindFirstChildOfClass("Humanoid")

if not root or not hum then return end

local ctrl = {f = 0, b = 0, l = 0, r = 0, up = 0, down = 0}
local lastctrl = {f = 0, b = 0, l = 0, r = 0, up = 0, down = 0}
local speed = 0
local maxspeed = 50

local bg = Instance.new("BodyGyro", root)
bg.P = 9e4
bg.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
bg.CFrame = root.CFrame

local bv = Instance.new("BodyVelocity", root)
bv.MaxForce = Vector3.new(9e9, 9e9, 9e9)
bv.Velocity = Vector3.new(0, 0, 0)

task.spawn(function()
	while true do
		task.wait()

		hum.PlatformStand = true

		local moving = (ctrl.f ~= 0 or ctrl.b ~= 0 or ctrl.l ~= 0 or ctrl.r ~= 0 or ctrl.up ~= 0 or ctrl.down ~= 0)

		if moving then
			speed += 0.6 + (speed / maxspeed)
			if speed > maxspeed then speed = maxspeed end
		elseif speed > 0 then
			speed -= 1
			if speed < 0 then speed = 0 end
		end

		local cam = workspace.CurrentCamera.CFrame

		-- Horizontal movement
		local moveDir =
			(cam.LookVector * (ctrl.f + ctrl.b)) +
			((cam * CFrame.new(ctrl.l + ctrl.r, 0, 0)).p - cam.p)

		-- Vertical movement (Q/E)
		local vertical = Vector3.new(0, (ctrl.up + ctrl.down), 0)

		if moving then
			bv.Velocity = (moveDir + vertical) * speed
			lastctrl = {f = ctrl.f, b = ctrl.b, l = ctrl.l, r = ctrl.r, up = ctrl.up, down = ctrl.down}
		elseif speed ~= 0 then
			local lastMove =
				(cam.LookVector * (lastctrl.f + lastctrl.b)) +
				((cam * CFrame.new(lastctrl.l + lastctrl.r, 0, 0)).p - cam.p)

			bv.Velocity = (lastMove + Vector3.new(0, lastctrl.up + lastctrl.down, 0)) * speed
		else
			bv.Velocity = Vector3.new(0, 0, 0)
		end

		bg.CFrame = cam
	end
end)

--// Controls
mouse.KeyDown:Connect(function(key)
	key = key:lower()

	if key == "w" then ctrl.f = 1
	elseif key == "s" then ctrl.b = -1
	elseif key == "a" then ctrl.l = -1
	elseif key == "d" then ctrl.r = 1
	elseif key == "q" then ctrl.up = 1       -- UP
	elseif key == "e" then ctrl.down = -1    -- DOWN
	end
end)

mouse.KeyUp:Connect(function(key)
	key = key:lower()

	if key == "w" then ctrl.f = 0
	elseif key == "s" then ctrl.b = 0
	elseif key == "a" then ctrl.l = 0
	elseif key == "d" then ctrl.r = 0
	elseif key == "q" then ctrl.up = 0
	elseif key == "e" then ctrl.down = 0
	end
end)

   end,
})

local Button = TransportationTab:CreateButton({
   Name = "Teleport Item",
   Callback = function()
		mouse = game.Players.LocalPlayer:GetMouse()
tool = Instance.new("Tool")
tool.RequiresHandle = false
tool.Name = "Teleport Tool"
tool.Activated:connect(function()
local pos = mouse.Hit+Vector3.new(0,2.5,0)
pos = CFrame.new(pos.X,pos.Y,pos.Z)
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = pos
end)
tool.Parent = game.Players.LocalPlayer.Backpack
   end,
})

local OtherTab = Window:CreateTab("Other Exploits👾", nil) -- Title, Image

local Button = OtherTab:CreateButton({
   Name = "Load Infinite Yield",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
   end,
})

local Button = OtherTab:CreateButton({
   Name = "Load XVC Hub",
   Callback = function()
		loadstring(game:HttpGet("https://pastebin.com/raw/Piw5bqGq"))()
   end,
})

local Button = Tab:CreateButton({
   Name = "Load Egor Script",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/loldo-g/RobloxEgorScript/refs/heads/main/script.lua"))()
   end,
})

local BloxFruitsTab = Window:CreateTab("Blox Fruits 🍎", nil)

local Button = Tab:CreateButton({
   Name = "Load Redz Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/realredz/BloxFruits/refs/heads/main/Source.lua"))()
   end,
})

local Button = BloxFruitsTab:CreateButton({
   Name = "Load Speed Hub X",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua"))()
   end,
})

local Button = BloxFruitsTab:CreateButton({
   Name = "Load Raito Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/Efe0626/RaitoHub/main/Script"))()
   end,
})

local Button = BloxFruitsTab:CreateButton({
   Name = "Load Banana Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/Nghia11n/Banana-Hub/main/bananahub.lua"))()
   end,
})

local Button = BloxFruitsTab:CreateButton({
   Name = "Load W Azure Hub",
   Callback = function()
		loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/3b2169cf53bc6104dabe8e19562e5cc2.lua"))()
   end,
})

local Button = BloxFruitsTab:CreateButton({
   Name = "Load HoHo Hub",
   Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ascn123/HOHO_H/main/Loading_UI'))()
   end,
})

local GaGTab = Window:CreateTab("Grow a Garden🥕", nil)

local Button = GaGTab:CreateButton({
   Name = "Load Dark Spawner",
   Callback = function()
		loadstring(game:HttpGet("https://pastefy.app/SC4qoDAW/raw"))()
   end,
})

local Button = GaGTab:CreateButton({
   Name = "Load GaG Script",
   Callback = function()
		loadstring(game:HttpGet("https://pastefy.app/3S5kIPpY/raw", true))()
   end,
})

local Button = GaGTab:CreateButton({
   Name = "Load BenGrowHub Freeze Trade",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmaadV99/ForceTrade/refs/heads/main/BenHub"))()
   end,
})

local Button = GaGTab:CreateButton({
   Name = "Load Mozil Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/MoziIOnTop/MoziIHub/refs/heads/main/GrowaGarden"))()
	end,
})

local 99NightsTab = Window:CreateTab("99 Nights🌌", nil)

local Button = 99NightsTab:CreateButton({
   Name = "Load VapeVoidWare",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/VapeVoidware/VW-Add/main/loader.lua", true))()
   end,
})

local Button = 99NightsTab:CreateButton({
   Name = "Load ToastyHubXD",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/nouralddin-abdullah/ToastyHub-XD/refs/heads/main/hub-main.lua"))()
   end,
})

local Button = 99NightsTab:CreateButton({
   Name = "Load SpeedHubX",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua", true))()
   end,
})

local Button = 99NightsTab:CreateButton({
   Name = "Load Syzen Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/Black69Weed-dev/99-Night-in-the-forest/main/99%20nights%20in%20the%20forest.lua"))()
   end,
})

local Button = 99NightsTab:CreateButton({
   Name = "Load Desire Hub V2",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/welomenchaina/Loader/refs/heads/main/DesireHubV2",true))()
   end,
})

local SaBTab = Window:CreateTab("SaB 🤡👶", nil) -- Title, Image

local Button = SaBTab:CreateButton({
   Name = "Load Lurk Hub",
   Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/egor2078f/lurkhackv4/refs/heads/main/main.lua', true))()
   end,
})

local Button = SaBTab:CreateButton({
   Name = "Load Ronix Hub (With Key)",
   Callback = function()
		loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/7d8a2a1a9a562a403b52532e58a14065.lua"))()
   end,
})

local Button = SaBTab:CreateButton({
   Name = "Load Speed Hub X",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua", true))()
   end,
})

local Button = SaBTab:CreateButton({
   Name = "Load Y Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/yue-os/script/refs/heads/main/Y-Hub"))()
   end,
})

local Button = SaBTab:CreateButton({
   Name = "Load EcstacyV2 Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/ecstacyV2/EcstacyV2/refs/heads/main2/EcstacyV2Real"))()
   end,
})

local FischTab = Window:CreateTab("Fisch🐟", nil) -- Title, Image

local Button = FischTab:CreateButton({
   Name = "Load Speed Hub X",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua", true))()
   end,
})

local Button = FischTab:CreateButton({
   Name = "Load UB Hub",
   Callback = function()
		loadstring(game:HttpGet("https://gitlab.com/r_soft/main/-/raw/main/LoadUB.lua"))()
   end,
})

local Button = FischTab:CreateButton({
   Name = "Load Bonk Hub",
   Callback = function()
		loadstring(game:HttpGet("https://bonkhub.online/loader.lua",true))()
   end,
})

local Button = FischTab:CreateButton({
   Name = "Load Radeon Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/RadeonScripts/RadeonHubMain/main/MainRobloxExploit"))()
   end,
})

local Button = FischTab:CreateButton({
   Name = "Load Space Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/IdiotHubz/Scripts/refs/heads/main/Fisch/SpaceHub.lua"))()
   end,
})

local PvsBTab = Window:CreateTab("PvsB👶🤡", nil) -- Title, Image

local Button = PvsBTab:CreateButton({
   Name = "Load Blue Hub X",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/Dev-BlueX/BlueX-Hub/refs/heads/main/Main.lua"))()
   end,
})

local Button = PvsBTab:CreateButton({
   Name = "Load Blossom Hub",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/BlossomHub123/Plants-Vs-Brainrots/refs/heads/main/.lua", true))()
   end,
})

local Button = PvsBTab:CreateButton({
   Name = "Load Speed Hub X",
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/AhmadV99/Speed-Hub-X/main/Speed%20Hub%20X.lua", true))()
   end,
})

local Button = PvsBTab:CreateButton({
   Name = "Load Space Hub",
   Callback = function()
		loadstring(game:HttpGet('https://raw.githubusercontent.com/ago106/SpaceHub/refs/heads/main/loader.lua'))()
   end,
})

local Button = PvsBTab:CreateButton({
   Name = "Load Walvy Hub,
   Callback = function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/Walvy666/Triplesix/main/loader.lua", true))()
   end,
})

