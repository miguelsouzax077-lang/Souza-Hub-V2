-- SERVICES
local plr = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local run = game:GetService("RunService")

-- CHARACTER
local char = plr.Character or plr.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local humanoid = char:WaitForChild("Humanoid")

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "SouzaHubV4"

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0,250,0,330)
main.Position = UDim2.new(0.35,0,0.25,0)
main.BackgroundColor3 = Color3.fromRGB(15,15,15)
main.Active = true
main.Draggable = true

-- TITLE RGB
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,30)
title.BackgroundTransparency = 1
title.Text = "🔥 Souza Premium"
title.Font = Enum.Font.GothamBold
title.TextSize = 18

task.spawn(function()
	while true do
		for i = 0,1,0.01 do
			title.TextColor3 = Color3.fromHSV(i,1,1)
			task.wait()
		end
	end
end)

-- SCROLL
local scroll = Instance.new("ScrollingFrame", main)
scroll.Size = UDim2.new(1,0,1,-30)
scroll.Position = UDim2.new(0,0,0,30)
scroll.CanvasSize = UDim2.new(0,0,0,600)
scroll.BackgroundTransparency = 1
scroll.ScrollBarThickness = 4

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0,6)

-- BUTTON
local function createButton(text, func)
	local btn = Instance.new("TextButton", scroll)
	btn.Size = UDim2.new(1,-10,0,35)
	btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Text = text
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(func)
end

-- BOX
local function createBox(placeholder)
	local box = Instance.new("TextBox", scroll)
	box.Size = UDim2.new(1,-10,0,35)
	box.PlaceholderText = placeholder
	box.Text = ""
	box.BackgroundColor3 = Color3.fromRGB(25,25,25)
	box.TextColor3 = Color3.new(1,1,1)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	return box
end

-- INPUTS
local walkBox = createBox("WalkSpeed (ex: 50)")
local flyBox = createBox("FlySpeed (ex: 80)")

-- WALK SPEED
createButton("Set WalkSpeed", function()
	local val = tonumber(walkBox.Text)
	if val then humanoid.WalkSpeed = val end
end)

-- FLY
local flying = false
local bv, bg
local flySpeed = 50

createButton("Fly ON/OFF", function()
	flying = not flying
	
	if flying then
		humanoid:ChangeState(Enum.HumanoidStateType.Physics)

		bv = Instance.new("BodyVelocity")
		bv.MaxForce = Vector3.new(1e5,1e5,1e5)
		bv.Parent = hrp

		bg = Instance.new("BodyGyro")
		bg.MaxTorque = Vector3.new(1e5,1e5,1e5)
		bg.CFrame = hrp.CFrame
		bg.Parent = hrp

		local val = tonumber(flyBox.Text)
		if val then flySpeed = val end
	else
		if bv then bv:Destroy() end
		if bg then bg:Destroy() end
		humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
	end
end)

run.RenderStepped:Connect(function()
	if flying and bv and bg then
		local cam = workspace.CurrentCamera
		local dir = Vector3.zero

		if uis:IsKeyDown(Enum.KeyCode.W) then dir += cam.CFrame.LookVector end
		if uis:IsKeyDown(Enum.KeyCode.S) then dir -= cam.CFrame.LookVector end
		if uis:IsKeyDown(Enum.KeyCode.A) then dir -= cam.CFrame.RightVector end
		if uis:IsKeyDown(Enum.KeyCode.D) then dir += cam.CFrame.RightVector end

		if dir.Magnitude > 0 then
			bv.Velocity = dir.Unit * flySpeed
		else
			bv.Velocity = Vector3.zero
		end

		bg.CFrame = cam.CFrame
	end
end)

-- MAGNET ORIGINAL
local magnetOn = false
local radius = 50
local fixed = {}
local welds = {}

local overlapParams = OverlapParams.new()
overlapParams.FilterType = Enum.RaycastFilterType.Blacklist
overlapParams.FilterDescendantsInstances = {char}

createButton("Magnet ON/OFF", function()
	magnetOn = not magnetOn
	
	if not magnetOn then
		-- 🔥 só solta quando desliga
		for obj, weld in pairs(welds) do
			if weld then weld:Destroy() end
		end
		
		table.clear(fixed)
		table.clear(welds)
	end
end)

run.Heartbeat:Connect(function()
	if not magnetOn then return end
	
	local parts = workspace:GetPartBoundsInRadius(hrp.Position, radius, overlapParams)

	for _, obj in pairs(parts) do
		if obj.Name == "TranspBox" and not obj.Anchored and not fixed[obj] then
			
			local distance = (obj.Position - hrp.Position).Magnitude
			local direction = (hrp.Position - obj.Position).Unit
			
			obj.AssemblyLinearVelocity = direction * 45

			if distance < 4 then
				obj.AssemblyLinearVelocity = Vector3.zero
				
				local weld = Instance.new("WeldConstraint")
				weld.Part0 = hrp
				weld.Part1 = obj
				weld.Parent = obj
				
				fixed[obj] = true
				welds[obj] = weld
			end
		end
	end
end)

-- MINIMIZAR
local minimized = false

local minBtn = Instance.new("TextButton", main)
minBtn.Size = UDim2.new(0,30,0,30)
minBtn.Position = UDim2.new(1,-35,0,0)
minBtn.Text = "-"
minBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
minBtn.TextColor3 = Color3.new(1,1,1)

minBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	
	if minimized then
		scroll.Visible = false
		main.Size = UDim2.new(0,250,0,30)
		minBtn.Text = "+"
	else
		scroll.Visible = true
		main.Size = UDim2.new(0,250,0,330)
		minBtn.Text = "-"
	end
end)

-- ================= RGB BORDA =================
local stroke = Instance.new("UIStroke", main)
stroke.Thickness = 2

task.spawn(function()
	while true do
		for i = 0,1,0.01 do
			stroke.Color = Color3.fromHSV(i,1,1)
			task.wait()
		end
	end
end)

-- ================= NOCLIP =================
local noclip = false

createButton("Noclip ON/OFF", function()
	noclip = not noclip
end)

run.Stepped:Connect(function()
	if noclip then
		for _, v in pairs(char:GetDescendants()) do
			if v:IsA("BasePart") then
				v.CanCollide = false
			end
		end
	end
end)
