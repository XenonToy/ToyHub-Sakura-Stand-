-- [[ UI COMBINED TAB WITH SUB-TABS & PROXIMITY PROMPT (PRESS E LONG) SYSTEM ]] --
-- [[ + FARM ALL MOBS TAB WITH INDIVIDUAL MOB SELECTION ]] --
-- [[ + ANTI-AFK + MINIMIZE WITH K ]] --

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local VirtualInputManager = game:GetService("VirtualInputManager")
local GuiService = game:GetService("GuiService")
local UserInputService = game:GetService("UserInputService")
local VirtualUser = game:GetService("VirtualUser")
local localPlayer = Players.LocalPlayer

local MapFolder = Workspace:WaitForChild("Map", 5)
local NpcFolder = MapFolder and MapFolder:WaitForChild("NPCs", 5)
local LivingFolder = Workspace:WaitForChild("Living", 5)

local _G = _G or {}
_G.AutoFarmQuest = false
_G.AutoFarmMobs = false
_G.FarmPosition = "Behind"
_G.FarmMobPosition = "Behind"
_G.SelectedMobs = {}

local currentSubTab = "NPCs"
local isMinimized = false

local oldUI = CoreGui:FindFirstChild("TeleportMenuGUI") or localPlayer:WaitForChild("PlayerGui"):FindFirstChild("TeleportMenuGUI")
if oldUI then oldUI:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportMenuGUI"
ScreenGui.ResetOnSpawn = false
pcall(function() ScreenGui.Parent = CoreGui end)
if not ScreenGui.Parent then ScreenGui.Parent = localPlayer:WaitForChild("PlayerGui") end

-- ============================================================
-- MINIMIZE BUTTON (sempre visibile)
-- ============================================================
local MinimizeHint = Instance.new("TextLabel")
MinimizeHint.Name = "MinimizeHint"
MinimizeHint.Size = UDim2.new(0, 120, 0, 22)
MinimizeHint.Position = UDim2.new(0.5, -160, 0.4, -215)
MinimizeHint.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
MinimizeHint.Text = "  [K] Minimize UI"
MinimizeHint.TextColor3 = Color3.fromRGB(150, 150, 150)
MinimizeHint.TextSize = 11
MinimizeHint.Font = Enum.Font.SourceSans
MinimizeHint.TextXAlignment = Enum.TextXAlignment.Left
MinimizeHint.BorderSizePixel = 0
MinimizeHint.Parent = ScreenGui
Instance.new("UICorner", MinimizeHint).CornerRadius = UDim.new(0, 4)

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 320, 0, 380)
MainFrame.Position = UDim2.new(0.5, -160, 0.4, -190)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 8)
MainCorner.Parent = MainFrame

-- Top Bar
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local TopCorner = Instance.new("UICorner")
TopCorner.CornerRadius = UDim.new(0, 8)
TopCorner.Parent = TopBar

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, -40, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "Toy Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 14
Title.Font = Enum.Font.SourceSansBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

local DestroyBtn = Instance.new("TextButton")
DestroyBtn.Name = "DestroyBtn"
DestroyBtn.Size = UDim2.new(0, 25, 0, 25)
DestroyBtn.Position = UDim2.new(1, -30, 0, 5)
DestroyBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
DestroyBtn.Text = "X"
DestroyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
DestroyBtn.TextSize = 14
DestroyBtn.Font = Enum.Font.SourceSansBold
DestroyBtn.Parent = TopBar
Instance.new("UICorner", DestroyBtn).CornerRadius = UDim.new(0, 5)

-- TAB BAR
local TabBar = Instance.new("Frame")
TabBar.Name = "TabBar"
TabBar.Size = UDim2.new(1, 0, 0, 30)
TabBar.Position = UDim2.new(0, 0, 0, 35)
TabBar.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
TabBar.BorderSizePixel = 0
TabBar.Parent = MainFrame

local HomeTabBtn = Instance.new("TextButton")
HomeTabBtn.Name = "HomeTabBtn"
HomeTabBtn.Size = UDim2.new(0.25, 0, 1, 0)
HomeTabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
HomeTabBtn.Text = "Players"
HomeTabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
HomeTabBtn.Font = Enum.Font.SourceSansBold
HomeTabBtn.TextSize = 10
HomeTabBtn.BorderSizePixel = 0
HomeTabBtn.Parent = TabBar

local CombinedTabBtn = Instance.new("TextButton")
CombinedTabBtn.Name = "CombinedTabBtn"
CombinedTabBtn.Size = UDim2.new(0.25, 0, 1, 0)
CombinedTabBtn.Position = UDim2.new(0.25, 0, 0, 0)
CombinedTabBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
CombinedTabBtn.Text = "Teleports"
CombinedTabBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
CombinedTabBtn.Font = Enum.Font.SourceSansBold
CombinedTabBtn.TextSize = 10
CombinedTabBtn.BorderSizePixel = 0
CombinedTabBtn.Parent = TabBar

local QuestTabBtn = Instance.new("TextButton")
QuestTabBtn.Name = "QuestTabBtn"
QuestTabBtn.Size = UDim2.new(0.25, 0, 1, 0)
QuestTabBtn.Position = UDim2.new(0.5, 0, 0, 0)
QuestTabBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
QuestTabBtn.Text = "Quest"
QuestTabBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
QuestTabBtn.Font = Enum.Font.SourceSansBold
QuestTabBtn.TextSize = 10
QuestTabBtn.BorderSizePixel = 0
QuestTabBtn.Parent = TabBar

local FarmTabBtn = Instance.new("TextButton")
FarmTabBtn.Name = "FarmTabBtn"
FarmTabBtn.Size = UDim2.new(0.25, 0, 1, 0)
FarmTabBtn.Position = UDim2.new(0.75, 0, 0, 0)
FarmTabBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
FarmTabBtn.Text = "⚔️Farm"
FarmTabBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
FarmTabBtn.Font = Enum.Font.SourceSansBold
FarmTabBtn.TextSize = 10
FarmTabBtn.BorderSizePixel = 0
FarmTabBtn.Parent = TabBar

