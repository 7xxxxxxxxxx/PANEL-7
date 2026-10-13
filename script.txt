local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

local parentGui = game:GetService("CoreGui")
pcall(function()
    if not parentGui or not pcall(function() return parentGui.Name end) then
        parentGui = player:WaitForChild("PlayerGui")
    end
end)

if parentGui:FindFirstChild("EvilScriptsTabbed") then
    parentGui.EvilScriptsTabbed:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "EvilScriptsTabbed"
ScreenGui.ResetOnSpawn = false
pcall(function() ScreenGui.Parent = parentGui end)

-- Botón Flotante "7"
local FloatingBtn = Instance.new("TextButton", ScreenGui)
FloatingBtn.Name = "FloatingBtn"
FloatingBtn.BackgroundColor3 = Color3.fromRGB(18, 22, 36)
FloatingBtn.Position = UDim2.new(0.04, 0, 0.25, 0)
FloatingBtn.Size = UDim2.new(0, 48, 0, 48)
FloatingBtn.Font = Enum.Font.SourceSansBold
FloatingBtn.Text = "7"
FloatingBtn.TextColor3 = Color3.fromRGB(0, 170, 255)
FloatingBtn.TextSize = 22
FloatingBtn.Active = true
FloatingBtn.Draggable = true

Instance.new("UICorner", FloatingBtn).CornerRadius = UDim.new(1, 0)
local BtnStroke = Instance.new("UIStroke", FloatingBtn)
BtnStroke.Color = Color3.fromRGB(0, 130, 255)
BtnStroke.Thickness = 2

-- Ventana Principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Name = "MainFrame"
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 17, 26)
MainFrame.Position = UDim2.new(0.15, 0, 0.15, 0)
MainFrame.Size = UDim2.new(0, 520, 0, 320)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = false
MainFrame.ClipsDescendants = true

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
local MainStroke = Instance.new("UIStroke", MainFrame)
MainStroke.Color = Color3.fromRGB(0, 140, 255)
MainStroke.Thickness = 2

-- Barra Superior
local TitleBar = Instance.new("Frame", MainFrame)
TitleBar.Size = UDim2.new(1, 0, 0, 36)
TitleBar.BackgroundColor3 = Color3.fromRGB(22, 26, 42)
Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", TitleBar)
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0.03, 0, 0, 0)
Title.Size = UDim2.new(0.8, 0, 1, 0)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "EVIL SCRIPTS // DELTA EXECUTOR"
Title.TextColor3 = Color3.fromRGB(120, 200, 255)
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", TitleBar)
CloseBtn.BackgroundColor3 = Color3.fromRGB(210, 45, 45)
CloseBtn.Position = UDim2.new(0.92, 0, 0.12, 0)
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Font = Enum.Font.SourceSansBold
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 4)

-- Contenedor Lateral (Pestañas)
local Sidebar = Instance.new("Frame", MainFrame)
Sidebar.Name = "Sidebar"
Sidebar.Position = UDim2.new(0, 0, 0, 36)
Sidebar.Size = UDim2.new(0, 140, 1, -36)
Sidebar.BackgroundColor3 = Color3.fromRGB(20, 23, 35)

local SidebarLayout = Instance.new("UIListLayout", Sidebar)
SidebarLayout.Padding = UDim.new(0, 6)
SidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder

local SidebarPadding = Instance.new("UIPadding", Sidebar)
SidebarPadding.PaddingTop = UDim.new(0, 8)
SidebarPadding.PaddingLeft = UDim.new(0, 6)
SidebarPadding.PaddingRight = UDim.new(0, 6)

-- Área de Contenido
local ContentArea = Instance.new("Frame", MainFrame)
ContentArea.Name = "ContentArea"
ContentArea.Position = UDim2.new(0, 140, 0, 36)
ContentArea.Size = UDim2.new(1, -140, 1, -36)
ContentArea.BackgroundTransparency = 1

----------------------------------------------------
-- SISTEMA DE PESTAÑAS Y KEY
----------------------------------------------------

local isKeyUnlocked = false
local CORRECT_KEY = "EVILSCRIPTS7"
local WHATSAPP_LINK = "https://chat.whatsapp.com/GyyFAPXm5z04Q4ozjxq5nW?s=cl&p=i&mlu=4&ilr=4"

