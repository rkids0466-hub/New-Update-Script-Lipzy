local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local MarketplaceService = game:GetService("MarketplaceService")
local RunService = game:GetService("RunService")
local Stats = game:GetService("Stats")

local LocalPlayer = Players.LocalPlayer

local AdminList = {
    ["LifzxAfter2"] = "ADMIN",
    ["Yanzzz_1023"] = "OWNER"
}

local userRole = AdminList[LocalPlayer.Name]
local isAdmin = userRole ~= nil

if game.CoreGui:FindFirstChild("StealAnEggGUI") then
    game.CoreGui.StealAnEggGUI:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "StealAnEggGUI"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 500, 0, 400)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 12, 28)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(140, 45, 220)
MainStroke.Thickness = 2
MainStroke.Parent = MainFrame

local OpenBanner = Instance.new("ImageButton")
OpenBanner.Name = "OpenBanner"
OpenBanner.Size = UDim2.new(0, 70, 0, 70)
OpenBanner.Position = UDim2.new(0, 20, 0.5, -35)
OpenBanner.BackgroundTransparency = 1
OpenBanner.Image = "rbxassetid://131766089166568"
OpenBanner.ScaleType = Enum.ScaleType.Fit
OpenBanner.Active = true
OpenBanner.Draggable = true
OpenBanner.Visible = false
OpenBanner.Parent = ScreenGui

local OpenBannerCorner = Instance.new("UICorner")
OpenBannerCorner.CornerRadius = UDim.new(0, 10)
OpenBannerCorner.Parent = OpenBanner

local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Color3.fromRGB(30, 18, 48)
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 10)
HeaderCorner.Parent = Header

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, -50, 1, 0)
TitleText.Position = UDim2.new(0, 15, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "LIPZY HUB V5.2"
TitleText.TextColor3 = Color3.fromRGB(200, 120, 255)
TitleText.TextSize = 18
TitleText.Font = Enum.Font.GothamBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Parent = Header

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 60)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14
CloseBtn.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseBtn

CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenBanner.Visible = true
end)

OpenBanner.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenBanner.Visible = false
end)

local Sidebar = Instance.new("Frame")
Sidebar.Size = UDim2.new(0, 130, 1, -50)
Sidebar.Position = UDim2.new(0, 10, 0, 45)
Sidebar.BackgroundColor3 = Color3.fromRGB(25, 15, 38)
Sidebar.BorderSizePixel = 0
Sidebar.Parent = MainFrame

local SidebarCorner = Instance.new("UICorner")
SidebarCorner.CornerRadius = UDim.new(0, 8)
SidebarCorner.Parent = Sidebar

local SidebarLayout = Instance.new("UIListLayout")
SidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
SidebarLayout.Padding = UDim.new(0, 6)
SidebarLayout.Parent = Sidebar

local SidebarPadding = Instance.new("UIPadding")
SidebarPadding.PaddingTop = UDim.new(0, 10)
SidebarPadding.PaddingLeft = UDim.new(0, 8)
SidebarPadding.PaddingRight = UDim.new(0, 8)
SidebarPadding.Parent = Sidebar

local ContentContainer = Instance.new("Frame")
ContentContainer.Size = UDim2.new(1, -160, 1, -55)
ContentContainer.Position = UDim2.new(0, 150, 0, 45)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainFrame

local Pages = {}

local function CreateTab(name, layoutOrder)
    local TabBtn = Instance.new("TextButton")
    TabBtn.Size = UDim2.new(1, 0, 0, 30)
    TabBtn.BackgroundColor3 = (layoutOrder == 1) and Color3.fromRGB(120, 40, 190) or Color3.fromRGB(40, 25, 60)
    TabBtn.Text = name
    TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    TabBtn.Font = Enum.Font.GothamBold
    TabBtn.TextSize = 11
    TabBtn.LayoutOrder = layoutOrder
    TabBtn.Parent = Sidebar

    local BtnCorner = Instance.new("UICorner")
    BtnCorner.CornerRadius = UDim.new(0, 6)
    BtnCorner.Parent = TabBtn

    local Page = Instance.new("Frame")
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.BackgroundTransparency = 1
    Page.Visible = (layoutOrder == 1)
    Page.Parent = ContentContainer

    Pages[name] = {Button = TabBtn, Page = Page}

    TabBtn.MouseButton1Click:Connect(function()
        for _, tabData in pairs(Pages) do
            tabData.Page.Visible = false
            tabData.Button.BackgroundColor3 = Color3.fromRGB(40, 25, 60)
        end
        Page.Visible = true
        TabBtn.BackgroundColor3 = Color3.fromRGB(120, 40, 190)
    end)

    return Page
end

local PlayerPage = CreateTab("PLAYER", 1)
local MainPage = CreateTab("MAIN", 2)
local TampilanPage = CreateTab("TAMPILAN", 3)
local SettingPage = CreateTab("SETTING", 4)
local OverlayPage = CreateTab("OVERLAY", 5)

local AdminPage = nil
if isAdmin then
    AdminPage = CreateTab("ADMIN", 6)
end

local function AddInfoLabel(container, title, value, order)
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 0, 25)
    Label.BackgroundTransparency = 1
    Label.Text = title .. ": " .. value
    Label.TextColor3 = Color3.fromRGB(200, 200, 220)
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 11
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.LayoutOrder = order
    Label.Parent = container
    return Label
end

