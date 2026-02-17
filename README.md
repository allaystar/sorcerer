--[[
    SCRIPT COM GUI - SORCERER TOWER DEFENSE [MEGUNA]
    Interface gráfica dentro do próprio jogo
    Pressione F9 para ver o console se algo não funcionar
--]]

-- CARREGANDO BIBLIOTECAS
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-backups-for-games/main/example.lua"))()
local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()
local replicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local VirtualUser = game:GetService("VirtualUser")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- VARIÁVEIS GLOBAIS
local dinheiroAtual = 0
local gemasAtuais = 0
local waveAtual = 0
local autoFarmAtivo = false
local autoClickAtivo = false
local godModeAtivo = false
local speedHackAtivo = false
local killAuraAtivo = false
local scamTradeAtivo = false
local teleportAtivo = false

-- CRIANDO A INTERFACE PRINCIPAL
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local TabButtons = Instance.new("Frame")
local ResourcesTab = Instance.new("TextButton")
local CombatTab = Instance.new("TextButton")
local TradeTab = Instance.new("TextButton")
local TeleportTab = Instance.new("TextButton")
local ContentFrame = Instance.new("Frame")

-- CONFIGURANDO A GUI
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.Name = "SorcererHackGUI"
ScreenGui.ResetOnSpawn = false

-- FRAME PRINCIPAL
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 500, 0, 400)
MainFrame.Active = true
MainFrame.Draggable = true

-- TÍTULO
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
Title.BorderSizePixel = 0
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Text = "🔮 SORCERER HACK v2.0 [MEGUNA]"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

-- BOTÃO FECHAR
CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
CloseButton.BorderSizePixel = 0
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 16
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- ABAS
TabButtons.Parent = MainFrame
TabButtons.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
TabButtons.BorderSizePixel = 0
TabButtons.Position = UDim2.new(0, 0, 0, 30)
TabButtons.Size = UDim2.new(1, 0, 0, 40)

-- FUNÇÃO PARA CRIAR ABAS
local function criarAba(texto, posX, cor)
    local botao = Instance.new("TextButton")
    botao.Parent = TabButtons
    botao.BackgroundColor3 = cor or Color3.fromRGB(55, 55, 65)
    botao.BorderSizePixel = 0
    botao.Position = UDim2.new(posX, 5, 0, 5)
    botao.Size = UDim2.new(0, 90, 0, 30)
    botao.Text = texto
    botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    botao.Font = Enum.Font.Gotham
    botao.TextSize = 14
    return botao
end

local aba1 = criarAba("💰 RECURSOS", 0, Color3.fromRGB(70, 130, 180))
local aba2 = criarAba("⚔️ COMBATE", 0.2, Color3.fromRGB(180, 70, 70))
local aba3 = criarAba("🤝 TRADE", 0.4, Color3.fromRGB(70, 180, 70))
local aba4 = criarAba("🌍 TELEPORT", 0.6, Color3.fromRGB(180, 180, 70))

-- FRAME DE CONTEÚDO
ContentFrame.Parent = MainFrame
ContentFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
ContentFrame.BorderSizePixel = 0
ContentFrame.Position = UDim2.new(0, 0, 0, 70)
ContentFrame.Size = UDim2.new(1, 0, 1, -70)

-- ==============================================
-- ABA 1: RECURSOS INFINITOS
-- ==============================================
local ResourcesFrame = Instance.new("Frame")
ResourcesFrame.Parent = ContentFrame
ResourcesFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
ResourcesFrame.BorderSizePixel = 0
ResourcesFrame.Size = UDim2.new(1, 0, 1, 0)
ResourcesFrame.Visible = true

-- TÍTULO DA ABA
local resTitle = Instance.new("TextLabel")
resTitle.Parent = ResourcesFrame
resTitle.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
resTitle.BorderSizePixel = 0
resTitle.Size = UDim2.new(1, -20, 0, 30)
resTitle.Position = UDim2.new(0, 10, 0, 10)
resTitle.Text = "💰 RECURSOS INFINITOS"
resTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
resTitle.Font = Enum.Font.GothamBold
resTitle.TextSize = 14

