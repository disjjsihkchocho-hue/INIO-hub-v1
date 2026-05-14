local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Gui = LocalPlayer:WaitForChild("Gui")

local INIO = {Ligado=false, Mar1=true, Missao=true, Coletar=true, Nivel=true, Puzzle=true, VelocidadeAtaque=0.2}

local Tela = Instance.new("ScreenGui")
Tela.Name = "INIO"
Tela.Parent = Gui

local Janela = Instance.new("Frame")
Janela.BackgroundColor3 = Color3.new(0.06, 0.06, 0.1)
Janela.BorderColor3 = Color3.new(0.27, 0.67, 0.98)
Janela.BorderSizePixel = 2
Janela.Position = UDim2.new(0.06, 0, 0.22, 0)
Janela.Size = UDim2.new(0, 270, 0, 400)
Janela.Parent = Tela

local Topo = Instance.new("Frame")
Topo.BackgroundColor3 = Color3.new(0.1, 0.22, 0.43)
Topo.Size = UDim2.new(1, 0, 0, 38)
Topo.BorderSizePixel = 0
Topo.Parent = Janela

local BtnAbrirFechar = Instance.new("TextButton")
BtnAbrirFechar.BackgroundColor3 = Color3.new(0.98, 0.27, 0.27)
BtnAbrirFechar.Position = UDim2.new(0.91, -28, 0.12, 0)
BtnAbrirFechar.Size = UDim2.new(0, 24, 0, 24)
BtnAbrirFechar.Text = ""
BtnAbrirFechar.TextTransparency = 1
BtnAbrirFechar.BorderSizePixel = 0
BtnAbrirFechar.Parent = Topo

local Corpo = Instance.new("Frame")
Corpo.BackgroundTransparency = 1
Corpo.Position = UDim2.new(0, 0, 0, 42)
Corpo.Size = UDim2.new(1, -16, 1, -46)
Corpo.Visible = true
Corpo.Parent = Janela

local JanelaFechada = false
BtnAbrirFechar.MouseButton1Click:Connect(function()
    JanelaFechada = not JanelaFechada
    TweenService:Create(Janela, TweenInfo.new(0.3), {
        Size = JanelaFechada and UDim2.new(0, 270, 0, 38) or UDim2.new(0, 270, 0, 400)
    }):Play()
    Corpo.Visible = not JanelaFechada
end)

local Arrastando, PosInicial, MouseInicial
Topo.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        Arrastando = true
        PosInicial = Janela.Position
        MouseInicial = i.Position
    end
end)
UserInputService.InputChanged:Connect(function(i)
    if Arrastando and i.UserInputType == Enum.UserInputType.MouseMovement then
        local Dif = i.Position - MouseInicial
        Janela.Position = UDim2.new(PosInicial.X.Scale, PosInicial.X.Offset + Dif.X, PosInicial.Y.Scale, PosInicial.Y.Offset + Dif.Y)
    end
end)
Topo.InputEnded:Connect(function() Arrastando = false end)

local function Botao(Y)
    local B = Instance.new("TextButton")
    B.BackgroundColor3 = Color3.new(0.08, 0.15, 0.24)
    B.BorderColor3 = Color3.new(0.35, 0.75, 0.98)
    B.BorderSizePixel = 1
    B.Position = UDim2.new(0, 0, 0, Y)
    B.Size = UDim2.new(1, 0, 0, 34)
    B.Text = ""
    B.TextTransparency = 1
    B.Parent = Corpo
    return B
end

local B1 = Botao(8)
local B2 = Botao(50)
local B3 = Botao(92)
local B4 = Botao(134)
local B5 = Botao(176)
local B6 = Botao(218)

B1.MouseButton1Click:Connect(function()
    INIO.Ligado = not INIO.Ligado
    B1.BackgroundColor3 = INIO.Ligado and Color3.new(0.08, 0.27, 0.12) or Color3.new(0.24, 0.08, 0.12)
end)

B2.MouseButton1Click:Connect(function()
    INIO.Missao = not INIO.Missao
    B2.BackgroundColor3 = INIO.Missao and Color3.new(0.08, 0.27, 0.12) or Color3.new(0.08, 0.15, 0.24)
end)

B3.MouseButton1Click:Connect(function()
    INIO.Coletar = not INIO.Coletar
    B3.BackgroundColor3 = INIO.Coletar and Color3.new(0.08, 0.27, 0.12) or Color3.new(0.08, 0.15, 0.24)
end)