local ProfileImage = Instance.new("ImageLabel")
ProfileImage.Size = UDim2.new(0, 70, 0, 70)
ProfileImage.Position = UDim2.new(0.5, -35, 0, 0)
ProfileImage.BackgroundColor3 = Color3.fromRGB(45, 25, 70)
ProfileImage.Image = Players:GetUserThumbnailAsync(
    LocalPlayer.UserId,
    Enum.ThumbnailType.HeadShot,
    Enum.ThumbnailSize.Size150x150
)
ProfileImage.Parent = PlayerPage

local ProfileCorner = Instance.new("UICorner")
ProfileCorner.CornerRadius = UDim.new(1, 0)
ProfileCorner.Parent = ProfileImage

local ProfileStroke = Instance.new("UIStroke")
ProfileStroke.Color = Color3.fromRGB(160, 60, 255)
ProfileStroke.Thickness = 2
ProfileStroke.Parent = ProfileImage

local UserBox = Instance.new("Frame")
UserBox.Size = UDim2.new(1, -20, 0, 28)
UserBox.Position = UDim2.new(0, 10, 0, 78)
UserBox.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
UserBox.BorderSizePixel = 0
UserBox.Parent = PlayerPage

local UserBoxCorner = Instance.new("UICorner")
UserBoxCorner.CornerRadius = UDim.new(0, 6)
UserBoxCorner.Parent = UserBox

local UserBoxStroke = Instance.new("UIStroke")
UserBoxStroke.Color = Color3.fromRGB(100, 40, 160)
UserBoxStroke.Thickness = 1
UserBoxStroke.Parent = UserBox

local UserLabel = Instance.new("TextLabel")
UserLabel.Size = UDim2.new(1, 0, 1, 0)
UserLabel.BackgroundTransparency = 1
UserLabel.Text = "@" .. LocalPlayer.Name .. " (" .. LocalPlayer.DisplayName .. ")"
UserLabel.TextColor3 = Color3.fromRGB(220, 180, 255)
UserLabel.Font = Enum.Font.GothamBold
UserLabel.TextSize = 11
UserLabel.Parent = UserBox

local PlayerInfoContainer = Instance.new("Frame")
PlayerInfoContainer.Size = UDim2.new(1, -20, 0, 120)
PlayerInfoContainer.Position = UDim2.new(0, 10, 0, 112)
PlayerInfoContainer.BackgroundColor3 = Color3.fromRGB(28, 16, 42)
PlayerInfoContainer.BorderSizePixel = 0
PlayerInfoContainer.Parent = PlayerPage

local PlayerInfoCorner = Instance.new("UICorner")
PlayerInfoCorner.CornerRadius = UDim.new(0, 8)
PlayerInfoCorner.Parent = PlayerInfoContainer

local PlayerInfoLayout = Instance.new("UIListLayout")
PlayerInfoLayout.SortOrder = Enum.SortOrder.LayoutOrder
PlayerInfoLayout.Padding = UDim.new(0, 2)
PlayerInfoLayout.Parent = PlayerInfoContainer

local PlayerInfoPadding = Instance.new("UIPadding")
PlayerInfoPadding.PaddingTop = UDim.new(0, 6)
PlayerInfoPadding.PaddingLeft = UDim.new(0, 10)
PlayerInfoPadding.PaddingRight = UDim.new(0, 10)
PlayerInfoPadding.Parent = PlayerInfoContainer

AddInfoLabel(PlayerInfoContainer, "Umur Akun", LocalPlayer.AccountAge .. " Hari", 1)

local AccountCreatedDate = os.date("%d %B %Y", os.time() - (LocalPlayer.AccountAge * 86400))
AddInfoLabel(PlayerInfoContainer, "Akun Dibuat", AccountCreatedDate, 2)

local TimeLabel = AddInfoLabel(PlayerInfoContainer, "Jam", os.date("%H:%M:%S WIB"), 3)
task.spawn(function()
    while task.wait(1) do
        if TimeLabel and TimeLabel.Parent then
            TimeLabel.Text = "Jam: " .. os.date("%H:%M:%S WIB")
        end
    end
end)

local GameName = "Steal an Egg"
pcall(function()
    local placeInfo = MarketplaceService:GetProductInfo(game.PlaceId)
    if placeInfo and placeInfo.Name then
        GameName = placeInfo.Name
    end
end)
AddInfoLabel(PlayerInfoContainer, "Nama Game", GameName, 4)

local StatsContainer = Instance.new("Frame")
StatsContainer.Size = UDim2.new(1, -20, 0, 105)
StatsContainer.Position = UDim2.new(0, 10, 0, 238)
StatsContainer.BackgroundTransparency = 1
StatsContainer.Parent = PlayerPage

local StatsLayout = Instance.new("UIListLayout")
StatsLayout.SortOrder = Enum.SortOrder.LayoutOrder
StatsLayout.Padding = UDim.new(0, 5)
StatsLayout.Parent = StatsContainer

