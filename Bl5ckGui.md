-- Bl5ckGui v2.1 - CORRIGIDO para Delta
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "🎮 Bl5ckGui v2.1",
   LoadingTitle = "Delta Fixed",
   LoadingSubtitle = "Poderes + 5 Scripts",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "Bl5ckGui",
      FileName = "config"
   },
   KeySystem = false
})

-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer

-- Espera character carregar
local function waitCharacter()
   local Character = Player.Character or Player.CharacterAdded:Wait()
   local Humanoid = Character:WaitForChild("Humanoid")
   local RootPart = Character:WaitForChild("HumanoidRootPart")
   return Character, Humanoid, RootPart
end

local Character, Humanoid, RootPart = waitCharacter()

-- Variáveis corrigidas (SEM vírgula múltipla)
local flying = false
local noclip = false
local infJump = false
local flySpeed = 50
local walkSpeed = 16
local jumpPower = 50
local bodyVelocity = nil
local noclipConnection = nil

-- Fly System CORRIGIDO
local function toggleFly(value)
   flying = value
   if value and RootPart then
      if bodyVelocity then bodyVelocity:Destroy() end
      bodyVelocity = Instance.new("BodyVelocity")
      bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
      bodyVelocity.Parent = RootPart
      
      local connection
      connection = RunService.Heartbeat:Connect(function()
         if flying and RootPart and RootPart.Parent then
            local cam = workspace.CurrentCamera
            local moveVector = Vector3.new(0, 0, 0)
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
               moveVector = moveVector + cam.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
               moveVector = moveVector - cam.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
               moveVector = moveVector - cam.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
               moveVector = moveVector + cam.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
               moveVector = moveVector + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
               moveVector = moveVector + Vector3.new(0, -1, 0)
            end
            
            bodyVelocity.Velocity = moveVector * flySpeed
         else
            connection:Disconnect()
         end
      end)
   else
      if bodyVelocity then
         bodyVelocity:Destroy()
         bodyVelocity = nil
      end
   end
end

-- Noclip System
local function toggleNoclip(value)
   noclip = value
   if noclipConnection then
      noclipConnection:Disconnect()
      noclipConnection = nil
   end
   if value then
      noclipConnection = RunService.Stepped:Connect(function()
         for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide ~= false then
               part.CanCollide = false
            end
         end
      end)
   end
end

-- ABA PODERES
local PowersTab = Window:CreateTab("⚡ Poderes", 5813850907)
PowersTab:CreateSection("Movimentação")

PowersTab:CreateToggle({
   Name = "✈️ Fly (WASD + Space/Shift)",
   CurrentValue = false,
   Flag = "FlyToggle",
   Callback = toggleFly
})

PowersTab:CreateToggle({
   Name = "👻 Noclip",
   CurrentValue = false,
   Flag = "NoclipToggle",
   Callback = toggleNoclip
})

PowersTab:CreateToggle({
   Name = "🦘 Infinite Jump",
   CurrentValue = false,
   Flag = "InfJumpToggle",
   Callback = function(Value)
      infJump = Value
   end
})

PowersTab:CreateSection("Sliders")
PowersTab:CreateSlider({
   Name = "Fly Speed",
   Range = {16, 300},
   Increment = 5,
   Suffix = " Speed",
   CurrentValue = 50,
   Flag = "FlySpeedSlider",
   Callback = function(Value)
      flySpeed = Value
   end
})

PowersTab:CreateSlider({
   Name = "Walk Speed",
   Range = {16, 200},
   Increment = 1,
   Suffix = " Speed",
   CurrentValue = 16,
   Flag = "WalkSpeedSlider",
   Callback = function(Value)
      walkSpeed = Value
      if Humanoid then Humanoid.WalkSpeed = Value end
   end
})

PowersTab:CreateSlider({
   Name = "Jump Power",
   Range = {50, 200},
   Increment = 5,
   Suffix = " Power",
   CurrentValue = 50,
   Flag = "JumpPowerSlider",
   Callback = function(Value)
      jumpPower = Value
      if Humanoid then Humanoid.JumpPower = Value end
   end
})

-- ABA SCRIPTS (5 scripts)
local ScriptsTab = Window:CreateTab("📦 Scripts", 5813850907)
ScriptsTab:CreateSection("5 Scripts Premium")

ScriptsTab:CreateButton({
   Name = "🚀 Unfall Script",
   Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/EchoX10/Unfall-Script/refs/heads/main/Unfall.md"))()
      Rayfield:Notify({Title = "Bl5ckGui", Content = "✅ Unfall carregado!", Duration = 3})
   end
})

ScriptsTab:CreateButton({
   Name = "⚔️ 1x4 GUI",
   Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/dragonballkaiiiiii-png/1x4-gui/refs/heads/main/README.mdkk"))()
      Rayfield:Notify({Title = "Bl5ckGui", Content = "✅ 1x4 GUI carregado!", Duration = 3})
   end
})

ScriptsTab:CreateButton({
   Name = "🎵 YouTube Music",
   Callback = function()
      loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-YouTube-Music-Player-72222"))()
      Rayfield:Notify({Title = "Bl5ckGui", Content = "✅ Music Player!", Duration = 3})
   end
})

ScriptsTab:CreateButton({
   Name = "👹 Stigma Rev 0",
   Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/C-Dr1ve/Executor-Remakes-In-Lua/refs/heads/main/Remakes/Stigma_Revision_0.lua"))()
      Rayfield:Notify({Title = "Bl5ckGui", Content = "✅ Stigma carregado!", Duration = 3})
   end
})

ScriptsTab:CreateButton({
   Name = "🔥 Pastefy Script",
   Callback = function()
      loadstring(game:HttpGet("https://pastefy.app/M0r9UCNQ/raw", true))()
      Rayfield:Notify({Title = "Bl5ckGui", Content = "✅ Pastefy carregado!", Duration = 3})
   end
})

-- Infinite Jump Connection
UserInputService.JumpRequest:Connect(function()
   if infJump then
      Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
   end
end)

-- Auto respawn handler
Player.CharacterAdded:Connect(function(newChar)
   Character = newChar
   Humanoid = Character:WaitForChild("Humanoid")
   RootPart = Character:WaitForChild("HumanoidRootPart")
   Humanoid.WalkSpeed = walkSpeed
   Humanoid.JumpPower = jumpPower
end)

Rayfield:LoadConfiguration()
Rayfield:Notify({
   Title = "Bl5ckGui v2.1 FIXED",
   Content = "✅ Erro corrigido! Fly: WASD+Space/Shift",
   Duration = 5
})