-- FUNÇÃO PARA CRIAR BOTÕES TOGGLE
local function criarToggle(pai, texto, posY, cor, callback)
    local frame = Instance.new("Frame")
    frame.Parent = pai
    frame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    frame.BorderSizePixel = 0
    frame.Position = UDim2.new(0, 10, 0, posY)
    frame.Size = UDim2.new(1, -20, 0, 40)
    
    local label = Instance.new("TextLabel")
    label.Parent = frame
    label.BackgroundTransparency = 1
    label.Position = UDim2.new(0, 10, 0, 0)
    label.Size = UDim2.new(0, 200, 1, 0)
    label.Text = texto
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local botao = Instance.new("TextButton")
    botao.Parent = frame
    botao.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    botao.BorderSizePixel = 0
    botao.Position = UDim2.new(1, -60, 0, 5)
    botao.Size = UDim2.new(0, 50, 0, 30)
    botao.Text = "OFF"
    botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    botao.Font = Enum.Font.GothamBold
    botao.TextSize = 12
    
    local ativo = false
    botao.MouseButton1Click:Connect(function()
        ativo = not ativo
        if ativo then
            botao.BackgroundColor3 = Color3.fromRGB(50, 255, 50)
            botao.Text = "ON"
        else
            botao.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            botao.Text = "OFF"
        end
        callback(ativo)
    end)
    
    return frame
end

-- FUNÇÃO PARA CRIAR BOTÃO NORMAL
local function criarBotao(pai, texto, posY, cor, callback)
    local botao = Instance.new("TextButton")
    botao.Parent = pai
    botao.BackgroundColor3 = cor or Color3.fromRGB(70, 130, 180)
    botao.BorderSizePixel = 0
    botao.Position = UDim2.new(0, 10, 0, posY)
    botao.Size = UDim2.new(1, -20, 0, 40)
    botao.Text = texto
    botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    botao.Font = Enum.Font.Gotham
    botao.TextSize = 14
    botao.MouseButton1Click:Connect(callback)
    return botao
end

-- CRIANDO OS HACKS DA ABA DE RECURSOS
criarToggle(ResourcesFrame, "💰 Dinheiro Infinito", 50, Color3.fromRGB(70, 130, 180), function(estado)
    print("Dinheiro infinito:", estado)
    if estado then
        spawn(function()
            while wait(0.1) do
                -- TENTA ENCONTRAR O DINHEIRO
                if player:FindFirstChild("leaderstats") then
                    for _, stat in pairs(player.leaderstats:GetChildren()) do
                        if stat:IsA("NumberValue") and (stat.Name:lower():find("cash") or stat.Name:lower():find("money") or stat.Name:lower():find("yen")) then
                            stat.Value = 999999999
                        end
                    end
                end
            end
        end)
    end
end)

criarToggle(ResourcesFrame, "💎 Gemas Infinitas", 100, Color3.fromRGB(180, 70, 180), function(estado)
    print("Gemas infinitas:", estado)
    if estado then
        spawn(function()
            while wait(0.1) do
                if player:FindFirstChild("leaderstats") then
                    for _, stat in pairs(player.leaderstats:GetChildren()) do
                        if stat:IsA("NumberValue") and (stat.Name:lower():find("gem") or stat.Name:lower():find("diamond") or stat.Name:lower():find("crystal")) then
                            stat.Value = 999999999
                        end
                    end
                end
            end
        end)
    end
end)

criarBotao(ResourcesFrame, "➕ Adicionar +10.000 Dinheiro", 150, Color3.fromRGB(50, 150, 50), function()
    if player:FindFirstChild("leaderstats") then
        for _, stat in pairs(player.leaderstats:GetChildren()) do
            if stat:IsA("NumberValue") and (stat.Name:lower():find("cash") or stat.Name:lower():find("money")) then
                stat.Value = stat.Value + 10000
            end
        end
    end
end)

