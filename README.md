local Players = game:GetService("Players")

-- 🔹 Ativar/Desativar Voo
local function toggleFly(player)
    local character = player.Character
    if not character then return end

    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    local existingVelocity = humanoidRootPart:FindFirstChild("FlyVelocity")

    if existingVelocity then
        existingVelocity:Destroy() -- Desativa o voo
    else
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 50, 0) -- Sobe no ar
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
        bodyVelocity.Name = "FlyVelocity"
        bodyVelocity.Parent = humanoidRootPart
    end
end

-- 🔹 Puxar Jogador para Você
local function pullPlayer(player, targetName)
    local target = Players:FindFirstChild(targetName)
    if target and target.Character then
        local humanoidRootPart = target.Character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            humanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
        end
    end
end

-- 🔹 Puxar Objetos/Vehicles para Você
local function pullObjects(player)
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Model") and obj.PrimaryPart then
            obj:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0))
        end
    end
end

-- 🔹 Matar Todos
local function killAll(player)
    for _, target in pairs(Players:GetPlayers()) do
        if target.Character then
            local humanoid = target.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = 0
            end
        end
    end
end

-- 🔹 Monitorar o Chat para Comandos
Players.PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        local args = string.split(msg, " ")

        if args[1] == "!fly" then
            toggleFly(player)
        elseif args[1] == "!pull" and args[2] then
            pullPlayer(player, args[2])
        elseif args[1] == "!pullall" then
