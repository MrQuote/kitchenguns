local plr = game.Players.LocalPlayer
local char = plr.Character
local mouse = plr:GetMouse()
local torso = char.Torso
local rs = torso["Right Shoulder"]
local ls = torso["Left Shoulder"]
local rh = torso["Right Hip"]
local lh = torso["Left Hip"]
local rj = char.HumanoidRootPart.RootJoint
local neck = torso.Neck
local animpose = "Idle"
local attacking = false
local cananim = true
local rage = false
local shield
local sprint = false
local canrage = true
local legs = true
local bc = char:WaitForChild("Body Colors")
local multiplier = 1
local lac = char["Body Colors"].LeftArmColor
local rac = char["Body Colors"].RightArmColor
local rlc = char["Body Colors"].RightArmColor
local llc = char["Body Colors"].LeftLegColor
local hc = char["Body Colors"].HeadColor
local tc = char["Body Colors"].TorsoColor
local humanoid = char:FindFirstChildOfClass("Humanoid")
local huge = Vector3.new(math.huge, math.huge, math.huge)
local mobs = Instance.new("Sound", char)
mobs.SoundId = "rbxassetid://245913129"
mobs.Looped = true
mobs.Volume = 3
humanoid.MaxHealth = math.huge
wait()
humanoid.Health = math.huge
humanoid.Name = "BOOM BOOM BOOOMMMM!"
mobs:Play()
if char:FindFirstChild("Animate") then
  char.Animate:Destroy()
end
if char:FindFirstChildOfClass("Humanoid"):FindFirstChild("Animator") then
  char:FindFirstChildOfClass("Humanoid").Animator:Destroy()
end
function legsonly()
  spawn(function()
    for i = 0, 10 do
      wait(0.001)
      if attacking then
        break
      end
    end
    if not attacking then
      legs = false
    end
  end)
end
function swait(t)
  if t == nil or t == 0 then
    game:service("RunService").Stepped:wait(0)
  else
    for i = 0, t do
      game:service("RunService").Stepped:wait(0)
    end
  end
end
function KICK(PLAYER)
  spawn(function()
    local function SKICK()
      if PLAYER.Character and PLAYER.Character:FindFirstChild("HumanoidRootPart") and PLAYER.Character:FindFirstChild("Torso") then
        PLAYER.Character.HumanoidRootPart.CFrame = CFrame.new(math.random(999000, 1001000), 1000000, 1000000)
        do
          local SP = Instance.new("SkateboardPlatform", PLAYER.Character)
          SP.Position = PLAYER.Character.HumanoidRootPart.Position
          SP.Transparency = 1
          spawn(function()
            repeat
              swait()
              if PLAYER.Character and PLAYER.Character:FindFirstChild("HumanoidRootPart") then
                SP.Position = PLAYER.Character.HumanoidRootPart.Position
              end
            until not game:GetService("Players"):FindFirstChild(PLAYER.Name)
          end)
          PLAYER.Character.Torso.Anchored = true
        end
      end
    end
    spawn(function()
      repeat
        wait()
        if PLAYER ~= nil then
          SKICK()
        end
      until not game:GetService("Players"):FindFirstChild(PLAYER.Name)
      if not game:GetService("Players"):FindFirstChild(PLAYER.Name) then
        print("REMOVED " .. PLAYER.Name)
      end
    end)
  end)
end
function hurt(hit, dmg)
  if hit.Parent then
    if hit.Parent:IsA("LocalScript") then
      print("bocks!11")
      hit.Parent:Destroy()
    end
    local hum = hit.Parent:FindFirstChildOfClass("Humanoid")
    if hum and hum.Parent.Name ~= plr.Name then
      if dmg == "Kill" or hum.Health > 100000 then
        hit.Parent:BreakJoints()
        return true
      else
        if math.random(0, 100) == 50 then
          hum.Health = hum.Health - dmg * multiplier * 2.5
        else
          hum.Health = hum.Health - dmg * multiplier
        end
        return true
      end
    end
  end
end
function soundeffect(id, volume, speed, parent)
  spawn(function()
    local s = Instance.new("Sound")
    s.SoundId = id
    s.Volume = volume
    s.PlaybackSpeed = speed
    s.Parent = parent
    s:Play()
    repeat
      wait()
    until not s.Playing
    s:Destroy()
  end)
end
function gethum(obj)
  if obj.Parent and obj.Parent:FindFirstChild("Humanoid") and obj.Parent.Name ~= plr.Name then
    return obj.Parent:FindFirstChildOfClass("Humanoid")
  end