-- ============================================================
-- PAGE: Players
-- ============================================================
local PlayersPage = Instance.new("ScrollingFrame")
PlayersPage.Name = "PlayersPage"
PlayersPage.Size = UDim2.new(1, -16, 1, -75)
PlayersPage.Position = UDim2.new(0, 8, 0, 70)
PlayersPage.BackgroundTransparency = 1
PlayersPage.BorderSizePixel = 0
PlayersPage.CanvasSize = UDim2.new(0, 0, 0, 0)
PlayersPage.ScrollBarThickness = 4
PlayersPage.Visible = true
PlayersPage.Parent = MainFrame

local PLayout = Instance.new("UIListLayout")
PLayout.Parent = PlayersPage
PLayout.Padding = UDim.new(0, 5)

-- ============================================================
-- PAGE: Teleport (Sub-tabs NPCs / Mobs)
-- ============================================================
local CombinedPage = Instance.new("Frame")
CombinedPage.Name = "CombinedPage"
CombinedPage.Size = UDim2.new(1, -16, 1, -75)
CombinedPage.Position = UDim2.new(0, 8, 0, 70)
CombinedPage.BackgroundTransparency = 1
CombinedPage.Visible = false
CombinedPage.Parent = MainFrame

local SubTabBar = Instance.new("Frame")
SubTabBar.Name = "SubTabBar"
SubTabBar.Size = UDim2.new(1, 0, 0, 25)
SubTabBar.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
SubTabBar.BorderSizePixel = 0
SubTabBar.Parent = CombinedPage

local NpcSubBtn = Instance.new("TextButton")
NpcSubBtn.Size = UDim2.new(0.5, -2, 1, 0)
NpcSubBtn.BackgroundColor3 = Color3.fromRGB(45, 60, 50)
NpcSubBtn.Text = "🤖 NPCs"
NpcSubBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
NpcSubBtn.Font = Enum.Font.SourceSansBold
NpcSubBtn.TextSize = 11
NpcSubBtn.BorderSizePixel = 0
NpcSubBtn.Parent = SubTabBar

local MobSubBtn = Instance.new("TextButton")
MobSubBtn.Size = UDim2.new(0.5, -2, 1, 0)
MobSubBtn.Position = UDim2.new(0.5, 2, 0, 0)
MobSubBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
MobSubBtn.Text = "💥 Mobs"
MobSubBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
MobSubBtn.Font = Enum.Font.SourceSansBold
MobSubBtn.TextSize = 11
MobSubBtn.BorderSizePixel = 0
MobSubBtn.Parent = SubTabBar

local UnifiedSearch = Instance.new("TextBox")
UnifiedSearch.Size = UDim2.new(1, 0, 0, 25)
UnifiedSearch.Position = UDim2.new(0, 0, 0, 30)
UnifiedSearch.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
UnifiedSearch.BorderSizePixel = 0
UnifiedSearch.PlaceholderText = "🔍 Search Name..."
UnifiedSearch.Text = ""
UnifiedSearch.TextColor3 = Color3.fromRGB(255, 255, 255)
UnifiedSearch.PlaceholderColor3 = Color3.fromRGB(120, 120, 120)
UnifiedSearch.TextSize = 12
UnifiedSearch.Font = Enum.Font.SourceSans
UnifiedSearch.Parent = CombinedPage

local CombinedScroll = Instance.new("ScrollingFrame")
CombinedScroll.Size = UDim2.new(1, 0, 1, -60)
CombinedScroll.Position = UDim2.new(0, 0, 0, 60)
CombinedScroll.BackgroundTransparency = 1
CombinedScroll.BorderSizePixel = 0
CombinedScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
CombinedScroll.ScrollBarThickness = 4
CombinedScroll.Parent = CombinedPage

local CLayout = Instance.new("UIListLayout")
CLayout.Parent = CombinedScroll
CLayout.Padding = UDim.new(0, 5)

-- ============================================================
-- PAGE: Quest Farm
-- ============================================================
local QuestPage = Instance.new("Frame")
QuestPage.Name = "QuestPage"
QuestPage.Size = UDim2.new(1, -16, 1, -75)
QuestPage.Position = UDim2.new(0, 8, 0, 70)
QuestPage.BackgroundTransparency = 1
QuestPage.Visible = false
QuestPage.Parent = MainFrame

local QuestInfoLabel = Instance.new("TextLabel")
QuestInfoLabel.Size = UDim2.new(1, 0, 0, 35)
QuestInfoLabel.Position = UDim2.new(0, 0, 0, 5)
QuestInfoLabel.BackgroundTransparency = 1
QuestInfoLabel.Text = "📜 Dynamic Auto Farm System\nรองรับระบบกดรับเควสค้าง 3 วินาที (Hold E)"
QuestInfoLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
QuestInfoLabel.Font = Enum.Font.SourceSansBold
QuestInfoLabel.TextSize = 12
QuestInfoLabel.Parent = QuestPage

local PosLabel = Instance.new("TextLabel")
PosLabel.Size = UDim2.new(1, 0, 0, 20)
PosLabel.Position = UDim2.new(0, 0, 0, 45)
PosLabel.BackgroundTransparency = 1
PosLabel.Text = "เลือกตำแหน่งวาร์ปฟาร์มมอนสเตอร์:"
PosLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
PosLabel.Font = Enum.Font.SourceSansBold
PosLabel.TextSize = 11
PosLabel.TextXAlignment = Enum.TextXAlignment.Left
PosLabel.Parent = QuestPage

local BehindBtn = Instance.new("TextButton")
BehindBtn.Size = UDim2.new(0.48, 0, 0, 30)
BehindBtn.Position = UDim2.new(0, 0, 0, 70)
BehindBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
BehindBtn.Text = "ข้างหลังmob ✔"
BehindBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
BehindBtn.Font = Enum.Font.SourceSansBold
BehindBtn.TextSize = 12
BehindBtn.Parent = QuestPage
Instance.new("UICorner", BehindBtn).CornerRadius = UDim.new(0, 4)

