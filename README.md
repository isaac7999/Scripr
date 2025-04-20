-- Anti Ban & Anti Detected UI para Mini City (Mobile Friendly)

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")
local StarterGui = game:GetService("StarterGui")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

if CoreGui:FindFirstChild("MiniCitySecureUI") then
    CoreGui.MiniCitySecureUI:Destroy()
end

local function proteger(obj)
    if syn and syn.protect_gui then
        syn.protect_gui(obj)
    elseif get_hidden_gui or gethui then
        local hiddenUI = get_hidden_gui and get_hidden_gui() or gethui()
        obj.Parent = hiddenUI
        return
    end
    obj.Parent = CoreGui
end

local KeyGui = Instance.new("ScreenGui")
KeyGui.Name = "MiniCitySecureUI"
KeyGui.ResetOnSpawn = false
proteger(KeyGui)

local KeyFrame = Instance.new("Frame")
KeyFrame.Size = UDim2.new(0, 300, 0, 200)
KeyFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
KeyFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
KeyFrame.BackgroundTransparency = 0.2
KeyFrame.AnchorPoint = Vector2.new(0.5, 0.5)
KeyFrame.Parent = KeyGui
Instance.new("UICorner", KeyFrame).CornerRadius = UDim.new(0, 12)

local KeyImage = Instance.new("ImageLabel")
KeyImage.Size = UDim2.new(0, 80, 0, 80)
KeyImage.Position = UDim2.new(0.5, -40, 0, 10)
KeyImage.BackgroundTransparency = 1
KeyImage.Image = "rbxassetid://79308101207172"
KeyImage.Parent = KeyFrame

local KeyBox = Instance.new("TextBox")
KeyBox.PlaceholderText = "Digite a KEY..."
KeyBox.Size = UDim2.new(0.8, 0, 0, 40)
KeyBox.Position = UDim2.new(0.1, 0, 0.65, 0)
KeyBox.Text = ""
KeyBox.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
KeyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyBox.Font = Enum.Font.Gotham
KeyBox.TextSize = 16
KeyBox.Parent = KeyFrame
Instance.new("UICorner", KeyBox).CornerRadius = UDim.new(0, 8)

