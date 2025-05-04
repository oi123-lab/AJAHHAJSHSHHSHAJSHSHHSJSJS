local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "COMBATE SH V4", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

--[[
Name = <string> - The name of the UI.
HidePremium = <bool> - Whether or not the user details shows Premium status or not.
SaveConfig = <bool> - Toggles the config saving in the UI.
ConfigFolder = <string> - The name of the folder where the configs are saved.
IntroEnabled = <bool> - Whether or not to show the intro animation.
IntroText = <string> - Text to show in the intro animation.
IntroIcon = <string> - URL to the image you want to use in the intro animation.
Icon = <string> - URL to the image you want displayed on the window.
CloseCallback = <function> - Function to execute when the window is closed.
]]



local Tab = Window:MakeTab({
	Name = "COMBATE SH V4",
	Icon = "rbxassetid://10734975692",
	PremiumOnly = false
})


    
    
    local Players = game:GetService("Players")
local playerNames = {}
for _, player in pairs(Players:GetPlayers()) do
    table.insert(playerNames, player.Name)
end

local selectedPlayerName = nil


Tab:AddDropdown({
    Name = "Selecionar Jogador",
    Options = playerNames,
    Callback = function(selected)
        selectedPlayerName = selected
    end
})


Tab:AddButton({
    Name = "Jogar Player pro espaço",
    Callback = function()
        if selectedPlayerName then
            local selectedPlayer = game.Players:FindFirstChild(selectedPlayerName)
            if selectedPlayer then
                local Player = game.Players.LocalPlayer
                local Character = Player.Character
                local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
                local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")
                local Vehicles = game.Workspace:FindFirstChild("Vehicles")
                local OldPos = RootPart and RootPart.CFrame

                if RootPart and Vehicles then
                    local PCar = Vehicles:FindFirstChild(Player.Name.."Car")
                    if not PCar then
                        RootPart.CFrame = CFrame.new(-3, -4, 2105)
                        task.wait(0.5)
                        game:GetService("ReplicatedStorage").RE["1Ca1r"]:FireServer("PickingBoat", "MilitaryBoatFree")
                        task.wait(0.5)
                        PCar = Vehicles:FindFirstChild(Player.Name.."Car")

                        if PCar then
                            local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
                            if Seat then
                                repeat
                                    task.wait()
                                    RootPart.CFrame = Seat.CFrame * CFrame.new(0, 2, 0)
                                until Humanoid and Humanoid.Sit
                            end
                        end
                    end

                    local TargetC = selectedPlayer.Character
                    local TargetH = TargetC and TargetC:FindFirstChildOfClass("Humanoid")
                    local TargetRP = TargetC and TargetC:FindFirstChild("HumanoidRootPart")

                    if PCar and TargetC and TargetH and TargetRP then
                        if not TargetH.Sit then
                            while not TargetH.Sit do
                                task.wait()
                                PCar:SetPrimaryPartCFrame(TargetRP.CFrame * CFrame.new(0, -2, 0))
                            end
                            task.wait(0.1)

                            local AngularVelocity = Instance.new("BodyAngularVelocity")
                            AngularVelocity.AngularVelocity = Vector3.new(99, 99, 99)
                            AngularVelocity.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                            AngularVelocity.Parent = PCar.PrimaryPart

                            task.wait(1)

                            local launchDirection = Vector3.new(math.random(-1, 1), 1, math.random(-1, 1)).unit * 50
                            local BodyVelocity = Instance.new("BodyVelocity")
                            BodyVelocity.Velocity = launchDirection
                            BodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                            BodyVelocity.Parent = PCar.PrimaryPart

                            task.wait(0.5)
                            BodyVelocity:Destroy()
                            AngularVelocity:Destroy()

                            if Humanoid then Humanoid.Sit = false end
                            task.wait(0.1)
                            if RootPart then RootPart.CFrame = OldPos end
                        end
                    end
                end
            else
                print("tu tento pega um jogador que kitou ou que ta bugado")
            end
        else
            print("seleciona um jogador filho da puta animal")
        end
    end
})



