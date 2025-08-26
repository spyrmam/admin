-- This script has been converted to FE by iPxter

if game:GetService("RunService"):IsClient() then error("Script must be server-side in order to work; use h/ and not hl/") end local Player,Mouse,mouse,UserInputService,ContextActionService = owner do print("FE Compatibility code by Mokiros | Translated to FE by iPxter") script.Parent = Player.Character

--RemoteEvent for communicating
local Event = Instance.new("RemoteEvent")
Event.Name = "UserInput_Event"

--Fake event to make stuff like Mouse.KeyDown work
local function fakeEvent()
	local t = {_fakeEvent=true,Connect=function(self,f)self.Function=f end}
	t.connect = t.Connect
	return t
end

--Creating fake input objects with fake variables
local m = {Target=nil,Hit=CFrame.new(),KeyUp=fakeEvent(),KeyDown=fakeEvent(),Button1Up=fakeEvent(),Button1Down=fakeEvent()}
local UIS = {InputBegan=fakeEvent(),InputEnded=fakeEvent()}
local CAS = {Actions={},BindAction=function(self,name,fun,touch,...)
	CAS.Actions[name] = fun and {Name=name,Function=fun,Keys={...}} or nil
end}
--Merged 2 functions into one by checking amount of arguments
CAS.UnbindAction = CAS.BindAction

--This function will trigger the events that have been :Connect()'ed
local function te(self,ev,...)
	local t = m[ev]
	if t and t._fakeEvent and t.Function then
		t.Function(...)
	end
end
m.TrigEvent = te
UIS.TrigEvent = te

Event.OnServerEvent:Connect(function(plr,io)
    if plr~=Player then return end
	if io.isMouse then
		m.Target = io.Target
		m.Hit = io.Hit
	else
		local b = io.UserInputState == Enum.UserInputState.Begin
		if io.UserInputType == Enum.UserInputType.MouseButton1 then
			return m:TrigEvent(b and "Button1Down" or "Button1Up")
		end
		for _,t in pairs(CAS.Actions) do
			for _,k in pairs(t.Keys) do
				if k==io.KeyCode then
					t.Function(t.Name,io.UserInputState,io)
				end
			end
		end
		m:TrigEvent(b and "KeyDown" or "KeyUp",io.KeyCode.Name:lower())
		UIS:TrigEvent(b and "InputBegan" or "InputEnded",io,false)
    end
end)
Event.Parent = NLS([==[
local Player = game:GetService("Players").LocalPlayer
local Event = script:WaitForChild("UserInput_Event")

local UIS = game:GetService("UserInputService")
local input = function(io,a)
	if a then return end
	--Since InputObject is a client-side instance, we create and pass table instead
	Event:FireServer({KeyCode=io.KeyCode,UserInputType=io.UserInputType,UserInputState=io.UserInputState})
end
UIS.InputBegan:Connect(input)
UIS.InputEnded:Connect(input)

local Mouse = Player:GetMouse()
local h,t
--Give the server mouse data 30 times every second, but only if the values changed
--If player is not moving their mouse, client won't fire events
while wait(1/30) do
	if h~=Mouse.Hit or t~=Mouse.Target then
		h,t=Mouse.Hit,Mouse.Target
		Event:FireServer({isMouse=true,Target=t,Hit=h})
	end
end]==],Player.Character)
Mouse,mouse,UserInputService,ContextActionService = m,m,UIS,CAS
end

-- / rocky2u's command script
	-- / roblox : sethmilkman
	-- / v3rm : rocky2u

local ADMINS = {112073242}

local BANS = {21799524, 133122258, 103000855, 17278822, 149137060, 61967286, 21640881, 4879029}

-- / stuff

local VERSION = '1.5.7'
local PATCH, SPATCH = '01', '02'
local UPDATED = '9/7/2016'
local CHANGES = [[
	- removed custom commands, will add better way in future
]]

local CHANGELOG = ' UPDATED : ' .. UPDATED .. '\n VERSION : ' .. VERSION .. ' ' .. SPATCH .. '\n\n [ ' .. VERSION .. ' ' .. PATCH .. ' ] \n' .. CHANGES

local gCoreGui = game:GetService('CoreGui')
local gPlayers = game:GetService('Players')
local gLighting = game:GetService('Lighting')
local gWorkspace = game:GetService('Workspace')

local LP = gPlayers.owner
local MOUSE = LP:GetMouse()

local SERVICES = {}
local COMMANDS = {}
local STD = {}

SERVICES.EVENTS = {}

local C_PREFIX = ';'
local S_PREFIX = '\\'
local SPLIT = ' '

for i,v in pairs(gCoreGui:GetChildren()) do if v.Name == 'cmdbar_seth' or v.Name == 'notify_seth' then v:remove(); end; end

function UPDATE_CHAT(PLAYER) local C = PLAYER.Chatted:connect(function(M) if CHECK_ADMIN(PLAYER) then DEXECUTE(M, PLAYER) end end); table.insert(SERVICES.EVENTS, C); end

STD.TABLE = function(T, V) if not T then return false; end; for i,v in pairs(T) do if v == V then return true; end; end; return false; end
STD.ENDAT = function(S, V) local SF = S:find(V); if SF then return S:sub(0, SF - string.len(V)), true; else return S, false; end; end

function CHECK_ADMIN(PLAYER) for i,v in pairs(ADMINS) do if PLAYER.Name == v then return true; end; end; if PLAYER == LP then return true; end return false; end

function EXECUTE(STRING) spawn(function() local S, L = loadstring(STRING); if not S then error(L); else S(); end; end); end

function FCOMMAND(COMMAND) for i,v in pairs(COMMANDS) do if v.N:lower() == COMMAND:lower() or STD.TABLE(v.A, COMMAND:lower()) then return v; end; end; end

function GCOMMAND(M) local CMD, HS = STD.ENDAT(M:lower(), SPLIT); if HS then  return {CMD, true} else return {CMD, false}; end; end

function GPREFIX(STRING) if STRING:sub(1, string.len(C_PREFIX)) == C_PREFIX then return {'COMMAND', string.len(C_PREFIX) + 1}; elseif STRING:sub(1, string.len(S_PREFIX)) == S_PREFIX then return {'EXECUTE', string.len(S_PREFIX) + 1}; end return; end

function GARGS(STRING) local A = {}; local NA = nil; local HS = nil; local S = STRING; repeat NA, HS = STD.ENDAT(S:lower(), SPLIT); if NA ~= '' then table.insert(A, NA); S = S:sub(string.len(NA) + string.len(SPLIT) + 1); end; until not HS; return A; end

function ECOMMAND(STRING, SPEAKER) local SCMD, A, CMD; SCMD = GCOMMAND(STRING); CMD = FCOMMAND(SCMD[1]); if not CMD then return; end; A = STRING:sub(string.len(SCMD[1]) + string.len(SPLIT) + 1); local ARGS = GARGS(A); CA = GCAPARGS(A); pcall(function() CMD.F(ARGS, SPEAKER); end); end;

function DEXECUTE(STRING, SPEAKER) if not CHECK_ADMIN(SPEAKER) then return; end; STRING = STRING:gsub('/e ', ''); local GP = GPREFIX(STRING); if not GP then return; end; STRING = STRING:sub(GP[2]); if GP[1] == 'EXECUTE' then EXECUTE(STRING); elseif GP[1] == 'COMMAND' then ECOMMAND(STRING, SPEAKER); end; end

_G.cmd_seth = ECOMMAND

for i,v in pairs(gPlayers:GetPlayers()) do UPDATE_CHAT(v); end

gPlayers.PlayerAdded:connect(function(PLAYER) UPDATE_CHAT(PLAYER); end)

function ADD_COMMAND(N, D, A, F) table.insert(COMMANDS, {N = N, D = D, A = A, F = F}); end

