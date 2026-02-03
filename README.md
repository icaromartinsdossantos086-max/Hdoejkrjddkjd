--// Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

--// GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AnimationHub"
ScreenGui.Parent = game.CoreGui

-- Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 400)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Parent = ScreenGui

-- Barra de pesquisa
local SearchBox = Instance.new("TextBox")
SearchBox.Size = UDim2.new(0, 280, 0, 30)
SearchBox.Position = UDim2.new(0, 10, 0, 10)
SearchBox.PlaceholderText = "Pesquise por animação ou emote..."
SearchBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SearchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
SearchBox.Parent = MainFrame

-- ScrollingFrame para listar animações
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Size = UDim2.new(0, 280, 0, 350)
ScrollFrame.Position = UDim2.new(0, 10, 0, 50)
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollFrame.ScrollBarThickness = 6
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.Parent = MainFrame

-- UIListLayout para organizar os botões
local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ScrollFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 5)

--// Lista de animações (você pode adicionar mais)
local animations = {
    {Name = "Dançar", Id = "rbxassetid://507766666"}, 
    {Name = "Sentar", Id = "rbxassetid://180435571"}, 
    {Name = "Acenar", Id = "rbxassetid://178130996"},
    {Name = "Feliz", Id = "rbxassetid://6074263717"},
    {Name = "Triste", Id = "rbxassetid://6074264749"}
}

-- Função para criar botões
local function createButton(anim)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 40)
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Text = anim.Name
    button.Parent = ScrollFrame

    button.MouseButton1Click:Connect(function()
        local animObj = Instance.new("Animation")
        animObj.AnimationId = anim.Id
        local track = Humanoid:LoadAnimation(animObj)
        track:Play()
    end)
end

-- Inicializa todos os botões
for _, anim in pairs(animations) do
    createButton(anim)
end

-- Atualiza a pesquisa
SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
    local search = SearchBox.Text:lower()
    ScrollFrame:ClearAllChildren()
    for _, anim in pairs(animations) do
        if anim.Name:lower():find(search) then
            createButton(anim)
        end
    end
end)