Tab:AddButton({
    Name = "CAR FLING V4",
    Callback = function()
             local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChildOfClass("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local Vehicles = game.Workspace:FindFirstChild("Vehicles")
local OldPos = RootPart.CFrame

if not Humanoid or not Vehicles then return end

local function GetCar()
    return Vehicles:FindFirstChild(Player.Name.."Car")
end

local PCar = GetCar()

if not PCar then
    RootPart.CFrame = CFrame.new(1118.81, 75.998, -1138.61)
    task.wait(0.5)
    local RemoteEvent = game:GetService("ReplicatedStorage"):FindFirstChild("RE")
    if RemoteEvent and RemoteEvent:FindFirstChild("1Ca1r") then
        RemoteEvent["1Ca1r"]:FireServer("PickingCar", "TowTruck")
    end
    task.wait(1)
    PCar = GetCar()
end

if PCar then
    local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
    if Seat and not Humanoid.Sit then
        repeat
            RootPart.CFrame = Seat.CFrame * CFrame.new(0, math.random(-1, 1), 0)
            task.wait()
        until Humanoid.Sit or not PCar.Parent
    end
end
        wait(0.2)
        
        local UserInputService = game:GetService("UserInputService")
        local RunService = game:GetService("RunService")
        local Mouse = Players.LocalPlayer:GetMouse()
        local Folder = Instance.new("Folder", game:GetService("Workspace"))
        local Part = Instance.new("Part", Folder)
        local Attachment1 = Instance.new("Attachment", Part)
        Part.Anchored = true
        Part.CanCollide = false
        Part.Transparency = 1

        local NetworkAccess = coroutine.create(function()
            settings().Physics.AllowSleep = false
            while RunService.RenderStepped:Wait() do
                for _, player in next, Players:GetPlayers() do
                    if player ~= Players.LocalPlayer then
                        player.MaximumSimulationRadius = 0
                        sethiddenproperty(player, "SimulationRadius", 2)
                    end
                end
                Players.LocalPlayer.MaximumSimulationRadius = math.pow(math.huge, math.huge)
                setsimulationradius(math.huge)
            end
        end)
        coroutine.resume(NetworkAccess)

        local function ForceVehicle(v)
            if v:IsA("Model") and v:FindFirstChildOfClass("VehicleSeat") then
                Mouse.TargetFilter = v
                for _, x in next, v:GetDescendants() do
                    if x:IsA("BodyAngularVelocity") or x:IsA("BodyForce") or x:IsA("BodyGyro") or x:IsA("BodyPosition") or x:IsA("BodyThrust") or x:IsA("BodyVelocity") or x:IsA("RocketPropulsion") then
                        x:Destroy()
                    end
                end
                if v:FindFirstChild("Attachment") then
                    v:FindFirstChild("Attachment"):Destroy()
                end
                if v:FindFirstChild("AlignPosition") then
                    v:FindFirstChild("AlignPosition"):Destroy()
                end
                if v:FindFirstChild("Torque") then
                    v:FindFirstChild("Torque"):Destroy()
                end
                for _, part in next, v:GetDescendants() do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                        local Torque = Instance.new("Torque", part)
                        Torque.Torque = Vector3.new(1000 * 102, 100000 * 102, 10000 * 12)
                        local AlignPosition = Instance.new("AlignPosition", part)
                        local Attachment2 = Instance.new("Attachment", part)
                        Torque.Attachment0 = Attachment2
                        AlignPosition.MaxForce = 99999
                        AlignPosition.MaxVelocity = math.huge
                        AlignPosition.Responsiveness = 200
                        AlignPosition.Attachment0 = Attachment2
                        AlignPosition.Attachment1 = Attachment1
                    end
                end
            end
        end

        for _, v in next, game:GetService("Workspace"):GetDescendants() do
            ForceVehicle(v)
        end

        game:GetService("Workspace").DescendantAdded:Connect(function(v)
            ForceVehicle(v)
        end)

        spawn(function()
            while true do
                if selectedPlayerName then
                    local player = Players:FindFirstChild(selectedPlayerName)
                    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                        local rootPart = player.Character.HumanoidRootPart
                        Attachment1.WorldCFrame = rootPart.CFrame
                    end
                end
                RunService.RenderStepped:Wait()
            end
        end)

        wait(4)
        
        local targetPosition = Vector3.new(67022016512, 67022016512, -30348027904)
        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)

        local function onPlayerSeated(player)
            if player and player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid and humanoid.SeatPart then
                    if humanoid.SeatPart.Parent:IsA("VehicleSeat") then
                        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
                    end
                end
            end
        end

        game:GetService("Players").PlayerAdded:Connect(function(player)
            if player.Name == selectedPlayerName then
                player.CharacterAdded:Connect(function(character)
                    local humanoid = character:WaitForChild("Humanoid")
                    humanoid.Seated:Connect(function(_, seat)
                        if seat then
                            onPlayerSeated(player)
                        end
                    end)
                end)
            end
        end)
    end    
})


Tab:AddButton({
    Name = "Fling Bus - All",
    Callback = function()
        local Player = game.Players.LocalPlayer
        local Character = Player.Character
        local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
        local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")
        local Vehicles = game.Workspace:FindFirstChild("Vehicles")

        
        if not Humanoid or not RootPart then
            warn("Jogador sem Humanoid/RootPart")
            return
        end

        -- Função para spawnar o barco
        local function spawnBoat()
            RootPart.CFrame = CFrame.new(1118.81, 75.998, -1138.61)
            task.wait(0.5)
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Ca1r"):FireServer("PickingCar", "SchoolBus")
            task.wait(1)
            return Vehicles:FindFirstChild(Player.Name.."Car")
        end

        -- Garante que o barco foi spawnado
        local PCar = Vehicles:FindFirstChild(Player.Name.."Car") or spawnBoat()
        if not PCar then
            warn("Falha ao spawnar o barco")
            return
        end

        -- Aguarda o assento do barco
        local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
        if not Seat then
            warn("Nenhum assento encontrado no barco")
            return
        end

        -- Tenta sentar no barco
        repeat 
            task.wait()
            RootPart.CFrame = Seat.CFrame * CFrame.new(0, math.random(-1, 1), 0)
        until Humanoid.Sit

        -- Criando e adicionando o SpinGyro no barco
        local SpinGyro = Instance.new("BodyGyro")
        SpinGyro.Parent = PCar.PrimaryPart
        SpinGyro.MaxTorque = Vector3.new(1e12, 1e12, 1e12) -- Torque maior para maior rotação
        SpinGyro.P = 1e12 -- Potência aumentada para rotação mais rápida
        SpinGyro.CFrame = PCar.PrimaryPart.CFrame

        print("SpinGyro ativado no barco! Rotação centrífuga aplicada!")

        -- Função para realizar o fling em um alvo específico
        local function flingTarget(TargetPlayer)
            local TargetC = TargetPlayer.Character
            local TargetH = TargetC and TargetC:FindFirstChildOfClass("Humanoid")
            local TargetRP = TargetC and TargetC:FindFirstChild("HumanoidRootPart")

            if not TargetRP or not TargetH then
                warn("Alvo sem Humanoid/RootPart")
                return
            end

            local angle = 0
            local radius = 5
            local speed = 0.1
            local expansionRate = 0.2

            -- Loop de 3 segundos para o fling no jogador atual
            local startTime = tick()
            while tick() - startTime < 3 and PCar and PCar.Parent do
                task.wait(0.1)

                local offsetX = math.cos(angle) * radius
                local offsetZ = math.sin(angle) * radius
                local moveTo = TargetRP.Position + Vector3.new(offsetX, 0, offsetZ)

                PCar:SetPrimaryPartCFrame(CFrame.new(moveTo) * CFrame.Angles(0, math.rad(180), 0))

                angle = angle + speed
                radius = math.max(radius - expansionRate, 1)

                if (PCar.PrimaryPart.Position - TargetRP.Position).Magnitude < 3 then
                    speed = speed + 0.2
                    expansionRate = expansionRate + 0.1
                end
            end
        end

        -- Loop para alternar entre os jogadores
        task.spawn(function()
            while PCar and PCar.Parent do
                for _, TargetPlayer in pairs(game.Players:GetPlayers()) do
                    if TargetPlayer ~= Player and not Whitelist[TargetPlayer.Name] then
                        flingTarget(TargetPlayer)
                    end
                end
            end
        end)
    end
})


