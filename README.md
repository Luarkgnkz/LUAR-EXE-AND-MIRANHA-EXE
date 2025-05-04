-- Carregar Rayfield
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

-- Criar Janela Principal
local Window = Rayfield:CreateWindow({
    Name = "Luar.Exe V2",
    LoadingTitle = "Assombra Chorão👻",
    LoadingSubtitle = "By. Luar.Exe and Miranha.Exe",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "Luar.Exe V2",
        FileName = "Config"
    },
    KeySystem = false,
})

-- 1. Aba Info
local InfoTab = Window:CreateTab("Info", 4483362458)
InfoTab:CreateParagraph({Title = "Chorou Pro Luar.Exe e Pro Miranha", Content = "Será???"})
InfoTab:CreateButton({
    Name = "Rejoin (Reentrar no servidor)",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game.Players.LocalPlayer)
    end
})

-- 2. Aba PVP
local PVPTab = Window:CreateTab("PVP", 4483362458)
local noclipEnabled, noclipConn = false, nil
PVPTab:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Callback = function(state)
        noclipEnabled = state
        if state then
            noclipConn = game:GetService("RunService").Stepped:Connect(function()
                local char = game.Players.LocalPlayer.Character
                if char then
                    for _, part in pairs(char:GetDescendants()) do
                        if part:IsA("BasePart") then part.CanCollide = false end
                    end
                end
            end)
        else
            if noclipConn then noclipConn:Disconnect() end
        end
    end
})
local originalWalkSpeed = 16
PVPTab:CreateToggle({
    Name = "Speed (Corrida Rápida)",
    CurrentValue = false,
    Callback = function(state)
        local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = state and 50 or originalWalkSpeed end
    end
})
PVPTab:CreateToggle({
    Name = "Anti-Queda",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            hum.StateChanged:Connect(function(old, new)
                if new == Enum.HumanoidStateType.Freefall then
                    hum:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end)
        end
    end
})

-- 3. Aba Farms
local FarmsTab = Window:CreateTab("Farms", 4483362458)
FarmsTab:CreateButton({
    Name = "Auto-Farm Lixos da Cidade",
    Callback = function()
        loadstring(game:HttpGet("https://pastefy.app/AADELC1Z/raw"))()
    end
})
FarmsTab:CreateButton({
    Name = "Auto-Farm Planta Suja",
    Callback = function()
        loadstring(game:HttpGet("https://pastefy.app/JN93Q78D/raw"))()
    end
})

-- 4. Aba Aimbot
local AimbotTab = Window:CreateTab("Aimbot", 4483362458)
AimbotTab:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end
})

-- 5. Aba Visuais
local VisualTab = Window:CreateTab("Visuais", 4483362458)
VisualTab:CreateToggle({
    Name = "ESP (pessoa + item)",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local Players = game:GetService("Players")
            local RunService = game:GetService("RunService")
            local function createESP(player)
                if player == Players.LocalPlayer then return end
                RunService.RenderStepped:Connect(function()
                    local char = player.Character
                    if char and char:FindFirstChild("Head") then
                        if not char:FindFirstChild("Highlight") then
                            local h = Instance.new("Highlight", char)
                            h.FillColor = Color3.fromRGB(255, 255, 0)
                            h.OutlineColor = Color3.fromRGB(255, 255, 255)
                        end
                    end
                end)
            end
            for _, plr in ipairs(Players:GetPlayers()) do createESP(plr) end
            Players.PlayerAdded:Connect(createESP)
        end
    end
})
VisualTab:CreateToggle({
    Name = "Dupe (Duplicar itens)",
    CurrentValue = false,
    Callback = function(state)
        if state then
            loadstring(game:HttpGet("https://dpaste.com/6XGGZGEXG.txt"))()
        end
    end
})
VisualTab:CreateButton({
    Name = "Infinite Yield",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end
})
VisualTab:CreateButton({
    Name = "God Mode",
    Callback = function()
        loadstring(game:HttpGet("https://pastebin.com/raw/V6wBhkfz"))()
    end
})
VisualTab:CreateButton({
    Name = "Anti-Staff",
    Callback = function()
        loadstring(game:HttpGet("https://pastefy.app/EU4G65R4/raw"))()
    end
})

-- 6. Aba Players
local PlayersTab = Window:CreateTab("Players", 4483362458)
PlayersTab:CreateButton({
    Name = "Bring All",
    Callback = function()
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Character and v ~= game.Players.LocalPlayer then
                v.Character:MoveTo(game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
            end
        end
    end
})

-- 7. Aba Trolls
local TrollsTab = Window:CreateTab("Trolls", 4483362458)
TrollsTab:CreateButton({
    Name = "Fake Lag",
    Callback = function()
        while true do
            task.wait(0.1)
            game:GetService("RunService").Stepped:Wait()
        end
    end
})
TrollsTab:CreateInput({
    Name = "Freezer (congelar jogador por nome)",
    PlaceholderText = "NomeDoJogador",
    RemoveTextAfterFocusLost = true,
    Callback = function(nick)
        local target = game.Players:FindFirstChild(nick)
        if target and target.Character then
            for _, part in pairs(target.Character:GetDescendants()) do
                if part:IsA("BasePart") then part.Anchored = true end
            end
        end
    end
})

-- Sistema Anti-Kick, Anti-Exploit, Anti-Ban, Anti-Blacklist
pcall(function()
    hookmetamethod(game, "__namecall", function(self, ...)
        local method = getnamecallmethod()
        if tostring(self) == "Kick" or method == "Kick" then
            return
        end
        return self(...)
    end)
end)

-- Proteção Anti-Blacklist/Exploit Extra
game.Players.LocalPlayer.CharacterAdded:Connect(function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("RemoteEvent") and v.Name:lower():find("ban") then
            v:Destroy()
        end
    end
end)
