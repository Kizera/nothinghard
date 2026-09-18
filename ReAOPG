local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local CoreGui = game:GetService("CoreGui")
local HttpService = game:GetService("HttpService")
local LocalPlayer = Players.LocalPlayer

getgenv().SetupDone = false 
getgenv().IsBuffing = false

-- ==========================================
-- 💾 ระบบ Auto-Save / Load Config
-- ==========================================
local ConfigName = "PremiumRaid_Config.json"
local Settings = {
    AutoClick = true,       -- ค่าเริ่มต้นให้คลิกเลย
    GoldenHeist = false,    -- โหมดดันทอง (พิกัดล็อค)
    AutoFarm = false,       -- โหมดฟาร์มทั่วไป
    Distance = 4,
    ScanRadius = 2500
}

local function LoadSettings()
    if isfile and readfile and isfile(ConfigName) then
        pcall(function()
            local decoded = HttpService:JSONDecode(readfile(ConfigName))
            for k, v in pairs(decoded) do Settings[k] = v end
        end)
    end
end
LoadSettings()

local function SaveSettings()
    if writefile then
        pcall(function() writefile(ConfigName, HttpService:JSONEncode(Settings)) end)
    end
end

-- ==========================================
-- 🪙 Tracker Golden Chips
-- ==========================================
local TRK_Name = "GoldenChipsTracker_V19"
local pUI = pcall(function() return CoreGui.Name end) and CoreGui or LocalPlayer.PlayerGui
if pUI:FindFirstChild(TRK_Name) then pUI[TRK_Name]:Destroy() end

local TrkGui = Instance.new("ScreenGui"); TrkGui.Name = TRK_Name; TrkGui.Parent = pUI
local TrkFrame = Instance.new("Frame"); TrkFrame.Size = UDim2.new(0, 220, 0, 45); TrkFrame.Position = UDim2.new(0, 15, 0.5, 0); TrkFrame.AnchorPoint = Vector2.new(0, 0.5); TrkFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30); TrkFrame.BorderSizePixel = 0; TrkFrame.Parent = TrkGui
local TrkCorner = Instance.new("UICorner"); TrkCorner.CornerRadius = UDim.new(0, 8); TrkCorner.Parent = TrkFrame
local TrkStroke = Instance.new("UIStroke"); TrkStroke.Color = Color3.fromRGB(255, 215, 0); TrkStroke.Thickness = 1.5; TrkStroke.Parent = TrkFrame
local TrkLabel = Instance.new("TextLabel"); TrkLabel.Size = UDim2.new(1, 0, 1, 0); TrkLabel.BackgroundTransparency = 1; TrkLabel.Text = "🪙 Golden Chips: Loading..."; TrkLabel.TextColor3 = Color3.fromRGB(255, 220, 50); TrkLabel.Font = Enum.Font.GothamBold; TrkLabel.TextSize = 15; TrkLabel.Parent = TrkFrame

task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            local stats = LocalPlayer:FindFirstChild("Stats")
            local chips = stats and stats:FindFirstChild("Golden Chips")
            if chips then TrkLabel.Text = "🪙 Golden Chips: " .. tostring(chips.Value):reverse():gsub("%d%d%d", "%1,"):reverse():gsub("^,", "") end
        end)
    end
end)

-- ==========================================
-- 🎨 สร้าง GUI แบบย่อ (มีแค่ปุ่มเปิดปิดหลัก)
-- ==========================================
local UI_Name = "PremiumRaidGUI_V19"
if pUI:FindFirstChild(UI_Name) then pUI[UI_Name]:Destroy() end

local ScreenGui = Instance.new("ScreenGui"); ScreenGui.Name = UI_Name; ScreenGui.Parent = pUI
local MainFrame = Instance.new("Frame"); MainFrame.Size = UDim2.new(0, 300, 0, 180); MainFrame.Position = UDim2.new(0.5, -150, 0.5, -90); MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20); MainFrame.Active = true; MainFrame.Draggable = true; MainFrame.Parent = ScreenGui
local MainCorner = Instance.new("UICorner"); MainCorner.CornerRadius = UDim.new(0, 8); MainCorner.Parent = MainFrame

local TopBar = Instance.new("Frame"); TopBar.Size = UDim2.new(1, 0, 0, 35); TopBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35); TopBar.Parent = MainFrame
local TopCorner = Instance.new("UICorner"); TopCorner.CornerRadius = UDim.new(0, 8); TopCorner.Parent = TopBar
local Title = Instance.new("TextLabel"); Title.Size = UDim2.new(1, -40, 1, 0); Title.Position = UDim2.new(0, 15, 0, 0); Title.BackgroundTransparency = 1; Title.Text = "V19: Auto Heist Locked"; Title.TextColor3 = Color3.fromRGB(255, 255, 255); Title.Font = Enum.Font.GothamBold; Title.TextSize = 14; Title.TextXAlignment = Enum.TextXAlignment.Left; Title.Parent = TopBar