Tab:AddButton({
    Name = "Bus Fling v2 alll",
    Callback = function()
        local Player = game.Players.LocalPlayer
        local Character = Player.Character
        local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
        local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")
        local Vehicles = workspace:FindFirstChild("Vehicles")
        local OldPos = RootPart and RootPart.CFrame

        if not Humanoid or not RootPart then return end

        local PCar = Vehicles:FindFirstChild(Player.Name.."Car")
        if not PCar then
            RootPart.CFrame = CFrame.new(1118.81, 75.998, -1138.61)
            task.wait(0.5)
            local Remote = game.ReplicatedStorage:FindFirstChild("RE") and game.ReplicatedStorage.RE:FindFirstChild("1Ca1r")
            if Remote then Remote:FireServer("PickingCar", "SchoolBus") end
            task.wait(0.5)
            PCar = Vehicles:FindFirstChild(Player.Name.."Car")
        end

        local timeout = 5
        while timeout > 0 and not PCar do
            task.wait(0.25)
            PCar = Vehicles:FindFirstChild(Player.Name.."Car")
            timeout -= 0.25
        end
        if not PCar then return end

        task.wait(0.5)
        if PCar and not Humanoid.Sit then
            local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
            if Seat then
                repeat task.wait()
                    RootPart.CFrame = Seat.CFrame
                until Humanoid.Sit
            end
        end

        -- Aplica BodyVelocity no carro
        for _, part in ipairs(PCar:GetDescendants()) do
            if part:IsA("BasePart") then
                local bv = Instance.new("BodyVelocity")
                bv.Velocity = Vector3.new(1e9, 1e9, 1e9)
                bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                bv.P = 500
                bv.Parent = part
            end
        end

        -- Loop contínuo para aplicar o fling em todos os jogadores
        task.spawn(function()
            local Angles = 0
            local YRotation = 0

            while PCar.Parent do
                for _, target in ipairs(game.Players:GetPlayers()) do
                    if target ~= Player then
                        local TargetC = target.Character
                        local TargetH = TargetC and TargetC:FindFirstChildOfClass("Humanoid")
                        local TargetRP = TargetC and TargetC:FindFirstChild("HumanoidRootPart")

                        if TargetC and TargetH and TargetRP then
                            -- Cria forças para fling
                            local attachment = Instance.new("Attachment", TargetRP)
                            local force = Instance.new("BodyVelocity")
                            force.Velocity = Vector3.new(1e9, 1e9, 1e9)
                            force.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                            force.P = 500
                            force.Parent = attachment

                            local startTime = tick()
                            while tick() - startTime < 2 and TargetRP.Parent and PCar.Parent do
                                task.wait()
                                Angles += 100
                                YRotation += 5000
                                local Rotation = CFrame.Angles(math.rad(Angles), math.rad(YRotation), 0)

                                local function flingAttack(offset)
                                    local newPos = TargetRP.Position + offset + (TargetH.MoveDirection * TargetRP.Velocity.Magnitude / 1.1)
                                    local newCF = CFrame.new(newPos) * Rotation
                                    PCar:SetPrimaryPartCFrame(newCF)
                                end

                                flingAttack(Vector3.new(0, 1, 0))
                                flingAttack(Vector3.new(0, -2.25, 5))
                                flingAttack(Vector3.new(0, 2.25, 0.25))
                                flingAttack(Vector3.new(-2.25, -1.5, 2.25))
                                flingAttack(Vector3.new(0, 1.5, 0))
                                flingAttack(Vector3.new(0, -1.5, 0))
                            end

                            -- Limpa forças ao trocar de alvo
                            attachment:Destroy()
                            force:Destroy()
                        end
                    end
                end
            end
        end)

        -- Cleanup final quando veículo for removido
        repeat task.wait() until not PCar or not PCar.Parent
        Humanoid.Sit = false
        task.wait(0.1)
        if OldPos then RootPart.CFrame = OldPos end
    end
})

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remotes = ReplicatedStorage:WaitForChild("Remotes")

local Target = nil

-- Função para obter os nomes dos jogadores
local function GetPlayerNames()
    local PlayerNames = {}
    for _, player in ipairs(Players:GetPlayers()) do
        table.insert(PlayerNames, player.Name)
    end
    return PlayerNames
