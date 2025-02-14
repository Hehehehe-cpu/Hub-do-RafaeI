
local autor = "Rafael"
local versao = "1.0"
local farmMode = "Auto" -- "Auto" ou "Manual"
local fruitToFarm = "Dragon Fruit" -- Nome do fruto que você deseja farmar
local farmArea = Vector3.new(0, 0, 0) -- Posição do local de farm

-- Funções
local function getClosestFruit()
    local closestFruit = nil
    local closestDistance = math.huge
    for _, fruit in pairs(game:GetService("Workspace"):GetDescendants()) do
        if fruit.Name == fruitToFarm and fruit:IsA("Part") then
            local distance = (fruit.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if distance < closestDistance then
                closestFruit = fruit
                closestDistance = distance
            end
        end
    end
    return closestFruit
end

local function farmFruit(fruit)
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = fruit.CFrame
    wait(0.5)
    game:GetService("ReplicatedStorage"):FindFirstChild("FruitPick"):FireServer(fruit)
end

local function checkForEnemies()
    for _, enemy in pairs(game:GetService("Workspace"):GetDescendants()) do
        if enemy:IsA("Model") and enemy.Name ~= "Player" then
            local distance = (enemy.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if distance < 10 then
                game:GetService("ReplicatedStorage"):FindFirstChild("Attack"):FireServer(enemy)
            end
        end
    end
end

-- Loop de farm
while true do
    if farmMode == "Auto" then
        local closestFruit = getClosestFruit()
        if closestFruit then
            farmFruit(closestFruit)
        end
        checkForEnemies()
    elseif farmMode == "Manual" then

    end
    wait(1)
end
`
