-- GUI
local screenGui = Instance.new("ScreenGui", game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"))
local toggleButton = Instance.new("TextButton", screenGui)
toggleButton.Size = UDim2.new(0, 40, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "N"
toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 20
toggleButton.Draggable = true

local running = false
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local PlaceId = game.PlaceId

-- تابع البحث عن الفواكه
local function checkShop()
    local suc, res = pcall(function()
        return HttpService:JSONDecode(game:HttpGet("https://www.bloxfruitshop.com/api/shop"))
    end)
    if suc and res then
        for _, fruit in pairs(res) do
            if fruit.Name == "Dragon" or fruit.Name == "Kitsune" then
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "فاكهة نادرة!",
                    Text = fruit.Name .. " متوفرة!",
                    Duration = 10
                })
                setclipboard("فاكهة " .. fruit.Name .. " متوفرة في المتجر!")
                return true
            end
        end
    end
    return false
end

-- تابع التنقل بين السيرفرات
local function serverHop()
    local servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Desc&limit=100"))
    for _, server in pairs(servers.data) do
        if server.playing < server.maxPlayers then
            wait(2)
            TeleportService:TeleportToPlaceInstance(PlaceId, server.id)
            return
        end
    end
end

-- عندما تضغط الزر
toggleButton.MouseButton1Click:Connect(function()
    running = not running
    toggleButton.Text = running and "On" or "N"
    if running then
        while running do
            local found = checkShop()
            if not found then
                serverHop()
            else
                break
            end
            wait(15)
        end
    end
end)
