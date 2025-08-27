--[[ Painel Debug Universal de Roblox - Botão SPAM Separado
by.icarodesenna
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Tela de Perfil Inicial
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "DebugGuiPerfil"
local perfilFrame = Instance.new("Frame", gui)
perfilFrame.Position = UDim2.new(0.5, -125, 0.5, -100)
perfilFrame.Size = UDim2.new(0, 250, 0, 180)
perfilFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
perfilFrame.BorderSizePixel = 0

local avatar = Instance.new("ImageLabel", perfilFrame)
avatar.Size = UDim2.new(0, 80, 0, 80)
avatar.Position = UDim2.new(0, 20, 0, 20)
avatar.BackgroundTransparency = 1
avatar.Image = "https://www.roblox.com/headshot-thumbnail/image?userId="..LocalPlayer.UserId.."&width=180&height=180&format=png"

local nomeLabel = Instance.new("TextLabel", perfilFrame)
nomeLabel.Size = UDim2.new(0, 120, 0, 40)
nomeLabel.Position = UDim2.new(0, 110, 0, 40)
nomeLabel.BackgroundTransparency = 1
nomeLabel.Text = LocalPlayer.Name
nomeLabel.TextColor3 = Color3.new(1,1,1)
nomeLabel.Font = Enum.Font.SourceSansBold
nomeLabel.TextSize = 22
nomeLabel.TextXAlignment = Enum.TextXAlignment.Left

local abrirBtn = Instance.new("TextButton", perfilFrame)
abrirBtn.Size = UDim2.new(0, 200, 0, 36)
abrirBtn.Position = UDim2.new(0.5, -100, 1, -50)
abrirBtn.BackgroundColor3 = Color3.fromRGB(70,130,180)
abrirBtn.TextColor3 = Color3.new(1,1,1)
abrirBtn.Font = Enum.Font.SourceSansBold
abrirBtn.Text = "Abrir Painel"
abrirBtn.TextSize = 20

-- Painel principal (inicialmente oculto)
local mainFrame = Instance.new("Frame", gui)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.Size = UDim2.new(0, 230, 0, 300)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.Visible = false

-- Botão para abrir/fechar painel
local toggleGuiBtn = Instance.new("TextButton", gui)
toggleGuiBtn.Position = UDim2.new(0, 10, 0, 10)
toggleGuiBtn.Size = UDim2.new(0, 120, 0, 30)
toggleGuiBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleGuiBtn.Text = "Fechar Painel"
toggleGuiBtn.TextColor3 = Color3.new(1,1,1)
toggleGuiBtn.Font = Enum.Font.SourceSansBold
toggleGuiBtn.TextSize = 14
toggleGuiBtn.Visible = false

abrirBtn.MouseButton1Click:Connect(function()
    perfilFrame.Visible = false
    mainFrame.Visible = true
    toggleGuiBtn.Visible = true
end)
toggleGuiBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
    toggleGuiBtn.Text = mainFrame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Mover painel (PC e celular)
local dragging = false
local dragStart, startPos
local function update(input)
    local delta = input.Position - dragStart
    local newPos = startPos + UDim2.new(0, delta.X, 0, delta.Y)
    mainFrame.Position = newPos
    toggleGuiBtn.Position = UDim2.new(newPos.X.Scale, newPos.X.Offset, newPos.Y.Scale, newPos.Y.Offset)
end
local function dragBegin(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end
toggleGuiBtn.InputBegan:Connect(dragBegin)
toggleGuiBtn.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
        update(input)
    end
end)
UIS.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
        update(input)
    end
end)

-- Criador de botões
local function createButton(text)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -10, 0, 30)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 14
    btn.Text = text
    btn.TextWrapped = true
    btn.TextXAlignment = Enum.TextXAlignment.Center
    btn.Position = UDim2.new(0, 5, 0, 0)
    return btn
end

local scroll = Instance.new("ScrollingFrame", mainFrame)
scroll.Position = UDim2.new(0, 0, 0, 0)
scroll.Size = UDim2.new(1, 0, 1, 0)
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
scroll.ScrollBarThickness = 6
scroll.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 4)
layout.SortOrder = Enum.SortOrder.LayoutOrder

-- ESP
        repeat task.wait() until v.Character and v.Character:FindFirstChild("HumanoidRootPart")
        local vHolder = Holder:FindFirstChild(v.Name)
        if not vHolder then
            vHolder = Instance.new("Folder", Holder)
            vHolder.Name = v.Name
        end
        vHolder:ClearAllChildren()
        local box = BoxTemplate:Clone()
        box.Name = v.Name.."Box"
        box.Adornee = v.Character.HumanoidRootPart
        box.Parent = vHolder
        box.Visible = true
        local nameTag = NameTagTemplate:Clone()
        nameTag.Name = v.Name.."NameTag"
        nameTag.Enabled = true
        nameTag.Parent = vHolder
        nameTag.Adornee = v.Character.Head or v.Character.HumanoidRootPart
        nameTag.Tag.Text = v.Name
    end
    
    local function UnloadCharacter(v)
        local vHolder = Holder:FindFirstChild(v.Name)
        if vHolder then
            vHolder:ClearAllChildren()
            vHolder:Destroy()
        end
    end

    for _,v in pairs(Players:GetPlayers()) do
        if v ~= player then
            coroutine.wrap(LoadCharacter)(v)
        end
    end

    Players.PlayerAdded:Connect(function(v)
        if v ~= player then
            coroutine.wrap(LoadCharacter)(v)
        end
    end)
    Players.PlayerRemoving:Connect(function(v)
        UnloadCharacter(v)
    end)