KeyBox.FocusLost:Connect(function(enter)
    if enter and KeyBox.Text == "12345678910" then
        StarterGui:SetCore("SendNotification", {
            Title = "Acesso Liberado",
            Text = "Bem-vindo, " .. player.Name,
            Duration = 5
        })
        KeyGui:Destroy()

        local MainGui = Instance.new("ScreenGui")
        MainGui.Name = "MiniCitySecureUI"
        MainGui.ResetOnSpawn = false
        proteger(MainGui)

        local MainFrame = Instance.new("Frame")
        MainFrame.Size = UDim2.new(0, 300, 0, 400)
        MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
        MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        MainFrame.BackgroundTransparency = 0.2
        MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
        MainFrame.Parent = MainGui
        Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

        local Title = Instance.new("TextLabel")
        Title.Text = "Mini City - Painel Seguro"
        Title.Size = UDim2.new(1, 0, 0, 40)
        Title.BackgroundTransparency = 1
        Title.TextColor3 = Color3.fromRGB(255, 255, 255)
        Title.Font = Enum.Font.GothamBold
        Title.TextSize = 20
        Title.Parent = MainFrame

        local function criarBotao(txt, posY)
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(0.8, 0, 0, 40)
            btn.Position = UDim2.new(0.1, 0, 0, posY)
            btn.Text = txt
            btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.Gotham
            btn.TextSize = 16
            btn.Parent = MainFrame
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
            return btn
        end

        local espAtivado = false
        local espBtn = criarBotao("ESP Players [OFF]", 50)
        espBtn.MouseButton1Click:Connect(function()
            espAtivado = not espAtivado
            espBtn.Text = espAtivado and "ESP Players [ON]" or "ESP Players [OFF]"
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= player and v.Character and v.Character:FindFirstChild("Head") then
                    local esp = v.Character:FindFirstChild("NameESP")
                    if esp then esp:Destroy() end
                    if espAtivado then
                        local bill = Instance.new("BillboardGui", v.Character)
                        bill.Name = "NameESP"
                        bill.Size = UDim2.new(0, 100, 0, 20)
                        bill.Adornee = v.Character.Head
                        bill.AlwaysOnTop = true
                        local label = Instance.new("TextLabel", bill)
                        label.Size = UDim2.new(1, 0, 1, 0)
                        label.BackgroundTransparency = 1
                        label.Text = v.Name
                        label.TextColor3 = Color3.fromRGB(255, 255, 255)
                        label.Font = Enum.Font.GothamBold
                        label.TextSize = 14
                    end
                end
            end
        end)

        local hpAtivado = false
        local hpBtn = criarBotao("ESP HP [OFF]", 100)
        hpBtn.MouseButton1Click:Connect(function()
            hpAtivado = not hpAtivado
            hpBtn.Text = hpAtivado and "ESP HP [ON]" or "ESP HP [OFF]"
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") then
                    local gui = v.Character:FindFirstChild("HPDisplay")
                    if gui then gui:Destroy() end
                    if hpAtivado then
                        local billboard = Instance.new("BillboardGui", v.Character)
                        billboard.Name = "HPDisplay"
                        billboard.Size = UDim2.new(0, 100, 0, 20)
                        billboard.StudsOffset = Vector3.new(0, 3, 0)
                        billboard.AlwaysOnTop = true

                        local text = Instance.new("TextLabel", billboard)
                        text.Size = UDim2.new(1, 0, 1, 0)
                        text.BackgroundTransparency = 1
                        text.TextColor3 = Color3.fromRGB(255, 0, 0)
                        text.Font = Enum.Font.GothamBold
                        text.TextSize = 14
                        text.TextStrokeTransparency = 0

                        RunService.RenderStepped:Connect(function()
                            if v.Character and v.Character:FindFirstChild("Humanoid") then
                                text.Text = "HP: " .. math.floor(v.Character.Humanoid.Health)
                            end
                        end)
                    end
                end
            end
        end)

        local revBtn = criarBotao("/revistar morto", 150)
        revBtn.MouseButton1Click:Connect(function()
            local success, err = pcall(function()
                local msg = "/revistar morto"
                if TextChatService:FindFirstChild("TextChannels") and TextChatService.TextChannels:FindFirstChild("RBXGeneral") then
                    TextChatService.TextChannels.RBXGeneral:SendAsync(msg)
                else
                    ReplicatedStorage:WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest"):FireServer(msg, "All")
                end
            end)
            if not success then
                warn("Falha ao enviar mensagem:", err)
            end
        end)

        -- Noclip
        local noclipAtivo = false
        local noclipBtn = criarBotao("Noclip [OFF]", 200)
        noclipBtn.MouseButton1Click:Connect(function()
            noclipAtivo = not noclipAtivo
            noclipBtn.Text = noclipAtivo and "Noclip [ON]" or "Noclip [OFF]"
        end)

        RunService.Stepped:Connect(function()
            if noclipAtivo and player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end)

        -- Anti Staff
        local antiStaff = false
        local antiStaffBtn = criarBotao("Anti staff [OFF]", 250)
        antiStaffBtn.MouseButton1Click:Connect(function()
            antiStaff = not antiStaff
            antiStaffBtn.Text = antiStaff and "Anti staff [ON]" or "Anti staff [OFF]"
        end)

        RunService.Heartbeat:Connect(function()
            if antiStaff then
                for _, v in pairs(Players:GetPlayers()) do
                    local team = tostring(v.Team)
                    if team:lower():match("staff") then
                        StarterGui:SetCore("SendNotification", {
                            Title = "AVISO",
                            Text = "UM STAFF ESTAVA NO SERVIDOR.",
                            Duration = 5
                        })
                        wait(1)
                        game:Shutdown()
                    end
                end
            end
        end)

        local dragging = false
        local dragStart, startPos

        MainFrame.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                dragStart = input.Position
                startPos = MainFrame.Position

                input.Changed:Connect(function()
                    if input.UserInputState == Enum.UserInputState.End then
                        dragging = false
                    end
                end)
            end
        end)

        MainFrame.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                local delta = input.Position - dragStart
                MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
    end
end)