end

-- Dropdown de seleção de jogador
local Dropdown = Tab:AddDropdown({
    Name = "Selecionar Jogador V2",
    Options = GetPlayerNames(),
    Default = Target,
    Callback = function(Value)
        Target = Value
    end
})

-- Atualiza opções do dropdown quando alguém entra ou sai
local function UpdateDropdown()
    Dropdown:Refresh(GetPlayerNames(), true)
end

Players.PlayerAdded:Connect(UpdateDropdown)
Players.PlayerRemoving:Connect(UpdateDropdown)

-- Botão de copiar avatar
Tab:AddButton({
    Name = "Copy Avatar",
    Callback = function()
        if not Target then return end

        local LP = Players.LocalPlayer
        local LChar = LP.Character
        local TPlayer = Players:FindFirstChild(Target)

        if TPlayer and TPlayer.Character then
            local LHumanoid = LChar and LChar:FindFirstChildOfClass("Humanoid")
            local THumanoid = TPlayer.Character:FindFirstChildOfClass("Humanoid")

            if LHumanoid and THumanoid then
                -- RESETAR LOCALPLAYER
                local LDesc = LHumanoid:GetAppliedDescription()

                -- Remover acessórios, roupas e face atuais
                for _, acc in ipairs(LDesc:GetAccessories(true)) do
                    if acc.AssetId and tonumber(acc.AssetId) then
                        Remotes.Wear:InvokeServer(tonumber(acc.AssetId))
                        task.wait(0.2)
                    end
                end

                if tonumber(LDesc.Shirt) then
                    Remotes.Wear:InvokeServer(tonumber(LDesc.Shirt))
                    task.wait(0.2)
                end

                if tonumber(LDesc.Pants) then
                    Remotes.Wear:InvokeServer(tonumber(LDesc.Pants))
                    task.wait(0.2)
                end

                if tonumber(LDesc.Face) then
                    Remotes.Wear:InvokeServer(tonumber(LDesc.Face))
                    task.wait(0.2)
                end

                -- COPIAR DO JOGADOR ALVO
                local PDesc = THumanoid:GetAppliedDescription()

                -- Enviar partes do corpo
                local argsBody = {
                    [1] = {
                        [1] = PDesc.Torso,
                        [2] = PDesc.RightArm,
                        [3] = PDesc.LeftArm,
                        [4] = PDesc.RightLeg,
                        [5] = PDesc.LeftLeg,
                        [6] = PDesc.Head
                    }
                }
                Remotes.ChangeCharacterBody:InvokeServer(unpack(argsBody))
                task.wait(0.5)

                if tonumber(PDesc.Shirt) then
                    Remotes.Wear:InvokeServer(tonumber(PDesc.Shirt))
                    task.wait(0.3)
                end

                if tonumber(PDesc.Pants) then
                    Remotes.Wear:InvokeServer(tonumber(PDesc.Pants))
                    task.wait(0.3)
                end

                if tonumber(PDesc.Face) then
                    Remotes.Wear:InvokeServer(tonumber(PDesc.Face))
                    task.wait(0.3)
                end

                for _, v in ipairs(PDesc:GetAccessories(true)) do
                    if v.AssetId and tonumber(v.AssetId) then
                        Remotes.Wear:InvokeServer(tonumber(v.AssetId))
                        task.wait(0.3)
                    end
                end

                local SkinColor = TPlayer.Character:FindFirstChild("Body Colors")
                if SkinColor then
                    Remotes.ChangeBodyColor:FireServer(tostring(SkinColor.HeadColor))
                    task.wait(0.3)
                end

                if tonumber(PDesc.IdleAnimation) then
                    Remotes.Wear:InvokeServer(tonumber(PDesc.IdleAnimation))
                    task.wait(0.3)
                end

                -- Nome, bio e cor
                local Bag = TPlayer:FindFirstChild("PlayersBag")
                if Bag then
                    if Bag:FindFirstChild("RPName") and Bag.RPName.Value ~= "" then
                        Remotes.RPNameText:FireServer("RolePlayName", Bag.RPName.Value)
                        task.wait(0.3)
                    end
                    if Bag:FindFirstChild("RPBio") and Bag.RPBio.Value ~= "" then
                        Remotes.RPNameText:FireServer("RolePlayBio", Bag.RPBio.Value)
                        task.wait(0.3)
                    end
                    if Bag:FindFirstChild("RPNameColor") then
                        Remotes.RPNameColor:FireServer("PickingRPNameColor", Bag.RPNameColor.Value)
                        task.wait(0.3)
                    end
                    if Bag:FindFirstChild("RPBioColor") then
                        Remotes.RPNameColor:FireServer("PickingRPBioColor", Bag.RPBioColor.Value)
                        task.wait(0.3)
                    end
                end
            end
        end
    end
})



