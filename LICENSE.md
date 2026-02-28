--========================================--
-- Darxz Zero Immortal ALL IN ONE
-- Single Script Version
--========================================--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local enabled = false

----------------------------------------------------
-- PROTECTION SYSTEM
----------------------------------------------------

local function protectCharacter(character)
	local hum = character:WaitForChild("Humanoid")

	local hb
	hb = RunService.Heartbeat:Connect(function()
		if not character.Parent then
			hb:Disconnect()
			return
		end

		if enabled then
			hum.MaxHealth = 100
			hum.Health = 0

			hum:SetStateEnabled(Enum.HumanoidStateType.Dead,false)
			hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll,false)
			hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false)
			hum:SetStateEnabled(Enum.HumanoidStateType.Physics,false)

			if hum:GetState() == Enum.HumanoidStateType.Dead then
				hum:ChangeState(Enum.HumanoidStateType.Running)
			end

			character.BreakJoints = function() end
		end
	end)
end

----------------------------------------------------
-- EXPLOSION BLOCK
----------------------------------------------------

workspace.DescendantAdded:Connect(function(obj)
	if enabled and obj:IsA("Explosion") then
		obj.BlastPressure = 0
		obj.BlastRadius = 0
	end
end)

----------------------------------------------------
-- PLAYER JOIN
----------------------------------------------------

Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function(char)
		protectCharacter(char)
	end)
end)

for _,plr in pairs(Players:GetPlayers()) do
	if plr.Character then
		protectCharacter(plr.Character)
	end
end

----------------------------------------------------
-- GUI CREATOR (SERVER SIDE CLONE TO PLAYER)
----------------------------------------------------

local function createGui(player)

	local gui = Instance.new("ScreenGui")
	gui.Name = "DarxzGodMode-By ShizkGG_4HCKR"
	gui.ResetOnSpawn = false
	gui.Parent = player:WaitForChild("PlayerGui")

	local frame = Instance.new("Frame", gui)
	frame.Size = UDim2.new(0.35,0,0.3,0)
	frame.Position = UDim2.new(0.325,0,0.35,0)
	frame.BackgroundColor3 = Color3.new(1,1,1)
	frame.Active = true
	frame.Draggable = true

	Instance.new("UICorner", frame).CornerRadius = UDim.new(0,20)

	local stroke = Instance.new("UIStroke", frame)
	stroke.Thickness = 3
	stroke.Color = Color3.new(1,1,1)

	local title = Instance.new("TextLabel", frame)
	title.Size = UDim2.new(1,0,0.25,0)
	title.BackgroundTransparency = 1
	title.Text = "Darxz Zero Immortal"
	title.TextScaled = true
	title.Font = Enum.Font.GothamBlack
	title.TextColor3 = Color3.new(0,0,0)

	local button = Instance.new("TextButton", frame)
	button.Size = UDim2.new(0.8,0,0.3,0)
	button.Position = UDim2.new(0.1,0,0.5,0)
	button.Text = "GODMODE : OFF"
	button.TextScaled = true
	button.Font = Enum.Font.GothamBold
	button.BackgroundColor3 = Color3.new(0,0,0)
	button.TextColor3 = Color3.new(1,1,1)

	Instance.new("UICorner", button).CornerRadius = UDim.new(0,15)

	local close = Instance.new("TextButton", frame)
	close.Size = UDim2.new(0.15,0,0.2,0)
	close.Position = UDim2.new(0.82,0,0.02,0)
	close.Text = "X"
	close.TextScaled = true
	close.BackgroundColor3 = Color3.new(0,0,0)
	close.TextColor3 = Color3.new(1,1,1)

	Instance.new("UICorner", close).CornerRadius = UDim.new(1,0)

	local click = Instance.new("Sound", frame)
	click.SoundId = "rbxassetid://117166992328611"
	click.Volume = 1

	button.MouseButton1Click:Connect(function()
		click:Play()
		enabled = not enabled

		if enabled then
			button.Text = "IMMORTAL : ON"
			button.BackgroundColor3 = Color3.fromRGB(0,255,150)
		else
			button.Text = "GODMODE : OFF"
			button.BackgroundColor3 = Color3.new(0,0,0)
		end
	end)

	close.MouseButton1Click:Connect(function()
		click:Play()
		gui.Enabled = false
	end