local Tabs = {}
local TabButtons = {}

local function CreateTab(tabName, requiresKey)
    local Scroll = Instance.new("ScrollingFrame", ContentArea)
    Scroll.Name = tabName .. "Scroll"
    Scroll.Size = UDim2.new(1, 0, 1, 0)
    Scroll.BackgroundTransparency = 1
    Scroll.Visible = false
    Scroll.ScrollBarThickness = 5
    Scroll.ScrollBarImageColor3 = Color3.fromRGB(0, 140, 255)
    
    local List = Instance.new("UIListLayout", Scroll)
    List.SortOrder = Enum.SortOrder.LayoutOrder
    List.Padding = UDim.new(0, 8)
    
    local Pad = Instance.new("UIPadding", Scroll)
    Pad.PaddingTop = UDim.new(0, 10)
    Pad.PaddingLeft = UDim.new(0, 10)
    Pad.PaddingRight = UDim.new(0, 10)
    
    List:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        Scroll.CanvasSize = UDim2.new(0, 0, 0, List.AbsoluteContentSize.Y + 20)
    end)
    
    local TabBtn = Instance.new("TextButton", Sidebar)
    TabBtn.Size = UDim2.new(1, 0, 0, 38)
    TabBtn.BackgroundColor3 = Color3.fromRGB(30, 35, 55)
    TabBtn.Font = Enum.Font.SourceSansBold
    TabBtn.Text = tabName
    TabBtn.TextColor3 = Color3.fromRGB(180, 200, 230)
    TabBtn.TextSize = 14
    Instance.new("UICorner", TabBtn).CornerRadius = UDim.new(0, 6)
    
    TabBtn.MouseButton1Click:Connect(function()
        if requiresKey and not isKeyUnlocked then
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Acceso Denegado",
                Text = "Ingresa la Key en la pestaña Principal para desbloquear.",
                Duration = 3
            })
            return
        end
        
        for _, t in pairs(Tabs) do t.Visible = false end
        for _, b in pairs(TabButtons) do 
            b.BackgroundColor3 = Color3.fromRGB(30, 35, 55) 
            b.TextColor3 = Color3.fromRGB(180, 200, 230)
        end
        Scroll.Visible = true
        TabBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 235)
        TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end)
    
    Tabs[tabName] = Scroll
    table.insert(TabButtons, TabBtn)
    return Scroll
end

local function AddActionBtn(parent, text, func)
    local Btn = Instance.new("TextButton", parent)
    Btn.Size = UDim2.new(1, 0, 0, 36)
    Btn.BackgroundColor3 = Color3.fromRGB(25, 30, 48)
    Btn.Font = Enum.Font.SourceSans
    Btn.Text = text
    Btn.TextColor3 = Color3.fromRGB(230, 240, 255)
    Btn.TextSize = 14
    Btn.TextXAlignment = Enum.TextXAlignment.Left
    
    local Pad = Instance.new("UIPadding", Btn)
    Pad.PaddingLeft = UDim.new(0, 10)
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 5)
    
    Btn.MouseButton1Click:Connect(function() pcall(func) end)
    return Btn
end

local function AddTitleHeader(parent, text)
    local Label = Instance.new("TextLabel", parent)
    Label.Size = UDim2.new(1, 0, 0, 24)
    Label.BackgroundTransparency = 1
    Label.Font = Enum.Font.SourceSansBold
    Label.Text = text
    Label.TextColor3 = Color3.fromRGB(0, 170, 255)
    Label.TextSize = 15
    Label.TextXAlignment = Enum.TextXAlignment.Left
    return Label
end

----------------------------------------------------
-- CREACIÓN DE PESTAÑAS
----------------------------------------------------

local TabPrincipal = CreateTab("Principal", false)
local TabJugadores = CreateTab("Jugadores", true)
local TabScripts   = CreateTab("Scripts", true)
local TabJuegos    = CreateTab("Juegos", true)

Tabs["Principal"].Visible = true
TabButtons[1].BackgroundColor3 = Color3.fromRGB(0, 120, 235)
TabButtons[1].TextColor3 = Color3.fromRGB(255, 255, 255)