Tab:AddButton({
    Name = "Boombox 100% All",
    Callback = function()
        local player = game.Players.LocalPlayer
        local playerGui = player:FindFirstChild("PlayerGui")
        if not playerGui then return end -- Verifica se o jogador tem PlayerGui

        local boombox
        local sg
        local savedID = nil  -- Variável para armazenar o ID salvo
        local listFrame = {}  -- Tabela para controlar múltiplos frames da boombox

        -- Função para criar a Boombox
        local function createBoombox()
            boombox = Instance.new("Tool")
            boombox.Name = "Boombox"
            boombox.RequiresHandle = true
            boombox.Parent = player.Backpack

            local handle = Instance.new("Part")
            handle.Name = "Handle"
            handle.Size = Vector3.new(1, 1, 1)
            handle.CanCollide = false
            handle.Anchored = false
            handle.Transparency = 1
            handle.Parent = boombox

            -- Função para criar a GUI de música
            local function createGui()
                -- Verifica se já existe um frame da boombox
                if listFrame[player.UserId] then
                    return  -- Interrompe a criação da GUI se já existir
                end

                -- Marca que a GUI foi criada para esse jogador
                listFrame[player.UserId] = true

                sg = Instance.new("ScreenGui")
                sg.Name = "ChooseSongGui"
                sg.Parent = playerGui  

                local frame = Instance.new("Frame")
                frame.Style = "RobloxRound"
                frame.Size = UDim2.new(0.25, 0, 0.25, 0)
                frame.Position = UDim2.new((1-frame.Size.X.Scale)/2, 0, (1-frame.Size.Y.Scale)/2, 0)
                frame.Parent = sg
                frame.Draggable = true

                local text = Instance.new("TextLabel")
                text.BackgroundTransparency = 1
                text.TextStrokeTransparency = 0
                text.TextColor3 = Color3.new(1, 1, 1)
                text.Size = UDim2.new(1, 0, 0.6, 0)
                text.TextScaled = true
                text.Text = "Lay down the beat!\nPut in the ID number for a song you love that's been uploaded to ROBLOX.\nLeave it blank to stop playing music."
                text.Parent = frame

                local input = Instance.new("TextBox")
                input.BackgroundColor3 = Color3.new(0, 0, 0)
                input.BackgroundTransparency = 0.5
                input.BorderColor3 = Color3.new(1, 1, 1)
                input.TextColor3 = Color3.new(1, 1, 1)
                input.TextStrokeTransparency = 1
                input.TextScaled = true
                input.Text = savedID or "142376088"  -- Usar o ID salvo ou o padrão
                input.Size = UDim2.new(1, 0, 0.2, 0)
                input.Position = UDim2.new(0, 0, 0.6, 0)
                input.Parent = frame

                local button = Instance.new("TextButton")
                button.Style = "RobloxButton"
                button.Size = UDim2.new(0.75, 0, 0.2, 0)
                button.Position = UDim2.new(0.125, 0, 0.8, 0)
                button.TextColor3 = Color3.new(1, 1, 1)
                button.TextStrokeTransparency = 0
                button.Text = "Play!"
                button.TextScaled = true
                button.Parent = frame

                -- Função para tocar a música no servidor
                local function playAudioAll(ID)
                    if type(ID) ~= "number" then
                        print("Insira um número válido!")
                        return
                    end

                    local ReplicatedStorage = game:GetService("ReplicatedStorage")
                    local GunSoundEvent = ReplicatedStorage:FindFirstChild("1Gu1nSound1s", true)
                    if GunSoundEvent then
                        GunSoundEvent:FireServer(workspace, ID, 1)
                    end
                end

                -- Função para tocar a música localmente
                local function playAudioLocal(ID)
                    local sound = Instance.new("Sound")
                    sound.SoundId = "rbxassetid://" .. ID
                    sound.Volume = 1
                    sound.Looped = false
                    sound.Parent = player.Character or workspace
                    sound:Play()
                    task.wait(3)
                    sound:Destroy()
                end

                -- Função do botão "Play!"
                button.MouseButton1Click:Connect(function()
                    local soundID = tonumber(input.Text)
                    if soundID then
                        savedID = soundID  -- Salva o ID que foi inserido
                        playAudioAll(soundID) -- Toca para o servidor
                        playAudioLocal(soundID) -- Toca para você também
                    else
                        print("ID inválido!")
                    end

                    -- Remove a GUI instantaneamente
                    if sg then
                        sg:Destroy()
                        listFrame[player.UserId] = nil  -- Libera a GUI para futuras interações
                    end
                end)

            end

            -- Evento ao equipar a Boombox
            boombox.Equipped:Connect(function()
                -- Cria a GUI quando a Boombox é equipada, se ainda não existir
                if not listFrame[player.UserId] then
                    createGui()
                end

                -- Comando para equipar visualmente o Boombox no avatar
                local args = {
                    [1] = 1018548665  -- ID original
                }
                game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
            end)

            -- Evento ao desequipar a Boombox
            boombox.Unequipped:Connect(function()
                -- Remove a GUI quando o item for desequipado
                if sg then
                    sg:Destroy()
                    sg = nil
                end

                -- Comando para remover a Boombox visual do avatar
                local args = {
                    [1] = 1018548665  -- ID original
                }
                game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))

                -- Libera a GUI para a criação de um novo frame caso o item seja re-equipado
                listFrame[player.UserId] = nil
            end)

            -- Evento para detectar se a Boombox foi deletada da mochila
            boombox.AncestryChanged:Connect(function(_, parent)
                if not parent and sg then
                    sg:Destroy()
                    sg = nil
                    listFrame[player.UserId] = nil  -- Permite criar a GUI novamente quando o item for removido
                end
            end)
        end

        -- Chama a função para criar a Boombox
        createBoombox()
    end
})




local function clickNormally(object)
    local clickDetector = object:FindFirstChildWhichIsA("ClickDetector")
    if clickDetector then
        fireclickdetector(clickDetector) -- Clica normalmente
    end
end


local function dupeItem(itemPath, maxTeleports)
    if itemPath then
        local teleportCount = 0
        while teleportCount < maxTeleports do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = itemPath.CFrame
            clickNormally(itemPath)
            teleportCount = teleportCount + 1
            wait(0.01) -- Tempo de espera para evitar travamento
        end
    end
end


