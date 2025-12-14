--// Library
local Lib = {}

function Lib:MakePrototypeLibrary(Title)
    local SG = Instance.new("ScreenGui")
    SG.Name = "PrototypeAstralic"
    SG.DisplayOrder = math.huge
    SG.ResetOnSpawn = false
    SG.IgnoreGuiInset = true
    SG.Parent = gethui and gethui() or game.CoreGui

    --// Main Frame
    local Main = Instance.new("Frame")
    Main.Name = "Main"
    Main.BackgroundColor3 = Color3.fromHex("121318")
    Main.Size = UDim2.new(0, 450, 0, 300)
    Main.Position = UDim2.new(0.5, -225, 0.5, -150)
    Main.AnchorPoint = Vector2.new(0.5, 0.5)
    Main.BorderSizePixel = 0
    Main.Active = true
    Main.Draggable = true
    Main.Parent = SG

    Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 8)

    --// Close Button
    local Close = Instance.new("TextButton")
    Close.Size = UDim2.new(0, 30, 0, 30)
    Close.Position = UDim2.new(1, -40, 0, 10)
    Close.Text = "X"
    Close.Font = Enum.Font.GothamBold
    Close.TextScaled = true
    Close.TextColor3 = Color3.fromHex("AFA1FD")
    Close.BackgroundTransparency = 1
    Close.Parent = Main

    Close.MouseButton1Click:Connect(function()
        SG:Destroy()
    end)

    --// Title
    local TitleUI = Instance.new("TextLabel")
    TitleUI.Size = UDim2.new(1, -60, 0, 30)
    TitleUI.Position = UDim2.new(0, 10, 0, 10)
    TitleUI.Text = Title
    TitleUI.Font = Enum.Font.GothamBold
    TitleUI.TextScaled = true
    TitleUI.TextXAlignment = Enum.TextXAlignment.Left
    TitleUI.TextColor3 = Color3.fromHex("AFA1FD")
    TitleUI.BackgroundTransparency = 1
    TitleUI.Parent = Main

    --// Toggle UI Button
    local ToggleUI = Instance.new("TextButton")
    ToggleUI.Size = UDim2.new(0, 32, 0, 32)
    ToggleUI.Position = UDim2.new(0, 10, 1, -42)
    ToggleUI.Text = "P"
    ToggleUI.Font = Enum.Font.GothamBold
    ToggleUI.TextScaled = true
    ToggleUI.TextColor3 = Color3.fromHex("AFA1FD")
    ToggleUI.BackgroundColor3 = Color3.fromHex("121318")
    ToggleUI.BackgroundTransparency = 0.4
    ToggleUI.Parent = SG

    Instance.new("UICorner", ToggleUI).CornerRadius = UDim.new(0, 8)

    ToggleUI.MouseButton1Click:Connect(function()
        Main.Visible = not Main.Visible
    end)

    --// Tabs List
    local TabsOpenContainer = Instance.new("ScrollingFrame")
    TabsOpenContainer.Size = UDim2.new(0, 105, 0, 245)
    TabsOpenContainer.Position = UDim2.new(0, 10, 0, 45)
    TabsOpenContainer.BackgroundColor3 = Color3.fromHex("17181D")
    TabsOpenContainer.BorderSizePixel = 0
    TabsOpenContainer.ScrollBarThickness = 0
    TabsOpenContainer.AutomaticCanvasSize = Enum.AutomaticSize.Y
    TabsOpenContainer.CanvasSize = UDim2.new(0,0,0,0)
    TabsOpenContainer.Parent = Main

    local TabListLayout = Instance.new("UIListLayout")
    TabListLayout.Padding = UDim.new(0, 4)
    TabListLayout.Parent = TabsOpenContainer

    --// Tabs Content
    local TabsContainer = Instance.new("Frame")
    TabsContainer.Size = UDim2.new(0, 315, 0, 245)
    TabsContainer.Position = UDim2.new(0, 125, 0, 45)
    TabsContainer.BackgroundColor3 = Color3.fromHex("17181D")
    TabsContainer.BorderSizePixel = 0
    TabsContainer.Parent = Main

    --// Tabs System
    local MakeATab = {}

    function MakeATab:MakeTab(TabTitle, Visible)
        local OpenTab = Instance.new("TextButton")
        OpenTab.Size = UDim2.new(1, 0, 0, 25)
        OpenTab.Text = TabTitle
        OpenTab.Font = Enum.Font.GothamBold
        OpenTab.TextScaled = true
        OpenTab.BackgroundColor3 = Color3.fromHex("1D1D27")
        OpenTab.TextColor3 = Visible and Color3.fromHex("AFA1FD") or Color3.fromHex("7A7A84")
        OpenTab.AutoButtonColor = false
        OpenTab.BorderSizePixel = 0
        OpenTab.Parent = TabsOpenContainer

        Instance.new("UICorner", OpenTab).CornerRadius = UDim.new(0, 8)

        local Tab = Instance.new("ScrollingFrame")
        Tab.Size = UDim2.new(1, 0, 1, 0)
        Tab.Visible = Visible or false
        Tab.ScrollBarThickness = 0
        Tab.BackgroundTransparency = 1
        Tab.AutomaticCanvasSize = Enum.AutomaticSize.Y
        Tab.CanvasSize = UDim2.new(0,0,0,0)
        Tab.Parent = TabsContainer

        local Layout = Instance.new("UIListLayout")
        Layout.Padding = UDim.new(0, 6)
        Layout.Parent = Tab

        OpenTab.MouseButton1Click:Connect(function()
            for _,v in ipairs(TabsOpenContainer:GetChildren()) do
                if v:IsA("TextButton") then
                    v.TextColor3 = Color3.fromHex("7A7A84")
                end
            end

            for _,v in ipairs(TabsContainer:GetChildren()) do
                if v:IsA("ScrollingFrame") then
                    v.Visible = false
                end
            end

            OpenTab.TextColor3 = Color3.fromHex("AFA1FD")
            Tab.Visible = true
        end)

        --// Elements
        local TabElements = {}

        function TabElements:Info(text)
            local Info = Instance.new("TextLabel")
            Info.Size = UDim2.new(1, 0, 0, 25)
            Info.Text = text
            Info.Font = Enum.Font.GothamBold
            Info.TextScaled = true
            Info.TextXAlignment = Enum.TextXAlignment.Left
            Info.TextColor3 = Color3.fromHex("AFA1FD")
            Info.BackgroundTransparency = 1
            Info.Parent = Tab
        end

        function TabElements:Button(text, callback)
            local Button = Instance.new("TextButton")
            Button.Size = UDim2.new(1, 0, 0, 25)
            Button.Text = text
            Button.Font = Enum.Font.GothamBold
            Button.TextScaled = true
            Button.TextXAlignment = Enum.TextXAlignment.Left
            Button.TextColor3 = Color3.fromHex("7A7A84")
            Button.BackgroundColor3 = Color3.fromHex("1D1D27")
            Button.BorderSizePixel = 0
            Button.Parent = Tab

            Instance.new("UICorner", Button).CornerRadius = UDim.new(0, 8)

            Button.MouseButton1Click:Connect(function()
                task.spawn(function()
                    Button.TextColor3 = Color3.fromHex("AFA1FD")
                    task.wait(0.3)
                    Button.TextColor3 = Color3.fromHex("7A7A84")
                end)

                if callback then
                    pcall(callback)
                end
            end)
        end

        function TabElements:Toggle(text, callback)
            local Toggle = Instance.new("TextButton")
            Toggle.Size = UDim2.new(1, 0, 0, 25)
            Toggle.Text = text
            Toggle.Font = Enum.Font.GothamBold
            Toggle.TextScaled = true
            Toggle.TextXAlignment = Enum.TextXAlignment.Left
            Toggle.TextColor3 = Color3.fromHex("7A7A84")
            Toggle.BackgroundColor3 = Color3.fromHex("1D1D27")
            Toggle.BorderSizePixel = 0
            Toggle.Parent = Tab

            Instance.new("UICorner", Toggle).CornerRadius = UDim.new(0, 8)

            local Enabled = false

            Toggle.MouseButton1Click:Connect(function()
                Enabled = not Enabled
                Toggle.TextColor3 = Enabled and Color3.fromHex("AFA1FD") or Color3.fromHex("7A7A84")

                if callback then
                    pcall(callback, Enabled)
                end
            end)
        end

        return TabElements
    end

    return MakeATab
end

return Lib
	