local function CreateStatBox(order, hasIcon)
    local Box = Instance.new("Frame")
    Box.Size = UDim2.new(1, 0, 0, 30)
    Box.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
    Box.BorderSizePixel = 0
    Box.LayoutOrder = order
    Box.Parent = StatsContainer

    local BoxCorner = Instance.new("UICorner")
    BoxCorner.CornerRadius = UDim.new(0, 6)
    BoxCorner.Parent = Box

    local BoxStroke = Instance.new("UIStroke")
    BoxStroke.Color = Color3.fromRGB(100, 40, 160)
    BoxStroke.Thickness = 1
    BoxStroke.Parent = Box

    local Icon = nil
    if hasIcon then
        Icon = Instance.new("ImageLabel")
        Icon.Size = UDim2.new(0, 18, 0, 18)
        Icon.Position = UDim2.new(0, 8, 0.5, -9)
        Icon.BackgroundTransparency = 1
        Icon.ScaleType = Enum.ScaleType.Fit
        Icon.Parent = Box
    end

    local Label = Instance.new("TextLabel")
    Label.Size = hasIcon and UDim2.new(1, -38, 1, 0) or UDim2.new(1, -20, 1, 0)
    Label.Position = hasIcon and UDim2.new(0, 32, 0, 0) or UDim2.new(0, 10, 0, 0)
    Label.BackgroundTransparency = 1
    Label.TextColor3 = Color3.fromRGB(220, 200, 255)
    Label.Font = Enum.Font.GothamBold
    Label.TextSize = 11
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Box

    return Label, Icon
end

local StatusLabel, StatusIcon = CreateStatBox(1, true)
StatusIcon.Image = "rbxassetid://95842412229310"

if isAdmin then
    StatusLabel.Text = "PREMIUM"
    StatusLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
else
    StatusLabel.Text = "FREE"
    StatusLabel.TextColor3 = Color3.fromRGB(150, 255, 150)
end

local FPSLabel, FPSIcon = CreateStatBox(2, true)
FPSIcon.Image = "rbxassetid://137800308683217"

local PingLabel, PingIcon = CreateStatBox(3, true)
PingIcon.Image = "rbxassetid://78681903515425"

local frameCount = 0
local lastTime = tick()
local currentFPS = 0

RunService.RenderStepped:Connect(function()
    frameCount = frameCount + 1
    local currentTime = tick()
    if currentTime - lastTime >= 1 then
        currentFPS = frameCount
        frameCount = 0
        lastTime = currentTime
    end
end)

task.spawn(function()
    while task.wait(0.2) do
        if FPSLabel and FPSLabel.Parent then
            FPSLabel.Text = "FPS: " .. currentFPS
        end
        if PingLabel and PingLabel.Parent then
            local rawPing = Stats.Network.ServerStatsItem["Data Ping"]:GetValue()
            local pingVal = math.floor(rawPing)
            
            PingLabel.Text = pingVal .. " ms"
        end
    end
end)