----------------------------------------------------
-- PESTAÑA PRINCIPAL (KEY Y WHATSAPP)
----------------------------------------------------

local KeySection = Instance.new("Frame", TabPrincipal)
KeySection.Size = UDim2.new(1, 0, 0, 230)
KeySection.BackgroundTransparency = 1

local KeyLayout = Instance.new("UIListLayout", KeySection)
KeyLayout.Padding = UDim.new(0, 8)

local WelcomeLabel = Instance.new("TextLabel", KeySection)
WelcomeLabel.Size = UDim2.new(1, 0, 0, 40)
WelcomeLabel.BackgroundTransparency = 1
WelcomeLabel.Font = Enum.Font.SourceSansBold
WelcomeLabel.Text = "ÚNETE AL CANAL DE WHATSAPP PARA SU KEY"
WelcomeLabel.TextColor3 = Color3.fromRGB(0, 180, 255)
WelcomeLabel.TextSize = 14
WelcomeLabel.TextWrapped = true

local CopyWpBtn = Instance.new("TextButton", KeySection)
CopyWpBtn.Size = UDim2.new(1, 0, 0, 36)
CopyWpBtn.BackgroundColor3 = Color3.fromRGB(37, 211, 102)
CopyWpBtn.Font = Enum.Font.SourceSansBold
CopyWpBtn.Text = "COPIAR LINK DE WHATSAPP"
CopyWpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CopyWpBtn.TextSize = 14
Instance.new("UICorner", CopyWpBtn).CornerRadius = UDim.new(0, 6)

CopyWpBtn.MouseButton1Click:Connect(function()
    pcall(function()
        setclipboard(WHATSAPP_LINK)
    end)
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Enlace Copiado",
        Text = "El link del canal de WhatsApp se copió al portapapeles.",
        Duration = 3
    })
end)

local KeyBox = Instance.new("TextBox", KeySection)
KeyBox.Size = UDim2.new(1, 0, 0, 38)
KeyBox.BackgroundColor3 = Color3.fromRGB(25, 30, 48)
KeyBox.Font = Enum.Font.SourceSansBold
KeyBox.PlaceholderText = "Ingresa tu Key aquí..."
KeyBox.PlaceholderColor3 = Color3.fromRGB(120, 140, 170)
KeyBox.Text = ""
KeyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyBox.TextSize = 14
Instance.new("UICorner", KeyBox).CornerRadius = UDim.new(0, 6)

local KeyBtn = Instance.new("TextButton", KeySection)
KeyBtn.Size = UDim2.new(1, 0, 0, 36)
KeyBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 235)
KeyBtn.Font = Enum.Font.SourceSansBold
KeyBtn.Text = "VERIFICAR KEY"
KeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyBtn.TextSize = 14
Instance.new("UICorner", KeyBtn).CornerRadius = UDim.new(0, 6)

local StatusLabel = Instance.new("TextLabel", KeySection)
StatusLabel.Size = UDim2.new(1, 0, 0, 20)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.Text = "Estado: Esperando Key..."
StatusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
StatusLabel.TextSize = 13

local WelcomeSuccessLabel = Instance.new("TextLabel", TabPrincipal)
WelcomeSuccessLabel.Size = UDim2.new(1, 0, 0, 100)
WelcomeSuccessLabel.BackgroundTransparency = 1
WelcomeSuccessLabel.Font = Enum.Font.SourceSansBold
WelcomeSuccessLabel.Text = "BIENVENIDO A EVILSCRIPTS"
WelcomeSuccessLabel.TextColor3 = Color3.fromRGB(0, 220, 255)
WelcomeSuccessLabel.TextSize = 20
WelcomeSuccessLabel.Visible = false

KeyBtn.MouseButton1Click:Connect(function()
    if KeyBox.Text == CORRECT_KEY then
        isKeyUnlocked = true
        KeySection.Visible = false
        WelcomeSuccessLabel.Visible = true
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Acceso Concedido",
            Text = "¡Key correcta! Todas las opciones han sido desbloqueadas.",
            Duration = 4
        })
    else
        StatusLabel.Text = "Key incorrecta. Revisa el canal de WhatsApp."
        StatusLabel.TextColor3 = Color3.fromRGB(255, 80, 80)
    end