criarBotao(ResourcesFrame, "➕ Adicionar +1.000 Gemas", 200, Color3.fromRGB(150, 50, 150), function()
    if player:FindFirstChild("leaderstats") then
        for _, stat in pairs(player.leaderstats:GetChildren()) do
            if stat:IsA("NumberValue") and (stat.Name:lower():find("gem") or stat.Name:lower():find("diamond")) then
                stat.Value = stat.Value + 1000
            end
        end
    end
end)

-- ==============================================
-- ABA 2: COMBATE
-- ==============================================
local CombatFrame = Instance.new("Frame")
CombatFrame.Parent = ContentFrame
CombatFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
CombatFrame.BorderSizePixel = 0
CombatFrame.Size = UDim2.new(1, 0, 1, 0)
CombatFrame.Visible = false

local combTitle = Instance.new("TextLabel")
combTitle.Parent = CombatFrame
combTitle.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
combTitle.BorderSizePixel = 0
combTitle.Size = UDim2.new(1, -20, 0, 30)
combTitle.Position = UDim2.new(0, 10, 0, 10)
combTitle.Text = "⚔️ HACKS DE COMBATE"
combTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
combTitle.Font = Enum.Font.GothamBold
combTitle.TextSize = 14

criarToggle(CombatFrame, "⚡ Auto Farm (Automático)", 50, Color3.fromRGB(200, 100, 0), function(estado)
    autoFarmAtivo = estado
    if estado then
        spawn(function()
            while autoFarmAtivo do
                -- Tenta farmar automaticamente
                local args = {
                    [1] = "ClaimDailyReward"
                }
                replicatedStorage:FindFirstChild("RemoteEvent"):FireServer(unpack(args))
                wait(1)
            end
        end)
    end
end)

criarToggle(CombatFrame, "🛡️ God Mode (Invencível)", 100, Color3.fromRGB(0, 200, 200), function(estado)
    godModeAtivo = estado
    if estado then
        spawn(function()
            while godModeAtivo do
                -- Tenta setar vida infinita
                if player.Character and player.Character:FindFirstChild("Humanoid") then
                    player.Character.Humanoid.MaxHealth = math.huge
                    player.Character.Humanoid.Health = math.huge
                end
                wait(0.1)
            end
        end)
    end
end)

criarToggle(CombatFrame, "💀 Kill Aura (Mata todos)", 150, Color3.fromRGB(200, 0, 0), function(estado)
    killAuraAtivo = estado
    if estado then
        spawn(function()
            while killAuraAtivo do
                local enemies = Workspace:FindFirstChild("Enemies") or Workspace:FindFirstChild("Mobs")
                if enemies then
                    for _, enemy in pairs(enemies:GetChildren()) do
                        if enemy:FindFirstChild("Humanoid") then
                            enemy.Humanoid.Health = 0
                        end
                    end
                end
                wait(0.5)
            end
        end)
    end
end)

criarBotao(CombatFrame, "⚔️ Matar TODOS os inimigos AGORA", 200, Color3.fromRGB(200, 0, 0), function()
    local enemies = Workspace:FindFirstChild("Enemies") or Workspace:FindFirstChild("Mobs")
    if enemies then
        for _, enemy in pairs(enemies:GetChildren()) do
            if enemy:FindFirstChild("Humanoid") then
                enemy.Humanoid.Health = 0
            end
        end
    end
end)

criarBotao(CombatFrame, "⏩ Pular Wave atual", 250, Color3.fromRGB(0, 150, 200), function()
    local remote = replicatedStorage:FindFirstChild("SkipWave") or replicatedStorage:FindFirstChild("NextWave")
    if remote then
        remote:FireServer()
    end
end)

-- ==============================================
-- ABA 3: TRADE / SCAM
-- ==============================================
local TradeFrame = Instance.new("Frame")
TradeFrame.Parent = ContentFrame
TradeFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
TradeFrame.BorderSizePixel = 0
TradeFrame.Size = UDim2.new(1, 0, 1, 0)
TradeFrame.Visible = false