local AboveBtn = Instance.new("TextButton")
AboveBtn.Size = UDim2.new(0.48, 0, 0, 30)
AboveBtn.Position = UDim2.new(0.52, 0, 0, 70)
AboveBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
AboveBtn.Text = "ข้างบนmob"
AboveBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
AboveBtn.Font = Enum.Font.SourceSansBold
AboveBtn.TextSize = 12
AboveBtn.Parent = QuestPage
Instance.new("UICorner", AboveBtn).CornerRadius = UDim.new(0, 4)

local ToggleFarmBtn = Instance.new("TextButton")
ToggleFarmBtn.Size = UDim2.new(1, 0, 0, 40)
ToggleFarmBtn.Position = UDim2.new(0, 0, 0, 115)
ToggleFarmBtn.BackgroundColor3 = Color3.fromRGB(180, 70, 70)
ToggleFarmBtn.Text = "🔴 START AUTO FARM QUEST"
ToggleFarmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleFarmBtn.Font = Enum.Font.SourceSansBold
ToggleFarmBtn.TextSize = 13
ToggleFarmBtn.Parent = QuestPage
Instance.new("UICorner", ToggleFarmBtn).CornerRadius = UDim.new(0, 6)

-- ============================================================
-- PAGE: Farm All Mobs
-- ============================================================
local FarmPage = Instance.new("Frame")
FarmPage.Name = "FarmPage"
FarmPage.Size = UDim2.new(1, -16, 1, -75)
FarmPage.Position = UDim2.new(0, 8, 0, 70)
FarmPage.BackgroundTransparency = 1
FarmPage.Visible = false
FarmPage.Parent = MainFrame

local FarmTopRow = Instance.new("Frame")
FarmTopRow.Size = UDim2.new(1, 0, 0, 28)
FarmTopRow.Position = UDim2.new(0, 0, 0, 0)
FarmTopRow.BackgroundTransparency = 1
FarmTopRow.Parent = FarmPage

local SelectAllBtn = Instance.new("TextButton")
SelectAllBtn.Size = UDim2.new(0.48, 0, 1, 0)
SelectAllBtn.BackgroundColor3 = Color3.fromRGB(50, 80, 50)
SelectAllBtn.Text = "✅ Select All"
SelectAllBtn.TextColor3 = Color3.fromRGB(200, 255, 200)
SelectAllBtn.Font = Enum.Font.SourceSansBold
SelectAllBtn.TextSize = 11
SelectAllBtn.BorderSizePixel = 0
SelectAllBtn.Parent = FarmTopRow
Instance.new("UICorner", SelectAllBtn).CornerRadius = UDim.new(0, 4)

local ClearAllBtn = Instance.new("TextButton")
ClearAllBtn.Size = UDim2.new(0.48, 0, 1, 0)
ClearAllBtn.Position = UDim2.new(0.52, 0, 0, 0)
ClearAllBtn.BackgroundColor3 = Color3.fromRGB(80, 50, 50)
ClearAllBtn.Text = "❌ Clear All"
ClearAllBtn.TextColor3 = Color3.fromRGB(255, 200, 200)
ClearAllBtn.Font = Enum.Font.SourceSansBold
ClearAllBtn.TextSize = 11
ClearAllBtn.BorderSizePixel = 0
ClearAllBtn.Parent = FarmTopRow
Instance.new("UICorner", ClearAllBtn).CornerRadius = UDim.new(0, 4)

local FarmPosRow = Instance.new("Frame")
FarmPosRow.Size = UDim2.new(1, 0, 0, 28)
FarmPosRow.Position = UDim2.new(0, 0, 0, 32)
FarmPosRow.BackgroundTransparency = 1
FarmPosRow.Parent = FarmPage

local FarmBehindBtn = Instance.new("TextButton")
FarmBehindBtn.Size = UDim2.new(0.48, 0, 1, 0)
FarmBehindBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
FarmBehindBtn.Text = "ข้างหลัง ✔"
FarmBehindBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
FarmBehindBtn.Font = Enum.Font.SourceSansBold
FarmBehindBtn.TextSize = 11
FarmBehindBtn.BorderSizePixel = 0
FarmBehindBtn.Parent = FarmPosRow
Instance.new("UICorner", FarmBehindBtn).CornerRadius = UDim.new(0, 4)

local FarmAboveBtn = Instance.new("TextButton")
FarmAboveBtn.Size = UDim2.new(0.48, 0, 1, 0)
FarmAboveBtn.Position = UDim2.new(0.52, 0, 0, 0)
FarmAboveBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
FarmAboveBtn.Text = "ข้างบน"
FarmAboveBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
FarmAboveBtn.Font = Enum.Font.SourceSansBold
FarmAboveBtn.TextSize = 11
FarmAboveBtn.BorderSizePixel = 0
FarmAboveBtn.Parent = FarmPosRow
Instance.new("UICorner", FarmAboveBtn).CornerRadius = UDim.new(0, 4)

local FarmSearch = Instance.new("TextBox")
FarmSearch.Size = UDim2.new(1, 0, 0, 24)
FarmSearch.Position = UDim2.new(0, 0, 0, 64)
FarmSearch.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
FarmSearch.BorderSizePixel = 0
FarmSearch.PlaceholderText = "🔍 Filter mobs..."
FarmSearch.Text = ""
FarmSearch.TextColor3 = Color3.fromRGB(255, 255, 255)
FarmSearch.PlaceholderColor3 = Color3.fromRGB(120, 120, 120)
FarmSearch.TextSize = 12
FarmSearch.Font = Enum.Font.SourceSans
FarmSearch.Parent = FarmPage
Instance.new("UICorner", FarmSearch).CornerRadius = UDim.new(0, 4)

