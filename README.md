local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local function setup(char)
    local humanoidRootPart = char:WaitForChild("HumanoidRootPart")
    local humanoid = char:WaitForChild("Humanoid")
    local lastFakeJump = 0

    -- Anti-ragdoll (opcional)
    RunService.Heartbeat:Connect(function()
        if char:FindFirstChild("Ragdoll") then
            char.Ragdoll:Destroy()
        end
    end)

    -- Detecta agarre y finge "estar en el aire" sin saltar visualmente
    RunService.Heartbeat:Connect(function()
        local now = tick()
        local isGrabbed =
            humanoidRootPart:FindFirstChildWhichIsA("AlignPosition") or
            humanoidRootPart:FindFirstChildWhichIsA("BodyPosition") or
            humanoidRootPart:FindFirstChildWhichIsA("VectorForce") or
            humanoidRootPart:FindFirstChildWhichIsA("BodyMover")

        if isGrabbed and now - lastFakeJump > 0.3 then
            lastFakeJump = now

            -- Forzar estado como si estuvieras en el aire
            humanoid:ChangeState(Enum.HumanoidStateType.Freefall)

            print("🌀 Simulando estar en el aire (sin salto real)")
        end
    end)
end

-- Inicial y para respawn
setup(character)
Players.LocalPlayer.CharacterAdded:Connect(setup)
