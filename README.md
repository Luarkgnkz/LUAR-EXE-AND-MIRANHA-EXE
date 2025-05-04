-- Carregar Rayfield
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

-- Anti-Kick, Anti-Exploit, Anti-Blacklist, Anti-Ban
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "Kick" or method == "kick" then
        return
    end
    return old(self, ...)
end)
hookfunction(Instance.new("RemoteEvent").FireServer, newcclosure(function(...) return end))
hookfunction(Instance.new("RemoteFunction").InvokeServer, newcclosure(function(...) return end))

-- Criar Janela
local Window = Rayfield:CreateWindow({
    Name = "Luar.Exe V2",
    LoadingTitle = "Assombra Chorão👻",
    LoadingSubtitle = "By. Luar.Exe and Miranha.Exe",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "Luar.Exe V2",
        FileName = "Config"
    }
})

-- Aba Info
local Info = Window:CreateTab("Info", 4483362458)
Info:CreateParagraph({Title = "Chorou Pro Luar.Exe e Pro Miranha", Content = "Será???"})
Info:CreateButton({
    Name = "Rejoin (PC/Mobile)",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game.Players.LocalPlayer)
    end
})

-- Aba PVP
local PVP = Window:CreateTab("PVP", 4483362458)
PVP:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Callback = function(state)
        if state then
            noclip = game:GetService("RunService").Stepped:Connect(function()
                for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end)
        else
            if noclip then noclip:Disconnect() end
        end
    end
})
PVP:CreateToggle({
    Name = "Speed",
    CurrentValue = false,
    Callback = function(state)
        local hum = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = state and 50 or 16 end
    end
})
PVP:CreateToggle({
    Name = "Anti-Queda",
    CurrentValue = false,
    Callback = function(state)
        local hum = game.Players.LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid")
        if hum and state then
            hum.StateChanged:Connect(function(_, new)
                if new == Enum.HumanoidStateType.Freefall then
                    hum:ChangeState(Enum.HumanoidStateType.GettingUp)
                end
            end)
        end
    end
})

-- Aba Visual
local Visual = Window:CreateTab("Visual", 4483362458)
Visual:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local function espPlayer(plr)
                if plr ~= game.Players.LocalPlayer then
                    plr.CharacterAdded:Connect(function(char)
                        wait(1)
                        local hl = Instance.new("Highlight", char)
                        hl.FillColor = Color3.fromRGB(255, 255, 0)
                        hl.OutlineColor = Color3.fromRGB(255, 255, 255)
                    end)
                    if plr.Character then
                        local hl = Instance.new("Highlight", plr.Character)
                        hl.FillColor = Color3.fromRGB(255, 255, 0)
                        hl.OutlineColor = Color3.fromRGB(255, 255, 255)
                    end
                end
            end
            for _, p in pairs(game.Players:GetPlayers()) do espPlayer(p) end
            game.Players.PlayerAdded:Connect(espPlayer)
        else
            for _, p in pairs(game.Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChildOfClass("Highlight") then
                    p.Character:FindFirstChildOfClass("Highlight"):Destroy()
                end
            end
        end
    end
})
Visual:CreateToggle({
    Name = "Dupe Itens",
    CurrentValue = false,
    Callback = function(state)
        if state then
            for _, item in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
                local clone = item:Clone()
                clone.Parent = game.Players.LocalPlayer.Backpack
            end
        end
    end
})

-- Aba Aimbot
local Aimbot = Window:CreateTab("Aimbot", 4483362458)
Aimbot:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end
})

-- Aba Trolls
local Trolls = Window:CreateTab("Trolls", 4483362458)
Trolls:CreateButton({
    Name = "Fake Lag",
    Callback = function()
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Velocity = Vector3.new(99999, 99999, 99999)
            end
        end
    end
})

-- Aba Players
local Players = Window:CreateTab("Players", 4483362458)
Players:CreateButton({
    Name = "Bring All",
    Callback = function()
        for _, p in pairs(game.Players:GetPlayers()) do
            if p.Character and p ~= game.Players.LocalPlayer then
                p.Character:MoveTo(game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
            end
        end
    end
})
Players:CreateButton({
    Name = "Mandar Revistar (100 mensagens)",
    Callback = function()
        for i = 1, 100 do
            game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Revistar!", "All")
        end
    end
})

-- Aba Farms
local Farms = Window:CreateTab("Farms", 4483362458)
Farms:CreateButton({
    Name = "Farm Lixo da Cidade",
    Callback = function()
        while task.wait() do
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "Lixo" and v:IsA("Part") then
                    firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v, 0)
                    wait()
                    firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v, 1)
                end
            end
        end
    end
})
Farms:CreateButton({
    Name = "Farm Planta Suja",
    Callback = function()
        while task.wait() do
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "Planta" and v:IsA("Part") then
                    firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v, 0)
                    wait()
                    firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v, 1)
                end
            end
        end
    end
})