local function lagarServer()
    local ghostMeterPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("GhostMeter")
    if ghostMeterPath then
        local teleportCount = 0
        local maxTeleports = 10000000000000000000000

        while teleportCount < maxTeleports do
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = ghostMeterPath.CFrame
            clickNormally(ghostMeterPath)
            teleportCount = teleportCount + 1
            wait(0) -- Executa rapidamente
        end
    else
        warn("Item GhostMeter nÃƒÂ£o encontrado.")
    end
end


Tab:AddButton({
    Name = "Dupe Livro",
    Callback = function()
        local bookPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_DayCare"):FindFirstChild("Tools"):FindFirstChild("Book")
        dupeItem(bookPath, 300)
    end
})


Tab:AddButton({
    Name = "Dupe Extintor",
    Callback = function()
        local fireXPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("FireX")
        dupeItem(fireXPath, 300)
    end
})


Tab:AddButton({
    Name = "Dupe Bola de Basquete",
    Callback = function()
        local basketballPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("Basketball")
        dupeItem(basketballPath, 300)
    end
})


Tab:AddButton({
    Name = "Dupe Maca",
    Callback = function()
        local stretcherPath = workspace:FindFirstChild("WorkspaceCom"):FindFirstChild("001_GiveTools"):FindFirstChild("Stretcher")
        dupeItem(stretcherPath, 300)
    end
})


Tab:AddButton({
    Name = "Lag Ghost Matter",
    Callback = function()
        lagarServer()
    end
})



local ESPEnabled = false
local CurrentColor = Color3.fromRGB(255, 255, 255) -- Cor inicial branca
local RainbowEnabled = false

local function createESP(player)
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
        return
    end

    -- Adicionar Highlight para destacar o jogador
    local highlight = Instance.new("Highlight")
    highlight.Name = "ESP_Highlight"
    highlight.Adornee = player.Character
    highlight.FillColor = CurrentColor
    highlight.FillTransparency = 0.5
    highlight.OutlineColor = Color3.fromRGB(0, 0, 0)
    highlight.OutlineTransparency = 0
    highlight.Parent = player.Character

    -- Criar BillboardGui para exibir as informações
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_Info"
    billboard.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
    billboard.Size = UDim2.new(0, 150, 0, 50)  -- Diminuindo o tamanho da GUI
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.Parent = player.Character

    -- Nome do jogador
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 2, 2.5, 4)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = CurrentColor
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.Code -- Fonte digital
    nameLabel.Parent = billboard


    -- Atualizar distância, saúde e status de sentado continuamente
    task.spawn(function()
        while ESPEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") do
            local distance = (player.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            distanceLabel.Text = string.format("Distância: %.2f", distance)
            healthLabel.Text = "Vida: " .. math.floor(player.Character.Humanoid.Health)
            sittingLabel.Text = player.Character.Humanoid.Sit == true and "Sentado: Sim" or "Sentado: Não"

            -- Atualizar cor dinamicamente
            highlight.FillColor = CurrentColor
            nameLabel.TextColor3 = CurrentColor
            distanceLabel.TextColor3 = CurrentColor
            ageLabel.TextColor3 = CurrentColor
            healthLabel.TextColor3 = CurrentColor
            sittingLabel.TextColor3 = CurrentColor

            task.wait(0.1)
        end
    end)
end

local function toggleESP(enabled)
    ESPEnabled = enabled
    if ESPEnabled then
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and not player.Character:FindFirstChild("ESP_Info") then
                createESP(player)
            end
        end

        game.Players.PlayerAdded:Connect(function(player)
            if ESPEnabled then
                player.CharacterAdded:Connect(function()
                    createESP(player)
                end)
            end
        end)

        game.Players.PlayerRemoving:Connect(function(player)
            if player.Character then
                if player.Character:FindFirstChild("ESP_Info") then
                    player.Character.ESP_Info:Destroy()
                end
                if player.Character:FindFirstChild("ESP_Highlight") then
                    player.Character.ESP_Highlight:Destroy()
                end
            end
        end)
    else
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character then
                if player.Character:FindFirstChild("ESP_Info") then
                    player.Character.ESP_Info:Destroy()
                end
                if player.Character:FindFirstChild("ESP_Highlight") then
                    player.Character.ESP_Highlight:Destroy()
                end
            end
        end
    end
end

local function setESPColor(color)
    CurrentColor = color
    RainbowEnabled = false -- Desativar Rainbow ao selecionar uma cor
end

local function enableRainbow()
    RainbowEnabled = true
    task.spawn(function()
        while RainbowEnabled do
            local time = tick() * 5
            CurrentColor = Color3.fromHSV((time % 360) / 360, 1, 1)
            task.wait(0.1)
        end
    end)
end


-- Conectar evento para reativar o ESP após a morte
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        -- Verificar se o ESP está ativado e recriar o ESP para o jogador
        if ESPEnabled then
            createESP(player)
        end
    end)
end)

