-- [[ OTIMIZAÇÃO AVANÇADA - ANTI-LAG & FPS BOOST ]] --
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer

-- Remove versão antiga do menu se reexecutado
if CoreGui:FindFirstChild("OtimizacaoAvancadaGui") then
    CoreGui.OtimizacaoAvancadaGui:Destroy()
end

-- Avisar no chat que carregou
game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
    Text = "[OTIMIZAÇÃO]: Script Carregado! Clique em 'UPD' para abrir/fechar.";
    Color = Color3.fromRGB(0, 150, 255);
    Font = Enum.Font.SourceSansBold;
})

-- Variáveis de Estado
local menuAberto = true
local texturasDesativadas = false
local graficoMassinhaAtivo = false
local otimizadorAtivo = false

-- Tabelas para armazenar estados originais das texturas/materiais para reversão
local texturasOriginais = {}
local materiaisOriginais = {}

-- [[ INTERFACE GRÁFICA (UI) ]] --
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "OtimizacaoAvancadaGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

-- Janela Flutuante / Botão "UPD" (Azul Gradiente com Preto)
local BotaoUPD = Instance.new("TextButton")
BotaoUPD.Name = "BotaoUPD"
BotaoUPD.Size = UDim2.new(0, 50, 0, 50)
BotaoUPD.Position = UDim2.new(1, -260, 0.6, 0)
BotaoUPD.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
BotaoUPD.Text = "UPD"
BotaoUPD.TextColor3 = Color3.fromRGB(255, 255, 255)
BotaoUPD.Font = Enum.Font.SourceSansBold
BotaoUPD.TextSize = 18
BotaoUPD.Active = true
BotaoUPD.Draggable = true
BotaoUPD.Parent = ScreenGui

local UICornerUPD = Instance.new("UICorner")
UICornerUPD.CornerRadius = UDim.new(0, 10)
UICornerUPD.Parent = BotaoUPD

local GradienteUPD = Instance.new("UIGradient")
GradienteUPD.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 120, 255)), -- Azul
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))       -- Preto
})
GradienteUPD.Rotation = 90
GradienteUPD.Parent = BotaoUPD

-- Menu Principal (Retângulo Compacto Vertical)
local FrameMenu = Instance.new("Frame")
FrameMenu.Name = "JanelaPrincipal"
FrameMenu.Size = UDim2.new(0, 190, 0, 420)
FrameMenu.Position = UDim2.new(1, -205, 0.25, 0)
FrameMenu.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
FrameMenu.Visible = true
FrameMenu.Active = true
FrameMenu.Draggable = true
FrameMenu.Parent = ScreenGui

local UICornerMenu = Instance.new("UICorner")
UICornerMenu.CornerRadius = UDim.new(0, 12)
UICornerMenu.Parent = FrameMenu

local GradienteMenu = Instance.new("UIGradient")
GradienteMenu.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 80, 200)), -- Azul Escuro
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))      -- Preto
})
GradienteMenu.Rotation = 90
GradienteMenu.Parent = FrameMenu

-- Título do Menu
local LabelTitulo = Instance.new("TextLabel")
LabelTitulo.Size = UDim2.new(1, 0, 0, 35)
LabelTitulo.BackgroundTransparency = 1
LabelTitulo.Text = "Otimização Avançada"
LabelTitulo.TextColor3 = Color3.fromRGB(255, 255, 255)
LabelTitulo.Font = Enum.Font.SourceSansBold
LabelTitulo.TextSize = 16
LabelTitulo.Parent = FrameMenu

-- Painel Fixo de FPS (Contagem de FPS do Celular)
local LabelFPS = Instance.new("TextLabel")
LabelFPS.Size = UDim2.new(1, -20, 0, 30)
LabelFPS.Position = UDim2.new(0, 10, 0, 35)
LabelFPS.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
LabelFPS.BackgroundTransparency = 0.4
-- Letra fina para estética limpa e profissional
LabelFPS.Font = Enum.Font.SourceSansLight
LabelFPS.TextSize = 15
LabelFPS.TextColor3 = Color3.fromRGB(0, 255, 150)
LabelFPS.Text = "FPS: Calculando..."
LabelFPS.Parent = FrameMenu

local UICornerFPS = Instance.new("UICorner")
UICornerFPS.CornerRadius = UDim.new(0, 6)
UICornerFPS.Parent = LabelFPS

-- Sistema Preciso de Contagem de FPS (Atualização Automática Fixo no Painel)
task.spawn(function()
    local tempoAntigo = os.clock()
    local frames = 0
    RunService.RenderStepped:Connect(function()
        frames = frames + 1
        local tempoAtual = os.clock()
        if tempoAtual - tempoAntigo >= 1 then
            LabelFPS.Text = "Performance: " .. frames .. " FPS"
            if frames >= 50 then
                LabelFPS.TextColor3 = Color3.fromRGB(0, 255, 150) -- Verde (Bom)
            elseif frames >= 30 then
                LabelFPS.TextColor3 = Color3.fromRGB(255, 180, 0) -- Laranja (Médio)
            else
                LabelFPS.TextColor3 = Color3.fromRGB(255, 50, 50)  -- Vermelho (Lag)
            end
            frames = 0
            tempoAntigo = tempoAtual
        end
    end)
end)