local ScriptList = {
    {Name = "CHILI HUB V2", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()]]},
    {Name = "THANHDUY HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/ThanhDuyHub/Game/refs/heads/main/Steal-An-Egg.lua"))()]]},
    {Name = "PULSE HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/PulseZax/Loader/refs/heads/main/.lua"))()]]},
    {Name = "SENA BETA", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://senahub.xyz/raw/loader"))()]]},
    {Name = "LENON HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/lennonxscripts/lennonhubv4/refs/heads/main/stealanegg",true))()]]},
    {Name = "DHII's HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/dhiisepertiorang/dhii_kick.lua/refs/heads/main/dhiitoaa.lua"))()]]},
    {Name = "BlyxoHUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://flowauth.net/v1/loaders/69d3463240384f3a73fbe32c178093a2.lua"))()]]},
    {Name = "FINDER PRIVAT SERVER", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/GlazeScripts/Private-Server-Finder/refs/heads/main/Glazehub.lua"))()]]},
    {Name = "MIRANDA HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/miirandahub/loader/refs/heads/main/mirandaafk.lua"))()]]},
    {Name = "HOSHI HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://hoshihub.site/loader.lua"))()]]},
    {Name = "OXIDE HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/xulfo/Oxide-Loader/main/Main.lua"))()]]},
    {Name = "NEVA HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/VEZ2/NEVAHUB/main/2"))()]]},
    {Name = "BK's HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://api.luarmor.net/files/v4/loaders/9ee4edde227ac85f50872bf9e4226508.lua"))()]]},
    {Name = "VINCITORE HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/tutorkah104-rgb/Steal-an-Egg/refs/heads/main/Vincitore.luau"))()]]},
    {Name = "LKZ HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/LucasggkX/LKZ-Hub/refs/heads/main/Loader.lua"))()]]},
    {Name = "SKIBIDI HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Script/refs/heads/main/Fn-stealanegg.lua"))()]]},
    {Name = "BP HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://rscripts.net/raw/open-src-premium-or-bphub_1790968708765_mTTlSoUSyY.txt"))()]]},
    {Name = "FOXNAME", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Script/refs/heads/main/Fn-stealanegg.lua"))()]]},
    {Name = "ZEROIN HUB", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://zeroinhub.com/api/script"))()]]},
    {Name = "FAKE ADMIN", KeyStatus = "NO KEY", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Dayvinksthik/Script/main/Games/JoshBNS-Crack.lua"))()]]},
    {Name = "POLLUTED HUN", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/PollutedHub/Sae/refs/heads/main/SaePolluted"))()]]},
    {Name = "SENA V6", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://senahub.xyz/senav6"))()]]},
    {Name = "AJJANS HUB", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://api.luarmor.net/files/v4/loaders/359e97f8618e9008afe5f496184ebb7c.lua"))()]]},
    {Name = "FYY COMMUNITY", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://FyyCommunity.my.id"))()]]},
    {Name = "LIMBO HUB", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://limbohub.my.id/loader.lua"))()]]},
    {Name = "UNKNOWN HUB", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://unknownhub.win/api/projects/54474b4c5d5a4f459909c4cb70e7b4f3/loader"))()]]},
    {Name = "CLOVER HUB", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://cloverhub.app/clover.lua"))()]]},
    {Name = "BIGFROOT", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/hanniii1/Loader/refs/heads/main/BFLoader.lua"))()]]},
    {Name = "SYS HUB", KeyStatus = "KEYLESS", Script = [[loadstring(game:HttpGet("https://syshub.fun/free"))()]]}
}

local OpenScriptBtn = Instance.new("TextButton")
OpenScriptBtn.Size = UDim2.new(1, -20, 0, 36)
OpenScriptBtn.Position = UDim2.new(0, 10, 0, 5)
OpenScriptBtn.BackgroundColor3 = Color3.fromRGB(140, 45, 220)
OpenScriptBtn.Text = "OPEN SCRIPT"
OpenScriptBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenScriptBtn.Font = Enum.Font.GothamBold
OpenScriptBtn.TextSize = 14
OpenScriptBtn.Parent = MainPage

local OpenScriptCorner = Instance.new("UICorner")
OpenScriptCorner.CornerRadius = UDim.new(0, 8)
OpenScriptCorner.Parent = OpenScriptBtn

local ScriptScroll = Instance.new("ScrollingFrame")
ScriptScroll.Size = UDim2.new(1, -20, 1, -50)
ScriptScroll.Position = UDim2.new(0, 10, 0, 46)
ScriptScroll.BackgroundTransparency = 1
ScriptScroll.BorderSizePixel = 0
ScriptScroll.ScrollBarThickness = 4
ScriptScroll.ScrollBarImageColor3 = Color3.fromRGB(140, 45, 220)
ScriptScroll.Parent = MainPage

local ScriptListLayout = Instance.new("UIListLayout")
ScriptListLayout.SortOrder = Enum.SortOrder.LayoutOrder
ScriptListLayout.Padding = UDim.new(0, 6)
ScriptListLayout.Parent = ScriptScroll

local SelectedScript = nil
local RegisteredItems = {}

for index, data in ipairs(ScriptList) do
    local ItemFrame = Instance.new("Frame")
    ItemFrame.Size = UDim2.new(1, -10, 0, 40)
    ItemFrame.BackgroundColor3 = Color3.fromRGB(32, 18, 50)
    ItemFrame.BorderSizePixel = 0
    ItemFrame.LayoutOrder = index
    ItemFrame.Parent = ScriptScroll

    local ItemCorner = Instance.new("UICorner")
    ItemCorner.CornerRadius = UDim.new(0, 6)
    ItemCorner.Parent = ItemFrame

    local ItemStroke = Instance.new("UIStroke")
    ItemStroke.Color = Color3.fromRGB(70, 40, 110)
    ItemStroke.Thickness = 1
    ItemStroke.Parent = ItemFrame

    local NameLabel = Instance.new("TextLabel")
    NameLabel.Size = UDim2.new(1, -80, 0, 20)
    NameLabel.Position = UDim2.new(0, 10, 0, 4)
    NameLabel.BackgroundTransparency = 1
    NameLabel.Text = data.Name
    NameLabel.TextColor3 = Color3.fromRGB(220, 200, 255)
    NameLabel.Font = Enum.Font.GothamBold
    NameLabel.TextSize = 12
    NameLabel.TextXAlignment = Enum.TextXAlignment.Left
    NameLabel.Parent = ItemFrame

    local StatusLabel = Instance.new("TextLabel")
    StatusLabel.Size = UDim2.new(1, -80, 0, 14)
    StatusLabel.Position = UDim2.new(0, 10, 0, 22)
    StatusLabel.BackgroundTransparency = 1
    StatusLabel.Text = "[" .. data.KeyStatus .. "]"
    StatusLabel.TextColor3 = (data.KeyStatus == "NO KEY") and Color3.fromRGB(100, 220, 120) or Color3.fromRGB(255, 180, 80)
    StatusLabel.Font = Enum.Font.Gotham
    StatusLabel.TextSize = 10
    StatusLabel.TextXAlignment = Enum.TextXAlignment.Left
    StatusLabel.Parent = ItemFrame

    local CheckBox = Instance.new("Frame")
    CheckBox.Size = UDim2.new(0, 22, 0, 22)
    CheckBox.Position = UDim2.new(1, -30, 0.5, -11)
    CheckBox.BackgroundColor3 = Color3.fromRGB(20, 12, 32)
    CheckBox.Parent = ItemFrame

    local CheckCorner = Instance.new("UICorner")
    CheckCorner.CornerRadius = UDim.new(0, 4)
    CheckCorner.Parent = CheckBox

    local CheckStroke = Instance.new("UIStroke")
    CheckStroke.Color = Color3.fromRGB(120, 60, 180)
    CheckStroke.Thickness = 1
    CheckStroke.Parent = CheckBox

    local CheckMark = Instance.new("TextLabel")
    CheckMark.Size = UDim2.new(1, 0, 1, 0)
    CheckMark.BackgroundTransparency = 1
    CheckMark.Text = ""
    CheckMark.TextColor3 = Color3.fromRGB(0, 255, 120)
    CheckMark.Font = Enum.Font.GothamBold
    CheckMark.TextSize = 14
    CheckMark.Parent = CheckBox

    local ClickBtn = Instance.new("TextButton")
    ClickBtn.Size = UDim2.new(1, 0, 1, 0)
    ClickBtn.BackgroundTransparency = 1
    ClickBtn.Text = ""
    ClickBtn.Parent = ItemFrame

    RegisteredItems[index] = {
        Frame = ItemFrame,
        NameLabel = NameLabel,
        CheckMark = CheckMark,
        Stroke = ItemStroke,
        Data = data
    }

    ClickBtn.MouseButton1Click:Connect(function()
        SelectedScript = data.Script
        for _, item in pairs(RegisteredItems) do
            item.CheckMark.Text = ""
            item.Stroke.Color = Color3.fromRGB(70, 40, 110)
        end
        CheckMark.Text = "✓"
        ItemStroke.Color = Color3.fromRGB(200, 100, 255)
    end)
end

RunService.RenderStepped:Connect(function()
    local hue = (tick() % 4) / 4
    local rainbowColor = Color3.fromHSV(hue, 1, 1)
    
    for _, item in pairs(RegisteredItems) do
        if item.CheckMark.Text == "✓" then
            item.NameLabel.TextColor3 = rainbowColor
        else
            item.NameLabel.TextColor3 = Color3.fromRGB(220, 200, 255)
        end
    end
end)

ScriptListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScriptScroll.CanvasSize = UDim2.new(0, 0, 0, ScriptListLayout.AbsoluteContentSize.Y + 10)
end)

OpenScriptBtn.MouseButton1Click:Connect(function()
    if SelectedScript then
        local success, err = pcall(function()
            loadstring(SelectedScript)()
        end)
        if not success then
            warn("Gagal mengeksekusi script: " .. tostring(err))
        end
    end
end)

local TampilanScroll = Instance.new("ScrollingFrame")
TampilanScroll.Size = UDim2.new(1, -10, 1, -10)
TampilanScroll.Position = UDim2.new(0, 5, 0, 5)
TampilanScroll.BackgroundTransparency = 1
TampilanScroll.BorderSizePixel = 0
TampilanScroll.ScrollBarThickness = 4
TampilanScroll.ScrollBarImageColor3 = Color3.fromRGB(140, 45, 220)
TampilanScroll.Parent = TampilanPage

local TampilanLayout = Instance.new("UIListLayout")
TampilanLayout.SortOrder = Enum.SortOrder.LayoutOrder
TampilanLayout.Padding = UDim.new(0, 8)
TampilanLayout.Parent = TampilanScroll

TampilanLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    TampilanScroll.CanvasSize = UDim2.new(0, 0, 0, TampilanLayout.AbsoluteContentSize.Y + 15)
end)

local function CreateToggle(name, order, onClick)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, -10, 0, 40)
    Frame.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
    Frame.BorderSizePixel = 0
    Frame.LayoutOrder = order
    Frame.Parent = TampilanScroll

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Frame

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(0.6, 0, 1, 0)
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = name
    Label.TextColor3 = Color3.fromRGB(220, 200, 255)
    Label.Font = Enum.Font.GothamBold
    Label.TextSize = 12
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Frame

    local ToggleBg = Instance.new("Frame")
    ToggleBg.Size = UDim2.new(0, 46, 0, 24)
    ToggleBg.Position = UDim2.new(1, -56, 0.5, -12)
    ToggleBg.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    ToggleBg.Parent = Frame

    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCorner.Parent = ToggleBg

    local ToggleCircle = Instance.new("Frame")
    ToggleCircle.Size = UDim2.new(0, 18, 0, 18)
    ToggleCircle.Position = UDim2.new(0, 3, 0.5, -9)
    ToggleCircle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    ToggleCircle.Parent = ToggleBg

    local CircleCorner = Instance.new("UICorner")
    CircleCorner.CornerRadius = UDim.new(1, 0)
    CircleCorner.Parent = ToggleCircle

    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 1, 0)
    Button.BackgroundTransparency = 1
    Button.Text = ""
    Button.Parent = Frame

    local state = false
    Button.MouseButton1Click:Connect(function()
        state = not state
        if state then
            TweenService:Create(ToggleCircle, TweenInfo.new(0.2), {
                Position = UDim2.new(1, -21, 0.5, -9),
                BackgroundColor3 = Color3.fromRGB(0, 255, 120)
            }):Play()
            TweenService:Create(ToggleBg, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(40, 120, 60)
            }):Play()
        else
            TweenService:Create(ToggleCircle, TweenInfo.new(0.2), {
                Position = UDim2.new(0, 3, 0.5, -9),
                BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            }):Play()
            TweenService:Create(ToggleBg, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            }):Play()
        end
        pcall(onClick, state)
    end)
end

CreateToggle("120 FPS", 1, function(state)
    if setfpscap then
        setfpscap(state and 120 or 60)
    end
end)

local defaultFOV = workspace.CurrentCamera.FieldOfView
CreateToggle("PC DISPLAY", 2, function(state)
    if state then
        workspace.CurrentCamera.FieldOfView = 90
    else
        workspace.CurrentCamera.FieldOfView = defaultFOV
    end
end)

local SliderFrame = Instance.new("Frame")
SliderFrame.Size = UDim2.new(1, -10, 0, 50)
SliderFrame.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
SliderFrame.BorderSizePixel = 0
SliderFrame.LayoutOrder = 3
SliderFrame.Parent = TampilanScroll

local SliderCorner = Instance.new("UICorner")
SliderCorner.CornerRadius = UDim.new(0, 8)
SliderCorner.Parent = SliderFrame

local SliderTitle = Instance.new("TextLabel")
SliderTitle.Size = UDim2.new(1, -24, 0, 20)
SliderTitle.Position = UDim2.new(0, 12, 0, 4)
SliderTitle.BackgroundTransparency = 1
SliderTitle.Text = "ZOOM PC DISTANCE: 0%"
SliderTitle.TextColor3 = Color3.fromRGB(220, 200, 255)
SliderTitle.Font = Enum.Font.GothamBold
SliderTitle.TextSize = 11
SliderTitle.TextXAlignment = Enum.TextXAlignment.Left
SliderTitle.Parent = SliderFrame

local SliderBar = Instance.new("Frame")
SliderBar.Size = UDim2.new(1, -24, 0, 8)
SliderBar.Position = UDim2.new(0, 12, 0, 30)
SliderBar.BackgroundColor3 = Color3.fromRGB(20, 12, 32)
SliderBar.BorderSizePixel = 0
SliderBar.Parent = SliderFrame

local SliderBarCorner = Instance.new("UICorner")
SliderBarCorner.CornerRadius = UDim.new(1, 0)
SliderBarCorner.Parent = SliderBar

local SliderFill = Instance.new("Frame")
SliderFill.Size = UDim2.new(0, 0, 1, 0)
SliderFill.BackgroundColor3 = Color3.fromRGB(140, 45, 220)
SliderFill.BorderSizePixel = 0
SliderFill.Parent = SliderBar

local SliderFillCorner = Instance.new("UICorner")
SliderFillCorner.CornerRadius = UDim.new(1, 0)
SliderFillCorner.Parent = SliderFill

local SliderKnob = Instance.new("Frame")
SliderKnob.Size = UDim2.new(0, 16, 0, 16)
SliderKnob.Position = UDim2.new(0, -8, 0.5, -8)
SliderKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
SliderKnob.Parent = SliderFill

local KnobCorner = Instance.new("UICorner")
KnobCorner.CornerRadius = UDim.new(1, 0)
KnobCorner.Parent = SliderKnob

local isDraggingZoom = false
local function UpdateZoom(input)
    local barPos = SliderBar.AbsolutePosition.X
    local barSize = SliderBar.AbsoluteSize.X
    local mouseX = input.Position.X
    local percentage = math.clamp((mouseX - barPos) / barSize, 0, 1)
    
    SliderFill.Size = UDim2.new(percentage, 0, 1, 0)
    local zoomPercent = math.floor(percentage * 50)
    SliderTitle.Text = "ZOOM PC DISTANCE: " .. zoomPercent .. "%"
    
    LocalPlayer.CameraMaxZoomDistance = 128 + (zoomPercent * 4)
    LocalPlayer.CameraMinZoomDistance = 0.5
end

SliderBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDraggingZoom = true
        UpdateZoom(input)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if isDraggingZoom and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        UpdateZoom(input)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDraggingZoom = false
    end
end)

local SavedMaterials = {}
CreateToggle("FPS BOOST", 4, function(state)
    local Terrain = workspace:FindFirstChildOfClass("Terrain")
    if state then
        if Terrain then
            Terrain.WaterWaveSize = 0
            Terrain.WaterWaveSpeed = 0
            Terrain.WaterReflectance = 0
            Terrain.WaterTransparency = 0
        end
        for _, v in ipairs(game:GetDescendants()) do
            if v:IsA("BasePart") then
                SavedMaterials[v] = v.Material
                v.Material = Enum.Material.SmoothPlastic
            elseif v:IsA("PostEffect") or v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = false
            end
        end
    else
        for part, mat in pairs(SavedMaterials) do
            if part and part.Parent then
                part.Material = mat
            end
        end
        SavedMaterials = {}
        for _, v in ipairs(game:GetDescendants()) do
            if v:IsA("PostEffect") or v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = true
            end
        end
    end
end)

local Lighting = game:GetService("Lighting")
local DayLoop = nil
CreateToggle("ANTI NIGHT", 5, function(state)
    if state then
        DayLoop = RunService.RenderStepped:Connect(function()
            Lighting.ClockTime = 12
        end)
    else
        if DayLoop then
            DayLoop:Disconnect()
            DayLoop = nil
        end
    end
end)

local NightLoop = nil
CreateToggle("ANTI-NOON", 6, function(state)
    if state then
        NightLoop = RunService.RenderStepped:Connect(function()
            Lighting.ClockTime = 0
        end)
    else
        if NightLoop then
            NightLoop:Disconnect()
            NightLoop = nil
        end
    end
end)

local origShadows = Lighting.GlobalShadows
CreateToggle("GRAPHIC SHADOW", 7, function(state)
    Lighting.GlobalShadows = state
    if state then
        Lighting.Technology = Enum.Technology.Future
    end
end)

if isAdmin and AdminPage then
    local MessageFrame = Instance.new("Frame")
    MessageFrame.Size = UDim2.new(1, -20, 0, 100)
    MessageFrame.Position = UDim2.new(0, 10, 0, 40)
    MessageFrame.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
    MessageFrame.BorderSizePixel = 0
    MessageFrame.Parent = AdminPage

    local MessageCorner = Instance.new("UICorner")
    MessageCorner.CornerRadius = UDim.new(0, 8)
    MessageCorner.Parent = MessageFrame

    local MessageBox = Instance.new("TextBox")
    MessageBox.Size = UDim2.new(1, -20, 0, 40)
    MessageBox.Position = UDim2.new(0, 10, 0, 10)
    MessageBox.BackgroundColor3 = Color3.fromRGB(25, 15, 38)
    MessageBox.Text = "PESAN.."
    MessageBox.TextColor3 = Color3.fromRGB(200, 200, 220)
    MessageBox.Font = Enum.Font.Gotham
    MessageBox.TextSize = 14
    MessageBox.ClearTextOnFocus = true
    MessageBox.Parent = MessageFrame

    local MessageCorner2 = Instance.new("UICorner")
    MessageCorner2.CornerRadius = UDim.new(0, 6)
    MessageCorner2.Parent = MessageBox

    local SendBtn = Instance.new("TextButton")
    SendBtn.Size = UDim2.new(1, -20, 0, 35)
    SendBtn.Position = UDim2.new(0, 10, 1, -45)
    SendBtn.BackgroundColor3 = Color3.fromRGB(120, 40, 190)
    SendBtn.Text = "SEND"
    SendBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    SendBtn.Font = Enum.Font.GothamBold
    SendBtn.TextSize = 14
    SendBtn.Parent = MessageFrame

    local SendCorner = Instance.new("UICorner")
    SendCorner.CornerRadius = UDim.new(0, 6)
    SendCorner.Parent = SendBtn

    SendBtn.MouseButton1Click:Connect(function()
        local msg = MessageBox.Text
        if msg == "" or msg == "PESAN.." then return end
        
        local NotifGui = Instance.new("ScreenGui")
        NotifGui.Name = "LipzyNotification"
        NotifGui.Parent = game.CoreGui
        NotifGui.IgnoreGuiInset = true
        
        local NotifFrame = Instance.new("Frame")
        NotifFrame.Size = UDim2.new(0, 500, 0, 100)
        NotifFrame.Position = UDim2.new(0.5, -250, 0, -150)
        NotifFrame.BackgroundColor3 = Color3.fromRGB(30, 18, 48)
        NotifFrame.BorderSizePixel = 0
        NotifFrame.Parent = NotifGui
        
        local NotifCorner = Instance.new("UICorner")
        NotifCorner.CornerRadius = UDim.new(0, 10)
        NotifCorner.Parent = NotifFrame
        
        local NotifStroke = Instance.new("UIStroke")
        NotifStroke.Color = Color3.fromRGB(140, 45, 220)
        NotifStroke.Thickness = 2
        NotifStroke.Parent = NotifFrame
        
        local NotifProfile = Instance.new("ImageLabel")
        NotifProfile.Size = UDim2.new(0, 60, 0, 60)
        NotifProfile.Position = UDim2.new(0, 20, 0.5, -30)
        NotifProfile.BackgroundColor3 = Color3.fromRGB(45, 25, 70)
        NotifProfile.Image = Players:GetUserThumbnailAsync(LocalPlayer.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size150x150)
        NotifProfile.Parent = NotifFrame
        
        local NotifProfileCorner = Instance.new("UICorner")
        NotifProfileCorner.CornerRadius = UDim.new(1, 0)
        NotifProfileCorner.Parent = NotifProfile
        
        local NotifProfileStroke = Instance.new("UIStroke")
        NotifProfileStroke.Color = Color3.fromRGB(255, 215, 0)
        NotifProfileStroke.Thickness = 2
        NotifProfileStroke.Parent = NotifProfile
        
        local CrownLabel = Instance.new("TextLabel")
        CrownLabel.Size = UDim2.new(1, -100, 0, 20)
        CrownLabel.Position = UDim2.new(0, 90, 0, 0)
        CrownLabel.BackgroundTransparency = 1
        CrownLabel.Text = "👑LIPZY HUB👑"
        CrownLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
        CrownLabel.Font = Enum.Font.GothamBold
        CrownLabel.TextSize = 14
        CrownLabel.TextXAlignment = Enum.TextXAlignment.Left
        CrownLabel.Parent = NotifFrame
        
        local NotifMsg = Instance.new("TextLabel")
        NotifMsg.Size = UDim2.new(1, -100, 1, -25)
        NotifMsg.Position = UDim2.new(0, 90, 0, 20)
        NotifMsg.BackgroundTransparency = 1
        NotifMsg.Text = msg
        NotifMsg.TextColor3 = Color3.fromRGB(255, 255, 255)
        NotifMsg.Font = Enum.Font.Gotham
        NotifMsg.TextSize = 16
        NotifMsg.TextXAlignment = Enum.TextXAlignment.Left
        NotifMsg.TextYAlignment = Enum.TextYAlignment.Top
        NotifMsg.Parent = NotifFrame
        
        TweenService:Create(NotifFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Position = UDim2.new(0.5, -250, 0, 50)
        }):Play()
        
        task.delay(3, function()
            TweenService:Create(NotifFrame, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
                Position = UDim2.new(0.5, -250, 0, -150)
            }):Play()
            task.wait(0.5)
            NotifGui:Destroy()
        end)
    end)

    local OwnerScriptYPos = 150

    local OwnerScriptFrame = Instance.new("Frame")
    OwnerScriptFrame.Size = UDim2.new(1, -20, 0, 40)
    OwnerScriptFrame.Position = UDim2.new(0, 10, 0, OwnerScriptYPos)
    OwnerScriptFrame.BackgroundColor3 = Color3.fromRGB(35, 20, 55)
    OwnerScriptFrame.BorderSizePixel = 0
    OwnerScriptFrame.Parent = AdminPage

    local OwnerScriptCorner = Instance.new("UICorner")
    OwnerScriptCorner.CornerRadius = UDim.new(0, 8)
    OwnerScriptCorner.Parent = OwnerScriptFrame

    local OwnerTextLabel = Instance.new("TextLabel")
    OwnerTextLabel.Size = UDim2.new(0, 150, 1, 0)
    OwnerTextLabel.Position = UDim2.new(0, 10, 0, 0)
    OwnerTextLabel.BackgroundTransparency = 1
    OwnerTextLabel.Text = "OWNER SCRIPT"
    OwnerTextLabel.TextColor3 = Color3.fromRGB(200, 200, 220)
    OwnerTextLabel.Font = Enum.Font.GothamBold
    OwnerTextLabel.TextSize = 14
    OwnerTextLabel.TextXAlignment = Enum.TextXAlignment.Left
    OwnerTextLabel.Parent = OwnerScriptFrame

    local ToggleBg = Instance.new("Frame")
    ToggleBg.Size = UDim2.new(0, 50, 0, 26)
    ToggleBg.Position = UDim2.new(1, -60, 0.5, -13)
    ToggleBg.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    ToggleBg.Parent = OwnerScriptFrame

    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCorner.Parent = ToggleBg

    local ToggleCircle = Instance.new("Frame")
    ToggleCircle.Size = UDim2.new(0, 20, 0, 20)
    ToggleCircle.Position = UDim2.new(0, 3, 0.5, -10)
    ToggleCircle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    ToggleCircle.Parent = ToggleBg

    local ToggleCircleCorner = Instance.new("UICorner")
    ToggleCircleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCircleCorner.Parent = ToggleCircle

    local isOwnerScriptEnabled = false
    local headBillboard = nil
    local rainbowConnection = nil

    local function ApplyBillboard(character)
        if headBillboard then headBillboard:Destroy() end
        if rainbowConnection then rainbowConnection:Disconnect() end

        if not character or not character:FindFirstChild("Head") then return end

        headBillboard = Instance.new("BillboardGui")
        headBillboard.Name = "OwnerScriptBillboard"
        headBillboard.Adornee = character.Head
        headBillboard.Size = UDim2.new(0, 220, 0, 50)
        headBillboard.StudsOffset = Vector3.new(0, 2.5, 0)
        headBillboard.AlwaysOnTop = true
        headBillboard.Parent = character.Head
        
        local ownerText = Instance.new("TextLabel")
        ownerText.Name = "OwnerText"
        ownerText.Size = UDim2.new(1, 0, 1, 0)
        ownerText.BackgroundTransparency = 1
        ownerText.Font = Enum.Font.GothamBold
        ownerText.TextSize = 24
        ownerText.Parent = headBillboard
        
        local textStroke = Instance.new("UIStroke")
        textStroke.Color = Color3.fromRGB(0, 0, 0)
        textStroke.Thickness = 3
        textStroke.Parent = ownerText

        if LocalPlayer.Name == "Yanzzz_1023" then
            ownerText.Text = "👑OWNER👑"
            ownerText.TextColor3 = Color3.fromRGB(255, 220, 0)
        elseif LocalPlayer.Name == "LifzxAfter2" then
            ownerText.Text = "👑ADMIN👑"
            rainbowConnection = RunService.RenderStepped:Connect(function()
                local hue = (tick() % 5) / 5
                ownerText.TextColor3 = Color3.fromHSV(hue, 1, 1)
            end)
        else
            ownerText.Text = "👑VIP👑"
            ownerText.TextColor3 = Color3.fromRGB(200, 100, 255)
        end
    end

    local OwnerScriptBtn = Instance.new("TextButton")
    OwnerScriptBtn.Size = UDim2.new(1, 0, 1, 0)
    OwnerScriptBtn.BackgroundTransparency = 1
    OwnerScriptBtn.Text = ""
    OwnerScriptBtn.Parent = OwnerScriptFrame

    OwnerScriptBtn.MouseButton1Click:Connect(function()
        isOwnerScriptEnabled = not isOwnerScriptEnabled
        
        if isOwnerScriptEnabled then
            TweenService:Create(ToggleCircle, TweenInfo.new(0.2), {
                Position = UDim2.new(1, -23, 0.5, -10),
                BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            }):Play()
            TweenService:Create(ToggleBg, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(40, 120, 40)
            }):Play()
            
            if LocalPlayer.Character then
                ApplyBillboard(LocalPlayer.Character)
            end
        else
            TweenService:Create(ToggleCircle, TweenInfo.new(0.2), {
                Position = UDim2.new(0, 3, 0.5, -10),
                BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            }):Play()
            TweenService:Create(ToggleBg, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            }):Play()
            
            if headBillboard then
                headBillboard:Destroy()
                headBillboard = nil
            end
            if rainbowConnection then
                rainbowConnection:Disconnect()
                rainbowConnection = nil
            end
        end
    end)

    LocalPlayer.CharacterAdded:Connect(function(char)
        if isOwnerScriptEnabled then
            char:WaitForChild("Head")
            ApplyBillboard(char)
        end
    end)
end

print("Lipzy Hub V5.2 Loaded Successfully!")
