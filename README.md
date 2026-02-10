local p = game:GetService("Players").LocalPlayer
local gui = p:WaitForChild("PlayerGui")
local inputS = game:GetService("UserInputService")

if gui:FindFirstChild("HELL_HUB") then gui.HELL_HUB:Destroy() end

local screen = Instance.new("ScreenGui", gui)
screen.Name = "HELL_HUB"
screen.ResetOnSpawn = false

local lang = "RU"

-- База данных кодов игр (Вставляй код игры между [[ ]])
local games_data = {
    ["CLICKER"] = [[ 
        print("Запуск кликера...")
        -- HELL CLICKER: HARDCORE 45 STAGES (No Reset Mode)
local HttpService = game:GetService("HttpService")
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")
local FileName = "HellHardcoreNoReset.txt"

-- Сокращение чисел
local function Format(n)
    local signs = {"", "K", "M", "B", "T", "Qd", "Qi", "Sx", "Sp", "Oc", "No", "Dc"}
    local i = 1
    while n >= 1000 and i < #signs do
        n = n / 1000
        i = i + 1
    end
    return (math.floor(n * 100) / 100) .. signs[i]
end

-- Загрузка данных
local function LoadData()
    local default = {Money = 0, Best = 0, Multi = 1, Auto = 0, Unlocked = 0}
    if isfile and isfile(FileName) then
        local success, result = pcall(function() return HttpService:JSONDecode(readfile(FileName)) end)
        if success and result then return result end
    end
    return default
end

local data = LoadData()
_G.HC_Money = data.Money
_G.HC_Best = data.Best
_G.HC_Multi = data.Multi
_G.HC_Auto = data.Auto
_G.HC_Unlocked = data.Unlocked or 0

-- Сохранение данных
local function SaveData()
    local payload = {
        Money = _G.HC_Money,
        Best = _G.HC_Best,
        Multi = _G.HC_Multi,
        Auto = _G.HC_Auto,
        Unlocked = _G.HC_Unlocked
    }
    writefile(FileName, HttpService:JSONEncode(payload))
end

-- Создание UI
if PlayerGui:FindFirstChild("HellNoReset") then PlayerGui.HellNoReset:Destroy() end
local ScreenGui = Instance.new("ScreenGui", PlayerGui); ScreenGui.Name = "HellNoReset"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 350, 0, 500); Main.Position = UDim2.new(0.5, -175, 0.5, -250)
Main.BackgroundColor3 = Color3.fromRGB(15, 0, 0); Main.Active = true; Main.Draggable = true
Instance.new("UICorner", Main)

-- Кнопка X
local Close = Instance.new("TextButton", Main)
Close.Size = UDim2.new(0, 35, 0, 35); Close.Position = UDim2.new(1, -40, 0, 5)
Close.Text = "X"; Close.BackgroundColor3 = Color3.fromRGB(180, 0, 0); Close.TextColor3 = Color3.new(1,1,1)
Close.Font = "GothamBold"; Close.MouseButton1Click:Connect(function() SaveData(); ScreenGui:Destroy() end)
Instance.new("UICorner", Close)

-- Рекорд
local BestL = Instance.new("TextLabel", Main)
BestL.Size = UDim2.new(0, 200, 0, 30); BestL.Position = UDim2.new(0, 10, 0, 5)
BestL.Text = "RECORD: $" .. Format(_G.HC_Best); BestL.TextColor3 = Color3.fromRGB(255, 200, 0)
BestL.BackgroundTransparency = 1; BestL.Font = "GothamBold"; BestL.TextXAlignment = "Left"

-- Баланс
local ML = Instance.new("TextLabel", Main)
ML.Size = UDim2.new(1, 0, 0, 50); ML.Position = UDim2.new(0, 0, 0, 40)
ML.Text = "$" .. Format(_G.HC_Money); ML.TextColor3 = Color3.new(1, 1, 1); ML.TextSize = 35; ML.Font = "GothamBold"; ML.BackgroundTransparency = 1

-- Кнопка клика
local Click = Instance.new("TextButton", Main)
Click.Size = UDim2.new(0, 100, 0, 100); Click.Position = UDim2.new(0.5, -50, 0, 100)
Click.Text = "TAP"; Click.BackgroundColor3 = Color3.fromRGB(200, 0, 0); Click.TextColor3 = Color3.new(1,1,1); Click.Font = "GothamBold"
Instance.new("UICorner", Click).CornerRadius = UDim.new(1, 0)

-- Магазин
local Shop = Instance.new("ScrollingFrame", Main)
Shop.Size = UDim2.new(1, -20, 0, 280); Shop.Position = UDim2.new(0, 10, 1, -290)
Shop.BackgroundTransparency = 1; Shop.CanvasSize = UDim2.new(0, 0, 25, 0); Shop.ScrollBarThickness = 4
Instance.new("UIListLayout", Shop).Padding = UDim.new(0, 5)

-- Генерация 45 ступеней
for i = 1, 45 do
    local isAuto = (i % 2 == 0)
    local price = 100 * (10 ^ (i * 1.15)) 
    local power = 10 * (8 ^ (i * 0.75))
    
    if i > _G.HC_Unlocked then
        local btn = Instance.new("TextButton", Shop)
        btn.Size = UDim2.new(1, -10, 0, 50); btn.BackgroundColor3 = Color3.fromRGB(35, 5, 5)
        btn.TextColor3 = Color3.new(1,1,1); btn.Font = "SourceSansBold"; btn.TextSize = 13
        btn.Text = "STAGE "..i..": "..(isAuto and "Auto" or "Click").."\nCost: $"..Format(price).." | +"..Format(power)
        Instance.new("UICorner", btn)
        
        btn.MouseButton1Click:Connect(function()
            if _G.HC_Money >= price then
                _G.HC_Money = _G.HC_Money - price
                _G.HC_Unlocked = i
                if isAuto then _G.HC_Auto = _G.HC_Auto + power else _G.HC_Multi = _G.HC_Multi + power end
                btn:Destroy()
                SaveData()
            end
        end)
    end
end

-- Основной цикл
task.spawn(function()
    local lastSave = tick()
    while ScreenGui.Parent do
        _G.HC_Money = _G.HC_Money + (_G.HC_Auto / 10)
        ML.Text = "$" .. Format(_G.HC_Money)
        
        if _G.HC_Money > _G.HC_Best then 
            _G.HC_Best = _G.HC_Money 
            BestL.Text = "RECORD: $" .. Format(_G.HC_Best)
        end
        
        if tick() - lastSave > 10 then
            SaveData()
            lastSave = tick()
        end
        task.wait(0.1)
    end
end)

Click.MouseButton1Click:Connect(function()
    _G.HC_Money = _G.HC_Money + _G.HC_Multi
    Click:TweenSize(UDim2.new(0, 90, 0, 90), "Out", "Quad", 0.05, true)
    task.wait(0.05); Click:TweenSize(UDim2.new(0, 100, 0, 100), "Out", "Quad", 0.05, true)
end)
    ]],
    ["SNAKE"] = [[ 
        print("Запуск змейки...")
       -- Snake: Final Edition with X and Restart
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("SnakeFinal") then PlayerGui.SnakeFinal:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "SnakeFinal"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 450, 0, 450)
MainFrame.Position = UDim2.new(0.5, -225, 0.5, -225)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.Active = true
MainFrame.Draggable = true

-- КНОПКА X (Закрыть)
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- КНОПКА RESTART
local RestartBtn = Instance.new("TextButton", MainFrame)
RestartBtn.Size = UDim2.new(0, 100, 0, 30)
RestartBtn.Position = UDim2.new(0, 140, 0, 10)
RestartBtn.Text = "RESTART"
RestartBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
RestartBtn.TextColor3 = Color3.new(1, 1, 1)
RestartBtn.Font = Enum.Font.GothamBold

local GameBoard = Instance.new("Frame", MainFrame)
GameBoard.Size = UDim2.new(0, 300, 0, 300)
GameBoard.Position = UDim2.new(0, 10, 0, 80)
GameBoard.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
GameBoard.ClipsDescendants = true

local ScoreLabel = Instance.new("TextLabel", MainFrame)
ScoreLabel.Size = UDim2.new(0, 120, 0, 30); ScoreLabel.Position = UDim2.new(0, 10, 0, 10)
ScoreLabel.Text = "SCORE: 0"; ScoreLabel.TextColor3 = Color3.new(1,1,1); ScoreLabel.Font = "GothamBold"; ScoreLabel.BackgroundTransparency = 1

local snake = {}
local direction = Vector2.new(1, 0)
local apple = nil
local score = 0
local playing = true

local function spawnApple()
    if apple then apple:Destroy() end
    apple = Instance.new("Frame", GameBoard)
    apple.Size = UDim2.new(0, 18, 0, 18)
    apple.BackgroundColor3 = Color3.new(1, 0, 0)
    apple.Position = UDim2.new(0, math.random(0, 14) * 20 + 1, 0, math.random(0, 14) * 20 + 1)
    Instance.new("UICorner", apple).CornerRadius = UDim.new(1, 0)
end

local function reset()
    for _, v in pairs(snake) do v:Destroy() end
    snake = {}
    score = 0
    direction = Vector2.new(1, 0)
    for i = 3, 1, -1 do
        local part = Instance.new("Frame", GameBoard)
        part.Size = UDim2.new(0, 18, 0, 18)
        part.BackgroundColor3 = Color3.new(0, 1, 0)
        part.Position = UDim2.new(0, i * 20 + 1, 0, 101)
        table.insert(snake, part)
    end
    spawnApple()
    playing = true
    ScoreLabel.Text = "SCORE: 0"
end

RestartBtn.MouseButton1Click:Connect(reset)

-- Управление
local function createArrow(pos, text, dir)
    local b = Instance.new("TextButton", MainFrame)
    b.Size = UDim2.new(0, 40, 0, 40); b.Position = pos; b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(60,60,60); b.TextColor3 = Color3.new(1,1,1)
    b.MouseButton1Click:Connect(function() 
        if dir == "U" and direction.Y == 0 then direction = Vector2.new(0, -1)
        elseif dir == "D" and direction.Y == 0 then direction = Vector2.new(0, 1)
        elseif dir == "L" and direction.X == 0 then direction = Vector2.new(-1, 0)
        elseif dir == "R" and direction.X == 0 then direction = Vector2.new(1, 0) end
    end)
end

createArrow(UDim2.new(0, 370, 0, 150), "↑", "U")
createArrow(UDim2.new(0, 370, 0, 230), "↓", "D")
createArrow(UDim2.new(0, 330, 0, 190), "←", "L")
createArrow(UDim2.new(0, 410, 0, 190), "→", "R")

reset()

task.spawn(function()
    while ScreenGui.Parent do
        if playing then
            local headPos = snake[1].Position
            local newX = headPos.X.Offset + (direction.X * 20)
            local newY = headPos.Y.Offset + (direction.Y * 20)
            
            if newX < 0 or newX >= 300 or newY < 0 or newY >= 300 then playing = false end
            for i = 2, #snake do if snake[i].Position == UDim2.new(0, newX, 0, newY) then playing = false end end
            
            if playing then
                local newPart = Instance.new("Frame", GameBoard)
                newPart.Size = UDim2.new(0, 18, 0, 18); newPart.BackgroundColor3 = Color3.new(0, 1, 0)
                newPart.Position = UDim2.new(0, newX, 0, newY)
                table.insert(snake, 1, newPart)
                
                if newPart.Position == apple.Position then
                    score = score + 1; ScoreLabel.Text = "SCORE: " .. score; spawnApple()
                else
                    snake[#snake]:Destroy(); table.remove(snake, #snake)
                end
            else
                ScoreLabel.Text = "GAME OVER!"
            end
        end
        task.wait(math.max(0.05, 0.18 - (score * 0.005)))
    end
end) 
    ]],
    ["ARCANOID"] = [[ print("-- Arkanoid: Close Button Edition
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("ArkanoidFinal") then PlayerGui.ArkanoidFinal:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "ArkanoidFinal"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 420, 0, 400)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.Active = true
MainFrame.Draggable = true

-- КНОПКА ЗАКРЫТЬ (X)
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local GameBoard = Instance.new("Frame", MainFrame)
GameBoard.Size = UDim2.new(0, 250, 0, 350)
GameBoard.Position = UDim2.new(0, 10, 0, 40)
GameBoard.BackgroundColor3 = Color3.new(0, 0, 0)
GameBoard.ClipsDescendants = true

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 0, 0, 5)
Title.Text = "ARKANOID"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.BackgroundTransparency = 1

-- Управление
local Controls = Instance.new("Frame", MainFrame)
Controls.Size = UDim2.new(0, 140, 0, 100)
Controls.Position = UDim2.new(0, 270, 0, 80)
Controls.BackgroundTransparency = 1

local function createBtn(pos, text)
    local b = Instance.new("TextButton", Controls)
    b.Size = UDim2.new(0, 65, 0, 65)
    b.Position = pos
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    b.TextColor3 = Color3.new(1, 1, 1)
    b.TextSize = 30
    return b
end

local btnLeft = createBtn(UDim2.new(0, 0, 0, 0), "◄")
local btnRight = createBtn(UDim2.new(1, -65, 0, 0), "►")

local RestartBtn = Instance.new("TextButton", MainFrame)
RestartBtn.Size = UDim2.new(0, 120, 0, 40)
RestartBtn.Position = UDim2.new(0, 280, 0, 300)
RestartBtn.Text = "RESTART"
RestartBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
RestartBtn.TextColor3 = Color3.new(1, 1, 1)

-- Логика
local paddlePos = 100
local ballPos = Vector2.new(125, 250)
local ballDir = Vector2.new(2.5, -2.5)
local bricks = {}
local playing = true
local ballSize = 10

local function spawnBrick(x, y, color)
    local b = Instance.new("Frame", GameBoard)
    b.Size = UDim2.new(0, 38, 0, 15)
    b.Position = UDim2.new(0, x * 41 + 4, 0, y * 18 + 10)
    b.BackgroundColor3 = color
    b.BorderSizePixel = 1
    table.insert(bricks, b)
end

local function generateRound()
    for _, v in pairs(bricks) do v:Destroy() end
    bricks = {}
    ballPos = Vector2.new(125, 250)
    local style = math.random(1, 4)
    if style == 1 then
        for y = 0, 3 do for x = 0, 5 do spawnBrick(x, y, Color3.fromHSV(y/4, 0.7, 1)) end end
    elseif style == 2 then
        for y = 0, 5 do for x = 0, 5 do if x % 2 == 0 then spawnBrick(x, y, Color3.new(0.4, 0.4, 1)) end end end
    elseif style == 3 then
        for y = 0, 5 do for x = 0, 5 do if (x+y) % 3 == 0 then spawnBrick(x, y, Color3.new(0.4, 1, 0.4)) end end end
    else
        for y = 0, 5 do for x = 0, 5 do if math.random() > 0.5 then spawnBrick(x, y, Color3.new(1, 0.4, 0.4)) end end end
    end
end

local paddle = Instance.new("Frame", GameBoard)
paddle.Size = UDim2.new(0, 55, 0, 10)
paddle.BackgroundColor3 = Color3.new(1, 1, 1)

local ball = Instance.new("Frame", GameBoard)
ball.Size = UDim2.new(0, ballSize, 0, ballSize)
ball.BackgroundColor3 = Color3.new(1, 1, 0)

local function update()
    if not playing then return end
    
    for i = 1, 2 do
        local nextPos = ballPos + (ballDir / 2)
        -- Коллизии
        if nextPos.X <= 0 or nextPos.X >= 250 - ballSize then ballDir = Vector2.new(-ballDir.X, ballDir.Y) break end
        if nextPos.Y <= 0 then ballDir = Vector2.new(ballDir.X, -ballDir.Y) break end
        if nextPos.Y >= 320 and nextPos.Y <= 330 and nextPos.X >= paddlePos - 5 and nextPos.X <= paddlePos + 55 then
            ballDir = Vector2.new(ballDir.X, -math.abs(ballDir.Y)) break
        end
        for idx, b in pairs(bricks) do
            local bx, by = b.Position.X.Offset, b.Position.Y.Offset
            if nextPos.X + ballSize >= bx and nextPos.X <= bx + 38 and nextPos.Y + ballSize >= by and nextPos.Y <= by + 15 then
                ballDir = Vector2.new(ballDir.X, -ballDir.Y)
                b:Destroy()
                table.remove(bricks, idx)
                break
            end
        end
        ballPos = nextPos
    end
    
    if ballPos.Y >= 340 then playing = false Title.Text = "GAME OVER" end
    if #bricks == 0 then generateRound() end
    
    ball.Position = UDim2.new(0, ballPos.X, 0, ballPos.Y)
    paddle.Position = UDim2.new(0, paddlePos, 0, 330)
end

local lD, rD = false, false
btnLeft.MouseButton1Down:Connect(function() lD = true end)
btnLeft.MouseButton1Up:Connect(function() lD = false end)
btnRight.MouseButton1Down:Connect(function() rD = true end)
btnRight.MouseButton1Up:Connect(function() rD = false end)

RestartBtn.MouseButton1Click:Connect(function()
    generateRound()
    ballDir = Vector2.new(2.5, -2.5)
    playing = true
    Title.Text = "ARKANOID"
end)

generateRound()
task.spawn(function()
    while ScreenGui.Parent do
        update()
        if lD then paddlePos = math.max(0, paddlePos - 6) end
        if rD then paddlePos = math.min(195, paddlePos + 6) end
        task.wait(0.02)
    end
end)") ]],
    ["2048"] = [[ print("-- ROBLOX 2048 COMPACT & SMOOTH (MOBILE READY)
local HttpService = game:GetService("HttpService")
local TweenService = game:GetService("TweenService")
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")
local FileName = "2048_Compact_Save.txt"

local function LoadBest()
    if isfile and isfile(FileName) then
        local success, result = pcall(function() return HttpService:JSONDecode(readfile(FileName)) end)
        if success and result then return result.Best or 0 end
    end
    return 0
end

local BestScore = LoadBest()
local CurrentScore = 0
local matrix = {{0,0,0,0},{0,0,0,0},{0,0,0,0},{0,0,0,0}}

-- UI
if PlayerGui:FindFirstChild("Game2048Compact") then PlayerGui.Game2048Compact:Destroy() end
local ScreenGui = Instance.new("ScreenGui", PlayerGui); ScreenGui.Name = "Game2048Compact"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 320, 0, 480); Main.Position = UDim2.new(0.5, -160, 0.5, -240)
Main.BackgroundColor3 = Color3.fromRGB(187, 173, 160); Main.Active = true; Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- Плавность для UI
local function Pop(obj)
    obj.Size = UDim2.new(0, 0, 0, 0)
    TweenService:Create(obj, TweenInfo.new(0.2, Enum.EasingStyle.Back), {Size = UDim2.new(0, 68, 0, 68)}):Play()
end

-- Верхняя панель
local Header = Instance.new("Frame", Main)
Header.Size = UDim2.new(1, -20, 0, 80); Header.Position = UDim2.new(0, 10, 0, 10); Header.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", Header)
Title.Text = "2048"; Title.Size = UDim2.new(0, 80, 1, 0); Title.TextColor3 = Color3.fromRGB(119, 110, 101)
Title.TextSize = 30; Title.Font = "GothamBold"; Title.BackgroundTransparency = 1

local ScoreBox = Instance.new("TextLabel", Header)
ScoreBox.Size = UDim2.new(0, 80, 0, 40); ScoreBox.Position = UDim2.new(0, 90, 0, 10)
ScoreBox.BackgroundColor3 = Color3.fromRGB(143, 130, 119); ScoreBox.TextColor3 = Color3.new(1,1,1)
ScoreBox.Text = "SCORE\n0"; ScoreBox.TextSize = 10; Instance.new("UICorner", ScoreBox)

local BestBox = Instance.new("TextLabel", Header)
BestBox.Size = UDim2.new(0, 80, 0, 40); BestBox.Position = UDim2.new(0, 180, 0, 10)
BestBox.BackgroundColor3 = Color3.fromRGB(143, 130, 119); BestBox.TextColor3 = Color3.new(1,1,1)
BestBox.Text = "BEST\n" .. BestScore; BestBox.TextSize = 10; Instance.new("UICorner", BestBox)

local Close = Instance.new("TextButton", Main)
Close.Size = UDim2.new(0, 25, 0, 25); Close.Position = UDim2.new(1, -30, 0, 5)
Close.Text = "X"; Close.BackgroundColor3 = Color3.fromRGB(200, 0, 0); Close.TextColor3 = Color3.new(1,1,1)
Close.Font = "GothamBold"; Close.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)
Instance.new("UICorner", Close)

-- Игровое поле (компактное)
local Grid = Instance.new("Frame", Main)
Grid.Size = UDim2.new(0, 300, 0, 300); Grid.Position = UDim2.new(0.5, -150, 0, 90)
Grid.BackgroundColor3 = Color3.fromRGB(143, 130, 119)
Instance.new("UICorner", Grid)

local cells = {}
for r = 1, 4 do
    for c = 1, 4 do
        local cell = Instance.new("TextLabel", Grid)
        cell.Size = UDim2.new(0, 68, 0, 68)
        cell.Position = UDim2.new(0, (c-1)*73 + 6, 0, (r-1)*73 + 6)
        cell.BackgroundColor3 = Color3.fromRGB(205, 193, 180)
        cell.Text = ""; cell.Font = "GothamBold"; cell.TextSize = 22; cell.TextColor3 = Color3.new(1,1,1)
        Instance.new("UICorner", cell)
        cells[(r-1)*4 + c] = cell
    end
end

local colors = {[2]=Color3.fromRGB(238,228,218),[4]=Color3.fromRGB(237,224,200),[8]=Color3.fromRGB(242,177,121),[16]=Color3.fromRGB(245,149,99),[32]=Color3.fromRGB(246,124,95),[64]=Color3.fromRGB(246,94,59),[128]=Color3.fromRGB(237,207,114),[256]=Color3.fromRGB(237,204,97),[512]=Color3.fromRGB(237,200,80),[1024]=Color3.fromRGB(237,197,63),[2048]=Color3.fromRGB(237,194,46)}

local function UpdateUI(spawning)
    for r=1,4 do for c=1,4 do
        local val = matrix[r][c]
        local cell = cells[(r-1)*4 + c]
        local oldText = cell.Text
        cell.Text = val == 0 and "" or tostring(val)
        cell.BackgroundColor3 = colors[val] or (val>0 and Color3.fromRGB(60,58,50) or Color3.fromRGB(205,193,180))
        cell.TextColor3 = (val <= 4) and Color3.fromRGB(119,110,101) or Color3.new(1,1,1)
        
        if spawning and oldText == "" and cell.Text ~= "" then Pop(cell) end
    end end
    ScoreBox.Text = "SCORE\n"..CurrentScore
    if CurrentScore > BestScore then
        BestScore = CurrentScore
        BestBox.Text = "BEST\n"..BestScore
        if writefile then writefile(FileName, HttpService:JSONEncode({Best = BestScore})) end
    end
end

local function SpawnTile()
    local empty = {}
    for r=1,4 do for c=1,4 do if matrix[r][c] == 0 then table.insert(empty, {r,c}) end end end
    if #empty > 0 then
        local p = empty[math.random(1,#empty)]
        matrix[p[1]][p[2]] = math.random() < 0.9 and 2 or 4
        return true
    end
end

local function Move(dir)
    local old = HttpService:JSONEncode(matrix)
    local function process(line)
        local n = {}
        for i=1,4 do if line[i] ~= 0 then table.insert(n, line[i]) end end
        for i=1,#n-1 do if n[i] == n[i+1] then n[i] *= 2; CurrentScore += n[i]; table.remove(n, i+1) end end
        while #n < 4 do table.insert(n, 0) end
        return n
    end
    for i=1,4 do
        local line = {}
        if dir=="Left" or dir=="Right" then
            for j=1,4 do line[j] = matrix[i][j] end
            if dir=="Right" then line = {line[4],line[3],line[2],line[1]} end
            line = process(line)
            if dir=="Right" then line = {line[4],line[3],line[2],line[1]} end
            for j=1,4 do matrix[i][j] = line[j] end
        else
            for j=1,4 do line[j] = matrix[j][i] end
            if dir=="Down" then line = {line[4],line[3],line[2],line[1]} end
            line = process(line)
            if dir=="Down" then line = {line[4],line[3],line[2],line[1]} end
            for j=1,4 do matrix[j][i] = line[j] end
        end
    end
    if old ~= HttpService:JSONEncode(matrix) then SpawnTile(); UpdateUI(true) end
end

-- Управление
local Controls = Instance.new("Frame", Main)
Controls.Size = UDim2.new(0, 150, 0, 80); Controls.Position = UDim2.new(0.5, -75, 1, -85); Controls.BackgroundTransparency = 1

local function Btn(name, pos, d)
    local b = Instance.new("TextButton", Controls)
    b.Size = UDim2.new(0, 40, 0, 40); b.Position = pos; b.Text = name
    b.BackgroundColor3 = Color3.fromRGB(119, 110, 101); b.TextColor3 = Color3.new(1,1,1)
    b.MouseButton1Click:Connect(function() Move(d) end)
    Instance.new("UICorner", b)
end
Btn("↑", UDim2.new(0.5, -20, 0, 0), "Up"); Btn("↓", UDim2.new(0.5, -20, 0, 42), "Down")
Btn("←", UDim2.new(0, 10, 0, 21), "Left"); Btn("→", UDim2.new(1, -50, 0, 21), "Right")

local Res = Instance.new("TextButton", Main)
Res.Size = UDim2.new(0, 70, 0, 25); Res.Position = UDim2.new(1, -80, 0, 50)
Res.Text = "Restart"; Res.BackgroundColor3 = Color3.fromRGB(119, 110, 101); Res.TextColor3 = Color3.new(1,1,1); Res.TextSize = 10
Res.MouseButton1Click:Connect(function() 
    matrix = {{0,0,0,0},{0,0,0,0},{0,0,0,0},{0,0,0,0}}; CurrentScore = 0; SpawnTile(); SpawnTile(); UpdateUI(true) 
end)
Instance.new("UICorner", Res)

game:GetService("UserInputService").InputBegan:Connect(function(i, g)
    if not g then
        if i.KeyCode == Enum.KeyCode.W or i.KeyCode == Enum.KeyCode.Up then Move("Up")
        elseif i.KeyCode == Enum.KeyCode.S or i.KeyCode == Enum.KeyCode.Down then Move("Down")
        elseif i.KeyCode == Enum.KeyCode.A or i.KeyCode == Enum.KeyCode.Left then Move("Left")
        elseif i.KeyCode == Enum.KeyCode.D or i.KeyCode == Enum.KeyCode.Right then Move("Right") end
    end
end)

SpawnTile(); SpawnTile(); UpdateUI(true)") ]],
    ["BIRD"] = [[ print("-- Flappy Bird: Balanced Physics Edition
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("FlappyBalanced") then PlayerGui.FlappyBalanced:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "FlappyBalanced"

-- Компактное окно
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 300, 0, 420)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(113, 197, 207)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame)

-- Верхняя панель
local TopBar = Instance.new("Frame", MainFrame)
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Instance.new("UICorner", TopBar)

local CloseBtn = Instance.new("TextButton", TopBar)
CloseBtn.Size = UDim2.new(0, 35, 1, 0); CloseBtn.Position = UDim2.new(1, -35, 0, 0)
CloseBtn.Text = "X"; CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1); CloseBtn.Font = "GothamBold"
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

local RestartBtn = Instance.new("TextButton", TopBar)
RestartBtn.Size = UDim2.new(0, 80, 0, 25); RestartBtn.Position = UDim2.new(0, 5, 0, 5)
RestartBtn.Text = "RESTART"; RestartBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
RestartBtn.TextColor3 = Color3.new(1, 1, 1); RestartBtn.TextSize = 12; RestartBtn.Font = "GothamBold"

_G.FlappyBest = _G.FlappyBest or 0
local BestLabel = Instance.new("TextLabel", TopBar)
BestLabel.Size = UDim2.new(0, 80, 1, 0); BestLabel.Position = UDim2.new(0, 90, 0, 0)
BestLabel.Text = "BEST: " .. _G.FlappyBest; BestLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
BestLabel.BackgroundTransparency = 1; BestLabel.Font = "GothamBold"; BestLabel.TextSize = 14

-- Поле
local GameBoard = Instance.new("Frame", MainFrame)
GameBoard.Size = UDim2.new(1, -10, 1, -120); GameBoard.Position = UDim2.new(0, 5, 0, 45)
GameBoard.BackgroundTransparency = 0.9; GameBoard.ClipsDescendants = true

local bird = Instance.new("Frame", GameBoard)
bird.Size = UDim2.new(0, 26, 0, 20); bird.BackgroundColor3 = Color3.fromRGB(255, 230, 0)
Instance.new("UICorner", bird)

local ScoreLabel = Instance.new("TextLabel", GameBoard)
ScoreLabel.Size = UDim2.new(1, 0, 0, 40); ScoreLabel.Text = "0"
ScoreLabel.TextColor3 = Color3.new(1,1,1); ScoreLabel.TextSize = 30; ScoreLabel.BackgroundTransparency = 1; ScoreLabel.Font = "GothamBold"

-- Кнопка UP
local UpBtn = Instance.new("TextButton", MainFrame)
UpBtn.Size = UDim2.new(1, -20, 0, 60); UpBtn.Position = UDim2.new(0, 10, 1, -70)
UpBtn.Text = "JUMP ↑"; UpBtn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
UpBtn.TextColor3 = Color3.new(1, 1, 1); UpBtn.Font = "GothamBold"; UpBtn.TextSize = 25
Instance.new("UICorner", UpBtn)

-- НАСТРОЙКИ ФИЗИКИ (Мягче)
local velocity = 0
local gravity = 0.35 -- Было 0.5, теперь падает медленнее
local flapPower = -5.5 -- Было -7, теперь прыжок не такой резкий
local pipes = {}
local playing = true
local score = 0

local function reset()
    for _, p in pairs(pipes) do p.top:Destroy(); p.bot:Destroy() end
    pipes = {}; score = 0; velocity = 0; playing = true
    bird.Position = UDim2.new(0, 40, 0.4, 0)
    ScoreLabel.Text = "0"
    BestLabel.Text = "BEST: " .. _G.FlappyBest
end

RestartBtn.MouseButton1Click:Connect(reset)
UpBtn.MouseButton1Click:Connect(function() if playing then velocity = flapPower end end)

local function spawnPipe()
    local gapY = math.random(30, 170)
    local gapSize = 100 -- Чуть увеличил проход
    local topP = Instance.new("Frame", GameBoard)
    topP.Size = UDim2.new(0, 50, 0, gapY); topP.Position = UDim2.new(1, 0, 0, 0); topP.BackgroundColor3 = Color3.fromRGB(40, 120, 40)
    local botP = Instance.new("Frame", GameBoard)
    botP.Size = UDim2.new(0, 50, 1, -(gapY + gapSize)); botP.Position = UDim2.new(1, 0, 0, gapY + gapSize); botP.BackgroundColor3 = Color3.fromRGB(40, 120, 40)
    table.insert(pipes, {top = topP, bot = botP, x = 300, passed = false})
end

reset()

task.spawn(function()
    local timer = 0
    while ScreenGui.Parent do
        if playing then
            timer = timer + 1
            if timer >= 80 then spawnPipe(); timer = 0 end
            
            velocity = velocity + gravity
            bird.Position = UDim2.new(0, 40, 0, bird.Position.Y.Offset + velocity)
            
            if bird.Position.Y.Offset < -20 or bird.Position.Y.Offset > GameBoard.AbsoluteSize.Y then playing = false end
            
            for i = #pipes, 1, -1 do
                local p = pipes[i]
                p.x = p.x - 2.5 -- Замедлил движение труб
                p.top.Position = UDim2.new(0, p.x, 0, 0)
                p.bot.Position = UDim2.new(0, p.x, 0, p.bot.Position.Y.Offset)
                
                if p.x < 66 and p.x > 14 then
                    if bird.Position.Y.Offset < p.top.Size.Y.Offset or bird.Position.Y.Offset + 20 > p.bot.Position.Y.Offset then
                        playing = false
                    end
                end
                
                if not p.passed and p.x < 40 then
                    p.passed = true; score = score + 1; ScoreLabel.Text = score
                end
                
                if p.x < -60 then p.top:Destroy(); p.bot:Destroy(); table.remove(pipes, i) end
            end
            
            if not playing then
                if score > _G.FlappyBest then _G.FlappyBest = score end
                BestLabel.Text = "BEST: " .. _G.FlappyBest
                ScoreLabel.Text = "GAME OVER"
            end
        end
        task.wait(0.016)
    end
end)") ]],
    ["RACING"] = [[ print("-- Racing: Pro Traffic (X + Restart + Best Score)
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("RacingPro") then PlayerGui.RacingPro:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "RacingPro"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 450, 0, 400)
MainFrame.Position = UDim2.new(0.5, -225, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.Active = true
MainFrame.Draggable = true

-- КНОПКА X (Закрыть)
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30); CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"; CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1); CloseBtn.Font = "GothamBold"
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- КНОПКА RESTART
local RestartBtn = Instance.new("TextButton", MainFrame)
RestartBtn.Size = UDim2.new(0, 100, 0, 30); RestartBtn.Position = UDim2.new(0, 175, 0, 10)
RestartBtn.Text = "RESTART"; RestartBtn.BackgroundColor3 = Color3.fromRGB(50, 120, 200)
RestartBtn.TextColor3 = Color3.new(1, 1, 1); RestartBtn.Font = "GothamBold"

-- Статистика (Score & Best)
local ScoreLabel = Instance.new("TextLabel", MainFrame)
ScoreLabel.Size = UDim2.new(0, 160, 0, 30); ScoreLabel.Position = UDim2.new(0, 270, 0, 50)
ScoreLabel.Text = "SCORE: 0"; ScoreLabel.TextColor3 = Color3.new(1, 1, 1); ScoreLabel.Font = "GothamBold"; ScoreLabel.TextSize = 22; ScoreLabel.BackgroundTransparency = 1

local BestLabel = Instance.new("TextLabel", MainFrame)
BestLabel.Size = UDim2.new(0, 160, 0, 30); BestLabel.Position = UDim2.new(0, 270, 0, 80)
BestLabel.Text = "BEST: " .. (_G.RacingBest or 0)
BestLabel.TextColor3 = Color3.fromRGB(255, 215, 0); BestLabel.Font = "GothamBold"; BestLabel.TextSize = 18; BestLabel.BackgroundTransparency = 1

local GameBoard = Instance.new("Frame", MainFrame)
GameBoard.Size = UDim2.new(0, 240, 0, 350); GameBoard.Position = UDim2.new(0, 15, 0, 40)
GameBoard.BackgroundColor3 = Color3.fromRGB(40, 40, 40); GameBoard.ClipsDescendants = true

local lanes = {10, 65, 125, 185}
local enemies = {}
local score = 0
local speed = 6
local playing = true
_G.RacingBest = _G.RacingBest or 0

local function createEnemy(laneIdx)
    local enemy = Instance.new("Frame", GameBoard)
    enemy.Size = UDim2.new(0, 45, 0, 65); enemy.Position = UDim2.new(0, lanes[laneIdx], 0, -80)
    enemy.BackgroundColor3 = Color3.fromHSV(math.random(), 0.6, 0.8)
    Instance.new("UICorner", enemy).CornerRadius = UDim.new(0, 8)
    table.insert(enemies, {obj = enemy, lane = laneIdx, y = -80})
end

local function reset()
    for _, e in pairs(enemies) do e.obj:Destroy() end
    enemies = {}; score = 0; speed = 6; playing = true
    ScoreLabel.Text = "SCORE: 0"
end

RestartBtn.MouseButton1Click:Connect(reset)

-- Управление
local btnL = Instance.new("TextButton", MainFrame)
btnL.Size = UDim2.new(0, 80, 0, 80); btnL.Position = UDim2.new(0, 275, 0, 180); btnL.Text = "<"
local btnR = Instance.new("TextButton", MainFrame)
btnR.Size = UDim2.new(0, 80, 0, 80); btnR.Position = UDim2.new(0, 355, 0, 180); btnR.Text = ">"

local car = Instance.new("Frame", GameBoard)
car.Size = UDim2.new(0, 45, 0, 65); car.BackgroundColor3 = Color3.new(1, 0.2, 0.2); Instance.new("UICorner", car)
local currentLane = 2

btnL.MouseButton1Click:Connect(function() if currentLane > 1 and playing then currentLane = currentLane - 1 end end)
btnR.MouseButton1Click:Connect(function() if currentLane < 4 and playing then currentLane = currentLane + 1 end end)

task.spawn(function()
    local spawnTimer = 0
    while ScreenGui.Parent do
        if playing then
            spawnTimer = spawnTimer + 1
            if spawnTimer >= math.max(12, 45 - math.floor(score/3)) then
                local laneCount = math.random(1, 3)
                local availableLanes = {1, 2, 3, 4}
                for i = 1, laneCount do
                    local pick = math.random(1, #availableLanes)
                    createEnemy(availableLanes[pick]); table.remove(availableLanes, pick)
                end
                spawnTimer = 0
            end
            
            car.Position = UDim2.new(0, lanes[currentLane], 0, 270)
            
            for i = #enemies, 1, -1 do
                local e = enemies[i]
                e.y = e.y + speed
                e.obj.Position = UDim2.new(0, lanes[e.lane], 0, e.y)
                
                if e.lane == currentLane and e.y + 60 > 270 and e.y < 330 then
                    playing = false
                    ScoreLabel.Text = "CRASH!"
                    if score > _G.RacingBest then
                        _G.RacingBest = score
                        BestLabel.Text = "BEST: " .. score
                    end
                end
                
                if e.y > 350 then
                    e.obj:Destroy(); table.remove(enemies, i)
                    score = score + 1
                    ScoreLabel.Text = "SCORE: " .. score
                    if score % 10 == 0 then speed = speed + 0.5 end
                end
            end
        end
        task.wait(0.02)
    end
end)") ]],
    ["WORDLE"] = [[ print("local p = game:GetService("Players").LocalPlayer
local pg = p:WaitForChild("PlayerGui")
if pg:FindFirstChild("WordleFinal") then pg.WordleFinal:Destroy() end

local sg = Instance.new("ScreenGui", pg); sg.Name = "WordleFinal"

-- ГЛАВНОЕ ОКНО
local main = Instance.new("Frame", sg)
main.Size = UDim2.new(0, 400, 0, 650)
main.Position = UDim2.new(0.5, -200, 0.5, -325)
main.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
main.Active = true; main.Draggable = true
Instance.new("UICorner", main)

local frameStroke = Instance.new("UIStroke", main)
frameStroke.Color = Color3.fromRGB(100, 100, 100)
frameStroke.Thickness = 1

-- Кнопка закрытия
local closeBtn = Instance.new("TextButton", main)
closeBtn.Size = UDim2.new(0, 35, 0, 35); closeBtn.Position = UDim2.new(1, -40, 0, 5)
closeBtn.Text = "X"; closeBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
closeBtn.TextColor3 = Color3.new(1, 1, 1); closeBtn.Font = "GothamBold"
Instance.new("UICorner", closeBtn)
closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 60); title.Text = "WORDLE"
title.TextColor3 = Color3.new(1, 1, 1); title.Font = "GothamBold"; title.TextSize = 30; title.BackgroundTransparency = 1

-- Сетка игры
local grid = Instance.new("Frame", main)
grid.Size = UDim2.new(0, 260, 0, 310); grid.Position = UDim2.new(0.5, -130, 0, 70)
grid.BackgroundTransparency = 1
local layout = Instance.new("UIGridLayout", grid)
layout.CellSize = UDim2.new(0, 48, 0, 48); layout.CellPadding = UDim2.new(0, 5, 0, 5)

local cells = {}
for i = 1, 30 do
    local cell = Instance.new("TextLabel", grid)
    cell.Text = ""; cell.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    cell.TextColor3 = Color3.new(1, 1, 1); cell.Font = "GothamBold"; cell.TextSize = 26
    local s = Instance.new("UIStroke", cell)
    s.Color = Color3.fromRGB(80, 80, 80); s.Thickness = 2
    Instance.new("UICorner", cell)
    cells[i] = cell
end

-- Клавиатура
local kb = Instance.new("Frame", main)
kb.Size = UDim2.new(1, -20, 0, 160); kb.Position = UDim2.new(0, 10, 0, 400); kb.BackgroundTransparency = 1
local kLayout = Instance.new("UIGridLayout", kb)
kLayout.CellSize = UDim2.new(0, 33, 0, 40); kLayout.CellPadding = UDim2.new(0, 4, 0, 4)

local dictionary = {"АРБУЗ", "БАНКА", "ВЕТЕР", "ГОРОД", "ДВЕРЬ", "ЗАМОК", "ИГРОК", "КНИГА", "ЛОДКА", "МЕТРО", "НОМЕР", "ОКЕАН", "ПЕСОК", "РЕЧКА", "СПОРТ", "ТУМАН", "ШКОЛА", "ЭКРАН", "ЯГОДА", "ПОЕЗД", "ПЛИТА", "РУЧКА", "СЛОВО", "ФАКЕЛ", "ПОЧТА", "ОБЛАКО", "ЭФИР", "ТЕКСТ", "ПРАВО"}
local alphabet = "АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ"
local currentWord, currentAttempt, guess = "", 1, ""
local isGameOver = false

function getChars(str)
    local chars = {}
    for c in str:gmatch("[%z\1-\127\194-\244][\128-\191]*") do table.insert(chars, c) end
    return chars
end

function startNewGame()
    currentWord = dictionary[math.random(#dictionary)]
    currentAttempt = 1; guess = ""; isGameOver = false
    title.Text = "WORDLE"; title.TextColor3 = Color3.new(1, 1, 1)
    for _, cell in ipairs(cells) do
        cell.Text = ""; cell.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        cell.UIStroke.Color = Color3.fromRGB(80, 80, 80)
    end
end

function check()
    if isGameOver then return end
    local gChars = getChars(guess)
    if #gChars < 5 then return end
    
    local targetChars = getChars(currentWord)
    local startIdx = (currentAttempt - 1) * 5
    local matched = {}

    for i = 1, 5 do
        local cell = cells[startIdx + i]
        if gChars[i] == targetChars[i] then
            cell.BackgroundColor3 = Color3.fromRGB(83, 141, 78)
            cell.UIStroke.Color = Color3.fromRGB(83, 141, 78)
            matched[i] = true
        end
    end
    
    for i = 1, 5 do
        local cell = cells[startIdx + i]
        if gChars[i] ~= targetChars[i] then
            local found = false
            for j = 1, 5 do
                if not matched[j] and gChars[i] == targetChars[j] then
                    cell.BackgroundColor3 = Color3.fromRGB(181, 159, 59)
                    cell.UIStroke.Color = Color3.fromRGB(181, 159, 59)
                    matched[j] = true; found = true; break
                end
            end
            if not found then
                cell.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                cell.UIStroke.Color = Color3.fromRGB(70, 70, 70)
            end
        end
    end

    if guess == currentWord then
        title.Text = "ПОБЕДА!"; title.TextColor3 = Color3.new(0, 1, 0)
        isGameOver = true
    elseif currentAttempt >= 6 then
        title.Text = "ОТВЕТ: " .. currentWord; title.TextColor3 = Color3.new(1, 0, 0)
        isGameOver = true
    else
        currentAttempt = currentAttempt + 1; guess = ""
    end
end

-- Клавиши букв
for _, char in ipairs(getChars(alphabet)) do
    local btn = Instance.new("TextButton", kb)
 btn.Text = char; btn.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
    btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = "GothamBold"; btn.TextSize = 14
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(function()
        if isGameOver then return end
        if #getChars(guess) < 5 then
            guess = guess .. char
            cells[(currentAttempt - 1) * 5 + #getChars(guess)].Text = char
        end
    end)
end

-- КНОПКИ УПРАВЛЕНИЯ
local btnFrame = Instance.new("Frame", main)
btnFrame.Size = UDim2.new(1, 0, 0, 50); btnFrame.Position = UDim2.new(0, 0, 0, 565); btnFrame.BackgroundTransparency = 1

local enter = Instance.new("TextButton", btnFrame)
enter.Size = UDim2.new(0, 90, 0, 40); enter.Position = UDim2.new(0.5, -45, 0, 0)
enter.Text = "ВВОД"; enter.BackgroundColor3 = Color3.fromRGB(83, 141, 78); enter.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", enter); enter.MouseButton1Click:Connect(check)

local del = Instance.new("TextButton", btnFrame)
del.Size = UDim2.new(0, 90, 0, 40); del.Position = UDim2.new(0, 15, 0, 0)
del.Text = "СТЕРЕТЬ"; del.BackgroundColor3 = Color3.fromRGB(120, 40, 40); del.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", del)
del.MouseButton1Click:Connect(function()
    if isGameOver then return end
    local gChars = getChars(guess)
    if #gChars > 0 then
        cells[(currentAttempt - 1) * 5 + #gChars].Text = ""
        local newGuess = ""
        for i = 1, #gChars - 1 do newGuess = newGuess .. gChars[i] end
        guess = newGuess
    end
end)

local restart = Instance.new("TextButton", btnFrame)
restart.Size = UDim2.new(0, 110, 0, 40); restart.Position = UDim2.new(1, -125, 0, 0)
restart.Text = "НОВАЯ ИГРА"; restart.BackgroundColor3 = Color3.fromRGB(50, 50, 150); restart.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", restart); restart.MouseButton1Click:Connect(startNewGame)

startNewGame()") ]],
    ["GALLOWS"] = [[ print("local p = game:GetService("Players").LocalPlayer
local pg = p:WaitForChild("PlayerGui")
if pg:FindFirstChild("GallowsKeyboard") then pg.GallowsKeyboard:Destroy() end

local sg = Instance.new("ScreenGui", pg); sg.Name = "GallowsKeyboard"

local main = Instance.new("Frame", sg)
main.Size = UDim2.new(0, 450, 0, 550) -- Увеличил размер под кнопки
main.Position = UDim2.new(0.5, -225, 0.5, -275)
main.BackgroundColor3 = Color3.fromRGB(25, 30, 35)
main.Active = true; main.Draggable = true
Instance.new("UICorner", main)

-- Кнопка закрытия (X)
local closeBtn = Instance.new("TextButton", main)
closeBtn.Size = UDim2.new(0, 35, 0, 35); closeBtn.Position = UDim2.new(1, -40, 0, 5)
closeBtn.Text = "X"; closeBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
closeBtn.TextColor3 = Color3.new(1, 1, 1); closeBtn.Font = "GothamBold"; closeBtn.TextSize = 20
Instance.new("UICorner", closeBtn)
closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)

local scoreLabel = Instance.new("TextLabel", main)
scoreLabel.Size = UDim2.new(1, -50, 0, 30); scoreLabel.Position = UDim2.new(0, 15, 0, 5)
scoreLabel.BackgroundTransparency = 1; scoreLabel.TextColor3 = Color3.new(0.6, 1, 0.6)
scoreLabel.Text = "СЕРИЯ ПОБЕД: 0"; scoreLabel.Font = "GothamBold"; scoreLabel.TextXAlignment = "Left"; scoreLabel.TextSize = 18

local art = Instance.new("TextLabel", main)
art.Size = UDim2.new(1, 0, 0, 120); art.Position = UDim2.new(0, 0, 0, 40)
art.BackgroundTransparency = 1; art.TextColor3 = Color3.new(1, 1, 1)
art.Font = "Code"; art.TextSize = 20; art.Text = ""

local wordDisplay = Instance.new("TextLabel", main)
wordDisplay.Size = UDim2.new(1, -20, 0, 50); wordDisplay.Position = UDim2.new(0, 10, 0, 160)
wordDisplay.BackgroundTransparency = 1; wordDisplay.TextColor3 = Color3.new(1, 1, 1)
wordDisplay.Font = "GothamBold"; wordDisplay.TextScaled = true

-- Сетка для кнопок
local keyContainer = Instance.new("Frame", main)
keyContainer.Size = UDim2.new(1, -20, 0, 280); keyContainer.Position = UDim2.new(0, 10, 0, 230)
keyContainer.BackgroundTransparency = 1

local layout = Instance.new("UIGridLayout", keyContainer)
layout.CellSize = UDim2.new(0, 35, 0, 35); layout.CellPadding = UDim2.new(0, 5, 0, 5)
layout.HorizontalAlignment = "Center"

local dictionary = {"МАШИНА", "СОБАКА", "АРБУЗ", "МОЛОКО", "ШКОЛА", "ЯБЛОКО", "ДОМИК", "КУРИЦА", "ДЕРЕВО", "ЦВЕТОК", "РАДУГА", "БАНАН", "ГРИБОК", "ЛОШАДЬ", "ОГУРЕЦ", "ПОМИДОР", "СОЛНЦЕ", "ОБЛАКО", "ТЕТРАДЬ", "МАЛИНА", "ПОЕЗД", "САМОЛЕТ", "РЫБКА", "ЗВЕЗДА", "КОРОВА", "УЛИТКА", "ЧАЙНИК", "КНИГА", "ЛОЖКА", "ВИЛКА", "ДИВАН", "ОКНО"}
local alphabet = "АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ"

local chosenWord, guessed, misses, winCount = "", {}, 0, 0
local stages = {
    "  +---+\n      |\n      |\n      |\n     ===",
    "  +---+\n  O   |\n      |\n      |\n     ===",
    "  +---+\n  O   |\n  |   |\n      |\n     ===",
    "  +---+\n  O   |\n /|   |\n      |\n     ===",
    "  +---+\n  O   |\n /|\\  |\n      |\n     ===",
    "  +---+\n  O   |\n /|\\  |\n /    |\n     ===",
    "  +---+\n  O   |\n /|\\  |\n / \\  |\n     ==="
}

local function getChars(str)
    local chars = {}
    for c in str:gmatch("[%z\1-\127\194-\244][\128-\191]*") do table.insert(chars, c) end
    return chars
end

local function update()
    art.Text = stages[misses + 1] or stages[7]
    local disp, win = "", true
    local wordChars = getChars(chosenWord)

    for _, c in ipairs(wordChars) do
        if guessed[c] then disp = disp .. c .. " " else disp = disp .. "_ "; win = false end
    end
    wordDisplay.Text = disp

    if win then 
        winCount = winCount + 1
        scoreLabel.Text = "СЕРИЯ ПОБЕД: " .. winCount
        wordDisplay.TextColor3 = Color3.new(0, 1, 0)
        task.wait(1.5); start()
    elseif misses >= 6 then
        winCount = 0
        scoreLabel.Text = "СЕРИЯ ПОБЕД: 0"
        wordDisplay.TextColor3 = Color3.new(1, 0, 0); wordDisplay.Text = chosenWord
        task.wait(2.5); start()
    end
end

function start()
    chosenWord = dictionary[math.random(#dictionary)]
    guessed = {}
    misses = 0
    wordDisplay.TextColor3 = Color3.new(1, 1, 1)
    
    -- Очистка и создание кнопок заново
    for _, child in ipairs(keyContainer:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end
    
    for _, char in ipairs(getChars(alphabet)) do
        local btn = Instance.new("TextButton", keyContainer)
        btn.Text = char; btn.Font = "GothamBold"; btn.TextSize = 18
        btn.BackgroundColor3 = Color3.fromRGB(45, 50, 60); btn.TextColor3 = Color3.new(1, 1, 1)
        Instance.new("UICorner", btn)
        
        btn.MouseButton1Click:Connect(function()
            if not guessed[char] and misses < 6 then
                guessed[char] = true
                btn.TextTransparency = 0.6; btn.AutoButtonColor = false; btn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
                
                local found = false
                for _, c in ipairs(getChars(chosenWord)) do
                    if c == char then found = true; break end
                end
                
                if not found then misses = misses + 1 end
                update()
            end
        end)
    end
    update()
end

start()") ]],
    ["CHESS"] = [[ print("-- ШАХМАТЫ: ВЕРСИЯ С КНОПКОЙ ЗАКРЫТИЯ (X)
math.randomseed(os.time())
local Player = game:GetService("Players").LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("RobloxChess") then PlayerGui.RobloxChess:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "RobloxChess"

local MainFrame = Instance.new("Frame", ScreenGui)
local Grid = Instance.new("Frame", MainFrame)
local Status = Instance.new("TextLabel", MainFrame)
local ResetBtn = Instance.new("TextButton", MainFrame)
local CloseBtn = Instance.new("TextButton", MainFrame) -- Кнопка X

-- Настройка главного окна
MainFrame.Size = UDim2.new(0, 360, 0, 480); MainFrame.Position = UDim2.new(0.5, -180, 0.5, -240)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30); MainFrame.Active = true; MainFrame.Draggable = true
Instance.new("UICorner", MainFrame)

-- Настройка кнопки закрытия (X)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(150, 40, 40)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.SourceSansBold
CloseBtn.TextSize = 18
Instance.new("UICorner", CloseBtn)

CloseBtn.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

-- Остальная логика игры
Grid.Size = UDim2.new(0, 320, 0, 320); Grid.Position = UDim2.new(0, 20, 0, 100)
Grid.BackgroundColor3 = Color3.new(0,0,0)

Status.Size = UDim2.new(1, -40, 0, 40); Status.Position = UDim2.new(0, 20, 0, 50)
Status.TextColor3 = Color3.new(1,1,1); Status.TextSize = 20; Status.Font = Enum.Font.SourceSansBold
Status.Text = "Ваш ход ⚪"

ResetBtn.Size = UDim2.new(0, 100, 0, 30); ResetBtn.Position = UDim2.new(0.5, -50, 1, -40)
ResetBtn.Text = "Заново"; ResetBtn.BackgroundColor3 = Color3.fromRGB(80, 20, 20); ResetBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", ResetBtn)

local PIECES = {
	White = {P="♟️", R="🏰", N="🐴", B="🐘", Q="👸", K="👑", Bg = Color3.new(1,1,1)},
	Black = {P="♟️", R="🏰", N="🐴", B="🐘", Q="👸", K="👑", Bg = Color3.new(0.1, 0.1, 0.1)}
}

local board = {}
local turn = "White"
local selected = nil
local isBotThinking = false
local gameEnded = false

function initBoard()
	board = {}
	for r=1,8 do 
		board[r] = {} 
		for c=1,8 do board[r][c] = {type = nil, color = nil} end 
	end
	local layout = {"R","N","B","Q","K","B","N","R"}
	for c=1,8 do
		board[1][c] = {type=layout[c], color="Black"}
		board[2][c] = {type="P", color="Black"}
		board[7][c] = {type="P", color="White"}
		board[8][c] = {type=layout[c], color="White"}
	end
	turn = "White"
	gameEnded = false
	selected = nil
	Status.Text = "Ваш ход ⚪"
end

function getRawMoves(r, c, bState)
	local moves = {}
	local p = bState[r][c]
	if not p or not p.type then return moves end
	local enemy = (p.color == "White") and "Black" or "White"
	local dir = (p.color == "White") and -1 or 1

	if p.type == "P" then
		if r+dir >= 1 and r+dir <= 8 and not bState[r+dir][c].type then
			table.insert(moves, {r=r+dir, c=c})
			if ((p.color=="White" and r==7) or (p.color=="Black" and r==2)) and not bState[r+dir*2][c].type then
				table.insert(moves, {r=r+dir*2, c=c})
			end
		end
		for _, dc in pairs({-1, 1}) do
			local nr, nc = r+dir, c+dc
			if nr>=1 and nr<=8 and nc>=1 and nc<=8 then
				if bState[nr][nc].color == enemy then table.insert(moves, {r=nr, c=nc}) end
			end
		end
	elseif p.type == "N" then
		for _, v in pairs({{2,1},{2,-1},{-2,1},{-2,-1},{1,2},{1,-2},{-1,2},{-1,-2}}) do
			local nr, nc = r+v[1], c+v[2]
			if nr>=1 and nr<=8 and nc>=1 and nc<=8 and bState[nr][nc].color ~= p.color then table.insert(moves, {r=nr, c=nc}) end
		end
	else
		local ds = (p.type=="R") and {{1,0},{-1,0},{0,1},{0,-1}} or (p.type=="B") and {{1,1},{1,-1},{-1,1},{-1,-1}} or {{1,0},{-1,0},{0,1},{0,-1},{1,1},{1,-1},{-1,1},{-1,-1}}
		for _, d in pairs(ds) do
			for i=1, (p.type=="K" and 1 or 7) do
				local nr, nc = r+d[1]*i, c+d[2]*i
				if nr<1 or nr>8 or nc<1 or nc>8 then break end
				if not bState[nr][nc].type then table.insert(moves, {r=nr, c=nc})
				elseif bState[nr][nc].color == enemy then table.insert(moves, {r=nr, c=nc}) break else break end
			end
		end
	end
	return moves
end

function isCheck(color, bState)
	local kr, kc
	for r=1,8 do for c=1,8 do if bState[r][c].type=="K" and bState[r][c].color==color then kr,kc=r,c break end end end
	if not kr then return false end
	for r=1,8 do for c=1,8 do
		if bState[r][c].color and bState[r][c].color ~= color then
			local moves = getRawMoves(r, c, bState)
			for _, m in pairs(moves) do if m.r == kr and m.c == kc then return true end end
		end
	end end
	return false
end

function getLegalMoves(r, c)
	local raw = getRawMoves(r, c, board)
	local legal = {}
	local myType, myColor = board[r][c].type, board[r][c].color
	
	for _, m in pairs(raw) do
		local targetType, targetColor = board[m.r][m.c].type, board[m.r][m.c].color
		board[m.r][m.c].type, board[m.r][m.c].color = myType, myColor
		board[r][c].type, board[r][c].color = nil, nil
		if not isCheck(myColor, board) then table.insert(legal, m) end
		board[r][c].type, board[r][c].color = myType, myColor
		board[m.r][m.c].type, board[m.r][m.c].color = targetType, targetColor
	end
	return legal
end

function redraw()
	Grid:ClearAllChildren()
	local lMoves = selected and getLegalMoves(selected.r, selected.c) or {}
	for r=1,8 do for c=1,8 do
		local b = Instance.new("TextButton", Grid)
		b.Size = UDim2.new(0.125, 0, 0.125, 0); b.Position = UDim2.new((c-1)*0.125, 0, (r-1)*0.125, 0); b.Text = ""; b.BorderSizePixel = 0
		local isL = false
		for _, m in pairs(lMoves) do if m.r==r and m.c==c then isL=true break end end
		local theme = (r+c)%2 == 0 and Color3.fromRGB(180, 180, 180) or Color3.fromRGB(100, 100, 100)
		b.BackgroundColor3 = (selected and selected.r==r and selected.c==c) and Color3.new(1,1,0) or isL and Color3.new(0,1,0) or theme
		
		local data = board[r][c]
		if data and data.type then
			local l = Instance.new("TextLabel", b); l.Size = UDim2.new(0.8,0,0.8,0); l.Position = UDim2.new(0.1,0,0.1,0)
			l.BackgroundColor3 = PIECES[data.color].Bg; l.Text = PIECES[data.color][data.type]; l.TextScaled = true; l.ZIndex = 5
			l.TextColor3 = (data.color == "White") and Color3.new(0,0,0) or Color3.new(1,1,1)
			Instance.new("UICorner", l).CornerRadius = UDim.new(1,0)
		end
		b.MouseButton1Click:Connect(function()
			if gameEnded or isBotThinking or turn ~= "White" then return end
			if data.color == "White" then selected = {r=r, c=c} redraw()
			elseif selected and isL then makeMove(selected.r, selected.c, r, c)
			else selected = nil redraw() end
		end)
	end end
end

function makeMove(r1, c1, r2, c2)
	board[r2][c2].type, board[r2][c2].color = board[r1][c1].type, board[r1][c1].color
	board[r1][c1].type, board[r1][c1].color = nil, nil
	if board[r2][c2].type == "P" and (r2==1 or r2==8) then board[r2][c2].type = "Q" end
	selected = nil
	turn = (turn == "White") and "Black" or "White"
	redraw()
	
	local hasMoves = false
	for r=1,8 do for c=1,8 do
		if board[r][c].color == turn and #getLegalMoves(r,c) > 0 then hasMoves = true break end
	end if hasMoves then break end end
	
	local check = isCheck(turn, board)
	if not hasMoves then
		gameEnded = true
		Status.Text = check and "МАТ! 💀" or "ПАТ! 🤝"
		return
	end
	Status.Text = check and "ШАХ! ⚠️" or (turn == "White" and "Ваш ход ⚪" or "Бот думает... 🤔")

	if turn == "Black" and not gameEnded then
		isBotThinking = true
		task.delay(0.7, function()
			local moves = {}
			for r=1,8 do for c=1,8 do
				if board[r][c].color == "Black" then
					local legals = getLegalMoves(r,c)
					for _, m in pairs(legals) do table.insert(moves, {r1=r, c1=c, r2=m.r, c2=m.c}) end
				end
			end end
			if #moves > 0 then
				local m = moves[math.random(#moves)]
				makeMove(m.r1, m.c1, m.r2, m.c2)
			end
			isBotThinking = false
		end)
	end
end

ResetBtn.MouseButton1Click:Connect(function()
	initBoard()
	redraw()
end)

initBoard()
redraw()") ]],
    ["CHECKERS"] = [[ print("-- Checkers: 👑 Kings + Smooth Tweens + Multi-Eat
local TweenService = game:GetService("TweenService")
local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
local MainFrame = Instance.new("Frame", ScreenGui)
local TitleBar = Instance.new("Frame", MainFrame)
local Grid = Instance.new("Frame", MainFrame)
local Status = Instance.new("TextLabel", MainFrame)
local ResetBtn = Instance.new("TextButton", MainFrame)
local CloseBtn = Instance.new("TextButton", TitleBar)

-- Настройки UI
MainFrame.Size = UDim2.new(0, 320, 0, 430)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -215)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

TitleBar.Size = UDim2.new(1, 0, 0, 35)
TitleBar.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CloseBtn.Size = UDim2.new(0, 35, 0, 35); CloseBtn.Position = UDim2.new(1, -35, 0, 0)
CloseBtn.Text = "✕"; CloseBtn.TextColor3 = Color3.new(1,1,1); CloseBtn.BackgroundTransparency = 1
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

Status.Size = UDim2.new(1, 0, 0, 30); Status.Position = UDim2.new(0, 0, 0, 40)
Status.Text = "Твой ход"; Status.TextColor3 = Color3.new(1,1,1); Status.BackgroundTransparency = 1; Status.TextSize = 18

Grid.Size = UDim2.new(0, 280, 0, 280); Grid.Position = UDim2.new(0.5, -140, 0, 80)
Grid.BackgroundColor3 = Color3.fromRGB(15, 15, 15)

ResetBtn.Size = UDim2.new(0, 140, 0, 35); ResetBtn.Position = UDim2.new(0.5, -70, 1, -45)
ResetBtn.Text = "СБРОСИТЬ"; ResetBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 215); ResetBtn.TextColor3 = Color3.new(1,1,1)

local board = {}
local selected = nil
local turn = "Player"
local isMoving = false

-- Логика ходов
function getMoves(r, c, onlyJumps)
	local moves = {}
	local p = board[r][c]
	if not p or not p.piece then return moves end
	local dirs = {{1,1},{1,-1},{-1,1},{-1,-1}}

	for _, d in pairs(dirs) do
		if p.type == "Normal" then
			if not onlyJumps then
				local nr, nc = r+d[1], c+d[2]
				local fwd = (p.piece == "Player" and d[1] == -1) or (p.piece == "Bot" and d[1] == 1)
				if fwd and nr>=1 and nr<=8 and nc>=1 and nc<=8 and not board[nr][nc].piece then
					table.insert(moves, {r=nr, c=nc, jump=false})
				end
			end
			local jr, jc = r+d[1]*2, c+d[2]*2
			if jr>=1 and jr<=8 and jc>=1 and jc<=8 then
				local mid = board[r+d[1]][c+d[2]]
				if mid.piece and mid.piece ~= p.piece and not board[jr][jc].piece then
					table.insert(moves, {r=jr, c=jc, jump=true})
				end
			end
		else -- Дамка 👑
			for i = 1, 7 do
				local nr, nc = r+d[1]*i, c+d[2]*i
				if nr<1 or nr>8 or nc<1 or nc>8 then break end
				if not board[nr][nc].piece then
					if not onlyJumps then table.insert(moves, {r=nr, c=nc, jump=false}) end
				else
					if board[nr][nc].piece ~= p.piece then
						local jr, jc = nr+d[1], nc+d[2]
						if jr>=1 and jr<=8 and jc>=1 and jc<=8 and not board[jr][jc].piece then
							table.insert(moves, {r=jr, c=jc, jump=true})
						end
					end
					break
				end
			end
		end
	end
	return moves
end

function render()
	for r=1,8 do for c=1,8 do
		local cell = board[r][c]
		cell.btn.BackgroundColor3 = (r+c)%2 ~= 0 and Color3.fromRGB(45,45,45) or Color3.fromRGB(30,30,30)
		if cell.piece then
			cell.btn.Text = cell.type == "King" and "👑" or "●"
			cell.btn.TextColor3 = cell.piece == "Player" and Color3.fromRGB(0,180,255) or Color3.fromRGB(255,50,50)
		else cell.btn.Text = "" end
	end end
	if selected then
		board[selected.r][selected.c].btn.BackgroundColor3 = Color3.fromRGB(80,80,0)
		for _, m in pairs(getMoves(selected.r, selected.c)) do
			board[m.r][m.c].btn.BackgroundColor3 = m.jump and Color3.fromRGB(150,100,0) or Color3.fromRGB(0,100,0)
		end
	end
end

function performMove(r1, c1, r2, c2)
	if isMoving then return end
	isMoving = true
	local p = board[r1][c1]
	local dist = math.abs(r1-r2)
	local targetPos = UDim2.new((c2-1)*0.125, 0, (r2-1)*0.125, 0)
	
	local tw = TweenService:Create(p.btn, TweenInfo.new(0.3, Enum.EasingStyle.Sine), {Position = targetPos})
	p.btn.ZIndex = 10
	tw:Play()
	
	tw.Completed:Connect(function()
		p.btn.ZIndex = 1
		p.btn.Position = UDim2.new((c1-1)*0.125, 0, (r1-1)*0.125, 0)
		
		if dist >= 2 then
			local dr, dc = (r2-r1)/dist, (c2-c1)/dist
			for i=1, dist-1 do board[r1+dr*i][c1+dc*i].piece = nil end
		end

		board[r2][c2].piece, board[r2][c2].type = p.piece, p.type
		board[r1][c1].piece = nil
		
		-- Становление королем 👑
		if (board[r2][c2].piece == "Player" and r2 == 1) or (board[r2][c2].piece == "Bot" and r2 == 8) then
			board[r2][c2].type = "King"
		end

		local nextJumps = getMoves(r2, c2, true)
		if dist >= 2 and #nextJumps > 0 then
			isMoving = false
			if turn == "Bot" then
				task.wait(0.5); performMove(r2, c2, nextJumps[1].r, nextJumps[1].c)
			else
				selected = {r=r2, c=c2}; Status.Text = "БЕЙ ЕЩЕ!"; render()
					end
		else
			turn = (turn == "Player") and "Bot" or "Player"
			isMoving = false; selected = nil
			Status.Text = (turn == "Bot") and "Бот думает..." or "Твой ход"
			render()
			if turn == "Bot" then task.wait(0.6); botThink() end
		end
	end)
end

function botThink()
	if turn ~= "Bot" or isMoving then return end
	local jumps, normals = {}, {}
	for r=1,8 do for c=1,8 do
		if board[r][c].piece == "Bot" then
			for _, m in pairs(getMoves(r,c)) do
				local data = {r1=r, c1=c, r2=m.r, c2=m.c}
				if m.jump then table.insert(jumps, data) else table.insert(normals, data) end
			end
		end
	end end
	local move = (#jumps > 0) and jumps[math.random(1,#jumps)] or (#normals > 0 and normals[math.random(1,#normals)] or nil)
	if move then performMove(move.r1, move.c1, move.r2, move.c2) else Status.Text = "ПОБЕДА!" end
end

-- Инициализация поля
for r=1,8 do
	board[r] = {}
	for c=1,8 do
		local b = Instance.new("TextButton", Grid)
		b.Size = UDim2.new(0.125, 0, 0.125, 0)
		b.Position = UDim2.new((c-1)*0.125, 0, (r-1)*0.125, 0)
		b.BorderSizePixel = 0; b.TextSize = 24; b.Font = Enum.Font.SourceSansBold
		board[r][c] = {btn = b, piece = nil, type = "Normal"}
		
		b.MouseButton1Click:Connect(function()
			if turn ~= "Player" or isMoving then return end
			if board[r][c].piece == "Player" then
				selected = {r=r, c=c}; render()
			elseif selected then
				for _, m in pairs(getMoves(selected.r, selected.c)) do
					if m.r == r and m.c == c then performMove(selected.r, selected.c, r, c); return end
				end
				selected = nil; render()
			end
		end)
	end
end

function reset()
	isMoving = false
	for r=1,8 do for c=1,8 do
		board[r][c].piece = nil; board[r][c].type = "Normal"
		if (r+c)%2 ~= 0 then
			if r <= 3 then board[r][c].piece = "Bot"
			elseif r >= 6 then board[r][c].piece = "Player" end
		end
	end end
	turn = "Player"; Status.Text = "Твой ход"; render()
end

ResetBtn.MouseButton1Click:Connect(reset)
reset()") ]],
    ["TIC-TAC"] = [[ print("-- Tic-Tac-Toe Smart & Colored (X - Blue, O - Red)
local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
local MainFrame = Instance.new("Frame", ScreenGui)
local TitleBar = Instance.new("Frame", MainFrame)
local GridFrame = Instance.new("Frame", MainFrame)
local Grid = Instance.new("UIGridLayout", GridFrame)
local Status = Instance.new("TextLabel", MainFrame)
local CloseBtn = Instance.new("TextButton", TitleBar)
local ResetBtn = Instance.new("TextButton", MainFrame)

-- Настройки окна
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -130)
MainFrame.Size = UDim2.new(0, 220, 0, 300)
MainFrame.BorderSizePixel = 0

-- Полоска для перетаскивания
TitleBar.Size = UDim2.new(1, 0, 0, 35)
TitleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
TitleBar.BorderSizePixel = 0

local dragging, dragInput, dragStart, startPos
TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true; dragStart = input.Position; startPos = MainFrame.Position
    end
end)
game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
TitleBar.InputEnded:Connect(function(input) dragging = false end)

-- Кнопка закрытия
CloseBtn.Size = UDim2.new(0, 35, 0, 35)
CloseBtn.Position = UDim2.new(1, -35, 0, 0)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.BorderSizePixel = 0
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- Статус игры
Status.Size = UDim2.new(1, 0, 0, 30)
Status.Position = UDim2.new(0, 0, 0, 40)
Status.Text = "Твой ход (X)"
Status.TextColor3 = Color3.new(1, 1, 1)
Status.BackgroundTransparency = 1
Status.TextSize = 20

-- Игровое поле
GridFrame.Size = UDim2.new(0, 190, 0, 190)
GridFrame.Position = UDim2.new(0, 15, 0, 75)
GridFrame.BackgroundTransparency = 1
Grid.CellSize = UDim2.new(0, 60, 0, 60)
Grid.CellPadding = UDim2.new(0, 5, 0, 5)

-- Кнопка Заново
ResetBtn.Size = UDim2.new(0, 120, 0, 30)
ResetBtn.Position = UDim2.new(0.5, -60, 1, -30)
ResetBtn.Text = "ИГРАТЬ ЗАНОВО"
ResetBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
ResetBtn.TextColor3 = Color3.new(1, 1, 1)
ResetBtn.BorderSizePixel = 0

local board = {}
local gameActive = true
local combos = {{1,2,3},{4,5,6},{7,8,9},{1,4,7},{2,5,8},{3,6,9},{1,5,9},{3,5,7}}

function checkWin(m)
    for _, c in pairs(combos) do
        if board[c[1]].Text == m and board[c[2]].Text == m and board[c[3]].Text == m then return true end
    end
    return false
end

function isFull()
    for i=1,9 do if board[i].Text == "" then return false end end
    return true
end

function botMove()
    if not gameActive then return end
    local move = nil
    
    -- Пытаемся выиграть
    for _, c in pairs(combos) do
        local countO, empty = 0, nil
        for _, p in pairs(c) do
            if board[p].Text == "O" then countO = countO + 1 elseif board[p].Text == "" then empty = p end
        end
        if countO == 2 and empty then move = empty break end
    end
    
    -- Блокируем игрока
    if not move then
        for _, c in pairs(combos) do
            local countX, empty = 0, nil
            for _, p in pairs(c) do
                if board[p].Text == "X" then countX = countX + 1 elseif board[p].Text == "" then empty = p end
            end
            if countX == 2 and empty then move = empty break end
        end
    end
    
    -- Центр или случайно
    if not move then
        if board[5].Text == "" then move = 5
        else
            local avail = {}
            for i=1,9 do if board[i].Text == "" then table.insert(avail, i) end end
            if #avail > 0 then move = avail[math.random(1, #avail)] end
        end
    end

    if move then
        board[move].Text = "O"
        board[move].TextColor3 = Color3.fromRGB(255, 50, 50) -- Красный О
        if checkWin("O") then Status.Text = "БОТ ВЫИГРАЛ!"; gameActive = false
        elseif isFull() then Status.Text = "НИЧЬЯ!"; gameActive = false
        else Status.Text = "Твой ход (X)" end
    end
end

ResetBtn.MouseButton1Click:Connect(function()
    for i=1,9 do board[i].Text = "" end
    gameActive = true; Status.Text = "Твой ход (X)"
end)

for i = 1, 9 do
    local btn = Instance.new("TextButton", GridFrame)
    btn.Text = ""
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextSize = 40
    btn.Font = Enum.Font.SourceSansBold
    board[i] = btn
    
    btn.MouseButton1Click:Connect(function()
        if btn.Text == "" and gameActive then
            btn.Text = "X"
            btn.TextColor3 = Color3.fromRGB(0, 150, 255) -- Синий X
            if checkWin("X") then Status.Text = "ТЫ ПОБЕДИЛ!"; gameActive = false
            elseif isFull() then Status.Text = "НИЧЬЯ!"; gameActive = false
            else
                Status.Text = "Бот думает..."
                task.wait(0.2)
                botMove()
            end
        end
    end)
end") ]],
    ["TETRIS"] = [[ print("-- Tetris Mobile Edition
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

local Speed = 0.5 -- Скорость падения (чем больше, тем медленнее)

if PlayerGui:FindFirstChild("TetrisMobile") then PlayerGui.TetrisMobile:Destroy() end

local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "TetrisMobile"

-- Основное окно
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 420, 0, 450)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -225)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Active = true
MainFrame.Draggable = true 

-- Игровое поле (Стакан)
local GameBoard = Instance.new("Frame", MainFrame)
GameBoard.Size = UDim2.new(0, 200, 0, 400)
GameBoard.Position = UDim2.new(0, 10, 0, 40)
GameBoard.BackgroundColor3 = Color3.new(0, 0, 0)
GameBoard.ClipsDescendants = true

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Text = "TETRIS"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.BackgroundTransparency = 1

local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.Text = "×"
CloseBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- Управление
local Controls = Instance.new("Frame", MainFrame)
Controls.Size = UDim2.new(0, 180, 0, 180)
Controls.Position = UDim2.new(0, 230, 0, 80)
Controls.BackgroundTransparency = 1

local function createBtn(pos, text, color)
    local b = Instance.new("TextButton", Controls)
    b.Size = UDim2.new(0, 50, 0, 50)
    b.Position = pos
    b.Text = text
    b.BackgroundColor3 = color or Color3.fromRGB(60, 60, 60)
    b.TextColor3 = Color3.new(1, 1, 1)
    b.Font = Enum.Font.GothamBold
    return b
end

local btnLeft = createBtn(UDim2.new(0, 0, 0.35, 0), "◄")
local btnRight = createBtn(UDim2.new(0.6, 0, 0.35, 0), "►")
local btnRotate = createBtn(UDim2.new(0.3, 0, 0, 0), "↻", Color3.fromRGB(80, 80, 150))
local btnDown = createBtn(UDim2.new(0.3, 0, 0.7, 0), "▼")

local RestartBtn = Instance.new("TextButton", MainFrame)
RestartBtn.Size = UDim2.new(0, 120, 0, 40)
RestartBtn.Position = UDim2.new(0, 255, 0, 300)
RestartBtn.Text = "RESTART"
RestartBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
RestartBtn.TextColor3 = Color3.new(1, 1, 1)

-- Логика Тетриса
local ROWS, COLS = 20, 10
local grid = {}
local currentPiece, currentPos, currentColor
local playing = false

local shapes = {
    {{1,1,1,1}}, -- I
    {{1,1},{1,1}}, -- O
    {{0,1,0},{1,1,1}}, -- T
    {{1,1,0},{0,1,1}}, -- Z
    {{0,1,1},{1,1,0}}  -- S
}

local colors = {Color3.new(0,1,1), Color3.new(1,1,0), Color3.new(0.6,0,1), Color3.new(1,0,0), Color3.new(0,1,0)}

local function clearGrid()
    grid = {}
    for r=1, ROWS do grid[r] = {} for c=1, COLS do grid[r][c] = 0 end end
end

local function draw()
    GameBoard:ClearAllChildren()
    for r=1, ROWS do
        for c=1, COLS do
            if grid[r][c] ~= 0 then
                local f = Instance.new("Frame", GameBoard)
                f.Size = UDim2.new(0, 18, 0, 18)
                f.Position = UDim2.new(0, (c-1)*20 + 1, 0, (r-1)*20 + 1)
                f.BackgroundColor3 = grid[r][c]
                f.BorderSizePixel = 0
            end
        end
    end
    if currentPiece then
        for r, row in pairs(currentPiece) do
            for c, val in pairs(row) do
                if val == 1 then
                    local f = Instance.new("Frame", GameBoard)
                    f.Size = UDim2.new(0, 18, 0, 18)
                    f.Position = UDim2.new(0, (currentPos.X+c-2)*20 + 1, 0, (currentPos.Y+r-2)*20 + 1)
                    f.BackgroundColor3 = currentColor
                    f.BorderSizePixel = 0
                end
            end
        end
    end
end

local function intersects(p, pos)
    for r, row in pairs(p) do
        for c, val in pairs(row) do
            if val == 1 then
                local nr, nc = pos.Y+r-1, pos.X+c-1
                if nr > ROWS or nc < 1 or nc > COLS or (nr > 0 and grid[nr][nc] ~= 0) then return true end
            end
        end
    end
    return false
end

local function spawnPiece()
    local id = math.random(#shapes)
    currentPiece = shapes[id]
    currentColor = colors[id]
    currentPos = Vector2.new(4, 1)
    if intersects(currentPiece, currentPos) then playing = false Title.Text = "GAME OVER" end
end

local function lockPiece()
    for r, row in pairs(currentPiece) do
        for c, val in pairs(row) do
            if val == 1 then grid[currentPos.Y+r-1][currentPos.X+c-1] = currentColor end
        end
    end
    -- Удаление линий
    for r=ROWS, 1, -1 do
        local full = true
        for c=1, COLS do if grid[r][c] == 0 then full = false end end
        if full then
            table.remove(grid, r)
            table.insert(grid, 1, {})
            for c=1, COLS do grid[1][c] = 0 end
        end
    end
    spawnPiece()
end

local function move(d)
    local np = currentPos + d
    if not intersects(currentPiece, np) then currentPos = np draw() return true end
    return false
end

local function rotate()
    local newP = {}
    for c=1, #currentPiece[1] do
        newP[c] = {}
        for r=#currentPiece, 1, -1 do table.insert(newP[c], currentPiece[r][c]) end
    end
    if not intersects(newP, currentPos) then currentPiece = newP draw() end
end

btnLeft.MouseButton1Click:Connect(function() move(Vector2.new(-1, 0)) end)
btnRight.MouseButton1Click:Connect(function() move(Vector2.new(1, 0)) end)
btnRotate.MouseButton1Click:Connect(rotate)
btnDown.MouseButton1Click:Connect(function() move(Vector2.new(0, 1)) end)

RestartBtn.MouseButton1Click:Connect(function()
    clearGrid()
    spawnPiece()
    playing = true
    Title.Text = "TETRIS"
end)

clearGrid()
spawnPiece()
playing = true

task.spawn(function()
    while true do
        if playing then
            if not move(Vector2.new(0, 1)) then lockPiece() end
            draw()
        end
        task.wait(Speed)
    end
end)") ]],
    ["MINES"] = [[ print("local ScreenGui = Instance.new("ScreenGui")
local Main = Instance.new("Frame")
local GridFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame") -- Для красоты и перетаскивания
local Title = Instance.new("TextLabel")
local CloseBtn = Instance.new("TextButton")
local Status = Instance.new("TextLabel")
local FlagBtn = Instance.new("TextButton")
local ResetBtn = Instance.new("TextButton")

-- Защита от повторного запуска (удаляем старый GUI если есть)
if game.CoreGui:FindFirstChild("RealMinesweeper_v2") then
    game.CoreGui.RealMinesweeper_v2:Destroy()
end

ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "RealMinesweeper_v2"
ScreenGui.ResetOnSpawn = false -- Не удаляется при смерти

-- Основной контейнер
Main.Parent = ScreenGui
Main.Size = UDim2.new(0, 320, 0, 410)
Main.Position = UDim2.new(0.5, -160, 0.5, -205)
Main.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true -- В Delta это обычно работает лучше всего

-- Верхняя панель
TopBar.Parent = Main
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
TopBar.BorderSizePixel = 0

Title.Parent = TopBar
Title.Size = UDim2.new(1, -40, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "Minesweeper Delta"
Title.TextColor3 = Color3.fromRGB(200, 200, 200)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 16

CloseBtn.Parent = TopBar
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
CloseBtn.BackgroundTransparency = 1
CloseBtn.TextSize = 18
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

Status.Parent = Main
Status.Size = UDim2.new(1, 0, 0, 30)
Status.Position = UDim2.new(0, 0, 0, 35)
Status.BackgroundTransparency = 1
Status.TextColor3 = Color3.new(1, 1, 1)
Status.Font = Enum.Font.Code
Status.TextSize = 18

GridFrame.Parent = Main
GridFrame.Position = UDim2.new(0, 10, 0, 70)
GridFrame.Size = UDim2.new(0, 300, 0, 300)
GridFrame.BackgroundTransparency = 1

-- Параметры игры
local size = 8
local bombCount = 10
local grid = {}
local buttons = {}
local isFlagMode = false
local gameOver = false
local tilesRevealed = 0

local function checkWin()
    -- Победа если: (Всего клеток - Бомбы) == Открытые клетки
    if tilesRevealed == (size * size - bombCount) then
        gameOver = true
        Status.Text = "🎉 ПОБЕДА! 🎉"
        Status.TextColor3 = Color3.new(0, 1, 0)
        -- Показываем где были бомбы (не взрывая)
        for i = 1, #grid do
            if grid[i].isBomb then
                buttons[i].Text = "💣"
                buttons[i].BackgroundColor3 = Color3.new(0, 0.8, 0) -- Зеленые бомбы
            end
        end
    end
end

local function reveal(x, y)
    local idx = (y - 1) * size + x
    -- Проверки границ и состояний
    if x < 1 or x > size or y < 1 or y > size then return end
    if grid[idx].revealed or grid[idx].flagged or gameOver then return end
    
    local cell = grid[idx]
    local btn = buttons[idx]
    
    cell.revealed = true
    tilesRevealed = tilesRevealed + 1
    
    if cell.isBomb then
        btn.BackgroundColor3 = Color3.new(0.8, 0, 0)
        btn.Text = "💥"
        gameOver = true
        Status.Text = "Game Over!"
        Status.TextColor3 = Color3.new(1, 0, 0)
        -- Показать остальные бомбы
        for i = 1, #grid do
            if grid[i].isBomb and not grid[i].revealed then
                buttons[i].Text = "💣"
                buttons[i].BackgroundColor3 = Color3.fromRGB(100, 50, 50)
            end
        end
    else
        btn.BackgroundColor3 = Color3.fromRGB(180, 180, 180) -- Светлый фон для открытой
        if cell.near > 0 then
            btn.Text = tostring(cell.near)
            -- Цвета цифр как в оригинале
            local c = {Color3.fromRGB(0,0,255), Color3.fromRGB(0,128,0), Color3.fromRGB(255,0,0), Color3.fromRGB(0,0,128)}
            btn.TextColor3 = c[cell.near] or Color3.new(0,0,0)
            btn.Font = Enum.Font.SourceSansBold
        else
            btn.Text = "" -- Пусто
            -- Безопасная рекурсия (Flood Fill)
            for dy = -1, 1 do
                for dx = -1, 1 do
                    if not (dx == 0 and dy == 0) then
                        reveal(x + dx, y + dy)
                    end
                end
            end
        end
        checkWin()
    end
end

local function initGame()
    gameOver = false
    tilesRevealed = 0
    Status.Text = "Бомб осталось: " .. bombCount
    Status.TextColor3 = Color3.new(1, 1, 1)
    GridFrame:ClearAllChildren()
    grid = {}
    buttons = {}
    
    -- Инициализация данных
    for i = 1, size * size do 
        table.insert(grid, {isBomb = false, revealed = false, flagged = false, near = 0}) 
    end
    
    -- Расстановка бомб
    local placed = 0
    while placed < bombCount do
        local r = math.random(1, size * size)
        if not grid[r].isBomb then
            grid[r].isBomb = true
            placed = placed + 1
        end
    end
    
    -- Подсчет соседей
    for y = 1, size do
        for x = 1, size do
            local idx = (y - 1) * size + x
            if not grid[idx].isBomb then
                local n = 0
                for dy = -1, 1 do
                    for dx = -1, 1 do
                        local nx, ny = x + dx, y + dy
                        if nx >= 1 and nx <= size and ny >= 1 and ny <= size then
                            local nIdx = (ny - 1) * size + nx
                            if grid[nIdx].isBomb then n = n + 1 end
                        end
                    end
                end
                grid[idx].near = n
            end
        end
    end
    
    -- Создание кнопок
    local cellSize = 300 / size
    for y = 1, size do
        for x = 1, size do
            local idx = (y - 1) * size + x
            local b = Instance.new("TextButton", GridFrame)
            b.Size = UDim2.new(0, cellSize - 2, 0, cellSize - 2)
            b.Position = UDim2.new(0, (x-1)*cellSize, 0, (y-1)*cellSize)
            b.BackgroundColor3 = Color3.fromRGB(80, 80, 90)
            b.Text = ""
            b.TextSize = 20
            b.TextColor3 = Color3.new(1,1,1)
            
            b.MouseButton1Click:Connect(function()
                if gameOver then return end
                            if isFlagMode then
                    if not grid[idx].revealed then
                        grid[idx].flagged = not grid[idx].flagged
                        b.Text = grid[idx].flagged and "🚩" or ""
                        b.TextColor3 = Color3.new(1, 0.5, 0)
                    end
                else
                    reveal(x, y)
                end
            end)
            buttons[idx] = b
        end
    end
end

-- Нижние кнопки управления
FlagBtn.Parent = Main
FlagBtn.Size = UDim2.new(0, 120, 0, 30)
FlagBtn.Position = UDim2.new(0, 20, 1, -35)
FlagBtn.Text = "⛏️ Копать"
FlagBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 60)
FlagBtn.TextColor3 = Color3.new(1,1,1)
FlagBtn.MouseButton1Click:Connect(function()
    isFlagMode = not isFlagMode
    if isFlagMode then
        FlagBtn.Text = "🚩 Флаг"
        FlagBtn.BackgroundColor3 = Color3.fromRGB(180, 80, 0)
    else
        FlagBtn.Text = "⛏️ Копать"
        FlagBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 60)
    end
end)

ResetBtn.Parent = Main
ResetBtn.Size = UDim2.new(0, 120, 0, 30)
ResetBtn.Position = UDim2.new(1, -140, 1, -35)
ResetBtn.Text = "Заново"
ResetBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
ResetBtn.TextColor3 = Color3.new(1,1,1)
ResetBtn.MouseButton1Click:Connect(initGame)

initGame()") ]]
}

local function drag(o, h)
    local active, start, startPos
    h.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            active = true; start = i.Position; startPos = o.Position
            i.Changed:Connect(function() if i.UserInputState == Enum.UserInputState.End then active = false end end)
        end
    end)
    inputS.InputChanged:Connect(function(i)
        if active and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
            local d = i.Position - start
            o.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y)
        end
    end)
end

local base = Instance.new("Frame", screen)
base.Size = UDim2.new(0, 320, 0, 420)
base.Position = UDim2.new(0.5, -160, 0.5, -210)
base.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
base.BorderSizePixel = 0
base.ClipsDescendants = true
Instance.new("UICorner", base).CornerRadius = UDim.new(0, 10)

-- Рандомные смайлы на фоне
local bg = Instance.new("Frame", base)
bg.Size = UDim2.new(1, 0, 1, 0); bg.BackgroundTransparency = 1
local sm = {"😀", "🔥", "🚀", "⚡", "🎮", "🎲", "🎯", "💎", "🌟", "😎"}
for i = 1, 35 do
    local l = Instance.new("TextLabel", bg)
    l.Text = sm[math.random(1, #sm)]; l.Position = UDim2.new(math.random(), 0, math.random(), 0)
    l.BackgroundTransparency = 1; l.TextTransparency = 0.75; l.Size = UDim2.new(0, 20, 0, 20)
end

local head = Instance.new("Frame", base)
head.Size = UDim2.new(1, 0, 0, 45); head.BackgroundColor3 = Color3.fromRGB(0, 140, 255)
Instance.new("UICorner", head)
drag(base, head)

local name = Instance.new("TextLabel", head)
name.Size = UDim2.new(1, 0, 1, 0); name.Position = UDim2.new(0, 12, 0, 0)
name.Text = "EPIC HUB | HELL_WORLD"; name.TextColor3 = Color3.new(1, 1, 1)
name.Font = "GothamBold"; name.TextSize = 13; name.BackgroundTransparency = 1; name.TextXAlignment = "Left"

local function btn(t, x, c, sz)
    local b = Instance.new("TextButton", head)
    b.Size = sz or UDim2.new(0, 28, 0, 28); b.Position = UDim2.new(1, x, 0, 8)
    b.BackgroundColor3 = c; b.Text = t; b.TextColor3 = Color3.new(1,1,1)
    b.Font = "GothamBold"; b.TextSize = 11; Instance.new("UICorner", b)
    return b
end

local cls = btn("X", -35, Color3.fromRGB(230, 50, 50))
local min = btn("_", -70, Color3.fromRGB(0, 100, 200))
local lbtn = btn("RU", -115, Color3.fromRGB(60, 60, 65), UDim2.new(0, 40, 0, 28))

local sc = Instance.new("ScrollingFrame", base)
sc.Size = UDim2.new(1, -10, 1, -60); sc.Position = UDim2.new(0, 5, 0, 50)
sc.BackgroundTransparency = 1; sc.CanvasSize = UDim2.new(0, 0, 0, 650); sc.ScrollBarThickness = 0
Instance.new("UIListLayout", sc).Padding = UDim.new(0, 5)

local list = {
    {"🕹️ ARCANOID", "🕹️ АРКАНОИД", "ARCANOID"},
    {"🔢 2048", "🔢 2048", "2048"},
    {"🔥 CLICKER", "🔥 КЛИКЕР", "CLICKER"},
    {"🐦 BIRD", "🐦 ПТИЦА", "BIRD"},
    {"🐍 SNAKE", "🐍 ЗМЕЙКА", "SNAKE"},
    {"🏎️ RACING", "🏎️ ГОНКИ", "RACING"},
    {"📝 WORDLE", "📝 ВОРДЛИ", "WORDLE"},
    {"😵 GALLOWS", "😵 ВИСЕЛИЦА", "GALLOWS"},
    {"♟️ CHESS", "♟️ ШАХМАТЫ", "CHESS"},
    {"⚪ CHECKERS", "⚪ ШАШКИ", "CHECKERS"},
    {"❌ TIC-TAC", "❌ КРЕСТИКИ", "TIC-TAC"},
    {"🧱 TETRIS", "🧱 ТЕТРИС", "TETRIS"},
    {"💣 MINES", "💣 САПЕР", "MINES"}
}

local btns = {}
for i, v in ipairs(list) do
    local b = Instance.new("TextButton", sc)
    b.Size = UDim2.new(0.95, 0, 0, 38)
    b.Text = (lang == "RU" and v[2] or v[1])
    b.BackgroundColor3 = Color3.fromRGB(35, 35, 40); b.TextColor3 = Color3.new(1, 1, 1)
    b.Font = "Gotham"; b.TextSize = 12; Instance.new("UICorner", b)
    
    b.MouseButton1Click:Connect(function()
        local code = games_data[v[3]]
        if code then
            local func, err = loadstring(code)
            if func then func() else warn("Ошибка: " .. err) end
        end
    end)
    btns[i] = b
end

lbtn.MouseButton1Click:Connect(function()
    lang = (lang == "RU" and "EN" or "RU")
    lbtn.Text = lang
    for i, v in ipairs(list) do btns[i].Text = (lang == "RU" and v[2] or v[1]) end
end)

local tog = Instance.new("TextButton", screen)
tog.Size = UDim2.new(0, 55, 0, 55); tog.Position = UDim2.new(0, 20, 0.7, 0)
tog.BackgroundColor3 = Color3.fromRGB(0, 140, 255); tog.Text = "😀"
tog.TextSize = 30; tog.Visible = false; Instance.new("UICorner", tog).CornerRadius = UDim.new(1, 0)
drag(tog, tog)

min.MouseButton1Click:Connect(function() base.Visible = false; tog.Visible = true end)
tog.MouseButton1Click:Connect(function() base.Visible = true; tog.Visible = false end)
cls.MouseButton1Click:Connect(function() screen:Destroy() end)
