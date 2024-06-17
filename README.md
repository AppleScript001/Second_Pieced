local NPCS = {}

    for i, v in pairs(workspace.Lives:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
            if not table.find(NPCS, tostring(v)) then
            table.insert(NPCS, tostring(v))
        end
    end
end
local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()

local X = Material.Load({
    Title = "Second Piece",
    Style = 1,
    SizeX = 350,
    SizeY = 350,
    Theme = "Dark",
    ColorOverrides = {
        MainFrame = Color3.fromRGB(0,0,0),
        Toggle = Color3.fromRGB(124,37,255),
        ToggleAccent = Color3.fromRGB(255,255,255), 
        Dropdown = Color3.fromRGB(124,37,255),
		DropdownAccent = Color3.fromRGB(255,255,255),
        Slider = Color3.fromRGB(124,37,255),
		SliderAccent = Color3.fromRGB(255,255,255),
        NavBarAccent = Color3.fromRGB(0,0,0),
        Content = Color3.fromRGB(0,0,0),
    }
})

local Y = X.New({
    Title = "Main"
})

local Z = X.New({
    Title = "LocalPlayer"
})

local Cred = X.New({
    Title = "Credits"
})
    Cred.Button({
    Text = "Free|Script",
    Callback = function()
        setclipboard("Good")
        toclipboard("Nice|Scripts")
    end,
})

local ii = Y.Dropdown({
    Text = "Select Mobs!",
    Callback = function(Value)
        getgenv().mobname = Value
end,
    Options = NPCS
})

local function getNPC()
    local dist, thing = math.huge
    for i, v in pairs(workspace.Lives:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).magnitude
            if mag < dist then
                dist = mag
                thing = v
            end
        end
    end
    return thing
end
Y.Button(
    {
        Text = "Refresh Mobs",
        Callback = function()
            table.clear(NPCS)
            for i, v in pairs(workspace.Lives:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(NPCS, tostring(v)) then
                        table.insert(NPCS, tostring(v))
                        ii:SetOptions(NPCS)
                    end
                end
            end
        end
    }
)
Y.Toggle({
    Text = "Auto Mob",
    Callback = function(Value)
        b = Value
        while b do task.wait()
                    pcall(function()
                    
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = getNPC().UpperTorso.CFrame
            end)
        end
	end,
    Enabled = false
})
Y.Toggle({
    Text = "Auto Attack",
    Callback = function(Value)
        a = Value
        while a do task.wait()
            local toolName = "Katana" -- Replace "YourToolNameHere" with the name of your tool

            local LocalPlayer = game:GetService("Players").LocalPlayer
            for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
                if v:IsA('Tool') and v.Name == toolName then
                    v.Parent = LocalPlayer.Character
                    break -- Stop the loop after picking up the tool
                end
            end
            
            LocalPlayer.Character:FindFirstChild(toolName):Activate()            
        end
	end,
    Enabled = false
})