end)

----------------------------------------------------
-- PESTAÑA: JUGADORES (TELEPORT POR NOMBRE DE USUARIO)
----------------------------------------------------
AddTitleHeader(TabJugadores, "Teleport por Username")

local UserBox = Instance.new("TextBox", TabJugadores)
UserBox.Size = UDim2.new(1, 0, 0, 38)
UserBox.BackgroundColor3 = Color3.fromRGB(25, 30, 48)
UserBox.Font = Enum.Font.SourceSansBold
UserBox.PlaceholderText = "Escribe el nombre de usuario..."
UserBox.PlaceholderColor3 = Color3.fromRGB(120, 140, 170)
UserBox.Text = ""
UserBox.TextColor3 = Color3.fromRGB(255, 255, 255)
UserBox.TextSize = 14
Instance.new("UICorner", UserBox).CornerRadius = UDim.new(0, 6)

local TpUserBtn = Instance.new("TextButton", TabJugadores)
TpUserBtn.Size = UDim2.new(1, 0, 0, 36)
TpUserBtn.BackgroundColor3 = Color3.fromRGB(0, 140, 255)
TpUserBtn.Font = Enum.Font.SourceSansBold
TpUserBtn.Text = "TELEPORTAR AL USUARIO"
TpUserBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
TpUserBtn.TextSize = 14
Instance.new("UICorner", TpUserBtn).CornerRadius = UDim.new(0, 6)

TpUserBtn.MouseButton1Click:Connect(function()
    local targetText = UserBox.Text:lower()
    if targetText == "" then return end
    
    local foundPlayer = nil
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= player then
            if p.Name:lower():sub(1, #targetText) == targetText or p.DisplayName:lower():sub(1, #targetText) == targetText then
                foundPlayer = p
                break
            end
        end
    end
    
    if foundPlayer and foundPlayer.Character and foundPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local myChar = player.Character
        if myChar and myChar:FindFirstChild("HumanoidRootPart") then
            myChar.HumanoidRootPart.CFrame = foundPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, 2, 0)
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Teleport Exitoso",
                Text = "Te has teleportado a " .. foundPlayer.Name,
                Duration = 3
            })
        end
    else
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Jugador no encontrado",
            Text = "No se encontró ningún usuario con ese nombre.",
            Duration = 3
        })
    end
end)

AddTitleHeader(TabJugadores, "Opciones de Sobreviviente")

AddActionBtn(TabJugadores, "Super Velocidad (WalkSpeed 45)", function()
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = 45
    end
end)

AddActionBtn(TabJugadores, "Restaurar Velocidad Normal (16)", function()
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = 16
    end
end)

AddActionBtn(TabJugadores, "Salto Infinito", function()
    UserInputService.JumpRequest:Connect(function()
        if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
            player.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end)
end)

AddActionBtn(TabJugadores, "Ver Jugadores en el Bosque (ESP)", function()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= player and p.Character and not p.Character:FindFirstChild("Highlight") then
            local hl = Instance.new("Highlight", p.Character)
            hl.FillColor = Color3.fromRGB(0, 170, 255)
        end
    end
end)

AddActionBtn(TabJugadores, "Teleport al Jugador más Cercano", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local myPos = char.HumanoidRootPart.Position
        local target = nil
        local dist = math.huge
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local d = (p.Character.HumanoidRootPart.Position - myPos).Magnitude
                if d < dist then
                    dist = d
                    target = p.Character.HumanoidRootPart
                end
            end
        end
        if target then
            char.HumanoidRootPart.CFrame = target.CFrame + Vector3.new(0, 2, 0)
        end
    end
end)

----------------------------------------------------
-- PESTAÑA: SCRIPTS
----------------------------------------------------
AddTitleHeader(TabScripts, "Scripts Populares")