local tradeTitle = Instance.new("TextLabel")
tradeTitle.Parent = TradeFrame
tradeTitle.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
tradeTitle.BorderSizePixel = 0
tradeTitle.Size = UDim2.new(1, -20, 0, 30)
tradeTitle.Position = UDim2.new(0, 10, 0, 10)
tradeTitle.Text = "🤝 SCAM TRADE (USE COM CUIDADO)"
tradeTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
tradeTitle.Font = Enum.Font.GothamBold
tradeTitle.TextSize = 14

-- CAIXA DE TEXTO PARA MENSAGEM
local msgBox = Instance.new("TextBox")
msgBox.Parent = TradeFrame
msgBox.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
msgBox.BorderSizePixel = 0
msgBox.Position = UDim2.new(0, 10, 0, 50)
msgBox.Size = UDim2.new(1, -20, 0, 40)
msgBox.PlaceholderText = "Digite sua mensagem de scam..."
msgBox.TextColor3 = Color3.fromRGB(255, 255, 255)
msgBox.Font = Enum.Font.Gotham
msgBox.TextSize = 14
msgBox.ClearTextOnFocus = false

-- LISTA DE MENSAGENS PRONTAS
local mensagens = {
    "🎁 TROCA RÁPIDA: Dou 10k GEMAS por 1k DINHEIRO!",
    "⚠️ URGENTE: Preciso passar meus itens lendários pra outro char!",
    "💰 GANHE 5x GEMAS - Só hoje! /trade [NOVO PLAYER]",
    "🆘 Alguém me ensina a trocar? Primeira vez aqui!",
    "🔄 DOUBLE GEMS TRADE - Só confiar! Já fiz com 10 pessoas!",
    "💎 Troco 100k DINHEIRO por 10 GEMAS (escrito errado de propósito)",
    "🔥 VENDENDO ITENS LENDÁRIOS por 1 GEMA CADA!",
    "🆓 FREE GEMS - Me add e /trade [FREE GEMS]",
    "🎮 TESTANDO SISTEMA DE TRADE - Quem testar ganha 5k bônus!",
    "🤔 COMO FUNCIONA O TRADE? Alguém me explica? (golpe do distraído)"
}

criarBotao(TradeFrame, "📨 Enviar Mensagem Personalizada", 100, Color3.fromRGB(50, 150, 200), function()
    if msgBox.Text ~= "" then
        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msgBox.Text, "All")
    end
end)