end

local function removeESP()
    local espFolder = game.CoreGui:FindFirstChild("ESP")
    if espFolder then espFolder:Destroy() end
end

-- Infinite Jump
local infJump = false
local infJumpBtn = createButton("Infinite Jump: OFF")
infJumpBtn.Parent = scroll
infJumpBtn.MouseButton1Click:Connect(function()
    infJump = not infJump
    infJumpBtn.Text = "Infinite Jump: " .. (infJump and "ON" or "OFF")
end)
UIS.JumpRequest:Connect(function()
    if infJump and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

-- Noclip
local noclip = false
local noclipBtn = createButton("Noclip: OFF")
noclipBtn.Parent = scroll
noclipBtn.MouseButton1Click:Connect(function()
    noclip = not noclip
    noclipBtn.Text = "Noclip: " .. (noclip and "ON" or "OFF")
end)
RunService.Stepped:Connect(function()
    if noclip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
    end
end)

local flyBtn = createButton("Fly gui v3: OFF")
flyBtn.MouseButton1Click:Connect(function()
    flyBtn.Text = "Fly gui v3: ON"
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-Gui-45909"))()
end)
flyBtn.Parent = scroll

-- ProximityPrompt sem tempo
for _, p in pairs(game:GetDescendants()) do
    if p:IsA("ProximityPrompt") then
        p.HoldDuration = 0
    end
end
game.DescendantAdded:Connect(function(d)
    if d:IsA("ProximityPrompt") then
        d.HoldDuration = 0
    end
end)

-- ========== BOTÃO SPAM ABILITY EVENT SEPARADO ==========
local remote = game:GetService("ReplicatedStorage"):FindFirstChild("AbilityEvent") -- Troque o nome se necessário!
local spamBtn = createButton("Abrir SPAM")
spamBtn.Parent = scroll

-- Modal Spam
local spamModal = Instance.new("Frame", gui)
spamModal.Size = UDim2.new(0, 200, 0, 120)
spamModal.Position = UDim2.new(0.5, -100, 0.5, -60)
spamModal.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
spamModal.Visible = false
spamModal.Active = true

local spamTitle = Instance.new("TextLabel", spamModal)
spamTitle.Size = UDim2.new(1, 0, 0, 36)
spamTitle.Position = UDim2.new(0, 0, 0, 0)
spamTitle.BackgroundTransparency = 1
spamTitle.Text = "SPAM AbilityEvent"
spamTitle.TextColor3 = Color3.fromRGB(255, 235, 78)
spamTitle.Font = Enum.Font.SourceSansBold
spamTitle.TextSize = 18

local spamToggleBtn = createButton("SPAM OFF")
spamToggleBtn.Size = UDim2.new(1, -20, 0, 40)
spamToggleBtn.Position = UDim2.new(0, 10, 0, 46)
spamToggleBtn.Parent = spamModal

local closeSpamBtn = createButton("Fechar")
closeSpamBtn.Size = UDim2.new(1, -20, 0, 30)
closeSpamBtn.Position = UDim2.new(0, 10, 1, -40)
closeSpamBtn.Parent = spamModal

local abilCon
spamToggleBtn.MouseButton1Click:Connect(function()
    if not abilCon then
        spamToggleBtn.Text = "SPAM ON"
        abilCon = RunService.RenderStepped:Connect(function()
            if remote then
                remote:FireServer(1) -- Troque o argumento se necessário
            end
        end)
    else
        spamToggleBtn.Text = "SPAM OFF"
        abilCon:Disconnect()
        abilCon = nil
    end
end)

spamBtn.MouseButton1Click:Connect(function()
    spamModal.Visible = true
end)
closeSpamBtn.MouseButton1Click:Connect(function()
    spamModal.Visible = false
    if abilCon then
        abilCon:Disconnect()
        abilCon = nil
        spamToggleBtn.Text = "SPAM OFF"
    end
end)

-- Fechar modal ao clicar fora
UIS.InputBegan:Connect(function(input, gp)
    if spamModal.Visible and input.UserInputType == Enum.UserInputType.MouseButton1 then
        local mouse = UIS:GetMouseLocation()
        local x, y = mouse.X, mouse.Y
        local absPos = spamModal.AbsolutePosition
        local absSize = spamModal.AbsoluteSize
        if not (x > absPos.X and x < absPos.X + absSize.X and y > absPos.Y and y < absPos.Y + absSize.Y) then
            spamModal.Visible = false
            if abilCon then
                abilCon:Disconnect()
                abilCon = nil
                spamToggleBtn.Text = "SPAM OFF"
            end
        end
    end
end)

-- === MOVIMENTAÇÃO LIVRE DO SPAM MODAL ===
local spamDragging = false
local spamDragStart, spamStartPos

local function spamUpdate(input)
    local delta = input.Position - spamDragStart
    local newPos = spamStartPos + UDim2.new(0, delta.X, 0, delta.Y)
    spamModal.Position = newPos
end

local function spamDragBegin(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        spamDragging = true
        spamDragStart = input.Position
        spamStartPos = spamModal.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                spamDragging = false
            end
        end)
    end
end

spamModal.InputBegan:Connect(spamDragBegin)
spamModal.InputChanged:Connect(function(input)
    if spamDragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
        spamUpdate(input)
    end
end)
UIS.InputChanged:Connect(function(input)
    if spamDragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
        spamUpdate(input)
    end
end)

-- Lista de Jogadores para TP (toggle)
local tpBtn = createButton("Mostrar Jogadores")
tpBtn.Name = "MostrarJogadoresBtn"
tpBtn.Parent = scroll
local function limparBotoesJogadores()
    for _, c in pairs(scroll:GetChildren()) do
        if c:IsA("TextButton") and c.Name ~= "MostrarJogadoresBtn" and c ~= tpBtn and c ~= espBtn and c ~= infJumpBtn and c ~= noclipBtn and c ~= flyBtn and c ~= spamBtn then
            c:Destroy()
        end
    end
end
local listaAberta = false
tpBtn.MouseButton1Click:Connect(function()
    if listaAberta then
        limparBotoesJogadores()
        listaAberta = false
        tpBtn.Text = "Mostrar Jogadores"
    else
        limparBotoesJogadores()
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local dist = "N/A"
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local d = (p.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                    dist = tostring(math.floor(d)) .. "m"
                end
                local playerBtn = createButton(p.Name .. " (" .. dist .. ")")
                playerBtn.MouseButton1Click:Connect(function()
                    LocalPlayer.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(2, 0, 0)
                end)
                playerBtn.Parent = scroll
            end
        end
        listaAberta = true
        tpBtn.Text = "Fechar Jogadores"
    end
end)

-- === SPEED CONTROL ===
local speedAtivo = false
local speedValor = 32 -- valor padrão de velocidade
local humanoidCon
local speedFrame = Instance.new("Frame", scroll)
speedFrame.Size = UDim2.new(1, -10, 0, 36)
speedFrame.BackgroundTransparency = 1
speedFrame.LayoutOrder = 99999

local speedBox = Instance.new("TextBox", speedFrame)
speedBox.Size = UDim2.new(0.5, -5, 1, 0)
speedBox.Position = UDim2.new(0, 0, 0, 0)
speedBox.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
speedBox.TextColor3 = Color3.new(1,1,1)
speedBox.Font = Enum.Font.SourceSansBold
speedBox.TextSize = 14
speedBox.Text = tostring(speedValor)
speedBox.PlaceholderText = "Velocidade"

local speedBtn = Instance.new("TextButton", speedFrame)
speedBtn.Size = UDim2.new(0.5, -5, 1, 0)
speedBtn.Position = UDim2.new(0.5, 5, 0, 0)
speedBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
speedBtn.TextColor3 = Color3.new(1,1,1)
speedBtn.Font = Enum.Font.SourceSansBold
speedBtn.TextSize = 14
speedBtn.Text = "Speed: OFF"

speedBtn.MouseButton1Click:Connect(function()
    speedAtivo = not speedAtivo
    speedBtn.Text = "Speed: " .. (speedAtivo and "ON" or "OFF")
    if humanoidCon then humanoidCon:Disconnect() humanoidCon = nil end
    if speedAtivo then
        speedValor = tonumber(speedBox.Text) or 32
        humanoidCon = RunService.Stepped:Connect(function()
            local char = LocalPlayer.Character
            if char and char:FindFirstChildOfClass("Humanoid") then
                char:FindFirstChildOfClass("Humanoid").WalkSpeed = speedValor
            end
        end)
    else
        local char = LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            char:FindFirstChildOfClass("Humanoid").WalkSpeed = 16
        end
    end
end)

speedBox.FocusLost:Connect(function()
    speedValor = tonumber(speedBox.Text) or 32
    if speedAtivo and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = speedValor
    end
end)

speedFrame.Parent = scroll

-- ProximityPrompt sem tempo
for _, p in pairs(game:GetDescendants()) do
	if p:IsA("ProximityPrompt") then
		p.HoldDuration = 0
	end
end
game.DescendantAdded:Connect(function(d)
	if d:IsA("ProximityPrompt") then
		d.HoldDuration = 0
	end
end)

-- Assinatura
local assinatura = Instance.new("TextLabel")
assinatura.Size = UDim2.new(1, -10, 0, 20)
assinatura.BackgroundTransparency = 1
assinatura.TextColor3 = Color3.fromRGB(200, 200, 200)
assinatura.Font = Enum.Font.SourceSansItalic
assinatura.TextSize = 14
assinatura.Text = "by.icarodesenna"
assinatura.TextXAlignment = Enum.TextXAlignment.Center
assinatura.Parent = scroll

RunService.RenderStepped:Connect(function()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10)
end)