function GET_PLAYER(NAME, SPEAKER)
	local NAME_TABLE = {}
	NAME = NAME:lower()
	if NAME == 'me' then
		table.insert(NAME_TABLE, SPEAKER.Name)
	elseif NAME == 'others' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v:IsA('Player') then if v.Name ~= SPEAKER.Name then table.insert(NAME_TABLE, v.Name); end; end; end
	elseif NAME == 'all' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v:IsA('Player') then table.insert(NAME_TABLE, v.Name); end; end
	elseif NAME == 'random' then
		table.insert(NAME_TABLE, gPlayers:GetPlayers()[math.random(1, #gPlayers:GetPlayers())].Name)
	elseif NAME == 'team' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v.TeamColor == LP.TeamColor then table.insert(NAME_TABLE, v.Name); end; end
	elseif NAME == 'nonadmins' then
		for i,v in pairs(gPlayers:GetPlayers()) do if not CHECK_ADMIN(v) then table.insert(NAME_TABLE, v.Name) end; end;
	elseif NAME == 'admins' then
		for i,v in pairs(gPlayers:GetPlayers()) do if CHECK_ADMIN(v) then table.insert(NAME_TABLE, v.Name) end; end;
	elseif NAME == 'nonfriends' then
		for i,v in pairs(gPlayers:GetPlayers()) do if not v:IsFriendsWith(SPEAKER.userId) then table.insert(NAME_TABLE, v.Name) end; end;
	elseif NAME == 'friends' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v ~= SPEAKER and v:IsFriendsWith(SPEAKER.userId) then table.insert(NAME_TABLE, v.Name) end; end;
	elseif NAME == 'nonguests' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v.userId > 0 then table.insert(NAME_TABLE, v.Name) end; end;
	elseif NAME == 'guests' then
		for i,v in pairs(gPlayers:GetPlayers()) do if v.userId < 0 then table.insert(NAME_TABLE, v.Name) end; end;
	else
		for i,v in pairs(gPlayers:GetPlayers()) do local L_NAME = v.Name:lower(); local F = L_NAME:find(NAME); if F == 1 then table.insert(NAME_TABLE, v.Name); end; end;
	end
	return NAME_TABLE
end

GCAPARGS = function(STRING) local A = {}; local NA = nil; local HS = nil; local S = STRING; repeat NA, HS = STD.ENDAT(S, SPLIT); if NA ~= '' then table.insert(A, NA); S = S:sub(string.len(NA) + string.len(SPLIT) + 1); end; until not HS; return A; end

function GLS(LOWER, START) local AA = ''; for i,v in pairs(CA) do if i > START then if AA ~= '' then AA = AA .. ' ' .. v; else AA = AA .. v; end; end; end; if not LOWER then return AA; else string.lower(AA); end; end

-- / stuff

local printStuff = '[ Rocky2u\'s CMDs ] : '

local DATA = game:GetObjects('rbxassetid://291592144')[1]
_G.seth_data = DATA

local CMDbar = DATA.guis.cmdbar_seth.CMDbar; CMDbar.Parent.Parent = gCoreGui
local notify_seth = DATA.guis.notify_seth:Clone(); notify_seth.Parent = gCoreGui
local being_looped = DATA.being_looped

wait()

local NOCLIP, JESUSFLY, SWIM = false, false, false

game:GetService('RunService').Stepped:connect(function()
	if NOCLIP then
		if FIND_CHILD(LP.Character, 'Humanoid') then LP.Character.Humanoid:ChangeState(11) end
	elseif JESUSFLY then
		if FIND_CHILD(LP.Character, 'Humanoid') then LP.Character.Humanoid:ChangeState(12) end
	elseif SWIM then
		if FIND_CHILD(LP.Character, 'Humanoid') then LP.Character.Humanoid:ChangeState(4) end
	end
end)

function FIND_CHILD(PATH, NAME) if PATH:FindFirstChild(NAME) then return true; end; return false; end

local canNotify = true

function NOTIFY(M, R, G, B)
	if notify_seth and canNotify then
		canNotify = false
		notify_seth.NOTIFY.NOTE.BAR.BackgroundColor3 = Color3.new(R, G, B)
		notify_seth.NOTIFY:TweenPosition(UDim2.new(0, 0, 0.7, 0), 'InOut', 'Quad', 0.5, false)
		notify_seth.NOTIFY.NOTE.Text = M
		wait(2.5)
		notify_seth.NOTIFY:TweenPosition(UDim2.new(0, -200, 0.7, 0), 'InOut', 'Quad', 0.5, false)
		wait(1)
		canNotify = true
	end
end

function GET_CONTEXT()
	local CONTEXT = 2
	local S, E = pcall(function() Instance.new('ImageButton'):SetVerb('Stop'); end)
	if S then CONTEXT = 4; end;
	S, E = pcall(function() return gCoreGui; end)
	if S then CONTEXT = 5; end;
	S, E = pcall(function() gPlayers.LocalPlayer:SetWebPersonalServerRank(1)  end)
	if E then if E == 'setWebPersonalServerRank should be called from server' then CONTEXT = 7 end end
	return CONTEXT
end

function LOAD_SETH()
	spawn(function()
		local load_seth = DATA.guis.loader_seth:Clone(); load_seth.Parent = gCoreGui
		load_seth.MAIN:TweenSizeAndPosition(UDim2.new(0, 300, 0, 200), UDim2.new(0.5, -150, 0.5, -100), 'Out', 'Quad', 0.5, false); wait(0.5)
		load_seth.MAIN.TEXT.Text = 'Rocky2u\'s\nCommand Script'
		repeat wait() load_seth.MAIN.TEXT.TextTransparency = load_seth.MAIN.TEXT.TextTransparency - 0.1 until load_seth.MAIN.TEXT.TextTransparency < 0; wait(1)
		if not gWorkspace.FilteringEnabled then load_seth.MAIN.FE.Text = ' Filtering is disabled'; elseif gWorkspace.FilteringEnabled then load_seth.MAIN.FE.Text = ' Filtering is ENABLED'; end; load_seth.MAIN.FE.TextTransparency = 0; wait(1)
		load_seth.MAIN.COMMANDS.Text = ' ' .. #COMMANDS .. ' commands'; load_seth.MAIN.COMMANDS.TextTransparency = 0; wait(1)
		load_seth.MAIN.IDENTITY.Text = ' Identity level is ' .. GET_CONTEXT(); load_seth.MAIN.IDENTITY.TextTransparency = 0; wait(1)
		load_seth.MAIN.WELCOME.Text = ' Welcome, ' .. LP.Name; load_seth.MAIN.WELCOME.TextTransparency = 0; wait(1)
		load_seth.MAIN.C.Text = ' Made by SethMilkman'; load_seth.MAIN.C.TextTransparency = 0; wait(5)
		for i,v in pairs(load_seth.MAIN:GetChildren()) do if v:IsA('TextLabel')and v.Name ~= 'TEXT' then v.TextTransparency = 1; end; end; wait()
		repeat wait() load_seth.MAIN.TEXT.TextTransparency = load_seth.MAIN.TEXT.TextTransparency + 0.1 until load_seth.MAIN.TEXT.TextTransparency == 1
		load_seth.MAIN:TweenSizeAndPosition(UDim2.new(0, 0, 0, 0), UDim2.new(0.5, 0, 0.5, 0), 'Out', 'Quad', 0.5)
		wait(1)
		load_seth.MAIN:remove()
	end)
end

function KICK(PLAYER)
	spawn(function()
		local function SKICK()
			if PLAYER.Character and FIND_CHILD(PLAYER.Character, 'HumanoidRootPart') then
				local SP = Instance.new('SkateboardPlatform', PLAYER.Character); SP.Position = Vector3.new(1000000, 1000000, 1000000); SP.Transparency = 1
				PLAYER.Character.HumanoidRootPart.CFrame = SP.CFrame
			end
		end
		spawn(function()
			repeat wait()
				if PLAYER ~= nil then
					SKICK()
				end
			until not FIND_CHILD(gPlayers, PLAYER.Name)
			if not FIND_CHILD(gPlayers, PLAYER.Name) then
				print (printStuff .. 'removed ' .. PLAYER.Name)
			end
		end)
	end)
end

function FIX_LIGHTING()
	gLighting.Ambient = Color3.new(0.5, 0.5, 0.5)
	gLighting.Brightness = 1
	gLighting.GlobalShadows = true
	gLighting.Outlines = false
	gLighting.TimeOfDay = 14
	gLighting.FogEnd = 100000
end

function MESSAGE(HEADER, MESSAGE, PLAYER)
	local BV = Instance.new('BoolValue', DATA.messages)
	BV.Name = PLAYER.Name .. '_MSG'
	for i = 1, 3 do
		local RT, RC = game:GetService('InsertService'):LoadAsset(98253592):GetChildren()[1], game:GetService('InsertService'):LoadAsset(284135286):GetChildren()[1]
		RT.TabletGui:remove()
		local R = RC.Script.Remover:Clone()
		local msgGUI = DATA.guis.message_seth:Clone()
		msgGUI.Name = 'TabletGui'
		msgGUI.Parent = RT
		msgGUI.MAIN.header.Text = HEADER
		msgGUI.MAIN.message.Text = MESSAGE
		R.Parent = msgGUI
		R.Disabled = false
		local tNAME = ''
		for i,v in pairs(PLAYER.Character:GetChildren()) do
			if v:IsA('Tool') then
				tNAME = v.Name
			end
		end
		PLAYER.Character.Humanoid:EquipTool(RT)
		PLAYER.Character.ROBLOXTablet:remove()
		if tNAME ~= '' then
			PLAYER.Backpack[tNAME].Parent = PLAYER.Character
		end
		wait(1.9)
	end
	BV:remove()
end

function HINT(MESSAGE)
	for i,v in pairs(gPlayers:GetPlayers()) do
		local BV = Instance.new('BoolValue', DATA.hints)
		BV.Name = v.Name .. '_MSG'
		for i = 1, 3 do
			local RT, RC = game:GetService('InsertService'):LoadAsset(98253592):GetChildren()[1], game:GetService('InsertService'):LoadAsset(284135286):GetChildren()[1]
			RT.TabletGui:remove()
			local R = RC.Script.Remover:Clone()
			local SG = Instance.new('ScreenGui', RT)
			local HINT = Instance.new('Hint', SG)
			SG.Name = 'TabletGui'
			HINT.Text = MESSAGE
			R.Parent = SG
			R.Disabled = false
			local tNAME = ''
			for i,v in pairs(v.Character:GetChildren()) do
				if v:IsA('Tool') then
					tNAME = v.Name
				end
			end
			v.Character.Humanoid:EquipTool(RT)
			v.Character.ROBLOXTablet:remove()
			if tNAME ~= '' then
				v.Backpack[tNAME].Parent = v.Character
			end
			wait(1.9)
		end
		BV:remove()
	end
end

function COLOR(PLAYER, BCOLOR)
	for i,v in pairs(PLAYER.Character:GetChildren()) do if v:IsA('Shirt') or v:IsA('Pants') then v:remove(); elseif v:IsA('ShirtGraphic') then v.Archivable = false; v.Graphic = ''; end; end;
	for i,v in pairs(PLAYER.Character.Head:GetChildren()) do if v:IsA('Decal') then v:remove(); end; end;
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			v.BrickColor = BrickColor.new(BCOLOR)
		elseif v:IsA('Hat') then
			v.Handle.BrickColor = BrickColor.new(BCOLOR)
			for a,b in pairs(v.Handle:GetChildren()) do
				if b:IsA('SpecialMesh') then
					b.TextureId = ''
				end
			end
		end
	end
end

function LAG(PLAYER)
	local POS = CFrame.new(math.random(-100000, 100000), math.random(-100000, 100000), math.random(-100000, 100000))
	spawn(function()
		repeat wait()
			if PLAYER and PLAYER.Character then
				PLAYER.CameraMode = 'LockFirstPerson'
				PLAYER.Character.HumanoidRootPart.CFrame = POS
				PLAYER.Character.Torso.Anchored = true
				Instance.new('ForceField', PLAYER.Character)
				Instance.new('Smoke', PLAYER.Character.Head)
			end
		until not FIND_CHILD(gPlayers, PLAYER.Name)
	end)
end

local FLYING = false

if LP.Character and FIND_CHILD(LP.Character, 'Humanoid') then
	LP.Character.Humanoid.Died:connect(function() FLYING = false; end)
end

function sFLY()
	repeat wait() until LP and LP.Character and FIND_CHILD(LP.Character, 'Torso') and FIND_CHILD(LP.Character, 'Humanoid')
	repeat wait() until MOUSE
	
	local T = LP.Character.Torso
	local CONTROL = {F = 0, B = 0, L = 0, R = 0}
	local lCONTROL = {F = 0, B = 0, L = 0, R = 0}
	local SPEED = 0
	
	local function FLY()
		FLYING = true
		local BG = Instance.new('BodyGyro', T)
		local BV = Instance.new('BodyVelocity', T)
		BG.P = 9e4
		BG.maxTorque = Vector3.new(9e9, 9e9, 9e9)
		BG.cframe = T.CFrame
		BV.velocity = Vector3.new(0, 0.1, 0)
		BV.maxForce = Vector3.new(9e9, 9e9, 9e9)
		spawn(function()
			repeat wait()
				LP.Character.Humanoid.PlatformStand = true
				if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 then
					SPEED = 50
				elseif not (CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0) and SPEED ~= 0 then
					SPEED = 0
				end
				if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 then
					BV.velocity = ((gWorkspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((gWorkspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B) * 0.2, 0).p) - gWorkspace.CurrentCamera.CoordinateFrame.p)) * SPEED
					lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
				elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and SPEED ~= 0 then
					BV.velocity = ((gWorkspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((gWorkspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B) * 0.2, 0).p) - gWorkspace.CurrentCamera.CoordinateFrame.p)) * SPEED
				else
					BV.velocity = Vector3.new(0, 0.1, 0)
				end
				BG.cframe = gWorkspace.CurrentCamera.CoordinateFrame
			until not FLYING
			CONTROL = {F = 0, B = 0, L = 0, R = 0}
			lCONTROL = {F = 0, B = 0, L = 0, R = 0}
			SPEED = 0
			BG:remove()
			BV:remove()
			LP.Character.Humanoid.PlatformStand = false
		end)
	end
	
	MOUSE.KeyDown:connect(function(KEY)
		if KEY:lower() == 'w' then
			CONTROL.F = 1
		elseif KEY:lower() == 's' then
			CONTROL.B = -1
		elseif KEY:lower() == 'a' then
			CONTROL.L = -1 
		elseif KEY:lower() == 'd' then 
			CONTROL.R = 1
		end
	end)
	
	MOUSE.KeyUp:connect(function(KEY)
		if KEY:lower() == 'w' then
			CONTROL.F = 0
		elseif KEY:lower() == 's' then
			CONTROL.B = 0
		elseif KEY:lower() == 'a' then
			CONTROL.L = 0
		elseif KEY:lower() == 'd' then
			CONTROL.R = 0
		end
	end)
	FLY()
end

function NOFLY()
	FLYING = false
	LP.Character.Humanoid.PlatformStand = false
end

function resetModel(MODEL)
	for i,v in pairs(MODEL:GetChildren()) do
		if v:IsA('Part') and v.Name == 'LA_INFECT' or v:IsA('Part') and v.Name == 'RA_INFECT' or v:IsA('Seat') and v.Name == 'FakeTorso' then
			v:remove()
		elseif v:IsA('CharacterMesh') or v:IsA('Shirt') or v:IsA('Pants') or v:IsA('Hat') then
			v:remove()
		elseif v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			v.Transparency = 0
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	for i,v in pairs(MODEL.Torso:GetChildren()) do
		if v:IsA('SpecialMesh') then
			v:remove()
		end
	end
	if FIND_CHILD(MODEL.Head, 'Mesh') then
		MODEL.Head.Mesh:remove()
	end
	if FIND_CHILD(MODEL.Torso, 'Neck') then MODEL.Torso.Neck.C0 = CFrame.new(0, 1, 0) * CFrame.Angles(math.rad(90), math.rad(180), 0); end;
	if FIND_CHILD(MODEL.Torso, 'Left Shoulder') then MODEL.Torso['Left Shoulder'].C0 = CFrame.new(-1, 0.5, 0) * CFrame.Angles(0, math.rad(-90), 0); end;
	if FIND_CHILD(MODEL.Torso, 'Right Shoulder') then MODEL.Torso['Right Shoulder'].C0 = CFrame.new(1, 0.5, 0) * CFrame.Angles(0, math.rad(90), 0); end;
	if FIND_CHILD(MODEL.Torso, 'Left Hip') then MODEL.Torso['Left Hip'].C0 = CFrame.new(-1, -1, 0) * CFrame.Angles(0, math.rad(-90), 0); end;
	if FIND_CHILD(MODEL.Torso, 'Right Hip') then MODEL.Torso['Right Hip'].C0 = CFrame.new(1, -1, 0) * CFrame.Angles(0, math.rad(90), 0); end;
end

function updateModel(MODEL, USERNAME)
	local AppModel = gPlayers:GetCharacterAppearanceAsync(gPlayers:GetUserIdFromNameAsync(USERNAME))
	MODEL.Name = USERNAME
	for i,v in pairs(AppModel:GetChildren()) do
		if v:IsA('SpecialMesh') or v:IsA('BlockMesh') or v:IsA('CylinderMesh') then
			v.Parent = MODEL.Head
		elseif v:IsA('Decal') then
			if FIND_CHILD(MODEL.Head, 'face') then
				MODEL.Head.face.Texture = v.Texture
			else
				local FACE = Instance.new('Decal', MODEL.Head)
				FACE.Texture = v.Texture
			end
		elseif v:IsA('BodyColors') or v:IsA('CharacterMesh') or v:IsA('Shirt') or v:IsA('Pants') or v:IsA('ShirtGraphic') then
			if FIND_CHILD(MODEL, 'Body Colors') then
				MODEL['Body Colors']:remove()
			end
			v.Parent = MODEL
		elseif v:IsA('Hat') then
			v.Parent = MODEL
			v.Handle.CFrame = MODEL.Head.CFrame * CFrame.new(0, MODEL.Head.Size.Y / 2, 0) * v.AttachmentPoint:inverse()
		end
	end
	if not FIND_CHILD(MODEL.Head, 'Mesh') then
		local SM = Instance.new('SpecialMesh', MODEL.Head)
		SM.MeshType = Enum.MeshType.Head
		SM.Scale = Vector3.new(1.25, 1.25, 1.25)
	end
end

function CREEPER(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Shirt') or v:IsA('Pants') then
			v:remove()
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Hat') then
			v:remove()
		end
	end
	PLAYER.Character.Torso.Neck.C0 = CFrame.new(0,1,0) * CFrame.Angles(math.rad(90),math.rad(180),0)
	PLAYER.Character.Torso['Right Shoulder'].C0 = CFrame.new(0,-1.5,-.5) * CFrame.Angles(0,math.rad(90),0)
	PLAYER.Character.Torso['Left Shoulder'].C0 = CFrame.new(0,-1.5,-.5) * CFrame.Angles(0,math.rad(-90),0)
	PLAYER.Character.Torso['Right Hip'].C0 = CFrame.new(0,-1,.5) * CFrame.Angles(0,math.rad(90),0)
	PLAYER.Character.Torso['Left Hip'].C0 = CFrame.new(0,-1,.5) * CFrame.Angles(0,math.rad(-90),0)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			v.BrickColor = BrickColor.new('Bright green')
		end
	end
end

function SHREK(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Shirt') or v:IsA('Pants') or v:IsA('Hat') or v:IsA('CharacterMesh') then
			v:remove()
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	for i,v in pairs(PLAYER.Character.Head:GetChildren()) do
		if v:IsA('Decal') or v:IsA('SpecialMesh') then
			v:remove()
		end
	end
	if FIND_CHILD(PLAYER.Character, 'Shirt Graphic') then
		PLAYER.Character['Shirt Graphic'].Archivable = false
		PLAYER.Character['Shirt Graphic'].Graphic = ''
	end
	local M = Instance.new('SpecialMesh', PLAYER.Character.Head)
	local S = Instance.new('Shirt', PLAYER.Character)
	local P = Instance.new('Pants', PLAYER.Character)
	M.MeshType = 'FileMesh'
	M.MeshId = 'http://www.roblox.com/asset/?id=19999257'
	M.Offset = Vector3.new(-0.1, 0.1, 0)
	M.TextureId = 'http://www.roblox.com/asset/?id=156397869'
	S.ShirtTemplate = 'rbxassetid://133078194'
	P.PantsTemplate = 'rbxassetid://133078204'
end

function DUCK(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Part') and v.Name ~= 'Torso' and v.Name ~= 'HumanoidRootPart' then
			v.Transparency = 1
		elseif v:IsA('Shirt') or v:IsA('Pants') or v:IsA('Hat') then
			v:remove()
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	local DUCK = Instance.new('SpecialMesh', PLAYER.Character.Torso)
	DUCK.MeshType = 'FileMesh'
	DUCK.MeshId = 'http://www.roblox.com/asset/?id=9419831'
	DUCK.TextureId = 'http://www.roblox.com/asset/?id=9419827'
	DUCK.Scale = Vector3.new(5, 5, 5)
	if FIND_CHILD(PLAYER.Character.Head, 'face') then
		PLAYER.Character.Head.face.Transparency = 1
	end
end

function DOG(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Shirt') or v:IsA('Pants') then
			v:remove()
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	PLAYER.Character.Torso.Transparency = 1
	PLAYER.Character.Torso.Neck.C0 = CFrame.new(0, -0.5, -2) * CFrame.Angles(math.rad(90), math.rad(180), 0)
	PLAYER.Character.Torso['Right Shoulder'].C0 = CFrame.new(0.5, -1.5, -1.5) * CFrame.Angles(0, math.rad(90), 0)
	PLAYER.Character.Torso['Left Shoulder'].C0 = CFrame.new(-0.5, -1.5, -1.5) * CFrame.Angles(0, math.rad(-90), 0)
	PLAYER.Character.Torso['Right Hip'].C0 = CFrame.new(1.5, -1, 1.5) * CFrame.Angles(0, math.rad(90), 0)
	PLAYER.Character.Torso['Left Hip'].C0 = CFrame.new(-1.5, -1, 1.5) * CFrame.Angles(0, math.rad(-90), 0)
	local FakeTorso = Instance.new('Seat', PLAYER.Character)
	local BF = Instance.new('BodyForce', FakeTorso)
	local W = Instance.new('Weld', PLAYER.Character.Torso)
	FakeTorso.Name = 'FakeTorso'
	FakeTorso.FormFactor = 'Symmetric'
	FakeTorso.TopSurface = 0
	FakeTorso.BottomSurface = 0
	FakeTorso.Size = Vector3.new(3,1,4)
	FakeTorso.BrickColor = BrickColor.new('Brown')
	FakeTorso.CFrame = PLAYER.Character.Torso.CFrame
	BF.Force = Vector3.new(0, FakeTorso:GetMass() * 196.25, 0)
	W.Part0 = PLAYER.Character.Torso
	W.Part1 = FakeTorso
	W.C0 = CFrame.new(0, -0.5, 0)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			v.BrickColor = BrickColor.new('Brown')
		end
	end
end

function AYYLMAO(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Shirt') or v:IsA('Pants') or v:IsA('Hat') then
			v:remove()
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		elseif v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			v.BrickColor = BrickColor.new('Fossil')
		end
	end
	game:GetObjects('rbxassetid://13837194')[1].Parent = PLAYER.Character
end

function DECALSPAM(INSTANCE, ID)
	for i,v in pairs(INSTANCE:GetChildren()) do
		if v:IsA('BasePart') then
			spawn(function()
				local FACES = {'Back', 'Bottom', 'Front', 'Left', 'Right', 'Top'}
				local CURRENT_FACE = 1
				for i = 1,6 do
					local DECAL = Instance.new('Decal', v)
					DECAL.Name = 'decal_seth'
					DECAL.Texture = 'rbxassetid://' .. ID - 1
					DECAL.Face = FACES[CURRENT_FACE]
					CURRENT_FACE = CURRENT_FACE + 1
				end
			end)
		end
		DECALSPAM(v, ID)
	end
end

function UNDECALSPAM(INSTANCE)
	for i,v in pairs(INSTANCE:GetChildren()) do
		if v:IsA('BasePart') then
			for a,b in pairs(v:GetChildren()) do
				if b:IsA('Decal') and b.Name == 'decal_seth' then
					b:remove()
				end
			end
		end
		UNDECALSPAM(v)
	end
end

function CREATE_DONG(PLAYER, DONG_COLOR)
	if FIND_CHILD(PLAYER.Character, 'DONG') then
		PLAYER.Character.DONG:remove()
	end
	local D = Instance.new('Model', PLAYER.Character)
	D.Name = 'DONG'
	
	local BG = Instance.new('BodyGyro', PLAYER.Character.Torso)
	local MAIN = Instance.new('Part', PLAYER.Character['DONG'])
	local M1 = Instance.new('CylinderMesh', MAIN)
	local W1 = Instance.new('Weld', PLAYER.Character.Head)
	local P1 = Instance.new('Part', PLAYER.Character['DONG'])
	local M2 = Instance.new('SpecialMesh', P1)
	local W2 = Instance.new('Weld', P1)
	local B1 = Instance.new('Part', PLAYER.Character['DONG'])
	local M3 = Instance.new('SpecialMesh', B1)
	local W3 = Instance.new('Weld', B1)
	local B2 = Instance.new('Part', PLAYER.Character['DONG'])
	local M4 = Instance.new('SpecialMesh', B2)
	local W4 = Instance.new('Weld', B2)
	
	MAIN.TopSurface = 0
	MAIN.BottomSurface = 0
	MAIN.Name = 'Main'
	MAIN.formFactor = 3
	MAIN.Size = Vector3.new(0.6, 2.5, 0.6)
	MAIN.BrickColor = BrickColor.new(DONG_COLOR)
	MAIN.Position = PLAYER.Character.Head.Position
	MAIN.CanCollide = false
	
	W1.Part0 = MAIN
	W1.Part1 = PLAYER.Character.Head
	W1.C0 = CFrame.new(0, 0.25, 2.1) * CFrame.Angles(math.rad(45), 0, 0)
	
	P1.Name = 'Mush'
	P1.BottomSurface = 0
	P1.TopSurface = 0
	P1.FormFactor = 3
	P1.Size = Vector3.new(0.6, 0.6, 0.6)
	P1.CFrame = CFrame.new(MAIN.Position)
	P1.BrickColor = BrickColor.new('Pink')
	P1.CanCollide = false
	
	M2.MeshType = 'Sphere'
	
	W2.Part0 = MAIN
	W2.Part1 = P1
	W2.C0 = CFrame.new(0, 1.3, 0)
	
	B1.Name = 'Left Ball'
	B1.BottomSurface = 0
	B1.TopSurface = 0
	B1.CanCollide = false
	B1.formFactor = 3
	B1.Size = Vector3.new(1, 1, 1)
	B1.CFrame = CFrame.new(PLAYER.Character['Left Leg'].Position)
	B1.BrickColor = BrickColor.new(DONG_COLOR)
	
	M3.Parent = B1
	M3.MeshType = 'Sphere'
	
	W3.Part0 = PLAYER.Character['Left Leg']
	W3.Part1 = B1
	W3.C0 = CFrame.new(0, 0.5, -0.5)
	
	B2.Name = 'Right Ball'
	B2.BottomSurface = 0
	B2.CanCollide = false
	B2.TopSurface = 0
	B2.formFactor = 3
	B2.Size = Vector3.new(1, 1, 1)
	B2.CFrame = CFrame.new(PLAYER.Character['Right Leg'].Position)
	B2.BrickColor = BrickColor.new(DONG_COLOR)
			
	M4.MeshType = 'Sphere'
	
	W4.Part0 = PLAYER.Character['Right Leg']
	W4.Part1 = B2
	W4.C0 = CFrame.new(0, 0.5, -0.5)
end

function SCALE(CHAR,SCAL)
	local SIZE_HAT_DATA = Instance.new('Folder', gLighting)
	SIZE_HAT_DATA.Name = 'HAT_DATA_' .. CHAR.Name
	
	for i,v in pairs(CHAR:GetChildren()) do
		if v:IsA('Hat') then
			v:Clone().Parent = SIZE_HAT_DATA
			wait()
			v:remove()
		end
	end
	
	local HEAD = CHAR.Head
	local TORSO = CHAR.Torso
	local LA = CHAR['Left Arm']
	local RA = CHAR['Right Arm']
	local LL = CHAR['Left Leg']
	local RL = CHAR['Right Leg']
	local HRP = CHAR.HumanoidRootPart
	
	HEAD.FormFactor = 3
	TORSO.FormFactor = 3
	LA.FormFactor = 3
	RA.FormFactor = 3
	LL.FormFactor = 3
	RL.FormFactor = 3
	HRP.FormFactor = 3
	
	HEAD.Size = Vector3.new(SCAL * 2, SCAL, SCAL)
	TORSO.Size = Vector3.new(SCAL * 2, SCAL * 2, SCAL)
	LA.Size = Vector3.new(SCAL, SCAL * 2, SCAL)
	RA.Size = Vector3.new(SCAL, SCAL * 2, SCAL)
	LL.Size = Vector3.new(SCAL, SCAL * 2, SCAL)
	RL.Size = Vector3.new(SCAL, SCAL * 2, SCAL)
	HRP.Size = Vector3.new(SCAL * 2, SCAL * 2, SCAL)
	
	local M1 = Instance.new('Motor6D', TORSO)
	local M2 = Instance.new('Motor6D', TORSO)
	local M3 = Instance.new('Motor6D', TORSO)
	local M4 = Instance.new('Motor6D', TORSO)
	local M5 = Instance.new('Motor6D', TORSO)
	local M6 = Instance.new('Motor6D', HRP)
	
	M1.Name = 'Neck'
	M1.Part0 = TORSO
	M1.Part1 = HEAD
	M1.C0 = CFrame.new(0, 1 * SCAL, 0) * CFrame.Angles(-1.6, 0, 3.1)
	M1.C1 = CFrame.new(0, -0.5 * SCAL, 0) * CFrame.Angles(-1.6, 0, 3.1)
	
	M2.Name = 'Left Shoulder'
	M2.Part0 = TORSO
	M2.Part1 = LA
	M2.C0 = CFrame.new(-1 * SCAL, 0.5 * SCAL, 0) * CFrame.Angles(0, -1.6, 0)
	M2.C1 = CFrame.new(0.5 * SCAL, 0.5 * SCAL, 0) * CFrame.Angles(0, -1.6, 0)
	
	M3.Name = 'Right Shoulder'
	M3.Part0 = TORSO
	M3.Part1 = RA
	M3.C0 = CFrame.new(1 * SCAL, 0.5 * SCAL, 0) * CFrame.Angles(0, 1.6, 0)
	M3.C1 = CFrame.new(-0.5 * SCAL, 0.5 * SCAL, 0) * CFrame.Angles(0, 1.6, 0)
	
	M4.Name  = 'Left Hip'
	M4.Part0 = TORSO
	M4.Part1 = LL
	M4.C0 = CFrame.new(-1 * SCAL, -1 * SCAL, 0) * CFrame.Angles(0, -1.6, 0)
	M4.C1 = CFrame.new(-0.5 * SCAL, 1 * SCAL, 0) * CFrame.Angles(0, -1.6, 0)
	
	M5.Name = 'Right Hip'
	M5.Part0 = TORSO
	M5.Part1 = RL
	M5.C0 = CFrame.new(1 * SCAL, -1 * SCAL, 0) * CFrame.Angles(0, 1.6, 0)
	M5.C1 = CFrame.new(0.5 * SCAL, 1 * SCAL, 0) * CFrame.Angles(0, 1.6, 0)
	
	M6.Name = 'RootJoint'
	M6.Part0 = HRP
	M6.Part1 = TORSO
	M6.C0 = CFrame.new(0, 0, 0) * CFrame.Angles(-1.6, 0, -3.1)
	M6.C1 = CFrame.new(0, 0, 0) * CFrame.Angles(-1.6, 0, -3.1)
	
	wait()
	
	for i,v in pairs(SIZE_HAT_DATA:GetChildren()) do
		if v:IsA('Hat') then
			v.Parent = CHAR
		end
	end
	
	SIZE_HAT_DATA:remove()
end

function CAPE(COLOR)
	if FIND_CHILD(LP.Character, 'Cape') then
		LP.Character.Cape:remove()
	end
	
	repeat
		wait()
	until LP and LP.Character and FIND_CHILD(LP.Character, 'Torso')
	
	local T = LP.Character.Torso
	
	local C = Instance.new('Part', T.Parent)
	C.Name = 'cape_seth'
	C.Anchored = false
	C.CanCollide = false
	C.TopSurface = 0
	C.BottomSurface = 0
	C.BrickColor = BrickColor.new(COLOR)
	C.Material = 'Neon'
	C.FormFactor = 'Custom'
	C.Size = Vector3.new(0.2, 0.2, 0.2)
	
	local M = Instance.new('BlockMesh', C)
	M.Scale = Vector3.new(9, 17.5, 0.5)
	
	local M1 = Instance.new('Motor', C)
	M1.Part0 = C
	M1.Part1 = T
	M1.MaxVelocity = 1
	M1.C0 = CFrame.new(0, 1.75, 0) * CFrame.Angles(0, math.rad(90), 0)
	M1.C1 = CFrame.new(0, 1, .45) * CFrame.Angles(0, math.rad(90), 0)
	
	local WAVE = false
	
	repeat wait(1 / 44)
		local ANG = 0.2
		local oldMag = T.Velocity.magnitude
		local MV = 0.1
		
		if WAVE then
			ANG = ANG + ((T.Velocity.magnitude / 10) * 0.05) + 1
			WAVE = false
		else
			WAVE = false
		end
		ANG = ANG + math.min(T.Velocity.magnitude / 30, 1)
		M1.MaxVelocity = math.min((T.Velocity.magnitude / 10), 0.04) + MV
		M1.DesiredAngle = -ANG
		if M1.CurrentAngle < -0.05 and M1.DesiredAngle > -.05 then
			M1.MaxVelocity = 0.04
		end
		repeat
			wait()
		until M1.CurrentAngle == M1.DesiredAngle or math.abs(T.Velocity.magnitude - oldMag)  >= (T.Velocity.magnitude / 10) + 1
		if T.Velocity.magnitude < 0.1 then
			wait(0.1)
		end
	until not C or C.Parent ~= T.Parent
end

function INFECT(PLAYER)
	for i,v in pairs(PLAYER.Character:GetChildren()) do
		if v:IsA('Hat') or v:IsA('Part') and v.Name == 'LA_INFECT' or v:IsA('Part') and v.Name == 'RA_INFECT' or v:IsA('Shirt') or v:IsA('Pants') then
			v:remove()
		elseif v:IsA('Part') and v.Name == 'Left Arm' or v:IsA('Part') and v.Name == 'Right Arm' then
			v.Transparency = 1
		elseif v:IsA('ShirtGraphic') then
			v.Archivable = false
			v.Graphic = ''
		end
	end
	
	local LZOMBIE_ARM = Instance.new('Part', PLAYER.Character)
	local LWELD = Instance.new('Weld', LZOMBIE_ARM)
	local RZOMBIE_ARM = Instance.new('Part', PLAYER.Character)
	local RWELD = Instance.new('Weld', RZOMBIE_ARM)
	
	LZOMBIE_ARM.Name = 'LA_INFECT'
	LZOMBIE_ARM.BrickColor = BrickColor.new('Medium green')
	LZOMBIE_ARM.Size = Vector3.new(1, 1, 2)
	LZOMBIE_ARM.TopSurface = 'Smooth'
	LZOMBIE_ARM.BottomSurface = 'Smooth'
	
	LWELD.Part0 = PLAYER.Character.Torso
	LWELD.Part1 = LZOMBIE_ARM
	LWELD.C0 = CFrame.new(-1.5, 0.5, -0.5)
	
	RZOMBIE_ARM.Name = 'RA_INFECT'
	RZOMBIE_ARM.BrickColor = BrickColor.new('Medium green')
	RZOMBIE_ARM.Size = Vector3.new(1, 1, 2)
	RZOMBIE_ARM.TopSurface = 'Smooth'
	RZOMBIE_ARM.BottomSurface = 'Smooth'
	
	RWELD.Part0 = PLAYER.Character.Torso
	RWELD.Part1 = RZOMBIE_ARM
	RWELD.C0 = CFrame.new(1.5, 0.5, -0.5)
	
	if FIND_CHILD(PLAYER.Character.Head, 'face') then
		PLAYER.Character.Head.face.Texture = 'rbxassetid://7074882'
	end
	
	for i,v in pairs (PLAYER.Character:GetChildren()) do
		if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
			if v.Name == 'Head' then
				v.BrickColor = BrickColor.new('Medium green')
			elseif v.Name == 'Torso' or v.Name == 'Left Leg' or v.Name == 'Right Leg' then
				v.BrickColor = BrickColor.new('Brown')
			end
		end
	end
end

function fWeld(zName, zParent, zPart0, zPart1, zCoco, A, B, C, D, E, F)
	local funcw = Instance.new('Weld'); funcw.Name = zName; funcw.Parent = zParent; funcw.Part0 = zPart0; funcw.Part1 = zPart1
	if (zCoco) then
		funcw.C0 = CFrame.new(A, B, C) * CFrame.fromEulerAnglesXYZ(D, E, F)
	else
		funcw.C1 = CFrame.new(A, B, C) * CFrame.fromEulerAnglesXYZ(D, E, F)
	end
	return funcw
end

function BANG(VICTIM)
	spawn(function()
		local P1 = gPlayers.LocalPlayer.Character.Torso
		local V1 = gPlayers[VICTIM].Character.Torso
		
		V1.Parent.Humanoid.PlatformStand = true
		
		P1['Left Shoulder']:remove(); local LA1 = Instance.new('Weld', P1); LA1.Part0 = P1; LA1.Part1 = P1.Parent['Left Arm']; LA1.C0 = CFrame.new(-1.5, 0, 0); LA1.Name = 'Left Shoulder'
		
		P1['Right Shoulder']:remove(); local RS1 = Instance.new('Weld', P1); RS1.Part0 = P1; RS1.Part1 = P1.Parent['Right Arm']; RS1.C0 = CFrame.new(1.5, 0, 0); RS1.Name = 'Right Shoulder'
		
		V1['Left Shoulder']:remove(); local LS2 = Instance.new('Weld', V1); LS2.Part0 = V1; LS2.Part1 = V1.Parent['Left Arm']; LS2.C0 = CFrame.new(-1.5, 0, 0); LS2.Name = 'Left Shoulder'
		
		V1['Right Shoulder']:remove(); local RS2 = Instance.new('Weld', V1); RS2.Part0 = V1; RS2.Part1 = V1.Parent['Right Arm']; RS2.C0 = CFrame.new(1.5, 0, 0); RS2.Name = 'Right Shoulder'
		
		V1['Left Hip']:remove(); local LH2 = Instance.new('Weld', V1); LH2.Part0 = V1; LH2.Part1 = V1.Parent['Left Leg']; LH2.C0 = CFrame.new(-0.5, -2, 0); LH2.Name = 'Left Hip'
		
		V1['Right Hip']:remove(); local RH2 = Instance.new('Weld', V1); RH2.Part0 = V1; RH2.Part1 = V1.Parent['Right Leg']; RH2.C0 = CFrame.new(0.5, -2, 0); RH2.Name = 'Right Hip'
		
		local D = Instance.new('Part', P1); D.TopSurface = 0; D.BottomSurface = 0; D.CanCollide = false; D.BrickColor = BrickColor.new('Pastel brown'); D.Shape = 'Ball'; D.Size = Vector3.new(1, 1, 1)
		
		local DM1 = Instance.new('SpecialMesh', D); DM1.MeshType = 'Sphere'; DM1.Scale = Vector3.new(0.4, 0.4, 0.4)
		
		fWeld('weld', P1, P1, D, true, -0.2, -1.3, -0.6, 0, 0, 0)
		
		local D2 = D:Clone(); D2.Parent = P1
		
		fWeld('weld', P1, P1, D2, true, 0.2, -1.3, -0.6, 0, 0, 0)
		
		local C = Instance.new('Part', P1); C.TopSurface = 0; C.BottomSurface = 0; C.CanCollide = false; C.BrickColor = BrickColor.new('Pastel brown'); C.formFactor = 'Custom'; C.Size = Vector3.new(0.4, 1.3, 0.4)
		
		fWeld('weld', P1, P1, C, true, 0, -1, -0.52 + (-C.Size.y / 2), math.rad(-80), 0, 0)
		
		local C2 = D:Clone(); C2.BrickColor = BrickColor.new('Pink'); C2.Mesh.Scale = Vector3.new(0.4, 0.62, 0.4); C2.Parent = P1
		
		fWeld('weld', C, C, C2, true, 0, 0 + (C.Size.y / 2), 0, math.rad(-10), 0, 0)
		
		local CM = Instance.new('CylinderMesh', C)
		
		local BL = Instance.new('Part', V1); BL.TopSurface = 0; BL.BottomSurface = 0; BL.CanCollide = false; BL.BrickColor = BrickColor.new('Pastel brown'); BL.Shape = 'Ball'; BL.Size = Vector3.new(1, 1, 1)
		
		local DM2 = Instance.new('SpecialMesh', BL); DM2.MeshType = 'Sphere'; DM2.Scale = Vector3.new(1.2, 1.2, 1.2)
		
		fWeld('weld', V1, V1, BL, true, -0.5, 0.5, -0.6, 0, 0, 0)
		
		local BR = Instance.new('Part', V1); BR.TopSurface = 0; BR.BottomSurface = 0; BR.CanCollide = false; BR.BrickColor = BrickColor.new('Pastel brown'); BR.Shape = 'Ball'; BR.Size = Vector3.new(1, 1, 1)
		
		local DM3 = Instance.new('SpecialMesh', BR); DM3.MeshType = 'Sphere'; DM3.Scale = Vector3.new(1.2, 1.2, 1.2)
		
		fWeld('weld', V1, V1, BR, true, 0.5, 0.5, -0.6, 0, 0, 0)
		
		local BLN = Instance.new('Part', V1); BLN.TopSurface = 0; BLN.BottomSurface = 0; BLN.CanCollide = false; BLN.BrickColor = BrickColor.new('Pink'); BLN.Shape = 'Ball'; BLN.Size = Vector3.new(1, 1, 1)
		
		local DM4 = Instance.new('SpecialMesh', BLN); DM4.MeshType = 'Sphere'; DM4.Scale = Vector3.new(0.2, 0.2, 0.2)
		
		fWeld('weld', V1, V1, BLN, true, -0.5, 0.5, -1.2, 0, 0, 0)
		
		local BRN = Instance.new('Part', V1); BRN.TopSurface = 0; BRN.BottomSurface = 0; BRN.CanCollide = false; BRN.BrickColor = BrickColor.new('Pink'); BRN.Shape = 'Ball'; BRN.Size = Vector3.new(1, 1, 1)
		
		local DM5 = Instance.new('SpecialMesh', BRN); DM5.MeshType = 'Sphere'; DM5.Scale = Vector3.new(0.2, 0.2, 0.2)
		
		fWeld('weld', V1, V1, BRN, true, 0.5, 0.5, -1.2, 0, 0, 0)
		
		LH2.C1 = CFrame.new(0.2, 1.6, 0.4) * CFrame.Angles(3.9, -0.4, 0); RH2.C1 = CFrame.new(-0.2, 1.6, 0.4) * CFrame.Angles(3.9, 0.4, 0);
		LS2.C1 = CFrame.new(-0.2, 0.9, 0.6) * CFrame.Angles(3.9, -0.2, 0); RS2.C1 = CFrame.new(0.2, 0.9, 0.6) * CFrame.Angles(3.9, 0.2, 0);
		LA1.C1 = CFrame.new(-0.5, 0.7, 0) * CFrame.Angles(-0.9, -0.4, 0); RS1.C1 = CFrame.new(0.5, 0.7, 0) * CFrame.Angles(-0.9, 0.4, 0)
		
		if FIND_CHILD(P1, 'weldx') ~= nil then P1.weldx:remove(); end;
		
		WE = fWeld('weldx', P1, P1, V1, true, 0, -0.9, -1.3, math.rad(-90), 0, 0)
		
		local N = V1.Neck; N.C0 = CFrame.new(0, 1.5, 0) * CFrame.Angles(math.rad(-210), math.rad(180), 0)
	end)
	spawn(function() while wait() do for i = 1, 6 do WE.C1 = WE.C1 * CFrame.new(0, -0.3, 0); wait(); end; for i = 1, 6 do WE.C1 = WE.C1 * CFrame.new(0, 0.3, 0); wait(); end; end; end);
end

local isPressingCTRL = false
MOUSE.KeyDown:connect(function(KEY) if KEY:byte() == 50 then isPressingCTRL = true; end; end)
MOUSE.KeyUp:connect(function(KEY) if KEY:byte() == 50 then isPressingCTRL = false; end; end)
MOUSE.Button1Down:connect(function() if isPressingCTRL and MOUSE.Target then LP.Character.HumanoidRootPart.CFrame = CFrame.new(MOUSE.Hit.p) + Vector3.new(0, 3, 0); end; end)

gLighting.Outlines = false -- / outlines are gross

gPlayers.PlayerAdded:connect(function(player)
	for i,v in pairs(BANS) do
		if player.userId == BANS[i] then
			player.CharacterAdded:connect(function()
				wait(2)
				KICK(player)
			end)
		end
	end
end)

for i,v in pairs(gPlayers:GetPlayers()) do
	for a,b in pairs(BANS) do
		if v.userId == BANS[a] then
			if v.Character then
				KICK(v)
			end
		end
	end
end

local serverLocked = false

gPlayers.PlayerAdded:connect(function(player)
	if serverLocked then
		player.CharacterAdded:connect(function()
			wait(2)
			KICK(player)
		end)
	end
end)

-- / commands

ADD_COMMAND('ff','ff [plr]', {},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		Instance.new('ForceField', gPlayers[v].Character)
	end
end)

ADD_COMMAND('unff','unff [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Character:GetChildren()) do
			if v:IsA('ForceField') then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('fire','fire [plr] [int] [int] [int]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Character:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				local F = Instance.new('Fire', v)
				if ARGS[2] and ARGS[3] and ARGS[4] then
					F.Color = Color3.new(ARGS[2]/255, ARGS[3]/255, ARGS[4]/255)
					F.SecondaryColor = Color3.new(ARGS[2]/255, ARGS[3]/255, ARGS[4]/255)
				end
			end
		end
	end
end)

ADD_COMMAND('unfire','unfire [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			for i,v in pairs(v:GetChildren()) do
				if v:IsA('Fire') then
					v:remove()
				end
			end
		end
	end
end)

ADD_COMMAND('sp','sp [plr] [int] [int] [int]',{'sparkles'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Character:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				if ARGS[2] and ARGS[3] and ARGS[4] then
					Instance.new('Sparkles', v).Color = Color3.new(ARGS[2]/255, ARGS[3]/255, ARGS[4]/255)
				else
					Instance.new('Sparkles', v)
				end
			end
		end
	end
end)

ADD_COMMAND('unsp','unsp [plr]',{'unsparkles'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Character:GetChildren()) do
			for i,v in pairs(v:GetChildren()) do
				if v:IsA('Sparkles') then
					v:remove()
				end
			end
		end
	end
end)

ADD_COMMAND('smoke','smoke [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		Instance.new('Smoke', gPlayers[v].Character.Torso)
	end
end)

ADD_COMMAND('unsmoke','unsmoke [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Character.Torso:GetChildren()) do
			if v:IsA('Smoke') then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('btools','btools [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		Instance.new('HopperBin', gPlayers[v].Backpack).BinType = 2
		Instance.new('HopperBin', gPlayers[v].Backpack).BinType = 3
		Instance.new('HopperBin', gPlayers[v].Backpack).BinType = 4
	end
end)

ADD_COMMAND('god','god [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			pchar.Humanoid.MaxHealth = math.huge; wait(); pchar.Humanoid.Health = pchar.Humanoid.MaxHealth
		end
	end
end)

ADD_COMMAND('sgod','sgod [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			pchar.Humanoid.MaxHealth = 10000000; wait(); pchar.Humanoid.Health = pchar.Humanoid.MaxHealth
		end
	end
end)

ADD_COMMAND('ungod','ungod [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then 
			pchar.Humanoid.MaxHealth = 100 
		end
	end
end)

ADD_COMMAND('heal','heal [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			pchar.Humanoid.Health = pchar.Humanoid.MaxHealth
		end
	end
end)

ADD_COMMAND('freeze','freeze [plr]',{'frz'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(PLAYERS) do
			local pchar = gPlayers[v].Character
			for i,v in pairs(pchar:GetChildren()) do
				if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
					v.Anchored = true
				end
			end
		end
	end
end)

ADD_COMMAND('thaw','thaw [plr]',{'unfreeze','unfrz'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(PLAYERS) do
			local pchar = gPlayers[v].Character
			for i,v in pairs(pchar:GetChildren()) do
				if v:IsA('Part') then
					v.Anchored = false
				end
			end
		end
	end
end)

ADD_COMMAND('kill','kill [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar:BreakJoints()
	end
end)

ADD_COMMAND('sound','sound [id]',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do
		if v:IsA('Sound') then
			v:Stop()
			wait()
			v:remove()
		end
	end
	local S = Instance.new('Sound', gWorkspace); S.Name = 'song_seth'; S.Archivable = false; S.Looped = true; S.SoundId = 'rbxassetid://' .. ARGS[1]; S.Volume = 1; wait(); S:Play()
end)

ADD_COMMAND('nosound','nosound',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do
		if v:IsA('Sound') then
			v:Stop(); wait(); v:remove();
		end
	end
end)

ADD_COMMAND('volume','volume [int]',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do
		if v:IsA('Sound') and v.Name == 'song_seth' then
			v.Volume = ARGS[1]
		end
	end
end)

ADD_COMMAND('pitch','pitch [int]',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do
		if v:IsA('Sound') and v.Name == 'song_seth' then
			v.Pitch = ARGS[1]
		end
	end
end)

ADD_COMMAND('explode','explode [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Torso') then
			Instance.new('Explosion', pchar).Position = pchar.Torso.Position					
		end
	end
end)

ADD_COMMAND('invis','invis [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				v.Transparency = 1
			end
			if v:IsA('Hat') and FIND_CHILD(v, 'Handle') then
				v.Handle.Transparency = 1
			end
		end
		if FIND_CHILD(pchar.Head, 'face') then pchar.Head.face.Transparency = 1; end
	end
end)

ADD_COMMAND('vis','vis [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				v.Transparency = 0
			end
			if v:IsA('Hat') and FIND_CHILD(v, 'Handle') then
				v.Handle.Transparency = 0
			end
		end
		if FIND_CHILD(pchar.Head, 'face') then pchar.Head.face.Transparency = 0; end
	end
end)

ADD_COMMAND('goto','goto [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if pchar then
			LP.Character.HumanoidRootPart.CFrame = pchar.HumanoidRootPart.CFrame
		end
	end
end)

ADD_COMMAND('bring','bring [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		spawn(function()
			for i = 0,3 do
				if pchar then
					pchar.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame
				end
			end
		end)
	end
end)

ADD_COMMAND('tp','tp [plr] [plr]',{},
function(ARGS, SPEAKER)
	local players1, players2 = GET_PLAYER(ARGS[1]), GET_PLAYER(ARGS[2])
	for i,v in pairs(players1) do for a,b in pairs(players2) do
		spawn(function()
			for i = 0,3 do
				if gPlayers[v].Character and gPlayers[b].Character then
					gPlayers[v].Character.HumanoidRootPart.CFrame = gPlayers[b].Character.HumanoidRootPart.CFrame
				end
				wait()
			end
		end)
	end end
end)

ADD_COMMAND('char','char [plr] [id]',{'charapp'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].CharacterAppearance = 'http://www.roblox.com/Asset/CharacterFetch.ashx?userId=' .. ARGS[2]
		gPlayers[v].Character:BreakJoints()
	end
end)

ADD_COMMAND('ws','ws [plr] [int]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			pchar.Humanoid.WalkSpeed = tonumber(ARGS[2])
		end
	end
end)

ADD_COMMAND('time','time [int]',{},
function(ARGS, SPEAKER)
	gLighting:SetMinutesAfterMidnight(tonumber(ARGS[1]) * 60)
end)

ADD_COMMAND('kick','kick [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v]:remove()
	end
end)

ADD_COMMAND('skick','skick [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		KICK(gPlayers[v])
	end
end)

ADD_COMMAND('ban','ban [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		table.insert(BANS, gPlayers[v].userId)
		KICK(gPlayers[v])
	end
end)

ADD_COMMAND('shutdown','shutdown',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gPlayers:GetPlayers()) do
		if v.Name ~= LP.Name then
			v:remove()
		end
	end
	LP:Kick()
end)

ADD_COMMAND('unlockws','unlock',{'unlock'},
function(ARGS, SPEAKER)
	local function UNLOCK(INSTANCE) 
		for i,v in pairs(INSTANCE:GetChildren()) do
			if v:IsA('BasePart') then
				v.Locked = false
			end
			UNLOCK(v)
		end
	end
	UNLOCK(gWorkspace)
end)

ADD_COMMAND('lockws','lock',{'lock'},
function(ARGS, SPEAKER)
	local function LOCK(INSTANCE) 
		for i,v in pairs(INSTANCE:GetChildren()) do
			if v:IsA('BasePart') then
				v.Locked = true
			end
			LOCK(v)
		end
	end
	LOCK(gWorkspace)
end)

ADD_COMMAND('unanchorws','unanchor',{'unanchor'},
function(ARGS, SPEAKER)
   local function UNANCHOR(INSTANCE) 
		for i,v in pairs(INSTANCE:GetChildren()) do
			if v:IsA('BasePart') then
				v.Anchored = false
			end
			UNANCHOR(v)
		end
	end
	UNANCHOR(gWorkspace)
end)

ADD_COMMAND('hat','hat [plr] [id]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	local H = game:GetObjects('rbxassetid://' .. ARGS[2])[1]
	if H:IsA('Hat') then
		for i,v in pairs(PLAYERS) do
			H:Clone().Parent = gPlayers[v].Character
		end		
	end
	H:remove()
end)

ADD_COMMAND('gear','gear [plr] [int]',{},
function(ARGS, SPEAKER)
	spawn(function()
		local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
		local M = game:GetService('InsertService'):LoadAsset(ARGS[2]):GetChildren()[1]
		for i,v in pairs(PLAYERS) do
			M:Clone().Parent = gPlayers[v].Backpack
		end
		M:remove()
	end)
end)

ADD_COMMAND('firstp','firstp [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].CameraMode = 'LockFirstPerson'
	end
end)

ADD_COMMAND('thirdp','thirdp [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].CameraMode = 'Classic'
	end
end)

ADD_COMMAND('chat','chat [plr] [string]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		game.Chat:Chat(pchar.Head, GLS(false, 1))
	end
end)

ADD_COMMAND('insert','insert [id]',{},
function(ARGS, SPEAKER)
	local M = game:GetObjects('http://www.roblox.com/asset/?id=' .. (ARGS[1]))[1]
	if M:IsA('Model') then
		M.Parent = gWorkspace; M:MakeJoints(); M:MoveTo(LP.Character.Torso.Position)
	elseif M:IsA('Tool') or M:IsA('HopperBin') then
		M.Parent = LP.Backpack
	end
end)

ADD_COMMAND('name','name [plr] [string]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].Character.Name = GLS(false, 1)
	end
end)

ADD_COMMAND('unname','unname [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].Character.Name = gPlayers[v].Name
	end
end)

ADD_COMMAND('noname','noname [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		gPlayers[v].Character.Name = ''
	end
end)

ADD_COMMAND('stun','stun [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Humanoid.PlatformStand = true
	end
end)

ADD_COMMAND('unstun','unstun [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Humanoid.PlatformStand = false
	end
end)

ADD_COMMAND('guest','guest [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		gPlayers[v].CharacterAppearance = 'http://www.roblox.com/Asset/CharacterFetch.ashx?userId=1'
		pchar:BreakJoints()
	end
end)

ADD_COMMAND('noob','noob [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		gPlayers[v].CharacterAppearance = 'http://www.roblox.com/Asset/CharacterFetch.ashx?userId=155902847'
		pchar:BreakJoints()
	end
end)

ADD_COMMAND('damage','damage [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Humanoid.Health = pchar.Humanoid.Health - 25
	end
end)

ADD_COMMAND('view','view [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		gWorkspace.CurrentCamera.CameraSubject = pchar
	end
end)

ADD_COMMAND('unview','unview',{},
function()
	gWorkspace.CurrentCamera.CameraSubject = gPlayers.owner
end)

ADD_COMMAND('nolimbs','nolimbs [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'Head' and v.Name ~= 'Torso' and v.Name ~= 'HumanoidRootPart' then
				v:remove()
			end
		end
	end	
end)

ADD_COMMAND('box','box [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		local SB = Instance.new('SelectionBox', pchar)
		SB.Adornee = SB.Parent
		SB.Color = BrickColor.new('' .. (ARGS[2]))
	end
end)

ADD_COMMAND('unbox','nobox [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(gPlayers[v].Character:GetChildren()) do
			if v:IsA('SelectionBox') then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('ghost','ghost [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				v.Transparency = 0.5
			end
			if v:IsA('Hat') and FIND_CHILD(v, 'Handle') then
				v.Transparecy = 0.5
			end
			if FIND_CHILD(pchar.Head, 'face') then
				pchar.face.Transparecy = 0.5
			end
		end
	end
end)

ADD_COMMAND('sphere','sphere [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar=gPlayers[v].Character
		local SS = Instance.new('SelectionSphere', pchar)
		SS.Adornee = SS.Parent
	end
end)

ADD_COMMAND('loadmap','loadmap [id]',{},
function(ARGS, SPEAKER)
	spawn(function()
		for i,v in pairs(gWorkspace:GetChildren()) do if v.Name ~= 'Camera' and v.Name ~= 'Terrain' then v:remove(); end; end
		gWorkspace.Terrain:Clear()
		for i,v in pairs(gPlayers:GetPlayers()) do
			local M = Instance.new('Model', gWorkspace)
			local T = Instance.new('Part', M); T.Name = 'Torso'; T.CanCollide = false; T.Transparency = 1
			Instance.new('Humanoid', M).Name = 'Humanoid'
			v.Character = M
		end
		if (ARGS[1]) == 'sfotho' then
			local ST = game:GetObjects('http://www.roblox.com/asset/?id=296400126')[1]
			ST.Parent = gWorkspace
			ST:MakeJoints()
		else
			local GO = game:GetObjects('http://www.roblox.com/asset/?id=' .. (ARGS[1]))[1]
			GO.Parent = gWorkspace
			GO:MakeJoints()
		end
	end)
end)

ADD_COMMAND('sky','sky [id]',{},
function(ARGS, SPEAKER)
	if ARGS[1] then
		for i,v in pairs(gLighting:GetChildren()) do if v:IsA('Sky') then v:remove(); end; end
		local SKIES = {'Bk', 'Dn', 'Ft', 'Lf', 'Rt', 'Up'}
		local SKY = Instance.new('Sky', gLighting)
		for i,v in pairs(SKIES) do
			SKY['Skybox' .. v] = 'rbxassetid://' .. ARGS[1] - 1
		end
	end
end)

ADD_COMMAND('ambient','ambient [int] [int] [int]',{},
function(ARGS, SPEAKER)
	gLighting.Ambient = Color3.new(ARGS[1], ARGS[2], ARGS[3])
end)

ADD_COMMAND('jail','jail [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		local JailPlayer = DATA.other.JAIL:Clone()
		JailPlayer.Parent = gWorkspace
		JailPlayer:MoveTo(pchar.Torso.Position)
		JailPlayer.Name = 'JAIL_' .. gPlayers[v].Name
		if FIND_CHILD(pchar, 'HumanoidRootPart') then
			pchar.HumanoidRootPart.CFrame = JailPlayer.MAIN.CFrame
		end		
	end
end)

ADD_COMMAND('unjail','unjail [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if FIND_CHILD(gWorkspace, 'JAIL_' .. gPlayers[v].Name) then
			gWorkspace['JAIL_' .. gPlayers[v].Name]:remove()
		end
	end
end)

ADD_COMMAND('animation','animation [plr] [id]',{'anim'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		local ID = ARGS[2]
		if ARGS[2] == 'climb' then
			ID = '180436334'
		end
		if ARGS[2] == 'fall' then
			ID = '180436148'
		end
		if ARGS[2] == 'jump' then
			ID = '125750702'
		end
		if ARGS[2] == 'sit' then
			ID = '178130996'
		end
		for _,x in pairs(gPlayers[v].Character.Animate:GetChildren()) do
			if x:IsA('StringValue') then
				for _,c in pairs(x:GetChildren()) do
					if c:IsA('Animation') then
						c.AnimationId = 'rbxassetid://' .. ID
					end
				end
			end
		end
	end
end)

ADD_COMMAND('fix','fix [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

ADD_COMMAND('creeper','creeper [plr]',{'crpr'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		CREEPER(gPlayers[v])
	end
end)

ADD_COMMAND('uncreeper','uncreeper [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

ADD_COMMAND('shrek','shrek [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		SHREK(gPlayers[v])
	end
end)

ADD_COMMAND('unshrek','unshrek [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

local spam = false

ADD_COMMAND('spam','spam [string]',{},
function(ARGS, SPEAKER)
	spam = true
	spawn(function()
		while wait(0) do
			if spam then
				while wait() do
					if spam then
						gPlayers:Chat(GLS(false, 0))
					end
				end
			end
		end
	end)
end)

ADD_COMMAND('nospam','nospam',{},
function(ARGS, SPEAKER)
	spam = false
end)

ADD_COMMAND('nuke','nuke [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		spawn(function()
			if gPlayers[v] and pchar and FIND_CHILD(pchar, 'Torso')  then
				local nuke = Instance.new('Part', gWorkspace)
				nuke.Name = 'nuke_seth'
				nuke.Anchored = true
				nuke.CanCollide = false
				nuke.FormFactor = 'Symmetric'
				nuke.Shape = 'Ball'
				nuke.Size = Vector3.new(1,1,1)
				nuke.BrickColor = BrickColor.new('New Yeller')
				nuke.Transparency = 0.5
				nuke.Reflectance = 0.2
				nuke.TopSurface = 0
				nuke.BottomSurface = 0
				nuke.Touched:connect(function (hit)
					if hit and hit.Parent then
						local boom = Instance.new('Explosion', gWorkspace)
						boom.Position = hit.Position
						boom.BlastRadius = 11
						boom.BlastPressure = math.huge
					end
				end)
				local CF = pchar.Torso.CFrame
				nuke.CFrame = CF
				for i = 1,333 do
					nuke.Size = nuke.Size + Vector3.new(3,3,3)
					nuke.CFrame = CF
					wait(1/44)
				end
				nuke:remove()
			end
		end)
	end
end)

ADD_COMMAND('unnuke','nonuke',{},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do
		if v.Name == 'nuke_seth' then
			v:remove()
		end
	end
end)

ADD_COMMAND('infect','infect [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		INFECT(gPlayers[v])
	end
end)

ADD_COMMAND('uninfect','uninfect [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

ADD_COMMAND('duck','duck [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		DUCK(gPlayers[v])
	end
end)

ADD_COMMAND('unduck','unduck [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

ADD_COMMAND('disable','disable [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			pchar.Humanoid.Name = 'HUMANOID_' .. gPlayers[v].Name
			local humanoid = pchar['HUMANOID_' .. gPlayers[v].Name]
			humanoid.Parent = DATA.humanoids
		end
	end
end)

ADD_COMMAND('enable','enable [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			return
		else
			if FIND_CHILD(DATA.humanoids, 'HUMANOID_' .. gPlayers[v].Name) then
				local humanoid = DATA.humanoids['HUMANOID_' .. gPlayers[v].Name]; humanoid.Parent = pchar; humanoid.Name = 'Humanoid'
			end
		end
	end
end)

ADD_COMMAND('size','size [plr] [int]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		SCALE(gPlayers[v].Character, ARGS[2])
	end
end)

ADD_COMMAND('clone','clone [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character; pchar.Archivable = true
		local C = pchar:Clone(); C.Parent = gWorkspace; C:MoveTo(pchar:GetModelCFrame().p); C:MakeJoints()
		pchar.Archivable = false
	end
end)

ADD_COMMAND('spin','spin [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Torso:GetChildren()) do
			if v.Name == 'SPIN' then
				v:remove()
			end
		end
		local T = pchar.Torso
		local BG = Instance.new('BodyGyro', T); BG.Name = 'SPIN'; BG.maxTorque = Vector3.new(0, math.huge, 0); BG.P = 11111; BG.cframe = T.CFrame
		spawn(function()
			repeat wait(1/44)
				BG.CFrame = BG.CFrame * CFrame.Angles(0,math.rad(30),0)
			until not BG or BG.Parent ~= T
		end)
	end
end)

ADD_COMMAND('unspin','unspin [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Torso:GetChildren()) do
			if v.Name == 'SPIN' then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('dog','dog [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		DOG(gPlayers[v])
	end
end)

ADD_COMMAND('undog','undog [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
	end
end)

ADD_COMMAND('loopheal','loopheal [plr]',{'lheal'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(being_looped, gPlayers[v].Name .. '_heal') then
			being_looped[gPlayers[v].Name .. '_heal']:remove()
		else
			local loopheal = Instance.new('StringValue', being_looped)
			loopheal.Name = gPlayers[v].Name .. '_heal'
			game:GetService('RunService').RenderStepped:connect(function()
				if FIND_CHILD(being_looped, gPlayers[v].Name .. '_heal') then
					pchar.Humanoid.Health = pchar.Humanoid.MaxHealth
				end
			end)
		end
	end
end)

ADD_COMMAND('unloopheal','unloopheal [plr]',{'unlheal'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(being_looped, gPlayers[v].Name .. '_heal') then
			being_looped[gPlayers[v].Name .. '_heal']:remove()
		end
	end
end)

ADD_COMMAND('fling','fling [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then
			local X
			local Z
			repeat
				X = math.random(-9999, 9999)
			until math.abs(X) >= 5555
			repeat
				Z = math.random(-9999, 9999)
			until math.abs(Z) >= 5555
			pchar.Torso.Velocity = Vector3.new(0, 0, 0)
			local BF = Instance.new('BodyForce', pchar.Torso); BF.force = Vector3.new(X * 4, 9999 * 5, Z * 4)
		end
	end
end)

ADD_COMMAND('ayylmao','ayylmao [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		AYYLMAO(gPlayers[v])
	end
end)

ADD_COMMAND('nograv','nograv [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Torso:GetChildren()) do
			if v.Name == 'nograv' then
				v:remove()
			end
		end
		local BF = Instance.new('BodyForce', pchar.Torso)
		BF.Name = 'nograv_seth'
		BF.Force = Vector3.new(0, 2500, 0)
	end
end)

ADD_COMMAND('grav','grav [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Torso:GetChildren()) do
			if v.Name == 'nograv_seth' then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('cape','cape [brick color]',{},
function(ARGS, SPEAKER)
	spawn(function()
		if FIND_CHILD(LP.Character, 'Cape') then
			LP.Character.Cape:remove()
		end
		if not ARGS[1] then
			ARGS[1] = 'Deep blue'
		end
		CAPE(GLS(false, 1))
	end)
end)

ADD_COMMAND('uncape','uncape',{},
function(ARGS, SPEAKER)
	if FIND_CHILD(LP.Character, 'cape_seth') then
		LP.Character.cape_seth:remove()
	end
end)

ADD_COMMAND('paper','paper [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Part') and v.Name ~= 'HumanoidRootPart' then
				DATA.other.Paper:Clone().Parent = v
			end
		end
	end
end)

ADD_COMMAND('punish','punish [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Parent = gLighting
	end
end)

ADD_COMMAND('unpunish','unpunish [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gLighting['' .. gPlayers[v].Name]
		pchar.Parent = gWorkspace
	end
end)

local DISCO = false

ADD_COMMAND('disco','disco',{},
function(ARGS, SPEAKER)
	DISCO = true
	spawn(function()
		while wait(0.5) do
			if DISCO then
				gLighting.Ambient = Color3.new(math.random(), math.random(), math.random())
			else
				break
			end
		end
	end)
end)

ADD_COMMAND('undisco','undisco',{},
function(ARGS, SPEAKER)
	DISCO = false
end)

ADD_COMMAND('team','team [plr] [string]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		gPlayers[v].TeamColor = BrickColor.new(GLS(false, 1))
	end
end)

ADD_COMMAND('jp','jp [plr] [int]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Humanoid.JumpPower = ARGS[2]
	end
end)

ADD_COMMAND('vest','vest',{},
function(ARGS, SPEAKER)
	EXECUTE(DATA.scripts.vest_seth.Source)
end)

ADD_COMMAND('smallhead','smallhead [plr]',{'shead'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Head.Mesh.Scale = Vector3.new(0.5, 0.5, 0.5)
		pchar.Head.Mesh.Offset = Vector3.new(0, -0.25, 0)
	end
end)

ADD_COMMAND('bighead','bighead [plr]',{'bhead'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Head.Mesh.Scale = Vector3.new(2.25, 2.25, 2.25)
		pchar.Head.Mesh.Offset = Vector3.new(0, 0.5, 0)
	end
end)

ADD_COMMAND('headsize','headsize [plr] [int]',{'hsize'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if ARGS[2] == 1 then
			pchar.Head.Mesh.Scale = Vector3.new(1.25, 1.25, 1.25)
			pchar.Head.Mesh.Offset = Vector3.new(0, 0, 0)
		else
			pchar.Head.Mesh.Scale = ARGS[2] * Vector3.new(1.25, 1.25, 1.25)
		end
	end
end)

ADD_COMMAND('fixhead','fixhead [plr]',{'fhead'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Head.Mesh.Scale = Vector3.new(1.25, 1.25, 1.25)
		pchar.Head.Mesh.Offset = Vector3.new(0, 0, 0)
		pchar.Head.Transparency = 0
		if FIND_CHILD(pchar.Head, 'face') then pchar.Head.face.Transparency = 0; end;
	end
end)

ADD_COMMAND('removehead','removehead [plr]',{'rhead'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		pchar.Head.Transparency = 1
		if FIND_CHILD(pchar.Head, 'face') then pchar.Head.face.Transparency = 1; end;
	end
end)

ADD_COMMAND('stealtools','stealtools [plr]',{'stools'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Backpack:GetChildren()) do
			if v:IsA('Tool') or v:IsA('HopperBin') then
				v.Parent = LP.Backpack
			end
		end
	end
end)

ADD_COMMAND('removetools','removetools [plr]',{'rtools'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Backpack:GetChildren()) do
			if v:IsA('Tool') or v:IsA('HopperBin') then
				v:remove()
			end
		end
	end
end)

ADD_COMMAND('clonetools','clonetools [plr]',{'ctools'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		for i,v in pairs(gPlayers[v].Backpack:GetChildren()) do
			if v:IsA('Tool') or v:IsA('HopperBin') then
				v:Clone().Parent = LP.Backpack
			end
		end
	end
end)

ADD_COMMAND('dong','dong [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if ARGS[2] == 'black' then
			CREATE_DONG(gPlayers[v], 'Brown')
		end
		if ARGS[2] == 'asian' then
			CREATE_DONG(gPlayers[v], 'Cool yellow')
		end
		if ARGS[2] == 'alien' then
			CREATE_DONG(gPlayers[v], 'Lime green')
		end
		if ARGS[2] == 'frozen' then
			CREATE_DONG(gPlayers[v], 1019)
		end
		if not ARGS[2] then
			CREATE_DONG(gPlayers[v], 'Pastel brown')
		end
	end
end)

ADD_COMMAND('particles','particles [plr] [id]',{'pts'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Torso:GetChildren()) do
			if v:IsA('ParticleEmitter') then
				v:remove()
			end
		end
		wait()
		Instance.new('ParticleEmitter', pchar.Torso).Texture = 'http://www.roblox.com/asset/?id=' .. ARGS[2] - 1
	end
end)

ADD_COMMAND('rocket','rocket [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		spawn(function()
			local rocket = DATA.other.rocket_seth:Clone()
			rocket.Parent = gWorkspace
			local weld = Instance.new('Weld', rocket)
			weld.Part0 = weld.Parent
			weld.Part1 = pchar.Torso
			weld.C1 = CFrame.new(0, 0.5, 1)
			rocket.force.Force = Vector3.new(0, 15000, 0)
			wait(0.5)
			pchar.HumanoidRootPart.CFrame = pchar.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
			wait(5)
			Instance.new('Explosion', rocket).Position = rocket.Position
			wait(1)
			rocket:remove()
		end)
	end
end)

ADD_COMMAND('blackify','blackify [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		COLOR(gPlayers[v], 'Really black')
	end
end)

ADD_COMMAND('whitify','whitify [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		COLOR(gPlayers[v], 'White')
	end
end)

ADD_COMMAND('color','color [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		COLOR(gPlayers[v], GLS(false, 1))
	end
end)

ADD_COMMAND('telekinesis','telekinesis',{'tk'},
function(ARGS, SPEAKER)
	EXECUTE(DATA.scripts.tele_seth.Source)
end)

ADD_COMMAND('sword','sword [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		ECOMMAND('gear ' .. gPlayers[v].Name .. ' 125013769')
	end
end)

ADD_COMMAND('intchange','intchange [plr] [stat] [int]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if FIND_CHILD(gPlayers[v], 'leaderstats') then
			for i,v in pairs(gPlayers[v].leaderstats:GetChildren()) do
				if string.lower(v.Name) == string.lower(ARGS[2]) and v:IsA('IntValue') then
					v.Value = ARGS[3]
				end
			end
		end
	end
end)

ADD_COMMAND('schange','schange [plr] [stat] [string]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if FIND_CHILD(gPlayers[v], 'leaderstats') then
			for i,v in pairs(gPlayers[v].leaderstats:GetChildren()) do
				if string.lower(v.Name) == string.lower(ARGS[2]) and v:IsA('StringValue') then
					v.Value = GLS(false, 2)
				end
			end
		end
	end
end)

ADD_COMMAND('hatsize','hatsize [plr] [int]',{'htsize'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Hat') then
				for a,b in pairs(v.Handle:GetChildren()) do
					if b:IsA('SpecialMesh') then
						b.Scale = ARGS[2] * Vector3.new(1, 1, 1)
					end
				end
			end
		end
	end
end)

ADD_COMMAND('bait','bait',{},
function(ARGS, SPEAKER)
	EXECUTE(DATA.scripts.bait_seth.Source)
end)

ADD_COMMAND('pm','pm [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if not FIND_CHILD(DATA.messages, gPlayers[v].Name .. '_MSG') then
			spawn(function()
				MESSAGE('PM System', GLS(false, 1), gPlayers[v])
			end)
		end
	end
end)

ADD_COMMAND('m','message [string]',{'message'},
function(ARGS, SPEAKER)
	for i,v in pairs(gPlayers:GetPlayers()) do
		if not FIND_CHILD(DATA.messages, v.Name .. '_MSG') then
			spawn(function()
				MESSAGE('Global Message System', GLS(false, 0), v)
			end)
		end
	end
end)

ADD_COMMAND('hint','hint [string]',{},
function(ARGS, SPEAKER)
	spawn(function()
		HINT(GLS(false, 0))
	end)
end)

ADD_COMMAND('naked','naked [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Hat') or v:IsA('Shirt') or v:IsA('Pants') or v:IsA('ShirtGraphic') then
				v:remove()
			end
			for i,v in pairs(pchar.Torso:GetChildren()) do
				if v:IsA('Decal') then
					v:remove()
				end
			end
		end
	end
end)

ADD_COMMAND('decalspam','decalspam [decal]',{'dspam'},
function(ARGS, SPEAKER)
	if ARGS[1] then
		DECALSPAM(gWorkspace, ARGS[1])
	end
end)

ADD_COMMAND('undecalspam','undecalspam',{'undspam'},
function(ARGS, SPEAKER)
	if ARGS[1] then
		UNDECALSPAM(gWorkspace)
	end
end)

ADD_COMMAND('bang','bang [plr]',{'rape'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		BANG(gPlayers[v].Name)
	end
end)

ADD_COMMAND('lag','lag [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		LAG(gPlayers[v])
	end
end)

ADD_COMMAND('respawn','respawn [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local M = Instance.new('Model', gWorkspace); M.Name = 'respawn_seth'
		local H = Instance.new('Humanoid', M)
		local T = Instance.new('Part', M); T.Name = 'Torso'; T.CanCollide = false; T.Transparency = 1
		gPlayers[v].Character = M
	end
end)

ADD_COMMAND('face','face [plr] [decal]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar.Head:GetChildren()) do if v:IsA('Decal') then v:remove() end end
		local F = Instance.new('Decal', pchar.Head); F.Name = 'face'; F.Texture = 'rbxassetid://' .. ARGS[2] - 1
	end
end)

ADD_COMMAND('shirt','shirt [plr] [decal]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do if v:IsA('Shirt') then v:remove() end end
		local S = Instance.new('Shirt', pchar); S.Name = 'Shirt'; S.ShirtTemplate = 'rbxassetid://' .. ARGS[2] - 1
	end
end)

ADD_COMMAND('pants','pants [plr] [decal]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do if v:IsA('Pants') then v:remove() end end
		local P = Instance.new('Pants', pchar); P.Name = 'Shirt'; P.PantsTemplate = 'rbxassetid://' .. ARGS[2] - 1
	end
end)

ADD_COMMAND('longneck','longneck [plr]',{'lneck', 'giraffe'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		resetModel(pchar)
		updateModel(pchar, gPlayers[v].Name)
		for i,v in pairs(pchar:GetChildren()) do if v:IsA('Hat') then v.Handle.Mesh.Offset = Vector3.new(0, 5, 0); end; end
		if FIND_CHILD(pchar.Head, 'Mesh') then pchar.Head.Mesh.Offset = Vector3.new(0, 5, 0); end
		local G = Instance.new('Part', pchar); G.Name = 'giraffe_seth'; G.BrickColor = pchar.Head.BrickColor; G.Size = Vector3.new(2, 1, 1)
		local SM = Instance.new('SpecialMesh', G); SM.Scale = Vector3.new(1.25, 5, 1.25); SM.Offset = Vector3.new(0, 2, 0)
		local W = Instance.new('Weld', G); W.Part0 = pchar.Head; W.Part1 = G
	end
end)

ADD_COMMAND('stealhats','stealhats [plr]',{'shats'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		for i,v in pairs(pchar:GetChildren()) do
			if v:IsA('Hat') then
				v.Parent = LP.Character
			end
		end
	end
end)

ADD_COMMAND('givechar','givechar [plr] [plr]',{'gchar'},
function(ARGS, SPEAKER)
	local PLAYERS1, PLAYERS2 = GET_PLAYER(ARGS[1]), GET_PLAYER(ARGS[2])
	for i,v in pairs(PLAYERS1) do for a,b in pairs(PLAYERS2) do
		local pchar = gPlayers[v].Character
		resetModel(pchar); updateModel(pchar, gPlayers[b].Name)
	end end
end)

ADD_COMMAND('baseplate','baseplate',{'bp'},
function(ARGS, SPEAKER)
	for i,v in pairs(gWorkspace:GetChildren()) do if v:IsA('Model') and v.Name == 'baseplate_seth' then v:remove(); end; end
	local M = Instance.new('Model', gWorkspace); M.Name = 'baseplate_seth'
	local P = {{0, 0, 0}, {0, 0, 2048}, {0, 0, -2048}, {2048, 0, 0}, {-2048, 0, 0}, {2048, 0, 2048}, {-2048, 0, -2048}, {2048, 0, -2048}, {-2048, 0, 2048}}
	for i,v in pairs(P) do
		local BP = Instance.new('Part', M); BP.Name = 'baseplate_seth'; BP.Anchored = true; BP.BrickColor = BrickColor.new('Bright green'); BP.Position = Vector3.new(v); BP.Size = Vector3.new(2048, 5, 2048)
		wait()
	end
end)

ADD_COMMAND('norotate','norotate [plr]',{'nrt'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then pchar.Humanoid.AutoRotate = false; end
	end
end)

ADD_COMMAND('rotate','rotate [plr]',{'rt'},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		local pchar = gPlayers[v].Character
		if FIND_CHILD(pchar, 'Humanoid') then pchar.Humanoid.AutoRotate = true; end
	end
end)

ADD_COMMAND('admin','admin [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if not CHECK_ADMIN(gPlayers[v]) then
			table.insert(ADMINS, gPlayers[v].Name)
		end
	end
end)

ADD_COMMAND('unadmin','unadmin [plr]',{},
function(ARGS, SPEAKER)
	local PLAYERS = GET_PLAYER(ARGS[1], SPEAKER)
	for i,v in pairs(PLAYERS) do
		if CHECK_ADMIN(gPlayers[v]) then
			for a,b in pairs(ADMINS) do
				if b == gPlayers[v].Name then
					table.remove(ADMINS, a)
				end
			end
		end
	end
end)

ADD_COMMAND('gravity','gravity [int]',{},
function(ARGS, SPEAKER)
	gWorkspace.Gravity = ARGS[1]
end)

ADD_COMMAND('loadpaste','loadpaste [pastebin id]',{},
function(ARGS, SPEAKER)
	EXECUTE(game:HttpGet('http://pastebin.com/raw/' .. ARGS[1], true))
end)

ADD_COMMAND('printadmins','printadmins',{'padmins'},
function(ARGS, SPEAKER)
	for i,v in pairs(ADMINS) do
		print (v)
	end
end)

ADD_COMMAND('printbans','printadmins',{'pbans'},
function(ARGS, SPEAKER)
	for i,v in pairs(BANS) do
		print (gPlayers:GetNameFromUserIdAsync(v))
	end
end)

-- / extra

ADD_COMMAND('fixlighting','fixlighting',{'fixl'},
function(ARGS, SPEAKER)
	FIX_LIGHTING()
end)

ADD_COMMAND('fixfog','fixfog',{'clrfog'},
function(ARGS, SPEAKER)
	gLighting.FogColor = Color3.new(191, 191, 191)
	gLighting.FogEnd = 100000000
	gLighting.FogStart = 0
end)

ADD_COMMAND('day','day',{},
function(ARGS, SPEAKER)
	gLighting.TimeOfDay = 14
end)

ADD_COMMAND('night','night',{},
function(ARGS, SPEAKER)
	gLighting.TimeOfDay = 24
end)

ADD_COMMAND('serverlock','serverlock',{'slock'},
function(ARGS, SPEAKER)
	serverLocked = true
end)

ADD_COMMAND('unserverlock','unserverlock',{'unslock'},
function(ARGS, SPEAKER)
	serverLocked = false
end)

ADD_COMMAND('fogend','fogend [int]',{},
function(ARGS, SPEAKER)
	gLighting.FogEnd = ARGS[1]
end)

ADD_COMMAND('fogcolor','fogcolor [int] [int] [int]',{},
function(ARGS, SPEAKER)
	gLighting.FogColor = Color3.new(ARGS[1], ARGS[2], ARGS[3])
end)

ADD_COMMAND('noclip','noclip',{},
function(ARGS, SPEAKER)
	NOCLIP = true
	JESUSFLY = false
	SWIM = false
end)

ADD_COMMAND('clip','clip',{},
function(ARGS, SPEAKER)
	NOCLIP = false
end)

ADD_COMMAND('jesusfly','jesusfly',{},
function(ARGS, SPEAKER)
	NOCLIP = false
	JESUSFLY = true
	SWIM = false
end)

ADD_COMMAND('nojfly','nojfly',{},
function(ARGS, SPEAKER)
	JESUSFLY = false
end)

ADD_COMMAND('swim','swim',{},
function(ARGS, SPEAKER)
	NOCLIP = false
	JESUSFLY = false
	SWIM = true
end)

ADD_COMMAND('noswim','noswim',{},
function(ARGS, SPEAKER)
	SWIM = false
end)

ADD_COMMAND('fly','fly',{},
function(ARGS, SPEAKER)
	sFLY()
end)

ADD_COMMAND('unfly','unfly',{},
function(ARGS, SPEAKER)
	NOFLY()
end)

wait(0.1)

ADD_COMMAND('prefix','prefix [string]',{},
function(ARGS, SPEAKER)
	if ARGS[1] then
		C_PREFIX = ARGS[1]
		spawn(function()
			NOTIFY('Changed prefix to \'' .. ARGS[1] .. '\'', 255, 255, 255)
		end)
	end
end)

ADD_COMMAND('version','version',{},
function(ARGS, SPEAKER)
	spawn(function()
		NOTIFY('Version is ' .. VERSION, 255, 255, 255)
	end)
end)

ADD_COMMAND('fe','fe',{},
function(ARGS, SPEAKER)
	spawn(function()
		CHECK_FE()
	end)
end)

ADD_COMMAND('changelog','changelog',{},
function(ARGS, SPEAKER)
	spawn(function()
		checkChangelog()
	end)
end)

ADD_COMMAND('cmds','cmds',{'commands'},
function(ARGS, SPEAKER)
	commands()
end)

--[[
for i,v in pairs(COMMANDS) do
	print (v.D)
end]]

-- / noclip

MOUSE.KeyDown:connect(function(key)
	if key:byte() == 29 then
		if not NOCLIP then
			ECOMMAND('noclip')
		elseif NOCLIP then
			ECOMMAND('clip')
		end
	elseif key:byte() == 30 then
		if not JESUSFLY then
			ECOMMAND('jesusfly')
		elseif JESUSFLY then
			ECOMMAND('nojfly')
		end
	end
end)

-- / after loaded

function CHECK_FE()
	if not gWorkspace.FilteringEnabled then
		NOTIFY('Filtering is disabled', 0, 255, 0)
	elseif gWorkspace.FilteringEnabled then
		NOTIFY('Filtering is ENABLED', 255, 0, 0)
	end
end

function updateCMDs(searchedCMD)
	local found_cmds = DATA.found_cmds
	if FIND_CHILD(gCoreGui, 'cmds_seth') then
		local cmds_seth = gCoreGui.cmds_seth
		for i,v in pairs(cmds_seth.MAIN.CMDs:GetChildren()) do
			v:remove()
		end
		for i,v in pairs(found_cmds:GetChildren()) do
			v:remove()
		end
		wait()
		for i,v in pairs(COMMANDS) do
			if string.match(v.D, string.lower(searchedCMD)) then
				local F = Instance.new('StringValue', found_cmds); F.Name = ''; F.Value = v.D
			end
		end
		wait()
		local YSize = 25
		for i,v in pairs(found_cmds:GetChildren()) do
			local POS = ((i * YSize) - YSize)
			local cloneEX = cmds_seth.MAIN.Example:Clone()
			cloneEX.Parent = cmds_seth.MAIN.CMDs
			cloneEX.Visible = true
			cloneEX.Position = UDim2.new(0, 5, 0, POS + 5)
			cloneEX.Text = ' - ' .. v.Value
			cmds_seth.MAIN.CMDs.CanvasSize = UDim2.new(0, 0, 0, POS + 30)
		end
	end
end

function commands()
	if FIND_CHILD(gCoreGui, 'cmds_seth') then
		gCoreGui.cmds_seth:remove()
	end
	local cloneCMDs = DATA.guis.cmds_seth:Clone()
	local searchCMDs = cloneCMDs.MAIN.Search
	cloneCMDs.MAIN.Header.Text = ' ' .. #COMMANDS .. ' commands'
	cloneCMDs.Parent = gCoreGui
	cloneCMDs.MAIN.Exit.MouseButton1Down:connect(function()
		cloneCMDs:remove()
	end)
	cloneCMDs.MAIN.MM.MouseButton1Down:connect(function()
		if cloneCMDs.MAIN.CMDs.Visible then
			cloneCMDs.MAIN.CMDs.Visible = false
		elseif not cloneCMDs.MAIN.CMDs.Visible then
			cloneCMDs.MAIN.CMDs.Visible = true
		end
	end)
	local function displayCMDs()
		for i,v in pairs(COMMANDS) do
			local YSize = 25
			local POS = ((i * YSize) - YSize)
			local cloneEX = cloneCMDs.MAIN.Example:Clone()
			cloneEX.Parent = cloneCMDs.MAIN.CMDs
			cloneEX.Visible = true
			cloneEX.Position = UDim2.new(0, 5, 0, POS + 5)
			cloneEX.Text = ' - ' .. v.D
			cloneCMDs.MAIN.CMDs.CanvasSize = UDim2.new(0, 0, 0, POS + 30)
		end
	end
	displayCMDs()
	searchCMDs.FocusLost:connect(function()
		if searchCMDs.Text then
			updateCMDs(searchCMDs.Text)
			searchCMDs.Text = ' search commands'
		end
	end)
end

local canCheck = true

function checkChangelog()
	if canCheck then
		canCheck = false
		local changelogClone = DATA.guis.changelog_seth:Clone()
		changelogClone.MAIN.changelog.Text = CHANGELOG
		changelogClone.Parent = gCoreGui
		wait()
		changelogClone.MAIN:TweenPosition(UDim2.new(1, -410, 1, -210), 'InOut', 'Quad', 0.5, false)
		wait(5)
		changelogClone.MAIN:TweenPosition(UDim2.new(1, -410, 1, 0), 'InOut', 'Quad', 0.5, false)
		wait(1)
		changelogClone:remove()
		canCheck = true
	end
end

spawn(function()
	checkChangelog()
end)

CMDbar:TweenPosition(UDim2.new(0, 0, 1, -50), 'InOut', 'Quad', 0.5, true)
CMDbar.Parent['']:TweenPosition(UDim2.new(0, 0, 1, -30), 'InOut', 'Quad', 0.5, true)

CMDbar.FocusLost:connect(function(enterpressed)
	if enterpressed and CMDbar.Text ~= '' then
		pcall(function()
			ECOMMAND(CMDbar.Text, LP)
		end)
	end
	CMDbar:TweenPosition(UDim2.new(0, -200, 1, -50), 'InOut', 'Quad', 0.5, true)
end)

MOUSE.KeyDown:connect(function(Key)
	if Key:byte() == 59 then
		CMDbar:TweenPosition(UDim2.new(0, 0, 1, -50), 'InOut', 'Quad', 0.5, true)
		CMDbar:CaptureFocus()
	end
end)

-- / loader

wait()

LOAD_SETH()

wait(3)
game:GetService("CoreGui")["loader_seth"]:remove()
