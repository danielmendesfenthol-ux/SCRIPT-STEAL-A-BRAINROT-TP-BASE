--// Função universal para tornar GUIs arrastáveis
local function MakeDraggable(frame, dragger)
    dragger = dragger or frame
    local dragging, dragInput, mousePos, framePos

    dragger.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            mousePos = input.Position
            framePos = frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    dragger.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - mousePos
            frame.Position = UDim2.new(framePos.X.Scale, framePos.X.Offset + delta.X, framePos.Y.Scale, framePos.Y.Offset + delta.Y)
        end
    end)
end


--// Criar ScreenGui
local gui = Instance.new("ScreenGui")
gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

--// PAINEL PRINCIPAL DECORADO
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 210, 0, 130)
main.Position = UDim2.new(0.05, 0, 0.4, 0)
main.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
main.BorderSizePixel = 0
main.Parent = gui

local corner = Instance.new("UICorner", main)
corner.CornerRadius = UDim.new(0, 12)

-- Bordas decorativas
local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(0, 140, 255)
stroke.Thickness = 3

local gradient = Instance.new("UIGradient", main)
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 100, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
})
gradient.Rotation = 45

-- Título
local title = Instance.new("TextLabel")
title.Parent = main
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "Painel Teleporte"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18

-- Botão TP Base
local tpBtn = Instance.new("TextButton")
tpBtn.Parent = main
tpBtn.Size = UDim2.new(0, 160, 0, 40)
tpBtn.Position = UDim2.new(0.5, -80, 0.55, -20)
tpBtn.BackgroundColor3 = Color3.fromRGB(0, 110, 255)
tpBtn.Text = "TP Base"
tpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tpBtn.Font = Enum.Font.GothamBold
tpBtn.TextSize = 16

local btnCorner = Instance.new("UICorner", tpBtn)
btnCorner.CornerRadius = UDim.new(0, 8)

-- PAINEL PRINCIPAL AGORA VIRANDO ARRASTÁVEL:
MakeDraggable(main, title)



--// FUNÇÃO DA MENSAGEM AO CLICAR
tpBtn.MouseButton1Click:Connect(function()

    -- Criar aviso
    local msg = Instance.new("Frame")
    msg.Size = UDim2.new(0, 250, 0, 70)
    msg.Position = UDim2.new(1, -260, 0, 20)
    msg.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    msg.BorderSizePixel = 0
    msg.Parent = gui

    local msgCorner = Instance.new("UICorner", msg)
    msgCorner.CornerRadius = UDim.new(0, 10)

    local msgStroke = Instance.new("UIStroke", msg)
    msgStroke.Color = Color3.fromRGB(0, 255, 110)
    msgStroke.Thickness = 3

    -- Texto da mensagem
    local label = Instance.new("TextLabel")
    label.Parent = msg
    label.Size = UDim2.new(1, -40, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = "✅️Teleportado com sucesso!"
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 16
    label.TextWrapped = true

    -- Botão X
    local close = Instance.new("TextButton")
    close.Parent = msg
    close.Size = UDim2.new(0, 25, 0, 25)
    close.Position = UDim2.new(1, -30, 0, 5)
    close.Text = "X"
    close.TextColor3 = Color3.fromRGB(255, 100, 100)
    close.BackgroundTransparency = 1
    close.Font = Enum.Font.GothamBold
    close.TextSize = 18

    close.MouseButton1Click:Connect(function()
        msg:Destroy()
    end)

    -- MENSAGEM ARRÁSTAVEL
    MakeDraggable(msg, label)

    -- Sumir depois de 10s
    task.delay(10, function()
        if msg then msg:Destroy() end
    end)

end)
