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
    Discord = { Enabled = false },
    KeySystem = false
})

-- 1. Info
local Info = Window:CreateTab("Info", 4483362458)
Info:CreateParagraph({ Title = "Chorou Pro Luar.Exe e Pro Miranha", Content = "Será???" })
Info:CreateButton({
    Name = "Rejoin (Reentrar no servidor)",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game.Players.LocalPlayer)
    end
})

-- 2. PVP
local PVP = Window:CreateTab("PVP", 4483362458)

-- No-Clip
local noclip
PVP:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Callback = function(on)
        if on then
            noclip = game:GetService("RunService").Stepped:Connect(function()
                for _,v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end)
        elseif noclip then noclip:Disconnect() end
    end
})

-- Speed
PVP:CreateToggle({
    Name = "Speed (Corrida Rápida)",
    CurrentValue = false,
    Callback = function(on)
        local h = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h.WalkSpeed = on and 50 or 16 end
    end
})

-- Anti-Queda
PVP:CreateToggle({
    Name = "Anti-Queda",
    CurrentValue = false,
    Callback = function(on)
        if on then
            game:GetService("RunService").Stepped:Connect(function()
                local h = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if h and h.FloorMaterial == Enum.Material.Air then
                    h:ChangeState(Enum.HumanoidStateType.Physics)
                end
            end)
        end
    end
})

-- 3. Farms
local Farms = Window:CreateTab("Farms", 4483362458)

Farms:CreateToggle({
    Name = "Auto-Farm Lixos da Cidade",
    CurrentValue = false,
    Callback = function(on)
        _G.LIXO = on
        while _G.LIXO do
            for _,v in pairs(workspace:GetDescendants()) do
                if v.Name == "Lixo" and v:IsA("Part") then
                    firetouchinterest(v, game.Players.LocalPlayer.Character.HumanoidRootPart, 0)
                end
            end
            task.wait(1)
        end
    end
})

Farms:CreateToggle({
    Name = "Auto-Farm Planta Suja",
    CurrentValue = false,
    Callback = function(on)
        _G.PLANTA = on
        while _G.PLANTA do
            for _,v in pairs(workspace:GetDescendants()) do
                if v.Name == "Planta" and v:IsA("Part") then
                    firetouchinterest(v, game.Players.LocalPlayer.Character.HumanoidRootPart, 0)
                end
            end
            task.wait(1)
        end
    end
})

-- 4. Aimbot
local Aimbot = Window:CreateTab("Aimbot", 4483362458)
Aimbot:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end
})

-- 5. Visuais
local Visuais = Window:CreateTab("Visuais", 4483362458)

-- ESP Funcional
Visuais:CreateToggle({
    Name = "ESP (Pessoas + Ferramentas)",
    CurrentValue = false,
    Callback = function(on)
        if on then
            loadstring(game:HttpGet("https://dpaste.com/6XGGZGEXG.txt"))()
        end
    end
})

-- Dupe (ligável/desligável)
Visuais:CreateToggle({
    Name = "Dupe (Duplicar Itens)",
    CurrentValue = false,
    Callback = function(on)
        if on then
            loadstring(game:HttpGet("https://raw.githubusercontent.com/MiranhaExploiter/Miranha/main/Dupe.lua"))()
        end
    end
})

-- 6. Players
local Players = Window:CreateTab("Players", 4483362458)

Players:CreateButton({
    Name = "Bring All",
    Callback = function()
        for _,plr in pairs(game.Players:GetPlayers()) do
            if plr.Character then
                plr.Character:MoveTo(game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
            end
        end
    end
})

-- 7. Trolls
local Trolls = Window:CreateTab("Trolls", 4483362458)

-- Fake Lag
Trolls:CreateButton({
    Name = "Fake Lag",
    Callback = function()
        local rs = game:GetService("RunService")
        rs:Set3dRenderingEnabled(false)
        wait(5)
        rs:Set3dRenderingEnabled(true)
    end
})

-- Freezer por nome
Trolls:CreateInput({
    Name = "Dar Freezer por Nome",
    PlaceholderText = "Digite o nome exato",
    RemoveTextAfterFocusLost = false,
    Callback = function(name)
        local target = game.Players:FindFirstChild(name)
        if target and target.Character then
            target.Character.HumanoidRootPart.Anchored = true
        end
    end
})

-- Sistema de Proteção
local function Protecao()
    local Players = game:GetService("Players")
    local LP = Players.LocalPlayer

    -- Anti-Kick
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local old = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        if method == "Kick" and self == LP then return end
        return old(self, ...)
    end)

    -- Anti-Ban / Anti-Exploit básico
    LP.CharacterAdded:Connect(function(char)
        char:WaitForChild("Humanoid").BreakJointsOnDeath = false
    end)
end

Protecao()