-- Container de Botões
local ScrollContainer = Instance.new("ScrollingFrame")
ScrollContainer.Size = UDim2.new(1, -10, 1, -85)
ScrollContainer.Position = UDim2.new(0, 5, 0, 75)
ScrollContainer.BackgroundTransparency = 1
ScrollContainer.CanvasSize = UDim2.new(0, 0, 0, 320)
ScrollContainer.ScrollBarThickness = 2
ScrollContainer.Parent = FrameMenu

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)
UIListLayout.Parent = ScrollContainer

-- Criador de Botões Estilizados do Menu
local function criarBotao(texto, ordem, corBase, callback)
    local Botao = Instance.new("TextButton")
    Botao.Size = UDim2.new(1, -6, 0, 45)
    Botao.BackgroundColor3 = corBase
    Botao.Text = texto
    Botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    Botao.Font = Enum.Font.SourceSansBold
    Botao.TextSize = 14
    Botao.LayoutOrder = ordem
    Botao.Parent = ScrollContainer
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 6)
    Corner.Parent = Botao
    
    Botao.MouseButton1Click:Connect(callback)
    return Botao
end

-- Lógica Liga/Desliga da Janela Flutuante "UPD"
BotaoUPD.MouseButton1Click:Connect(function()
    menuAberto = not menuAberto
    FrameMenu.Visible = menuAberto
end)

-- [[ FUNÇÃO 1: DESATIVAR TEXTURAS ]] --
local BotaoTexturas = criarBotao("Remover Texturas: OFF", 1, Color3.fromRGB(40, 40, 40), function() end)

local function alternarTexturas()
    texturasDesativadas = not texturasDesativadas
    if texturasDesativadas then
        BotaoTexturas.Text = "Remover Texturas: LIGADO"
        BotaoTexturas.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        
        -- Varre o mapa removendo texturas visuais pesadas
        for _, obj in ipairs(game:GetDescendants()) do
            if obj:IsA("Texture") or obj:IsA("Decal") or obj:IsA("Sky") then
                texturasOriginais[obj] = obj.Parent
                obj.Parent = nil -- Esconde temporariamente do motor gráfico
            end
        end
    else
        BotaoTexturas.Text = "Remover Texturas: OFF"
        BotaoTexturas.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        
        -- Devolve as texturas para os lugares originais
        for obj, pai in pairs(texturasOriginais) do
            if obj and pai then
                obj.Parent = pai
            end
        end
        texturasOriginais = {}
    end
end
BotaoTexturas.MouseButton1Click:Connect(alternarTexturas)

-- [[ FUNÇÃO 2: GRÁFICO MASSINHA (ESTILO FREE FIRE) ]] --
local BotaoMassinha = criarBotao("Gráfico Massinha: OFF", 2, Color3.fromRGB(40, 40, 40), function() end)

local function alternarMassinha()
    graficoMassinhaAtivo = not graficoMassinhaAtivo
    if graficoMassinhaAtivo then
        BotaoMassinha.Text = "Gráfico Massinha: LIGADO"
        BotaoMassinha.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        
        -- Converte todos os materiais realistas em "SmoothPlastic" liso (Efeito Massinha)
        for _, obj in ipairs(game:GetDescendants()) do
            if obj:IsA("BasePart") and not obj:IsA("MeshPart") then
                materiaisOriginais[obj] = obj.Material
                obj.Material = Enum.Material.SmoothPlastic
            end
        end
    else
        BotaoMassinha.Text = "Gráfico Massinha: OFF"
        BotaoMassinha.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        
        -- Devolve os materiais originais de relevo e reflexo do jogo
        for obj, material em pairs(materiaisOriginais) do
            if obj then
                obj.Material = material
            end
        end
        materiaisOriginais = {}
    end
end
BotaoMassinha.MouseButton1Click:Connect(alternarMassinha)

-- [[ FUNÇÃO 3: OTIMIZADOR DE FPS & REDUTOR DE LAG / INTERNET ]] --
local BotaoOtimizar = criarBotao("Boost Otimizador: OFF", 3, Color3.fromRGB(40, 40, 40), function() end)

BotaoOtimizar.MouseButton1Click:Connect(function()
    otimizadorAtivo = not otimizadorAtivo
    if otimizadorAtivo then
        BotaoOtimizar.Text = "Boost Otimizador: LIGADO"
        BotaoOtimizar.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        
        -- Otimizações internas e agressivas de renderização do Roblox
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        Lighting.GlobalShadows = false
        Lighting.OutdoorLightDirection = Vector3.new(0, 0, 0)
        
        -- Reduz processamento de efeitos atmosféricos pesados que causam lag térmico
        for _, obj in ipairs(Lighting:GetDescendants()) do
            if obj:IsA("PostEffect") or obj:IsA("BloomEffect") or obj:IsA("BlurEffect") or obj:IsA("DepthOfFieldEffect") then
                obj.Enabled = false
            end
        end
        
        -- Altera física de rede interna para priorizar dados e ping baixo (evita lag de internet)
        settings().Network.IncomingReplicationLag = 0
    else
        BotaoOtimizar.Text = "Boost Otimizador: OFF"
        BotaoOtimizar.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        
        -- Restaura qualidade padrão
        settings().Rendering.QualityLevel = Enum.QualityLevel.Default
        Lighting.GlobalShadows = true
        
        for _, obj in ipairs(Lighting:GetDescendants()) do
