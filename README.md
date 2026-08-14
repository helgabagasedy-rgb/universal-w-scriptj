--================================================--
--                 SLEEPY HUB V9
--================================================--

local Players=game:GetService("Players")
local UIS=game:GetService("UserInputService")
local RS=game:GetService("RunService")

local plr=Players.LocalPlayer

local KEY="GAME-2026"
local LOAD_TIME=5
local MUSIC_ID="rbxassetid://76650356472656"

local char,hum,root
local infJump=false
local wall=false
local saved=nil

--================================================--
-- CHARACTER
--================================================--

local function setup(c)
	char=c
	hum=c:WaitForChild("Humanoid")
	root=c:WaitForChild("HumanoidRootPart")
	hum.UseJumpPower=true
end

if plr.Character then setup(plr.Character) end
plr.CharacterAdded:Connect(setup)

--================================================--
-- GUI
--================================================--

local gui=Instance.new("ScreenGui")
gui.Name="SleepyHub"
gui.ResetOnSpawn=false
gui.IgnoreGuiInset=true
gui.Parent=plr:WaitForChild("PlayerGui")

local function new(class,parent,p)
	local x=Instance.new(class,parent)
	for k,v in pairs(p or {}) do x[k]=v end
	return x
end

local function round(x,r)
	local c=Instance.new("UICorner",x)
	c.CornerRadius=UDim.new(0,r or 10)
end

local function btn(parent,text,pos,size)
	local b=new("TextButton",parent,{
		Text=text,
		Position=pos,
		Size=size,
		BackgroundColor3=Color3.fromRGB(28,33,44),
		TextColor3=Color3.new(1,1,1),
		TextSize=14,
		Font=Enum.Font.GothamBold,
		BorderSizePixel=0
	})
	round(b,10)
	return b
end

--================================================--
-- LOADING 5 DETIK
--================================================--

local load=new("Frame",gui,{
	Size=UDim2.fromScale(1,1),
	BackgroundColor3=Color3.fromRGB(8,10,15),
	BorderSizePixel=0
})

new("TextLabel",load,{
	Size=UDim2.new(1,0,0,60),
	Position=UDim2.new(0,0,.39,0),
	BackgroundTransparency=1,
	Text="SLEEPY HUB",
	TextColor3=Color3.new(1,1,1),
	TextSize=32,
	Font=Enum.Font.GothamBlack
})

new("TextLabel",load,{
	Size=UDim2.new(1,0,0,30),
	Position=UDim2.new(0,0,.48,0),
	BackgroundTransparency=1,
	Text="LOADING...",
	TextColor3=Color3.fromRGB(100,170,255),
	TextSize=13,
	Font=Enum.Font.GothamBold
})

local barBack=new("Frame",load,{
	Size=UDim2.new(0,260,0,7),
	Position=UDim2.new(.5,-130,.54,0),
	BackgroundColor3=Color3.fromRGB(30,35,45),
	BorderSizePixel=0
})
round(barBack,8)

local bar=new("Frame",barBack,{
	Size=UDim2.new(0,0,1,0),
	BackgroundColor3=Color3.fromRGB(60,150,255),
	BorderSizePixel=0
})
round(bar,8)

local percent=new("TextLabel",load,{
	Size=UDim2.new(1,0,0,25),
	Position=UDim2.new(0,0,.57,0),
	BackgroundTransparency=1,
	Text="0%",
	TextColor3=Color3.fromRGB(170,180,200),
	TextSize=12,
	Font=Enum.Font.GothamBold
})

for i=1,100 do
	bar.Size=UDim2.new(i/100,0,1,0)
	percent.Text=i.."%"
	task.wait(LOAD_TIME/100)
end

load:Destroy()

--================================================--
-- KEY SYSTEM
--================================================--

local keyFrame=new("Frame",gui,{
	Size=UDim2.new(0,330,0,235),
	Position=UDim2.new(.5,-165,.5,-117),
	BackgroundColor3=Color3.fromRGB(17,20,28),
	BorderSizePixel=0
})
round(keyFrame,18)