-- Mob list scroll
-- Ridotto per fare spazio ai bottoni sotto (Anti-AFK + Start)
local FarmScroll = Instance.new("ScrollingFrame")
FarmScroll.Size = UDim2.new(1, 0, 1, -168)
FarmScroll.Position = UDim2.new(0, 0, 0, 92)
FarmScroll.BackgroundTransparency = 1
FarmScroll.BorderSizePixel = 0
FarmScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
FarmScroll.ScrollBarThickness = 4
FarmScroll.Parent = FarmPage

local FarmLayout = Instance.new("UIListLayout")
FarmLayout.Parent = FarmScroll
FarmLayout.Padding = UDim.new(0, 4)

-- Toggle Farm Mob (sopra anti-afk)
local ToggleMobFarmBtn = Instance.new("TextButton")
ToggleMobFarmBtn.Size = UDim2.new(1, 0, 0, 34)
ToggleMobFarmBtn.Position = UDim2.new(0, 0, 1, -110)
ToggleMobFarmBtn.BackgroundColor3 = Color3.fromRGB(180, 70, 70)
ToggleMobFarmBtn.Text = "🔴 START FARM SELECTED MOBS"
ToggleMobFarmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleMobFarmBtn.Font = Enum.Font.SourceSansBold
ToggleMobFarmBtn.TextSize = 12
ToggleMobFarmBtn.Parent = FarmPage
Instance.new("UICorner", ToggleMobFarmBtn).CornerRadius = UDim.new(0, 6)

-- Anti-AFK Toggle
local AntiAfkBtn = Instance.new("TextButton")
AntiAfkBtn.Name = "AntiAfkBtn"
AntiAfkBtn.Parent = FarmPage
AntiAfkBtn.Size = UDim2.new(1, 0, 0, 30)
AntiAfkBtn.Position = UDim2.new(0, 0, 1, -74)
AntiAfkBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
AntiAfkBtn.Text = "🛡️ Anti-AFK: OFF"
AntiAfkBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
AntiAfkBtn.Font = Enum.Font.SourceSansBold
AntiAfkBtn.TextSize = 12
Instance.new("UICorner", AntiAfkBtn).CornerRadius = UDim.new(0, 6)

-- 2. "วางโค้ด Auto Block ชุดนี้ ต่อท้ายทันทีหลังจากจบ AntiAfkBtn"
local AutoBlockBtn = Instance.new("TextButton")
AutoBlockBtn.Name = "AutoBlockBtn"
AutoBlockBtn.Size = UDim2.new(1, 0, 0, 30)
AutoBlockBtn.Position = UDim2.new(0, 0, 1, -38)
AutoBlockBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
AutoBlockBtn.Text = "🛡️ Auto Block: OFF"
AutoBlockBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
AutoBlockBtn.Font = Enum.Font.SourceSansBold
AutoBlockBtn.TextSize = 12
AutoBlockBtn.Parent = FarmPage
Instance.new("UICorner", AutoBlockBtn).CornerRadius = UDim.new(0, 6)

-- 3. Logic สำหรับ Auto Block (วางไว้หลังสร้างปุ่ม)
local isAutoBlockEnabled = false
local function fireWarp(state)
    local remote
    pcall(function()
        remote = game:GetService("ReplicatedStorage")
            :WaitForChild("ABC - First Priority")
            :WaitForChild("Utility")
            :WaitForChild("Modules")
            :WaitForChild("Warp")
            :WaitForChild("Index")
            :WaitForChild("Event")
            :WaitForChild("Reliable")
    end)
    
    if remote then
        local args = {
            buffer.fromstring("\024"),
            buffer.fromstring("\254\002\000\006\001F\005" .. (state and "\001" or "\000"))
        }
        remote:FireServer(unpack(args))
    end
end

AutoBlockBtn.MouseButton1Click:Connect(function()
    isAutoBlockEnabled = not isAutoBlockEnabled
    AutoBlockBtn.Text = "🛡️ Auto Block: " .. (isAutoBlockEnabled and "ON" or "OFF")
    AutoBlockBtn.BackgroundColor3 = isAutoBlockEnabled and Color3.fromRGB(70, 100, 70) or Color3.fromRGB(60, 60, 70)
    fireWarp(isAutoBlockEnabled)
end)

----=======
---collect box
----=======
local function collectItems()
    -- รายชื่อไอเทมที่ต้องการเก็บ
    local itemsToCollect = {
        ["BoxDrop1"] = true,
        ["sukunaFinger"] = true,
        ["ItemDrop"] = true
    }
    
    local itemFolder = workspace:FindFirstChild("Item") -- เปลี่ยนชื่อ Folder ตรงนี้ถ้าจำเป็น
    if itemFolder then
        for _, item in pairs(itemFolder:GetChildren()) do
            -- เช็คว่าชื่อไอเทมอยู่ในลิสต์ที่เราต้องการไหม
            if itemsToCollect[item.Name] and item:IsA("BasePart") then
                local lp = game.Players.LocalPlayer
                if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
                    lp.Character.HumanoidRootPart.CFrame = item.CFrame
                    
                    -- กด E ค้าง 2 วินาที
                    local VIM = game:GetService("VirtualInputManager")
                    VIM:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                    task.wait(2.1)
                    VIM:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                    return true
                end
            end
        end
    end
    return false
end

-- ============================================================
-- AUTO CANVAS
-- ============================================================
PLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    PlayersPage.CanvasSize = UDim2.new(0, 0, 0, PLayout.AbsoluteContentSize.Y)
end)
CLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    CombinedScroll.CanvasSize = UDim2.new(0, 0, 0, CLayout.AbsoluteContentSize.Y)
end)
FarmLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    FarmScroll.CanvasSize = UDim2.new(0, 0, 0, FarmLayout.AbsoluteContentSize.Y)
end)

-- ============================================================
-- ANTI-AFK SYSTEM
-- ============================================================
local antiAfkEnabled = false
local antiAfkConnection = nil

