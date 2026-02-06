-- Ключ на эту неделю: 0206
local CORRECT_KEY = "0206" 
local KEY_URL = "https://link-target.net/3336252/0qvWbqqskeQZ"

local Player = game:GetService("Players").LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Удаляем старое, если есть
if PlayerGui:FindFirstChild("MiniGamesHub") then PlayerGui.MiniGamesHub:Destroy() end

local sg = Instance.new("ScreenGui")
sg.Name = "MiniGamesHub"
sg.Parent = PlayerGui
sg.ResetOnSpawn = false

-- ГЛАВНОЕ ОКНО
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 250, 0, 350)
main.Position = UDim2.new(0.5, -125, 0.5, -175)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.BorderSizePixel = 2
main.Active = true
main.Draggable = true -- Теперь должно работать
main.Parent = sg

-- Заголовок
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "MINI-GAMES HUB"
title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Parent = main

-- === БЛОК КЛЮЧА ===
local keyGroup = Instance.new("Frame", main)
keyGroup.Size = UDim2.new(1, 0, 1, -40)
keyGroup.Position = UDim2.new(0, 0, 0, 40)
keyGroup.BackgroundTransparency = 1

local input = Instance.new("TextBox", keyGroup)
input.Size = UDim2.new(0, 200, 0, 40)
input.Position = UDim2.new(0.5, -100, 0.2, 0)
input.PlaceholderText = "Введи ключ..."
input.Text = ""

local check = Instance.new("TextButton", keyGroup)
check.Size = UDim2.new(0, 200, 0, 40)
check.Position = UDim2.new(0.5, -100, 0.45, 0)
check.Text = "ПРОВЕРИТЬ"
check.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
check.TextColor3 = Color3.new(1, 1, 1)

local getLink = Instance.new("TextButton", keyGroup)
getLink.Size = UDim2.new(0, 200, 0, 30)
getLink.Position = UDim2.new(0.5, -100, 0.7, 0)
getLink.Text = "ПОЛУЧИТЬ КЛЮЧ (LINK)"
getLink.BackgroundColor3 = Color3.fromRGB(0, 80, 150)
getLink.TextColor3 = Color3.new(1, 1, 1)

-- === БЛОК ИГР (СКРЫТ) ===
local gameGroup = Instance.new("ScrollingFrame", main)
gameGroup.Size = UDim2.new(1, 0, 1, -40)
gameGroup.Position = UDim2.new(0, 0, 0, 40)
gameGroup.BackgroundTransparency = 1
gameGroup.Visible = false
gameGroup.CanvasSize = UDim2.new(0, 0, 0, 400)

local layout = Instance.new("UIListLayout", gameGroup)
layout.HorizontalAlignment = "Center"
layout.Padding = UDim.new(0, 5)

-- Список игр
local games = {
    {name = "📝 Wordle", url = "https://raw.githubusercontent.com/Xkaka228X/Wordle/refs/heads/main/README.md"},
    {name = "😵 Виселица", url = "https://raw.githubusercontent.com/Xkaka228X/Gallows/refs/heads/main/README.md"},
    {name = "♟️ Шахматы", url = "https://raw.githubusercontent.com/farfr737-wq/Chess/refs/heads/main/README.md"},
    {name = "⚪ Шашки", url = "https://raw.githubusercontent.com/Xkaka228X/Checkers/refs/heads/main/README.md"},
    {name = "❌ Крестики-Нолики", url = "https://raw.githubusercontent.com/Xkaka228X/Tic-tac-teo/refs/heads/main/README.md"},
    {name = "🧱 Тетрис", url = "https://raw.githubusercontent.com/Xkaka228X/Tetris/refs/heads/main/README.md"},
    {name = "🐍 Змейка", url = "https://raw.githubusercontent.com/Xkaka228X/Snake/refs/heads/main/README.md"},
    {name = "💣 Сапер", url = "https://raw.githubusercontent.com/Xkaka228X/Minesweeper/refs/heads/main/README.md"}
}

for _, g in ipairs(games) do
    local btn = Instance.new("TextButton", gameGroup)
    btn.Size = UDim2.new(0, 220, 0, 35)
    btn.Text = g.name
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.MouseButton1Click:Connect(function()
        loadstring(game:HttpGet(g.url))()
    end)
end

-- Кнопка сворачивания
local minBtn = Instance.new("TextButton", sg)
minBtn.Size = UDim2.new(0, 40, 0, 40)
minBtn.Position = UDim2.new(0, 10, 0.5, -20)
minBtn.Text = "H"
minBtn.Visible = false
minBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
minBtn.TextColor3 = Color3.new(1, 1, 1)

-- ЛОГИКА
check.MouseButton1Click:Connect(function()
    if input.Text == CORRECT_KEY then
        keyGroup.Visible = false
        gameGroup.Visible = true
    else
        input.Text = ""
        input.PlaceholderText = "НЕВЕРНО!"
    end
end)

getLink.MouseButton1Click:Connect(function()
    if setclipboard then setclipboard(KEY_URL) end
    getLink.Text = "СКОПИРОВАНО!"
    wait(2)
    getLink.Text = "ПОЛУЧИТЬ КЛЮЧ (LINK)"
end)

-- Свернуть/Развернуть
local close = Instance.new("TextButton", main)
close.Size = UDim2.new(0, 30, 0, 30)
close.Position = UDim2.new(1, -30, 0, 5)
close.Text = "_"
close.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
close.TextColor3 = Color3.new(1, 1, 1)

close.MouseButton1Click:Connect(function()
    main.Visible = false
    minBtn.Visible = true
end)

minBtn.MouseButton1Click:Connect(function()
    main.Visible = true
    minBtn.Visible = false
end)