new("TextLabel",keyFrame,{
	Size=UDim2.new(1,0,0,50),
	Position=UDim2.new(0,0,0,12),
	BackgroundTransparency=1,
	Text="🔐  SLEEPY HUB",
	TextColor3=Color3.new(1,1,1),
	TextSize=22,
	Font=Enum.Font.GothamBlack
})

new("TextLabel",keyFrame,{
	Size=UDim2.new(1,0,0,20),
	Position=UDim2.new(0,0,0,58),
	BackgroundTransparency=1,
	Text="ENTER ACCESS KEY",
	TextColor3=Color3.fromRGB(140,150,165),
	TextSize=11,
	Font=Enum.Font.GothamBold
})

local keyBox=new("TextBox",keyFrame,{
	Size=UDim2.new(1,-40,0,42),
	Position=UDim2.new(0,20,0,85),
	BackgroundColor3=Color3.fromRGB(28,33,43),
	BorderSizePixel=0,
	PlaceholderText="ENTER KEY",
	Text="",
	TextColor3=Color3.new(1,1,1),
	TextSize=14,
	Font=Enum.Font.GothamBold,
	ClearTextOnFocus=false
})
round(keyBox,10)

local unlock=btn(
	keyFrame,
	"UNLOCK  →",
	UDim2.new(0,20,0,138),
	UDim2.new(1,-40,0,42)
)

unlock.BackgroundColor3=Color3.fromRGB(55,120,215)

local keyStatus=new("TextLabel",keyFrame,{
	Size=UDim2.new(1,-40,0,25),
	Position=UDim2.new(0,20,0,187),
	BackgroundTransparency=1,
	Text="",
	TextColor3=Color3.fromRGB(255,80,80),
	TextSize=12,
	Font=Enum.Font.GothamBold
})

--================================================--
-- MAIN GUI
--================================================--

local main=new("Frame",gui,{
	Size=UDim2.new(0,330,0,450),
	Position=UDim2.new(.5,-165,.5,-225),
	BackgroundColor3=Color3.fromRGB(14,17,24),
	BorderSizePixel=0,
	Visible=false
})
round(main,18)

local header=new("Frame",main,{
	Size=UDim2.new(1,0,0,65),
	BackgroundColor3=Color3.fromRGB(20,24,33),
	BorderSizePixel=0
})
round(header,18)

new("TextLabel",header,{
	Size=UDim2.new(1,-60,1,0),
	Position=UDim2.new(0,18,0,0),
	BackgroundTransparency=1,
	Text="SLEEPY HUB",
	TextColor3=Color3.new(1,1,1),
	TextSize=21,
	Font=Enum.Font.GothamBlack,
	TextXAlignment=Enum.TextXAlignment.Left
})

local minimize=btn(
	header,
	"—",
	UDim2.new(1,-50,0,13),
	UDim2.new(0,38,0,38)
)

local content=new("Frame",main,{
	Size=UDim2.new(1,-30,1,-80),
	Position=UDim2.new(0,15,0,75),
	BackgroundTransparency=1
})

--================================================--
-- FEATURES
--================================================--

local inf=btn(
	content,
	"⚡ Infinite Jump   [ OFF ]",
	UDim2.new(0,0,0,0),
	UDim2.new(1,0,0,45)
)

local ww=btn(
	content,
	"🧱 Wall Walk   [ OFF ]",
	UDim2.new(0,0,0,52),
	UDim2.new(1,0,0,45)
)

local jump=new("TextBox",content,{
	Size=UDim2.new(.48,0,0,42),
	Position=UDim2.new(0,0,0,105),
	BackgroundColor3=Color3.fromRGB(28,33,43),
	BorderSizePixel=0,
	Text="50",
	PlaceholderText="Jump Power",
	TextColor3=Color3.new(1,1,1),
	TextSize=14,
	Font=Enum.Font.GothamBold
})
round(jump,10)

