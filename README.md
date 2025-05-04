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
    Discord = {
        Enabled = false,
        Invite = "",
        RememberJoins = true
    },
    KeySystem = false, 
})

-- 1. Aba Info
local InfoTab = Window:CreateTab("Info", 4483362458)
InfoTab:CreateParagraph({
    Title = "Chorou Pro Luar.Exe e Pro Miranha",
    Content = "Será???"
})

-- Botão Rejoin (Reentrar no servidor)
InfoTab:CreateButton({
    Name = "Rejoin (Reentrar no servidor)",
    Callback = function()
        local TeleportService = game:GetService("TeleportService")
        local Players = game:GetService("Players")
        TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, Players.LocalPlayer)
    end
})

-- 2. Aba PVP
local PVP_T = Window:CreateTab("PVP", 4483362458)

-- No-Clip funcional
local noclipConnection
local noclipEnabled = false

PVP_T:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Flag = "NoClipToggle",
    Callback = function(state)
        noclipEnabled = state
        if noclipEnabled then
            noclipConnection = game:GetService("RunService").Stepped:Connect(function()
                local char = game.Players.LocalPlayer.Character
                if char and char:FindFirstChildOfClass("Humanoid") then
                    char:FindFirstChildOfClass("Humanoid"):ChangeState(11)
                    for _, part in pairs(char:GetDescendants()) do
                        if part:IsA("BasePart") and part.CanCollide == true then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        else
            if noclipConnection then noclipConnection:Disconnect() end
            local char = game.Players.LocalPlayer.Character
            if char then
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end,
})

-- Speed funcional
local originalWalkSpeed = 16
local speedEnabled = false

PVP_T:CreateToggle({
    Name = "Speed (Corrida Rápida)",
    CurrentValue = false,
    Flag = "SpeedToggle",
    Callback = function(state)
        speedEnabled = state
        local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            if speedEnabled then
                originalWalkSpeed = humanoid.WalkSpeed
                humanoid.WalkSpeed = 50
            else
                humanoid.WalkSpeed = originalWalkSpeed
            end
        end
    end,
})

-- Anti-dano de queda
local fallDamageEnabled = false

PVP_T:CreateToggle({
    Name = "Anti-Queda",
    CurrentValue = false,
    Flag = "AntiFallToggle",
    Callback = function(state)
        fallDamageEnabled = state
        if fallDamageEnabled then
            local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:GetPropertyChangedSignal("FloorMaterial"):Connect(function()
                    if humanoid.FloorMaterial == Enum.Material.Air then
                        humanoid:ChangeState(Enum.HumanoidStateType.Physics)
                    end
                end)
            end
        end
    end,
})

-- 3. Aba Aimbot
local AimbotTab = Window:CreateTab("Aimbot", 4483362458)

AimbotTab:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end,
})

-- 4. Aba Visual
local VisualTab = Window:CreateTab("Visual", 4483362458)

-- Adicionando o ESP (PESSOA + ITEM EQUIPADO)
local espEnabled = false
local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local runService = game:GetService("RunService")

local function espPlayer(player)
    if player == localPlayer then return end

    local function createESP(item)
        if not item:FindFirstChild("ESP_HIGHLIGHT") then
            local highlight = Instance.new("Highlight")
            highlight.Name = "ESP_HIGHLIGHT"
            highlight.FillColor = Color3.fromRGB(255, 255, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.FillTransparency = 0.2
            highlight.OutlineTransparency = 0
            highlight.Adornee = item
            highlight.Parent = item
        end
    end

    local function createItemESP()
        local billboard = Instance.new("BillboardGui")
        billboard.Name = "ESP_ITEM"
        billboard.Size = UDim2.new(0, 150, 0, 25)
        billboard.StudsOffset = Vector3.new(0, 3, 0)
        billboard.AlwaysOnTop = true

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, 0, 1, 0)
        label.BackgroundTransparency = 1
        label.TextColor3 = Color3.new(1, 1, 1)
        label.TextStrokeTransparency = 0
        label.TextScaled = true
        label.Font = Enum.Font.SourceSansBold
        label.Name = "TextLabel"
        label.Parent = billboard
        return billboard, label
    end

    local billboard, label = createItemESP()

    runService.RenderStepped:Connect(function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character:FindFirstChild("Head") then
            createESP(player.Character)
            local tool = player.Character:FindFirstChildOfClass("Tool") or player.Backpack:FindFirstChildOfClass("Tool")
            local toolName = tool and tool.Name or ""

            if toolName ~= "" then
                label.Text = "[" .. toolName .. "]"
                if not player.Character.Head:FindFirstChild("ESP_ITEM") then
                    local clone = billboard:Clone()
                    clone.TextLabel.Text = "[" .. toolName .. "]"
                    clone.Parent = player.Character.Head
                else
                    player.Character.Head.ESP_ITEM.TextLabel.Text = "[" .. toolName .. "]"
                end
            else
                if player.Character.Head:FindFirstChild("ESP_ITEM") then
                    player.Character.Head.ESP_ITEM.TextLabel.Text = ""
                end
            end
        end
    end)
end

VisualTab:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Flag = "ESP_Toggle",
    Callback = function(state)
        espEnabled = state
        if espEnabled then
            for _, player in pairs(players:GetPlayers()) do
                espPlayer(player)
            end
            players.PlayerAdded:Connect(function(player)
                player.CharacterAdded:Connect(function()
                    wait(1)
                    espPlayer(player)
                end)
            end)
        end
    end
})

-- 5. Aba Trolls
local TrollsTab = Window:CreateTab("Trolls", 4483362458)

TrollsTab:CreateButton({
    Name = "Fake Lag",
    Callback = function()
        -- Função de Fake Lag (ligar/desligar)
    end,
})

-- 6. Aba Players
local PlayersTab = Window:CreateTab("Players", 4483362458)

PlayersTab:CreateButton({
    Name = "Bring All",
    Callback = function()
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character then
                player.Character:MoveTo(game.Players.LocalPlayer.Character.HumanoidRootPart.Position)
            end
        end
    end
})

-- 7. Proteções Anti-Kick, Anti-Exploit e Anti-Blacklist
local function antiKick()
    game:GetService("Players").PlayerAdded:Connect(function(player)
        player.OnTeleport = function(state)
            if state == Enum.TeleportState.Started then
                game:GetService("TeleportService"):Teleport(game.PlaceId)
            end
        end
    end)
end

local function antiExploit()
    -- Proteção anti-exploit
end

local function antiBlacklist()
    -- Proteção anti-blacklist
end

-- Iniciando as proteções
antiKick()
antiExploit()
antiBlacklist()
