local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({ 
    Title = "HunterX " .. Fluent.Version,
    SubTitle = "by IM PEET",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

local Tabs = {
    Main = Window:AddTab({ Title = "HunterX", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}



Tabs.Main:AddSection("Webhook")

local Input = Tabs.Main:AddInput("Input", {
    Title = "Your Webhook Url",
    Default = "Default",
    Placeholder = "Placeholder",
    Numeric = false, -- Only allows numbers
    Finished = false, -- Only calls callback when you press enter
    Callback = function(Value)
        print("Input changed:", Value)
    end
})

Input:OnChanged(function()
    print("Input updated:", Input.Value)
end)


Tabs.Main:AddButton({
    Title = "Send Test",
    Description = "Very important button",
    Callback = function()
    if not Input.Value then return end;
    if not string.find(Input.Value, "https://discord.com/api/webhooks") then return end;
    local My_Request = (syn and syn.request) or (http and http.request) or request;
    My_Request({
        Url = Input.Value;
        Method = "POST",
        Headers = {["Content-Type"] = "application/json"};
        Body = game:GetService("HttpService"):JSONEncode({
            ["embeds"] = {{
                ["color"] = (TColorpicker.Value and tonumber(to_hex(TColorpicker.Value))) or tonumber("0x4934eb");
                ["title"] = "Krypton Notification";
                ["fields"] = {{
                    ["name"] = "Just Test.";
                    ["value"] = "Hello World!";
                    ["inline"] = false;
                }}
            }}
        });
    });
    end
})


local TColorpicker = Tabs.Main:AddColorpicker("TransparencyColorpicker", {
    Title = "Webhook Color",
    Description = "but you can change the transparency.",
    Transparency = 0,
    Default = Color3.new(0.431372, 0.196078, 0.807843)
})

TColorpicker:OnChanged(function()
    print(
        "TColorpicker changed:", TColorpicker.Value,
        "Transparency:", TColorpicker.Transparency
    )
end)


Tabs.Main:AddSection("Server Hop")