AddActionBtn(TabScripts, "Cargar Infinite Yield (Admin)", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

local NoticeLabel = Instance.new("TextLabel", TabScripts)
NoticeLabel.Size = UDim2.new(1, 0, 0, 50)
NoticeLabel.BackgroundTransparency = 1
NoticeLabel.Font = Enum.Font.SourceSansBold
NoticeLabel.Text = "POR AHORA SOLO DISPONIBLE ESE PRONTO TRAERÁN MÁS"
NoticeLabel.TextColor3 = Color3.fromRGB(255, 170, 0)
NoticeLabel.TextSize = 13
NoticeLabel.TextWrapped = true

----------------------------------------------------
-- PESTAÑA: JUEGOS Y RECURSOS
----------------------------------------------------
AddTitleHeader(TabJuegos, "Herramientas de Supervivencia")

AddActionBtn(TabJuegos, "Teleportar Comida Cercana", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local root = char.HumanoidRootPart
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name:lower():find("food") or obj.Name:lower():find("meat") or obj.Name:lower():find("berry") or obj.Name:lower():find("comida") then
                if obj:IsA("BasePart") then obj.CFrame = root.CFrame + Vector3.new(0, 2, 0) end
            end
        end
    end
end)

AddActionBtn(TabJuegos, "Teleportar Piedra y Minerales", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local root = char.HumanoidRootPart
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name:lower():find("stone") or obj.Name:lower():find("rock") or obj.Name:lower():find("metal") or obj.Name:lower():find("iron") then
                if obj:IsA("BasePart") then obj.CFrame = root.CFrame + Vector3.new(0, 2, 0) end
            end
        end
    end
end)

AddActionBtn(TabJuegos, "Teleportar Cofres y Cajas de Suministros", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local root = char.HumanoidRootPart
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name:lower():find("chest") or obj.Name:lower():find("box") or obj.Name:lower():find("crate") or obj.Name:lower():find("caja") or obj.Name:lower():find("suministros") then
                if obj:IsA("BasePart") then 
                    obj.CFrame = root.CFrame + Vector3.new(0, 2, 0)
                elseif obj:IsA("Model") and obj.PrimaryPart then
                    obj:SetPrimaryPartCFrame(root.CFrame + Vector3.new(0, 2, 0))
                end
            end
        end
    end
end)

AddActionBtn(TabJuegos, "Romper Árboles Cercanos", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local root = char.HumanoidRootPart
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name:lower():find("tree") or obj.Name:lower():find("arbol") then
                if obj:IsA("Model") then obj:BreakJoints()
                elseif obj:IsA("BasePart") and (obj.Position - root.Position).Magnitude < 40 then obj:Destroy() end
            end
        end
    end
end)

AddActionBtn(TabJuegos, "Visión Nocturna (FullBright Noche)", function()
    game.Lighting.Brightness = 3
    game.Lighting.ClockTime = 14
    game.Lighting.GlobalShadows = false
end)

AddActionBtn(TabJuegos, "Eliminar Niebla Oscura del Bosque", function()
    game.Lighting.FogEnd = 100000
end)

AddActionBtn(TabJuegos, "Aumentar Campo de Visión (FOV 120)", function()
    workspace.CurrentCamera.FieldOfView = 120
end)

AddActionBtn(TabJuegos, "Quitar FOV (Restaurar FOV Normal 70)", function()
    workspace.CurrentCamera.FieldOfView = 70
end)

AddActionBtn(TabJuegos, "Activar Modo NoClip (Atravesar Árboles)", function()
    RunService.Stepped:Connect(function()
        if player.Character then
            for _, p in pairs(player.Character:GetChildren()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
    end)
end)

AddActionBtn(TabJuegos, "Anti-AFK Automático", function()
    local vu = game:GetService("VirtualUser")
    player.Idled:Connect(function()
        vu:CaptureController()
        vu:ClickButton2(Vector2.new())
    end)
end)

----------------------------------------------------
-- ANIMACIONES DE APERTURA
----------------------------------------------------
local function AnimateOpen()
    MainFrame.Visible = true
    MainFrame.Size = UDim2.new(0, 0, 0, 0)
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out)
    local tween = TweenService:Create(MainFrame, tweenInfo, {Size = UDim2.new(0, 520, 0, 320)})
    tween:Play()
end

CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

FloatingBtn.MouseButton1Click:Connect(function()
    if not MainFrame.Visible then
        AnimateOpen()
    else
        MainFrame.Visible = false
    end
end)