-- Dropdown para seleção de cor
Tab:AddDropdown({
    Name = "Cores do ESP",
    Options = {"Azul", "Vermelho", "Verde", "Amarelo", "Roxo", "Cinza", "Preto", "Branco", "Laranja", "Rosa", "Marrom", "Rainbow"},
    Default = {"Branco"},
    Callback = function(selected)
        if selected == "Azul" then
            setESPColor(Color3.fromRGB(0, 0, 255))
        elseif selected == "Vermelho" then
            setESPColor(Color3.fromRGB(255, 0, 0))
        elseif selected == "Verde" then
            setESPColor(Color3.fromRGB(0, 255, 0))
        elseif selected == "Amarelo" then
            setESPColor(Color3.fromRGB(255, 255, 0))
        elseif selected == "Roxo" then
            setESPColor(Color3.fromRGB(128, 0, 128))
        elseif selected == "Cinza" then
            setESPColor(Color3.fromRGB(128, 128, 128))
        elseif selected == "Preto" then
            setESPColor(Color3.fromRGB(0, 0, 0))
        elseif selected == "Branco" then
            setESPColor(Color3.fromRGB(255, 255, 255))
        elseif selected == "Laranja" then
            setESPColor(Color3.fromRGB(255, 165, 0))
        elseif selected == "Rosa" then
            setESPColor(Color3.fromRGB(255, 192, 203))
        elseif selected == "Marrom" then
            setESPColor(Color3.fromRGB(139, 69, 19))
        elseif selected == "Rainbow" then
            enableRainbow()
        end
    end
})


-- Toggle para ativar/desativar ESP
Tab:AddToggle({
    Name = "ESP",
    Default = false, -- Inicialmente desativado
    Callback = function(Value)
        toggleESP(Value)
    end
})

local bloco = Instance.new("Part")
bloco.Size = Vector3.new(4993, 15, 4999)
bloco.Position = Vector3.new(101, -446, -180)
bloco.Anchored = true
bloco.BrickColor = BrickColor.new("Bright White")
bloco.Parent = workspace

bloco.Touched:Connect(function(hit)
    local player = game.Players:GetPlayerFromCharacter(hit.Parent)
    if player then
       print("ola mundo")
wait(0.8) game:GetService("ReplicatedStorage").RE["1Ca1r"]:FireServer("DeleteAllVehicles")
    end
end)
wait(0.2)
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

character:WaitForChild("HumanoidRootPart").Changed:Connect(function()
    if character.HumanoidRootPart.Position.Y < -50 then
        character:SetPrimaryPartCFrame(CFrame.new(0, 10, 0))
    end
end)
wait(0.1)

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")

local playerNames = {}
for _, player in pairs(Players:GetPlayers()) do
    table.insert(playerNames, player.Name)
end

local selectedPlayerName = nil

Tab:AddDropdown({
    Name = "Target",
    Options = playerNames,
    Callback = function(selected)
        selectedPlayerName = selected
    end
})

-- FunÃ§Ã£o que forÃ§a os veÃ­culos a se moverem
local function ForceVehicle(v)
    if v:IsA("Model") and v:FindFirstChildOfClass("VehicleSeat") then
        for _, x in next, v:GetDescendants() do
            if x:IsA("BodyAngularVelocity") or x:IsA("BodyForce") or x:IsA("BodyGyro") or x:IsA("BodyPosition") or x:IsA("BodyThrust") or x:IsA("BodyVelocity") or x:IsA("RocketPropulsion") then
                x:Destroy()
            end
        end
        if v:FindFirstChild("Attachment") then
            v:FindFirstChild("Attachment"):Destroy()
        end
        if v:FindFirstChild("AlignPosition") then
            v:FindFirstChild("AlignPosition"):Destroy()
        end
        if v:FindFirstChild("Torque") then
            v:FindFirstChild("Torque"):Destroy()
        end
        for _, part in next, v:GetDescendants() do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end

        local function kill(TargetRP, offset, rotation)
            TargetRP.CFrame = TargetRP.CFrame * offset * rotation
        end

        local function updateVehicle()
            if v and v.PrimaryPart then
                local TargetRP = v.PrimaryPart
                local TargetH = v:FindFirstChildWhichIsA("Humanoid")

                if TargetH then
                    local moveDir = TargetH.MoveDirection
                    local speed = TargetRP.Velocity.Magnitude

                    kill(TargetRP, CFrame.new(0, 3, 0) + moveDir * speed / 1.05, CFrame.Angles(math.rad(20), 0, 0))
                    kill(TargetRP, CFrame.new(0, -1.5, 2) + moveDir * speed / 1.05, CFrame.Angles(math.rad(15), 0, 0))
                    kill(TargetRP, CFrame.new(2, 1.5, 2.25)  + moveDir * speed / 1.10, CFrame.Angles(math.rad(50), 0, 0))
                    kill(TargetRP, CFrame.new(-2.25, -1.5, 2.25) + moveDir * speed / 1.10, CFrame.Angles(math.rad(30), 0, 0))
                    kill(TargetRP, CFrame.new(0, 1.5, 0) + moveDir * speed / 1.05, CFrame.Angles(math.rad(10), 0, 0))
                    kill(TargetRP, CFrame.new(0, -1.5, 0) + moveDir * speed / 1.05, CFrame.Angles(math.rad(5), 0, 0))
                end
            end
        end

        RunService.RenderStepped:Connect(updateVehicle)
    end
end

-- Aplicar funÃ§Ã£o ForceVehicle em veÃ­culos existentes
for _, v in next, Workspace:GetDescendants() do
    ForceVehicle(v)
end

Workspace.DescendantAdded:Connect(function(v)
    ForceVehicle(v)
end)