local function setAntiAfk(state)
    antiAfkEnabled = state
    if state then
        AntiAfkBtn.BackgroundColor3 = Color3.fromRGB(50, 120, 50)
        AntiAfkBtn.TextColor3 = Color3.fromRGB(200, 255, 200)
        AntiAfkBtn.Text = "🛡️ Anti-AFK: ON"
        antiAfkConnection = localPlayer.Idled:Connect(function()
            VirtualUser:Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
            task.wait(1)
            VirtualUser:Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
        end)
    else
        AntiAfkBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
        AntiAfkBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
        AntiAfkBtn.Text = "🛡️ Anti-AFK: OFF"
        if antiAfkConnection then
            antiAfkConnection:Disconnect()
            antiAfkConnection = nil
        end
    end
end

AntiAfkBtn.MouseButton1Click:Connect(function()
    setAntiAfk(not antiAfkEnabled)
end)

-- ============================================================
-- MINIMIZE WITH K
-- ============================================================
local function setMinimized(state)
    isMinimized = state
    if state then
        -- Nasconde tutto tranne la TopBar (35px) + TabBar (30px) = 65px
        MainFrame:TweenSize(
            UDim2.new(0, 320, 0, 65),
            Enum.EasingDirection.Out,
            Enum.EasingStyle.Quad,
            0.2,
            true
        )
        -- Nasconde il contenuto
        PlayersPage.Visible = false
        CombinedPage.Visible = false
        QuestPage.Visible = false
        FarmPage.Visible = false
        TabBar.Visible = false
        Title.Text = "Toy Hub  [minimized - K to expand]"
        Title.TextSize = 11
    else
        MainFrame:TweenSize(
            UDim2.new(0, 320, 0, 380),
            Enum.EasingDirection.Out,
            Enum.EasingStyle.Quad,
            0.2,
            true,
            function()
                TabBar.Visible = true
                -- Ripristina la pagina attiva
                local activeHome = HomeTabBtn.BackgroundColor3 == Color3.fromRGB(50, 50, 60)
                local activeCombined = CombinedTabBtn.BackgroundColor3 == Color3.fromRGB(50, 50, 60)
                local activeQuest = QuestTabBtn.BackgroundColor3 == Color3.fromRGB(50, 50, 60)
                local activeFarm = FarmTabBtn.TextColor3 == Color3.fromRGB(255, 200, 100)

                if activeFarm then
                    FarmPage.Visible = true
                elseif activeQuest then
                    QuestPage.Visible = true
                elseif activeCombined then
                    CombinedPage.Visible = true
                else
                    PlayersPage.Visible = true
                end
            end
        )
        Title.Text = "Toy Hub"
        Title.TextSize = 14
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.K then
        setMinimized(not isMinimized)
    end
end)

-- ============================================================
-- FARM MOB LIST
-- ============================================================
local mobCheckboxMap = {}

local function buildFarmMobList()
    for _, child in ipairs(FarmScroll:GetChildren()) do
        if child:IsA("Frame") then child:Destroy() end
    end
    mobCheckboxMap = {}

    if not LivingFolder then return end

    local seen = {}
    local searchText = string.lower(FarmSearch.Text)

    for _, mob in ipairs(LivingFolder:GetChildren()) do
        if mob:IsA("Model") and mob:FindFirstChild("HumanoidRootPart") then
            local name = mob.Name
            if not seen[name] then
                if searchText == "" or string.find(string.lower(name), searchText) then
                    seen[name] = true

                    local Row = Instance.new("Frame")
                    Row.Size = UDim2.new(1, -4, 0, 30)
                    Row.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
                    Row.BorderSizePixel = 0
                    Row.Parent = FarmScroll
                    Instance.new("UICorner", Row).CornerRadius = UDim.new(0, 4)

                    local CheckBtn = Instance.new("TextButton")
                    CheckBtn.Size = UDim2.new(0, 28, 0, 28)
                    CheckBtn.Position = UDim2.new(0, 1, 0, 1)
                    CheckBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
                    CheckBtn.Text = _G.SelectedMobs[name] and "✔" or ""
                    CheckBtn.TextColor3 = Color3.fromRGB(100, 220, 100)
                    CheckBtn.Font = Enum.Font.SourceSansBold
                    CheckBtn.TextSize = 14
                    CheckBtn.BorderSizePixel = 0
                    CheckBtn.Parent = Row
                    Instance.new("UICorner", CheckBtn).CornerRadius = UDim.new(0, 4)

                    local NameLabel = Instance.new("TextLabel")
                    NameLabel.Size = UDim2.new(1, -70, 1, 0)
                    NameLabel.Position = UDim2.new(0, 34, 0, 0)
                    NameLabel.BackgroundTransparency = 1
                    NameLabel.Text = "💥 " .. name
                    NameLabel.TextColor3 = _G.SelectedMobs[name] and Color3.fromRGB(100, 220, 100) or Color3.fromRGB(220, 200, 200)
                    NameLabel.Font = Enum.Font.SourceSansBold
                    NameLabel.TextSize = 12
                    NameLabel.TextXAlignment = Enum.TextXAlignment.Left
                    NameLabel.Parent = Row

                    local HpLabel = Instance.new("TextLabel")
                    HpLabel.Size = UDim2.new(0, 60, 1, 0)
                    HpLabel.Position = UDim2.new(1, -62, 0, 0)
                    HpLabel.BackgroundTransparency = 1
                    HpLabel.TextColor3 = Color3.fromRGB(160, 160, 160)
                    HpLabel.Font = Enum.Font.SourceSans
                    HpLabel.TextSize = 10
                    HpLabel.TextXAlignment = Enum.TextXAlignment.Right
                    HpLabel.Parent = Row

                    local function updateHp()
                        local found = LivingFolder:FindFirstChild(name)
                        if found then
                            local hp = found:FindFirstChildOfClass("Humanoid")
                            HpLabel.Text = hp and ("HP:" .. math.floor(hp.Health)) or "?"
                        else
                            HpLabel.Text = "Gone"
                        end
                    end
                    updateHp()
                    task.spawn(function()
                        while Row.Parent do
                            updateHp()
                            task.wait(1)
                        end
                    end)

                    mobCheckboxMap[name] = {Row = Row, Check = CheckBtn, Label = NameLabel}

                    local function toggleSelect()
                        _G.SelectedMobs[name] = not _G.SelectedMobs[name]
                        if _G.SelectedMobs[name] then
                            CheckBtn.Text = "✔"
                            CheckBtn.BackgroundColor3 = Color3.fromRGB(40, 70, 40)
                            NameLabel.TextColor3 = Color3.fromRGB(100, 220, 100)
                            Row.BackgroundColor3 = Color3.fromRGB(35, 55, 35)
                        else
                            CheckBtn.Text = ""
                            CheckBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
                            NameLabel.TextColor3 = Color3.fromRGB(220, 200, 200)
                            Row.BackgroundColor3 = Color3.fromRGB(45, 45, 52)
                        end
                    end

                    CheckBtn.MouseButton1Click:Connect(toggleSelect)
                    Row.InputBegan:Connect(function(input)
                        if input.UserInputType == Enum.UserInputType.MouseButton1 then
                            toggleSelect()
                        end
                    end)
                end
            end
        end
    end