end
function smooth(obj)
  local sides = {
    "Left",
    "Right",
    "Top",
    "Bottom",
    "Front",
    "Back"
  }
  for i, v in pairs(sides) do
    obj[v .. "Surface"] = "SmoothNoOutlines"
  end
end
function fade(obj, dest, grow)
  spawn(function()
    local oldcf = obj.CFrame
    for i = 0, 10 do
      if grow then
        obj.Size = obj.Size + Vector3.new(1, 1, 1)
        obj.CFrame = oldcf
      end
      obj.Transparency = obj.Transparency + 0.1
      swait()
    end
    if dest then
      obj:Destroy()
    end
  end)
end
local keyamount = 0
mouse.KeyDown:connect(function(key)
  if key == "w" or key == "a" or key == "s" or key == "d" then
    keyamount = keyamount + 1
    if animpose ~= "Falling" then
      if keyamount > 3 then
        keyamount = 0
      end
      animpose = "Walking"
    end
  end
end)
mouse.KeyUp:connect(function(key)
  if key == "w" or key == "a" or key == "s" or key == "d" then
    keyamount = keyamount - 1
    if keyamount < 0 then
      keyamount = 0
    end
    if keyamount == 0 then
      animpose = "Idle"
    end
  end
end)
local gun = Instance.new("Part")
gun.Size = Vector3.new(3.175, 1.916, 0.465)
gun.CanCollide = false
local m = Instance.new("SpecialMesh", gun)
m.MeshId = "rbxassetid://468351345"
m.TextureId = "rbxassetid://468351348"
m.Scale = Vector3.new(0.1, 0.1, 0.1)
gun.CFrame = char.Torso.CFrame
gun.Parent = char
local gunw = Instance.new("Weld", gun)
gunw.Part0 = gun
gunw.Part1 = char["Right Arm"]
gunw.C0 = CFrame.new(-1.7838248, -0.410839319, 0, -0.0871557146, -0.996194541, 0, 0.996194541, -0.0871557146, 0, 0, 0, 1)
mouse.Button1Down:connect(function()
  local cf = gun.CFrame * CFrame.new(0, 0, 0.958)
  local mag = (gun.Position - mouse.Hit.p).magnitude
  local p = Instance.new("Part")
  p.CanCollide = false
  p.Anchored = false
  p.BrickColor = BrickColor.new("Institutional white")
  p.Size = Vector3.new(0.2, 0.2, mag)
  smooth(p)
  p.Material = "Neon"
  p.CFrame = CFrame.new(gun.Position, mouse.Hit.p) * CFrame.new(0, 0, -mag / 2)
  local m = Instance.new("SpecialMesh", p)
  m.MeshType = "Brick"
  p.Parent = workspace
  p.Touched:connect(function(hit)
    hurt(hit, "Kill")
    if hit.Size.X > 100 and 100 < hit.Size.Z and hit.Size.Y < 3 then
    elseif hit.Parent and hit.Parent.Name ~= plr.Name then
      fade(hit, true)
    end
  end)
  local bp = Instance.new("BodyPosition", p)
  bp.MaxForce = huge
  bp.Position = p.Position
  local saved = p.CFrame
  for i = 1, 10 do
    p.Size = p.Size + Vector3.new(0.01, 0.01, 0.01)
    p.CFrame = saved
    p.Velocity = Vector3.new(0, 0, 100)
    p.Transparency = p.Transparency + 0.1
    wait()
  end
  p:Destroy()
end)
mouse.KeyDown:connect(function(key)
  if key == "l" then
    function a(b)
      pcall(function()
        for i, v in pairs(b:children()) do
          pcall(function()
            if v:IsA("BasePart") and v.Parent and v.Parent.Name == "WafflesAreVeryGood" and v.Anchored then
              v.Anchored = false
            end
            if v:IsA("Sound") and v.Parent.Name ~= "WafflesAreVeryGood" then
              v:Destroy()
            end
            if v:IsA("ParticleEmitter") then
              v:Destroy()
            end
            a(v)
          end)
        end
      end)
    end
    a(game)
  end
  if key == "q" then
    local cf = gun.CFrame * CFrame.new(0, 0, 0.958)
    local mag = (gun.Position - mouse.Hit.p).magnitude
    local p = Instance.new("Part")
    p.CanCollide = false
    p.Anchored = false
    p.BrickColor = BrickColor.new("Really red")
    p.Size = Vector3.new(0.2, 0.2, mag)
    smooth(p)
    p.Material = "Neon"
    p.CFrame = CFrame.new(gun.Position, mouse.Hit.p) * CFrame.new(0, 0, -mag / 2)
    local m = Instance.new("SpecialMesh", p)
    m.MeshType = "Brick"
    p.Parent = workspace
    p.Touched:connect(function(hit)
      if gethum(hit) then
        for i, v in pairs(hit.Parent:children()) do
          if v:IsA("Model") then
            v:BreakJoints()
          end
          local ok = false
          for i, e in pairs({
            "Right Arm",
            "Left Arm",
            "Right Leg",
            "Left Leg",
            "Head",
            "Torso",
            "HumanoidRootPart"
          }) do
            if v.Name == e then
              ok = true
            end
          end
          if v:IsA("BasePart") and not ok then
            fade(v, true)
          end
        end
      end
      if hit:FindFirstChildOfClass("TouchTransmitter") then
        hit:FindFirstChildOfClass("TouchTransmitter"):Destroy()
      end
    end)
    local bp = Instance.new("BodyPosition", p)
    bp.MaxForce = huge
    bp.Position = p.Position
    local saved = p.CFrame
    for i = 1, 10 do
      p.Size = p.Size + Vector3.new(0.01, 0.01, 0.01)
      p.CFrame = saved
      p.Velocity = Vector3.new(0, 0, 100)
      p.Transparency = p.Transparency + 0.1
      wait()
    end
    p:Destroy()
  end
  if key == "e" then
    local cf = gun.CFrame * CFrame.new(0, 0, 0.958)
    local mag = (gun.Position - mouse.Hit.p).magnitude
    local p = Instance.new("Part")
    p.CanCollide = false
    p.Anchored = false
    p.BrickColor = BrickColor.new("New Yeller")
    p.Size = Vector3.new(0.2, 0.2, mag)
    smooth(p)
    p.Material = "Neon"
    p.CFrame = CFrame.new(gun.Position, mouse.Hit.p) * CFrame.new(0, 0, -mag / 2)
    local m = Instance.new("SpecialMesh", p)
    m.MeshType = "Brick"
    p.Parent = workspace
    p.Touched:connect(function(hit)
      if gethum(hit) then
        local target = game.Players:FindFirstChild(hit.Parent.Name)
        if target then
          KICK(target)
        end
      end
    end)
    local bp = Instance.new("BodyPosition", p)
    bp.MaxForce = huge
    bp.Position = p.Position
    local saved = p.CFrame
    for i = 1, 10 do
      p.Size = p.Size + Vector3.new(0.01, 0.01, 0.01)
      p.CFrame = saved
      p.Velocity = Vector3.new(0, 0, 100)
      p.Transparency = p.Transparency + 0.1
      wait()
    end
    p:Destroy()
  end
  if key == "r" then
    local cf = gun.CFrame * CFrame.new(0, 0, 0.958)
    local mag = (gun.Position - mouse.Hit.p).magnitude
    local p = Instance.new("Part")
    p.CanCollide = false
    p.Anchored = false
    p.BrickColor = BrickColor.new("Lime green")
    p.Size = Vector3.new(0.2, 0.2, mag)
    smooth(p)
    p.Material = "Neon"
    p.CFrame = CFrame.new(gun.Position, mouse.Hit.p) * CFrame.new(0, 0, -mag / 2)
    local m = Instance.new("SpecialMesh", p)
    m.MeshType = "Brick"
    p.Parent = workspace
    p.Touched:connect(function(hit)
      local hum = gethum(hit)
      if hum then
        hum.Health = hum.MaxHealth
      end
    end)
    local bp = Instance.new("BodyPosition", p)
    bp.MaxForce = huge
    bp.Position = p.Position
    local saved = p.CFrame
    for i = 1, 10 do
      p.Size = p.Size + Vector3.new(0.01, 0.01, 0.01)
      p.CFrame = saved
      p.Velocity = Vector3.new(0, 0, 100)
      p.Transparency = p.Transparency + 0.1
      wait()
    end
    p:Destroy()
  end
end)
game:GetService("RunService").Stepped:connect(function()
  local x, z
  local dir = CFrame.new(char.HumanoidRootPart.Position, mouse.Hit.p).lookVector
  x = dir.X
  z = dir.Z
  local cf = CFrame.new(char.HumanoidRootPart.Position, char.HumanoidRootPart.Position + Vector3.new(x, 0, z))
  char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame:Lerp(cf, 0.5)
  humanoid.AutoRotate = false
end)
while wait() do
  if animpose == "Walking" and cananim and legs then
    for i = 0, 0.7, 0.1 do
      if animpose == "Walking" and cananim and legs then
        ls.C0 = ls.C0:Lerp(CFrame.new(-1, 0.5, 0, 0, 0.104528472, -0.994522035, 0, 0.994522035, 0.104528472, 1, 0, 0), 0.2)
        rs.C0 = rs.C0:Lerp(CFrame.new(1.54167628, 0.0454798974, 0, -0.482965499, -0.871292651, -0.087155737, -0.0422539636, -0.0762281716, 0.996195912, -0.874620378, 0.484809875, 0), 0.2)
        neck.C0 = neck.C0:Lerp(CFrame.new(0, 1, 0, -0.500000656, -0.866026223, 0, -1.61309954E-9, 9.31323796E-10, 1.00000024, -0.866026342, 0.500000715, -1.86264515E-9), 0.2)
        rj.C0 = rj.C0:Lerp(CFrame.new(0, 0, 0, -0.469472021, 0.882948279, 0, 0, 0, 1, 0.882948279, 0.469472021, 0), 0.2)
        lh.C0 = lh.C0:Lerp(CFrame.new(-1, -1, 0, -0.0219629817, 0.02712203, -0.999390841, -0.628937364, 0.776673257, 0.0348994955, 0.777146697, 0.6293208, 0), 0.4)
        rh.C0 = rh.C0:Lerp(CFrame.new(1, -1, 0, 0.0238014236, -0.0255239103, 0.999390841, -0.681583524, 0.73090899, 0.0348994955, -0.731354535, -0.681998909, 0), 0.4)
        wait()
      else
        break
      end
    end
    for i = 0, 0.7, 0.1 do
      if animpose == "Walking" and cananim and legs then
        ls.C0 = ls.C0:Lerp(CFrame.new(-1, 0.5, 0, 0, 0.104528472, -0.994522035, 0, 0.994522035, 0.104528472, 1, 0, 0), 0.2)
        rs.C0 = rs.C0:Lerp(CFrame.new(1.54167628, 0.0454798974, 0, -0.482965499, -0.871292651, -0.087155737, -0.0422539636, -0.0762281716, 0.996195912, -0.874620378, 0.484809875, 0), 0.2)
        neck.C0 = neck.C0:Lerp(CFrame.new(0, 1, 0, -0.500000656, -0.866026223, 0, -1.61309954E-9, 9.31323796E-10, 1.00000024, -0.866026342, 0.500000715, -1.86264515E-9), 0.2)
        rj.C0 = rj.C0:Lerp(CFrame.new(0, 0, 0, -0.469472021, 0.882948279, 0, 0, 0, 1, 0.882948279, 0.469472021, 0), 0.2)
        lh.C0 = lh.C0:Lerp(CFrame.new(-1, -1, 0, 0.0205134545, 0.0282343514, -0.999390841, 0.587428331, 0.808525503, 0.0348994955, 0.809018135, -0.587786257, 0), 0.4)
        rh.C0 = rh.C0:Lerp(CFrame.new(1, -1, 0, -0.0224330258, -0.0267346334, 0.999390841, 0.642397523, 0.765579402, 0.0348994955, -0.76604569, 0.642788768, 0), 0.4)
        wait()
      else
        break
      end
    end
  end
  if animpose == "Idle" and cananim and legs then
    ls.C0 = ls.C0:Lerp(CFrame.new(-1, 0.5, 0, 0, 0.104528472, -0.994522035, 0, 0.994522035, 0.104528472, 1, 0, 0), 0.2)
    rs.C0 = rs.C0:Lerp(CFrame.new(1.54167628, 0.0454798974, 0, -0.482965499, -0.871292651, -0.087155737, -0.0422539636, -0.0762281716, 0.996195912, -0.874620378, 0.484809875, 0), 0.2)
    neck.C0 = neck.C0:Lerp(CFrame.new(0, 1, 0, -0.500000656, -0.866026223, 0, -1.61309954E-9, 9.31323796E-10, 1.00000024, -0.866026342, 0.500000715, -1.86264515E-9), 0.2)
    rj.C0 = rj.C0:Lerp(CFrame.new(0, 0, 0, -0.469472021, 0.882948279, 0, 0, 0, 1, 0.882948279, 0.469472021, 0), 0.2)
    lh.C0 = lh.C0:Lerp(CFrame.new(-1, -1, 0, 0, 0.0523359552, -0.99862957, 0, 0.99862957, 0.0523359552, 1, 0, 0), 0.5)
    rh.C0 = rh.C0:Lerp(CFrame.new(1, -1, 0, 0, -0.0523359627, 0.998629689, 0, 0.998629689, 0.0523359627, -1, 0, 0), 0.5)
  end
end