-- FunÃ§Ã£o para monitorar assentos e executar o script no jogador selecionado
local function monitorSeats()
    for _, seat in pairs(Workspace:GetDescendants()) do
        if seat:IsA("Seat") or seat:IsA("VehicleSeat") then
            seat:GetPropertyChangedSignal("Occupant"):Connect(function()
                if seat.Occupant then
                    local occupantPlayer = Players:GetPlayerFromCharacter(seat.Occupant.Parent)
                    if occupantPlayer and occupantPlayer.Name == selectedPlayerName then
                        ForceVehicle(seat.Parent)
                    end
                end
            end)
        end
    end

    Workspace.DescendantAdded:Connect(function(descendant)
        if descendant:IsA("Seat") or descendant:IsA("VehicleSeat") then
            descendant:GetPropertyChangedSignal("Occupant"):Connect(function()
                if descendant.Occupant then
                    local occupantPlayer = Players:GetPlayerFromCharacter(descendant.Occupant.Parent)
                    if occupantPlayer and occupantPlayer.Name == selectedPlayerName then
                        ForceVehicle(descendant.Parent)
                    end
                end
            end)
        end
    end)
end

monitorSeats()
Tab:AddButton({
    Name = "Car Kill v2",
    Callback = function()
        local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChildOfClass("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local Vehicles = game.Workspace:FindFirstChild("Vehicles")
local OldPos = RootPart.CFrame

if not Humanoid or not Vehicles then return end

local function GetCar()
    return Vehicles:FindFirstChild(Player.Name.."Car")
end

local PCar = GetCar()

if not PCar then
    RootPart.CFrame = CFrame.new(1118.81, 75.998, -1138.61)
    task.wait(0.5)
    local RemoteEvent = game:GetService("ReplicatedStorage"):FindFirstChild("RE")
    if RemoteEvent and RemoteEvent:FindFirstChild("1Ca1r") then
        RemoteEvent["1Ca1r"]:FireServer("PickingCar", "SchoolBus")
    end
    task.wait(1)
    PCar = GetCar()
end

if PCar then
    local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
    if Seat and not Humanoid.Sit then
        repeat
            RootPart.CFrame = Seat.CFrame * CFrame.new(0, math.random(-1, 1), 0)
            task.wait()
        until Humanoid.Sit or not PCar.Parent
    end
end
        wait(0.2)
        
        local UserInputService = game:GetService("UserInputService")
        local RunService = game:GetService("RunService")
        local Mouse = Players.LocalPlayer:GetMouse()
        local Folder = Instance.new("Folder", game:GetService("Workspace"))
        local Part = Instance.new("Part", Folder)
        local Attachment1 = Instance.new("Attachment", Part)
        Part.Anchored = true
        Part.CanCollide = false
        Part.Transparency = 1

        local NetworkAccess = coroutine.create(function()
            settings().Physics.AllowSleep = false
            while RunService.RenderStepped:Wait() do
                for _, player in next, Players:GetPlayers() do
                    if player ~= Players.LocalPlayer then
                        player.MaximumSimulationRadius = 0
                        sethiddenproperty(player, "SimulationRadius", 2)
                    end
                end
                Players.LocalPlayer.MaximumSimulationRadius = math.pow(math.huge, math.huge)
                setsimulationradius(math.huge)
            end
        end)
        coroutine.resume(NetworkAccess)

        local function ForceVehicle(v)
            if v:IsA("Model") and v:FindFirstChildOfClass("VehicleSeat") then
                Mouse.TargetFilter = v
                for _, x in next, v:GetDescendants() do
                    if x:IsA("BodyAngularVelocity") or x:IsA("BodyForce") or x:IsA("BodyGyro") or x:IsA("BodyPosition") or x:IsA("BodyThrust") or x:IsA("BodyVelocity") or x:IsA("RocketPropulsion") then
                        x:Destroy()
                    end
                end
                if v:FindFirstChild("Attachment") then
                    v:FindFirstChild("Attachment"):Destroy()
                end
                if v:FindFirstChild("AlignPosition") then
                    v:FindFirstChild("AlignPosition"):Destroy()
                end
                if v:FindFirstChild("Torque") then
                    v:FindFirstChild("Torque"):Destroy()
                end
                for _, part in next, v:GetDescendants() do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                        local Torque = Instance.new("Torque", part)
                        Torque.Torque = Vector3.new(1000 * 102, 10000 * 102, 1000 * 100)
                        local AlignPosition = Instance.new("AlignPosition", part)
                        local Attachment2 = Instance.new("Attachment", part)
                        Torque.Attachment0 = Attachment2
                        AlignPosition.MaxForce = 99999
                        AlignPosition.MaxVelocity = math.huge
                        AlignPosition.Responsiveness = 200
                        AlignPosition.Attachment0 = Attachment2
                        AlignPosition.Attachment1 = Attachment1
                    end
                end
            end
        end

        for _, v in next, game:GetService("Workspace"):GetDescendants() do
            ForceVehicle(v)
        end

        game:GetService("Workspace").DescendantAdded:Connect(function(v)
            ForceVehicle(v)
        end)

        spawn(function()
            while true do
                if selectedPlayerName then
                    local player = Players:FindFirstChild(selectedPlayerName)
                    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                        local rootPart = player.Character.HumanoidRootPart
                        Attachment1.WorldCFrame = rootPart.CFrame
                    end
                end
                RunService.RenderStepped:Wait()
            end
        end)

        wait(4)
        
        local targetPosition = Vector3.new(101, -446, -180)
        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)

        local function onPlayerSeated(player)
            if player and player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid and humanoid.SeatPart then
                    if humanoid.SeatPart.Parent:IsA("VehicleSeat") then
                        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
                    end
                end
            end
        end

        game:GetService("Players").PlayerAdded:Connect(function(player)
            if player.Name == selectedPlayerName then
                player.CharacterAdded:Connect(function(character)
                    local humanoid = character:WaitForChild("Humanoid")
                    humanoid.Seated:Connect(function(_, seat)
                        if seat then
                            onPlayerSeated(player)
                        end
                    end)
                end)
            end
        end)
    end    