local speed=new("TextBox",content,{
	Size=UDim2.new(.48,0,0,42),
	Position=UDim2.new(.52,0,0,105),
	BackgroundColor3=Color3.fromRGB(28,33,43),
	BorderSizePixel=0,
	Text="16",
	PlaceholderText="WalkSpeed",
	TextColor3=Color3.new(1,1,1),
	TextSize=14,
	Font=Enum.Font.GothamBold
})
round(speed,10)

local apply=btn(
	content,
	"✓  APPLY SETTINGS",
	UDim2.new(0,0,0,157),
	UDim2.new(1,0,0,43)
)

apply.BackgroundColor3=Color3.fromRGB(50,120,215)

local quick=btn(
	content,
	"📌  PRESS IF YOU NEED",
	UDim2.new(0,0,0,210),
	UDim2.new(1,0,0,43)
)

new("TextLabel",content,{
	Size=UDim2.new(1,0,0,25),
	Position=UDim2.new(0,0,0,260),
	BackgroundTransparency=1,
	Text="● SYSTEM READY",
	TextColor3=Color3.fromRGB(80,210,120),
	TextSize=11,
	Font=Enum.Font.GothamBold
})

--================================================--
-- QUICK PANEL
--================================================--

local qp=new("Frame",gui,{
	Size=UDim2.new(0,330,0,260),
	Position=UDim2.new(.5,-165,.5,-130),
	BackgroundColor3=Color3.fromRGB(14,17,24),
	BorderSizePixel=0,
	Visible=false
})
round(qp,18)

new("TextLabel",qp,{
	Size=UDim2.new(1,-60,0,55),
	Position=UDim2.new(0,18,0,5),
	BackgroundTransparency=1,
	Text="📌 QUICK TOOLS",
	TextColor3=Color3.new(1,1,1),
	TextSize=20,
	Font=Enum.Font.GothamBlack,
	TextXAlignment=Enum.TextXAlignment.Left
})

local back=btn(
	qp,
	"‹",
	UDim2.new(1,-50,0,12),
	UDim2.new(0,38,0,38)
)

local save=btn(
	qp,
	"📍  SAVE POSITION",
	UDim2.new(0,15,0,70),
	UDim2.new(1,-30,0,50)
)

local tp=btn(
	qp,
	"🌀  TP TO SAVED POSITION",
	UDim2.new(0,15,0,130),
	UDim2.new(1,-30,0,50)
)

local qs=new("TextLabel",qp,{
	Size=UDim2.new(1,-30,0,30),
	Position=UDim2.new(0,15,0,195),
	BackgroundTransparency=1,
	Text="NO POSITION SAVED",
	TextColor3=Color3.fromRGB(140,150,165),
	TextSize=11,
	Font=Enum.Font.GothamBold
})

--================================================--
-- MUSIC SETELAH GUI DIBUKA
--================================================--

local music=Instance.new("Sound")
music.Name="SleepyHubMusic"
music.SoundId=MUSIC_ID
music.Volume=.5
music.Looped=true
music.Parent=gui

--================================================--
-- INFINITE JUMP
--================================================--

UIS.JumpRequest:Connect(function()
	if infJump and hum then
		hum:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)

inf.MouseButton1Click:Connect(function()
	infJump=not infJump
	inf.Text="⚡ Infinite Jump   [ "..(infJump and "ON" or "OFF").." ]"
end)

--================================================--
-- WALL WALK
--================================================--

ww.MouseButton1Click:Connect(function()
	wall=not wall
	ww.Text="🧱 Wall Walk   [ "..(wall and "ON" or "OFF").." ]"
end)

RS.Heartbeat:Connect(function()

	if not wall or not hum or not root then return end

	local dir=hum.MoveDirection
	if dir.Magnitude==0 then return end

	local rp=RaycastParams.new()
	rp.FilterType=Enum.RaycastFilterType.Exclude
	rp.FilterDescendantsInstances={char}

	local hit=workspace:Raycast(
		root.Position,
		dir.Unit*3,
		rp
	)

	if hit then
		local v=root.AssemblyLinearVelocity
		root.AssemblyLinearVelocity=Vector3.new(v.X,35,v.Z)
	end

end)