criarBotao(TradeFrame, "📢 Enviar Scam Automático", 150, Color3.fromRGB(200, 100, 0), function()
    scamTradeAtivo = not scamTradeAtivo
    if scamTradeAtivo then
        spawn(function()
            while scamTradeAtivo do
                local msg = mensagens[math.random(1, #mensagens)]
                game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msg, "All")
                wait(10) -- Envia a cada 10 segundos
            end
        end)
    end
end)

criarBotao(TradeFrame, "🔍 Procurar Vítimas no Chat", 200, Color3.fromRGB(200, 50, 50), function()
    spawn(function()
        for i = 1, 10 do
            game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Alguém quer trocar? Tenho itens raros!", "All")
            wait(2)
            game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Quem quiser trade, me chama no privado!", "All")
            wait(2)
        end
    end)
end)

criarBotao(TradeFrame, "💀 GOLPE RÁPIDO (Auto-trade scam)", 250, Color3.fromRGB(150, 0, 0), function()
    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("⚠️ SISTEMA DE TRADE QUEBRADO - Receba 2x itens! Testem!", "All")
    -- ATENÇÃO: Isso é apenas uma simulação, não executa scam real
end)

-- ==============================================
-- ABA 4: TELEPORT
-- ==============================================
local TeleportFrame = Instance.new("Frame")
TeleportFrame.Parent = ContentFrame
TeleportFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
TeleportFrame.BorderSizePixel = 0
TeleportFrame.Size = UDim2.new(1, 0, 1, 0)
TeleportFrame.Visible = false

local teleTitle = Instance.new("TextLabel")
teleTitle.Parent = TeleportFrame
teleTitle.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
teleTitle.BorderSizePixel = 0
teleTitle.Size = UDim2.new(1, -20, 0, 30)
teleTitle.Position = UDim2.new(0, 10, 0, 10)
teleTitle.Text = "🌍 TELEPORTE"
teleTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
teleTitle.Font = Enum.Font.GothamBold
teleTitle.TextSize = 14

local waveBox = Instance.new("TextBox")
waveBox.Parent = TeleportFrame
waveBox.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
waveBox.BorderSizePixel = 0
waveBox.Position = UDim2.new(0, 10, 0, 50)
waveBox.Size = UDim2.new(1, -20, 0, 40)
waveBox.PlaceholderText = "Digite o número da wave (ex: 50)"
waveBox.TextColor3 = Color3.fromRGB(255, 255, 255)
waveBox.Font = Enum.Font.Gotham
waveBox.TextSize = 14

criarBotao(TeleportFrame, "🚀 Teleportar para Wave", 100, Color3.fromRGB(0, 150, 200), function()
    if waveBox.Text ~= "" then
        local waveNum = tonumber(waveBox.Text)
        if waveNum then
            local remote = replicatedStorage:FindFirstChild("TeleportToWave") or 
                          replicatedStorage:FindFirstChild("LoadWave") or
                          replicatedStorage:FindFirstChild("StartWave")
            if remote then
                remote:FireServer(waveNum)
            end
        end
    end
end)

criarBotao(TeleportFrame, "👾 Teleportar para Inimigos", 150, Color3.fromRGB(200, 0, 0), function()
    local enemies = Workspace:FindFirstChild("Enemies") or Workspace:FindFirstChild("Mobs")
    if enemies and #enemies:GetChildren() > 0 then
        local target = enemies:GetChildren()[1]
        if target and target:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
        end
    end
end)

criarBotao(TeleportFrame, "🏃 Speed Hack (10x velocidade)", 200, Color3.fromRGB(0, 200, 0), function()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 160
    end
end)

criarBotao(TeleportFrame, "🔄 Resetar velocidade normal", 250, Color3.fromRGB(200, 200, 0), function()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 16
    end
end)

-- CONTROLE DAS ABAS
aba1.MouseButton1Click:Connect(function()
    ResourcesFrame.Visible = true
    CombatFrame.Visible = false
    TradeFrame.Visible = false
    TeleportFrame.Visible = false
    aba1.BackgroundColor3 = Color3.fromRGB(100, 100, 200)
    aba2.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba3.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba4.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
end)

aba2.MouseButton1Click:Connect(function()
    ResourcesFrame.Visible = false
    CombatFrame.Visible = true
    TradeFrame.Visible = false
    TeleportFrame.Visible = false
    aba1.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba2.BackgroundColor3 = Color3.fromRGB(200, 100, 100)
    aba3.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba4.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
end)

aba3.MouseButton1Click:Connect(function()
    ResourcesFrame.Visible = false
    CombatFrame.Visible = false
    TradeFrame.Visible = true
    TeleportFrame.Visible = false
    aba1.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba2.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba3.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
    aba4.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
end)

aba4.MouseButton1Click:Connect(function()
    ResourcesFrame.Visible = false
    CombatFrame.Visible = false
    TradeFrame.Visible = false
    TeleportFrame.Visible = true
    aba1.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba2.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba3.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
    aba4.BackgroundColor3 = Color3.fromRGB(200, 200, 100)
end)

-- HOTKEY PARA ABRIR/FECHAR A GUI (F6)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.F6 then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

print("✅ GUI CARREGADA! Pressione F6 para abrir/fechar")
print("💰 Recursos infinitos disponíveis")
print("🤝 Scam trade na aba 3")