local function CreateToggle(parent, text, flag, yPos)
    local Frame = Instance.new("Frame"); Frame.Size = UDim2.new(1, -20, 0, 40); Frame.Position = UDim2.new(0, 10, 0, yPos); Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35); Frame.Parent = parent
    local Corner = Instance.new("UICorner"); Corner.CornerRadius = UDim.new(0, 6); Corner.Parent = Frame
    local Label = Instance.new("TextLabel"); Label.Size = UDim2.new(1, -60, 1, 0); Label.Position = UDim2.new(0, 15, 0, 0); Label.BackgroundTransparency = 1; Label.Text = text; Label.TextColor3 = Color3.fromRGB(220, 220, 220); Label.Font = Enum.Font.GothamBold; Label.TextSize = 14; Label.TextXAlignment = Enum.TextXAlignment.Left; Label.Parent = Frame
    local CheckboxBg = Instance.new("TextButton"); CheckboxBg.Size = UDim2.new(0, 25, 0, 25); CheckboxBg.Position = UDim2.new(1, -35, 0.5, -12.5); CheckboxBg.BackgroundColor3 = Color3.fromRGB(50, 50, 50); CheckboxBg.Text = ""; CheckboxBg.Parent = Frame
    local CCorner = Instance.new("UICorner"); CCorner.CornerRadius = UDim.new(0, 4); CCorner.Parent = CheckboxBg
    local CheckboxFill = Instance.new("Frame"); CheckboxFill.Size = UDim2.new(1, -4, 1, -4); CheckboxFill.Position = UDim2.new(0, 2, 0, 2); CheckboxFill.BackgroundColor3 = Color3.fromRGB(60, 200, 60); CheckboxFill.Visible = Settings[flag]; CheckboxFill.Parent = CheckboxBg
    local FCorner = Instance.new("UICorner"); FCorner.CornerRadius = UDim.new(0, 4); FCorner.Parent = CheckboxFill
    
    CheckboxBg.MouseButton1Click:Connect(function() 
        Settings[flag] = not Settings[flag]
        CheckboxFill.Visible = Settings[flag]
        SaveSettings() 
        if flag == "GoldenHeist" and Settings.GoldenHeist then getgenv().SetupDone = false end -- รีเซ็ตลอจิกให้ทำใหม่
    end)
end

CreateToggle(MainFrame, "🔥 ล็อคพิกัดดันทองคำ + ออโต้บัพ", "GoldenHeist", 45)
CreateToggle(MainFrame, "⚔️ ออโต้คลิก (Auto Click)", "AutoClick", 90)

-- ==========================================
-- 🧠 ลอจิก ล็อคพิกัด + บัพตามคิว
-- ==========================================
local function CastBuffSequence(hum, char)
    getgenv().IsBuffing = true
    
    -- ฟังก์ชันร่ายบัพ
    local function cast(wName, keyStr)
        local tool = LocalPlayer.Backpack:FindFirstChild(wName) or char:FindFirstChild(wName)
        if tool then
            hum:EquipTool(tool)
            task.wait(0.6)
            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode[keyStr], false, game)
            task.wait(0.1)
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode[keyStr], false, game)
            task.wait(1.5)
        end
    end
    
    -- 1. ไล่กดบัพ
    cast("God of Stands", "U")
    cast("Frost Bazooka", "F")
    cast("Dragon Slayer", "U")
    cast("Reincarnated Slime", "U")
    
    -- 2. ถือ God of Stands รอ
    local mainWep = LocalPlayer.Backpack:FindFirstChild("God of Stands") or char:FindFirstChild("God of Stands")
    if mainWep then hum:EquipTool(mainWep); task.wait(0.6) end
    
    -- 3. เปิดฮาคิ (J) เป็นอย่างสุดท้าย
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.J, false, game)
    task.wait(0.1)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.J, false, game)
    task.wait(0.5)
    
    getgenv().SetupDone = true
    getgenv().IsBuffing = false
end

-- รีเซ็ตค่าเมื่อตัวละครตาย/เกิดใหม่
LocalPlayer.CharacterAdded:Connect(function()
    getgenv().SetupDone = false
    getgenv().IsBuffing = false
end)

-- Main Loop
if getgenv().FarmLoop then getgenv().FarmLoop:Disconnect() end
getgenv().FarmLoop = RunService.Heartbeat:Connect(function()
    if not Settings.GoldenHeist then return end
    
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChild("Humanoid")
    
    if hrp and hum and hum.Health > 0 then
        -- ล็อคพิกัดและ Anchored ให้ยืนนิ่ง
        hrp.CFrame = CFrame.new(-219, 15086, 1741)
        hrp.Anchored = true
        
        -- ทำการบัพถ้ายังไม่ได้ทำ
        if not getgenv().SetupDone and not getgenv().IsBuffing then
            task.spawn(CastBuffSequence, hum, char)
        end
    end
end)

-- ==========================================
-- ⚔️ Auto Click
-- ==========================================
task.spawn(function()
    while task.wait(0.1) do
        -- จะคลิกก็ต่อเมื่อเปิด AutoClick และ จัดการบัพเสร็จแล้ว (ไม่คลิกแทรกตอนร่ายเวทย์)
        if Settings.AutoClick and getgenv().SetupDone and not getgenv().IsBuffing then
            pcall(function()
                local cam = workspace.CurrentCamera
                local midX = cam.ViewportSize.X / 2
                local midY = cam.ViewportSize.Y / 2
                VirtualInputManager:SendMouseButtonEvent(midX, midY, 0, true, game, 1)
                task.wait(0.05)
                VirtualInputManager:SendMouseButtonEvent(midX, midY, 0, false, game, 1)
            end)
        end
    end
end)