--================================================--
-- APPLY
--================================================--

apply.MouseButton1Click:Connect(function()

	if not hum then return end

	local jp=tonumber(jump.Text)
	local ws=tonumber(speed.Text)

	if jp then
		hum.UseJumpPower=true
		hum.JumpPower=math.clamp(jp,0,500)
	end

	if ws then
		hum.WalkSpeed=math.clamp(ws,0,200)
	end

	apply.Text="✓ SETTINGS APPLIED"

	task.delay(1,function()
		apply.Text="✓  APPLY SETTINGS"
	end)

end)

--================================================--
-- SAVE POSITION
--================================================--

save.MouseButton1Click:Connect(function()

	if root then
		saved=root.CFrame
		qs.Text="✓ POSITION SAVED"
		qs.TextColor3=Color3.fromRGB(80,210,120)
	end

end)

--================================================--
-- TP POSITION
--================================================--

tp.MouseButton1Click:Connect(function()

	if not saved then
		qs.Text="✕ NO POSITION SAVED"
		qs.TextColor3=Color3.fromRGB(255,80,80)
		return
	end

	if root then
		root.CFrame=saved+Vector3.new(0,3,0)
		qs.Text="✓ TELEPORTED"
		qs.TextColor3=Color3.fromRGB(80,210,120)
	end

end)

--================================================--
-- QUICK
--================================================--

quick.MouseButton1Click:Connect(function()
	main.Visible=false
	qp.Visible=true
end)

back.MouseButton1Click:Connect(function()
	qp.Visible=false
	main.Visible=true
end)

--================================================--
-- MINIMIZE
--================================================--

local mini=btn(
	gui,
	"⚡",
	UDim2.new(1,-70,.5,-28),
	UDim2.new(0,55,0,55)
)

mini.TextSize=25
mini.Visible=false
round(mini,28)

minimize.MouseButton1Click:Connect(function()
	main.Visible=false
	qp.Visible=false
	mini.Visible=true
end)

mini.MouseButton1Click:Connect(function()
	main.Visible=true
	mini.Visible=false
end)

--================================================--
-- DRAG
--================================================--

local function drag(frame,handle)

	local dragging=false
	local start
	local position

	handle.InputBegan:Connect(function(i)

		if i.UserInputType==Enum.UserInputType.Touch
		or i.UserInputType==Enum.UserInputType.MouseButton1 then

			dragging=true
			start=i.Position
			position=frame.Position

		end

	end)

	handle.InputEnded:Connect(function(i)

		if i.UserInputType==Enum.UserInputType.Touch
		or i.UserInputType==Enum.UserInputType.MouseButton1 then

			dragging=false

		end

	end)

	UIS.InputChanged:Connect(function(i)

		if not dragging then return end

		if i.UserInputType==Enum.UserInputType.Touch
		or i.UserInputType==Enum.UserInputType.MouseMovement then

			local d=i.Position-start

			frame.Position=UDim2.new(
				position.X.Scale,
				position.X.Offset+d.X,
				position.Y.Scale,
				position.Y.Offset+d.Y
			)

		end

	end)

end

drag(main,header)

--================================================--
-- KEY CHECK
--================================================--

local function checkKey()

	if keyBox.Text==KEY then

		keyStatus.Text="✓ ACCESS GRANTED"
		keyStatus.TextColor3=Color3.fromRGB(80,220,120)

		task.wait(.4)

		keyFrame.Visible=false
		main.Visible=true

		-- MULAI MUSIK SETELAH GUI TERBUKA
		music:Play()

	else

		keyStatus.Text="✕ INVALID KEY"
		keyStatus.TextColor3=Color3.fromRGB(255,80,80)
		keyBox.Text=""

	end

end

unlock.MouseButton1Click:Connect(checkKey)

keyBox.FocusLost:Connect(function(enter)
	if enter then
		checkKey()
	end
end)

--================================================--
-- START
--================================================--

main.Visible=false
qp.Visible=false
mini.Visible=false
keyFrame.Visible=true

print("SLEEPY HUB V9 LOADED")