end

SelectAllBtn.MouseButton1Click:Connect(function()
    if LivingFolder then
        for _, mob in ipairs(LivingFolder:GetChildren()) do
            if mob:IsA("Model") then
                _G.SelectedMobs[mob.Name] = true
            end
        end
    end
    buildFarmMobList()
end)

ClearAllBtn.MouseButton1Click:Connect(function()
    _G.SelectedMobs = {}
    buildFarmMobList()
end)

FarmSearch:GetPropertyChangedSignal("Text"):Connect(buildFarmMobList)

FarmBehindBtn.MouseButton1Click:Connect(function()
    _G.FarmMobPosition = "Behind"
    FarmBehindBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
    FarmBehindBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    FarmBehindBtn.Text = "ข้างหลัง ✔"
    FarmAboveBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    FarmAboveBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    FarmAboveBtn.Text = "ข้างบน"
end)

FarmAboveBtn.MouseButton1Click:Connect(function()
    _G.FarmMobPosition = "Above"
    FarmAboveBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
    FarmAboveBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    FarmAboveBtn.Text = "ข้างบน ✔"
    FarmBehindBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    FarmBehindBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    FarmBehindBtn.Text = "ข้างหลัง"
end)

ToggleMobFarmBtn.MouseButton1Click:Connect(function()
    _G.AutoFarmMobs = not _G.AutoFarmMobs
    if _G.AutoFarmMobs then
        ToggleMobFarmBtn.BackgroundColor3 = Color3.fromRGB(70, 150, 70)
        ToggleMobFarmBtn.Text = "🟢 FARMING MOBS ACTIVE..."
    else
        ToggleMobFarmBtn.BackgroundColor3 = Color3.fromRGB(180, 70, 70)
        ToggleMobFarmBtn.Text = "🔴 START FARM SELECTED MOBS"
    end
end)