B4.MouseButton1Click:Connect(function()
    INIO.Nivel = not INIO.Nivel
    B4.BackgroundColor3 = INIO.Nivel and Color3.new(0.08, 0.27, 0.12) or Color3.new(0.08, 0.15, 0.24)
end)

B5.MouseButton1Click:Connect(function()
    INIO.Puzzle = not INIO.Puzzle
    B5.BackgroundColor3 = INIO.Puzzle and Color3.new(0.08, 0.27, 0.12) or Color3.new(0.08, 0.15, 0.24)
end)

B6.MouseButton1Click:Connect(function()
    INIO.VelocidadeAtaque = math.max(0.05, INIO.VelocidadeAtaque - 0.03)
end)
B6.MouseButton2Click:Connect(function()
    INIO.VelocidadeAtaque = math.min(1, INIO.VelocidadeAtaque + 0.03)
end)

task.spawn(function()
    while true do
        task.wait(0.05)
        if not INIO.Ligado then continue end

        local Personagem = LocalPlayer.Character
        if not Personagem or not Personagem:FindFirstChild("HumanoidRootPart") or Personagem.Humanoid.Health <= 0 then
            task.wait(2)
            continue
        end
        local HRP = Personagem.HumanoidRootPart

        local NoMar1 = workspace:FindFirstChild("Sea_1") or workspace:FindFirstChild("PrimeiroMar") or workspace:FindFirstChild("Sea1")
        if not NoMar1 and INIO.Mar1 then
            pcall(function() ReplicatedStorage:WaitForChild("Remotes", 3):WaitForChild("Teleport", 3):InvokeServer("Sea1") end)
            task.wait(5)
            continue
        end

        if INIO.Missao then
            pcall(function()
                local NPC = workspace:FindFirstChild("QuestGiver_Sea1", true)
                if NPC then
                    HRP.CFrame = CFrame.new(NPC.Head.Position + Vector3.new(0, 3, -4))
                    task.wait(0.7)
                    ReplicatedStorage.Remotes.AceitarMissao:FireServer(NPC.Name)
                end
                local Inimigo = workspace:FindFirstChild("Mob_Sea1", true)
                if Inimigo and Inimigo:FindFirstChild("Humanoid") and Inimigo.Humanoid.Health > 0 then
                    HRP.CFrame = CFrame.new(Inimigo.Head.Position + Vector3.new(0, 3, -3))
                    task.wait(INIO.VelocidadeAtaque)
                    ReplicatedStorage.Remotes.Atacar:FireServer()
                end
                if LocalPlayer.leaderstats.Level.Value > 5 then
                    local Entregar = workspace:FindFirstChild("QuestEnd_Sea1", true)
                    if Entregar then
                        HRP.CFrame = CFrame.new(Entregar.Head.Position + Vector3.new(0, 3, -4))
                        task.wait(0.7)
                        ReplicatedStorage.Remotes.EntregarMissao:FireServer()
                    end
                end
            end)
        end

        if INIO.Coletar then
            pcall(function()
                for _, Objeto in ipairs(workspace:GetDescendants()) do
                    if Objeto:IsA("BasePart") and (Objeto.Name == "Fruit" or Objeto.Name == "Coin" or Objeto.Name == "Chest" or Objeto.Name == "Item") and Objeto:FindFirstAncestor("Sea_1") then
                        if (HRP.Position - Objeto.Position).Magnitude < 60 then
                            HRP.CFrame = CFrame.new(Objeto.Position + Vector3.new(0, 1.8, 0))
                            task.wait(0.12)
                        end
                    end
                end
            end)
        end

        if INIO.Nivel then
            pcall(function()
                local Lvl = LocalPlayer.leaderstats.Level.Value
                local Exp = LocalPlayer.leaderstats.Exp.Value
                if Exp >= Lvl * 160 then
                    ReplicatedStorage.Remotes.UpLevel:FireServer()
                    task.wait(1)
                end
            end)
        end

        if INIO.Puzzle then
            pcall(function()
                local QuebraCabeca = workspace:FindFirstChild("Puzzles_Sea1", true)
                if QuebraCabeca then
                    for _, Parte in ipairs(QuebraCabeca:GetChildren()) do
                        if Parte:IsA("BasePart") and not Parte:GetAttribute("Feito") then
                            HRP.CFrame = CFrame.new(Parte.Position + Vector3.new(0, 2.5, 0))
                            task.wait(0.28)
                            ReplicatedStorage.Remotes.Interagir:FireServer(Parte.Name)
                            Parte:SetAttribute("Feito", true)
                        end
                    end
                end
            end)
        end
    end
end)