-- ============================================================
-- AUTO FARM MOBS LOOP
-- 2. AUTO FARM MOBS LOOP (แก้ไขเพิ่มส่วนเก็บกล่อง)
-- ============================================================
task.spawn(function()
    while task.wait(0.05) do
        if _G.AutoFarmMobs then
            local myCharacter = localPlayer.Character
            local myRoot = myCharacter and myCharacter:FindFirstChild("HumanoidRootPart")
            if not myRoot or not LivingFolder then continue end

            local remote
            pcall(function()
                remote = game:GetService("ReplicatedStorage"):WaitForChild("ABC - First Priority"):WaitForChild("Utility"):WaitForChild("Modules"):WaitForChild("Warp"):WaitForChild("Index"):WaitForChild("Event"):WaitForChild("Reliable")
            end)
            if not remote then continue end

            local foundMob = false
            for _, mob in ipairs(LivingFolder:GetChildren()) do
                if not _G.AutoFarmMobs then break end
                if not (mob:IsA("Model") and _G.SelectedMobs[mob.Name]) then continue end

                local mobRoot = mob:FindFirstChild("HumanoidRootPart")
                local mobHp = mob:FindFirstChildOfClass("Humanoid")
                if not mobRoot or not mobHp or mobHp.Health <= 0 then continue end
                
                foundMob = true -- เจอมอนสเตอร์!
                
                if _G.FarmMobPosition == "Above" then
                    myRoot.CFrame = mobRoot.CFrame * CFrame.new(0, 3.5, 0)
                else
                    myRoot.CFrame = mobRoot.CFrame * CFrame.new(0, 0, 3)
                end

                pcall(function() remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\003LMB")) end)
                task.wait(0.05)
                pcall(function() remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\001E")) end)
                task.wait(0.05)
                pcall(function() remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\001R")) end)
                task.wait(0.05)
            end

            -- ถ้าวนลูปจบแล้วไม่เจอมอนสเตอร์เลย ให้ไปเก็บกล่อง
            if not foundMob then
                collectItems()
            end
        end
    end
end)

--===========================================================
-- QUEST FARM SYSTEM
-- ============================================================
BehindBtn.MouseButton1Click:Connect(function()
    _G.FarmPosition = "Behind"
    BehindBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
    BehindBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    BehindBtn.Text = "ข้างหลังmob ✔"
    AboveBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    AboveBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    AboveBtn.Text = "ข้างบนmob"
end)

AboveBtn.MouseButton1Click:Connect(function()
    _G.FarmPosition = "Above"
    AboveBtn.BackgroundColor3 = Color3.fromRGB(60, 80, 110)
    AboveBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    AboveBtn.Text = "ข้างบนmob ✔"
    BehindBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    BehindBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    BehindBtn.Text = "ข้างหลังmob"
end)

local function getActiveQuestData()
    local targetFolder = localPlayer:FindFirstChild("QuestFolder")
        or localPlayer:FindFirstChild("quest forder")
        or localPlayer:FindFirstChild("Quest")
    if not targetFolder then return nil end

    for _, item in ipairs(targetFolder:GetChildren()) do
        if item:IsA("StringValue") or item:IsA("IntValue") then
            return {Name = item.Name, Value = item.Value}
        elseif item:IsA("Folder") or item:IsA("Configuration") then
            local targetName = item:FindFirstChild("Target") or item:FindFirstChild("Monster")
            if targetName then return {Name = targetName.Value, Value = item.Name} end
            return {Name = item.Name, Value = nil}
        end
    end

    if targetFolder:FindFirstChild("128") then
        return {Name = "John Zen'in", QuestGiver = "Naobitoi Zen'in"}
    end
    return nil
end

local function getMonster(questName)
    if not LivingFolder or not questName then return nil end
    for _, monster in ipairs(LivingFolder:GetChildren()) do
        if monster:FindFirstChild("HumanoidRootPart") and monster:FindFirstChildOfClass("Humanoid") then
            local hp = monster:FindFirstChildOfClass("Humanoid")
            if hp and hp.Health > 0 then
                if string.find(string.lower(monster.Name), string.lower(questName)) or monster.Name == questName then
                    return monster
                elseif questName == "128" and monster.Name == "John Zen'in" then
                    return monster
                end
            end
        end
    end
    for _, monster in ipairs(LivingFolder:GetChildren()) do
        if monster.Name == "John Zen'in" and monster:FindFirstChild("HumanoidRootPart") then
            local hp = monster:FindFirstChildOfClass("Humanoid")
            if hp and hp.Health > 0 then return monster end
        end
    end
    return nil
end

task.spawn(function()
    while task.wait(0.1) do
        if _G.AutoFarmQuest then
            local myCharacter = localPlayer.Character
            local myRoot = myCharacter and myCharacter:FindFirstChild("HumanoidRootPart")

            if myRoot then
                local questData = getActiveQuestData()

                if questData then
                    local targetMonster = getMonster(questData.Name)
                    if targetMonster then
                        local monsterRoot = targetMonster:FindFirstChild("HumanoidRootPart")
                        local monsterHumanoid = targetMonster:FindFirstChildOfClass("Humanoid")

                        if monsterRoot and monsterHumanoid and monsterHumanoid.Health > 0 then
                            if _G.FarmPosition == "Above" then
                                myRoot.CFrame = monsterRoot.CFrame * CFrame.new(0, 3.5, 0)
                            else
                                myRoot.CFrame = monsterRoot.CFrame * CFrame.new(0, 0, 3)
                            end

                            local remote
                            pcall(function()
                                remote = game:GetService("ReplicatedStorage")
                                    :WaitForChild("ABC - First Priority")
                                    :WaitForChild("Utility")
                                    :WaitForChild("Modules")
                                    :WaitForChild("Warp")
                                    :WaitForChild("Index")
                                    :WaitForChild("Event")
                                    :WaitForChild("Reliable")
                            end)

                            if remote then
                                pcall(function()
                                    remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\003LMB"))
                                end)
                                task.wait(0.05)
                                pcall(function()
                                    remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\001E"))
                                end)
                                task.wait(0.05)
                                pcall(function()
                                    remote:FireServer(buffer.fromstring("\024"), buffer.fromstring("\254\001\000\006\001R"))
                                end)
                                task.wait(0.05)
                            end
                        end
                    end
                else
                    local npcName = (questData and questData.QuestGiver) or "Naobitoi Zen'in"
                    local targetNpc = NpcFolder and NpcFolder:FindFirstChild(npcName)

                    if targetNpc and targetNpc:FindFirstChild("HumanoidRootPart") then
                        myRoot.CFrame = targetNpc.HumanoidRootPart.CFrame * CFrame.new(0, 0, -2.5) * CFrame.Angles(0, math.rad(180), 0)
                        task.wait(0.2)

                        local prompt = targetNpc:FindFirstChildWhichIsA("ProximityPrompt", true)

                        if prompt then
                            local holdTime = prompt.HoldDuration > 0 and (prompt.HoldDuration + 0.1) or 0.2
                            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                            prompt:InputHoldBegin()
                            task.wait(holdTime)
                            prompt:InputHoldEnd()
                            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                        else
                            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                            task.wait(3.1)
                            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                        end

                        pcall(function()
                            local activeGui = localPlayer.PlayerGui:FindFirstChildOfClass("ScreenGui")
                            if activeGui then
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                                task.wait(0.05)
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                            end
                        end)
                        task.wait(0.5)
                    end
                end
            end
        end
    end
end)

ToggleFarmBtn.MouseButton1Click:Connect(function()
    _G.AutoFarmQuest = not _G.AutoFarmQuest
    if _G.AutoFarmQuest then
        ToggleFarmBtn.BackgroundColor3 = Color3.fromRGB(70, 150, 70)
        ToggleFarmBtn.Text = "🟢 AUTO FARMING ACTIVE..."
    else
        ToggleFarmBtn.BackgroundColor3 = Color3.fromRGB(180, 70, 70)
        ToggleFarmBtn.Text = "🔴 START AUTO FARM QUEST"
    end
end)

-- ============================================================
-- PLAYER LIST
-- ============================================================
local function updatePlayerList()
    for _, child in ipairs(PlayersPage:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= localPlayer then
            local Btn = Instance.new("TextButton")
            Btn.Size = UDim2.new(1, -4, 0, 30)
            Btn.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
            Btn.Text = "  " .. player.DisplayName .. " (@" .. player.Name .. ")"
            Btn.TextColor3 = Color3.fromRGB(220, 220, 220)
            Btn.TextSize = 13
            Btn.Font = Enum.Font.SourceSans
            Btn.TextXAlignment = Enum.TextXAlignment.Left
            Btn.Parent = PlayersPage
            Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)

            Btn.MouseButton1Click:Connect(function()
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    local myRoot = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
                    if myRoot then
                        myRoot.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0, 2, 2)
                    end
                end
            end)
        end
    end
end

-- ============================================================
-- TELEPORT LIST
-- ============================================================
local function updateCombinedList()
    for _, child in ipairs(CombinedScroll:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end

    local searchText = string.lower(UnifiedSearch.Text)

    if currentSubTab == "NPCs" then
        if NpcFolder then
            for _, npc in ipairs(NpcFolder:GetChildren()) do
                if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
                    if searchText == "" or string.find(string.lower(npc.Name), searchText) then
                        local Btn = Instance.new("TextButton")
                        Btn.Size = UDim2.new(1, -4, 0, 30)
                        Btn.BackgroundColor3 = Color3.fromRGB(45, 60, 50)
                        Btn.Text = "  🤖 NPC: " .. npc.Name
                        Btn.TextColor3 = Color3.fromRGB(230, 255, 230)
                        Btn.TextSize = 12
                        Btn.Font = Enum.Font.SourceSansBold
                        Btn.TextXAlignment = Enum.TextXAlignment.Left
                        Btn.Parent = CombinedScroll
                        Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)

                        Btn.MouseButton1Click:Connect(function()
                            local myRoot = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
                            if myRoot and npc:FindFirstChild("HumanoidRootPart") then
                                myRoot.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 2, 2)
                            end
                        end)
                    end
                end
            end
        end
    elseif currentSubTab == "Mobs" then
        if LivingFolder then
            for _, mob in ipairs(LivingFolder:GetChildren()) do
                if mob:IsA("Model") and mob:FindFirstChild("HumanoidRootPart") then
                    if searchText == "" or string.find(string.lower(mob.Name), searchText) then
                        local humanoid = mob:FindFirstChildOfClass("Humanoid")
                        local healthText = humanoid and (" [HP:" .. math.floor(humanoid.Health) .. "]") or ""

                        local Btn = Instance.new("TextButton")
                        Btn.Size = UDim2.new(1, -4, 0, 30)
                        Btn.BackgroundColor3 = Color3.fromRGB(65, 45, 45)
                        Btn.Text = "  💥 Mob: " .. mob.Name .. healthText
                        Btn.TextColor3 = Color3.fromRGB(255, 230, 230)
                        Btn.TextSize = 12
                        Btn.Font = Enum.Font.SourceSansBold
                        Btn.TextXAlignment = Enum.TextXAlignment.Left
                        Btn.Parent = CombinedScroll
                        Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)

                        Btn.MouseButton1Click:Connect(function()
                            local myRoot = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
                            if myRoot and mob:FindFirstChild("HumanoidRootPart") then
                                myRoot.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
                            end
                        end)
                    end
                end
            end
        end
    end
end

UnifiedSearch:GetPropertyChangedSignal("Text"):Connect(updateCombinedList)

-- ============================================================
-- TAB SWITCHING
-- ============================================================
NpcSubBtn.MouseButton1Click:Connect(function()
    currentSubTab = "NPCs"
    NpcSubBtn.BackgroundColor3 = Color3.fromRGB(45, 60, 50); NpcSubBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    MobSubBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45); MobSubBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    updateCombinedList()
end)

MobSubBtn.MouseButton1Click:Connect(function()
    currentSubTab = "Mobs"
    MobSubBtn.BackgroundColor3 = Color3.fromRGB(65, 45, 45); MobSubBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    NpcSubBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45); NpcSubBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
    updateCombinedList()
end)

local function resetTabColors()
    for _, btn in ipairs({HomeTabBtn, CombinedTabBtn, QuestTabBtn, FarmTabBtn}) do
        btn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        btn.TextColor3 = Color3.fromRGB(150, 150, 150)
    end
end

HomeTabBtn.MouseButton1Click:Connect(function()
    resetTabColors()
    PlayersPage.Visible = true; CombinedPage.Visible = false; QuestPage.Visible = false; FarmPage.Visible = false
    HomeTabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60); HomeTabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    updatePlayerList()
end)

CombinedTabBtn.MouseButton1Click:Connect(function()
    resetTabColors()
    PlayersPage.Visible = false; CombinedPage.Visible = true; QuestPage.Visible = false; FarmPage.Visible = false
    CombinedTabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60); CombinedTabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    updateCombinedList()
end)

QuestTabBtn.MouseButton1Click:Connect(function()
    resetTabColors()
    PlayersPage.Visible = false; CombinedPage.Visible = false; QuestPage.Visible = true; FarmPage.Visible = false
    QuestTabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60); QuestTabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
end)

FarmTabBtn.MouseButton1Click:Connect(function()
    resetTabColors()
    PlayersPage.Visible = false; CombinedPage.Visible = false; QuestPage.Visible = false; FarmPage.Visible = true
    FarmTabBtn.BackgroundColor3 = Color3.fromRGB(80, 55, 30); FarmTabBtn.TextColor3 = Color3.fromRGB(255, 200, 100)
    buildFarmMobList()
end)

-- ============================================================
-- EVENT LISTENERS
-- ============================================================
Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)

if NpcFolder then
    NpcFolder.ChildAdded:Connect(updateCombinedList)
    NpcFolder.ChildRemoved:Connect(updateCombinedList)
end

if LivingFolder then
    LivingFolder.ChildAdded:Connect(function()
        updateCombinedList()
        if FarmPage.Visible then buildFarmMobList() end
    end)
    LivingFolder.ChildRemoved:Connect(function()
        updateCombinedList()
        if FarmPage.Visible then buildFarmMobList() end
    end)
end

-- DESTROY
DestroyBtn.MouseButton1Click:Connect(function()
    _G.AutoFarmQuest = false
    _G.AutoFarmMobs = false
    if antiAfkConnection then antiAfkConnection:Disconnect() end
    ScreenGui:Destroy()
end)

-- INIT
updatePlayerList()
updateCombinedList()
