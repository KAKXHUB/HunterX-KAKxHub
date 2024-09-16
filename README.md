_G.Start = true
local DalayTime = 15
local NotificationFruit = true
local NotificationAuraFruit = true 
local NotificationBox = true
local SelectNotificationFruit = ""
local SelecNotificationBox = ""
local YourWebhookUrl = "https://discord.com/api/webhooks/921002942620893184/yritWiOPovRsBch9EMzTWuh0aKHWZy8e5KYuKrvbdU0PnxJbaSoVOnIGIFPj6tUIUe19"
local StopHopServerWhenFoundSelectDFBox = true
local NotSameServerListMax = 5

local Cache = { DevConfig = {} };
Cache.DevConfig["ListOfRareDveilFruit"] = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://raw.githubusercontent.com/KangKung02/just-bin/main/OPL_LF.json"));
Cache.DevConfig["ListOfBox"] = {"Common Box", "Uncommon Box", "Rare Box", "Ultra Rare Box"};
Cache.DevConfig["ListOfRareBox"] = {"Rare Box", "Ultra Rare Box"};


local Local_Found_DF_BOX;
spawn(function()
    while wait() do
        pcall(function()
            if not (NotificationFruit or NotificationAuraFruit or NotificationBox) then return end;
            wait(5);
            local Detector = {
                ListOfRareFruit = {};
                ListOfNormalFruit = {};
                ListOfNormalBox = {};
                ListOfRareBox = {};
            };
            
            function Detector:Add(Argument)
                if Argument.IsFruit then
                    if Argument["IsRare"] then
                        table.insert(self.ListOfRareFruit, Argument);
                    else
                        table.insert(self.ListOfNormalFruit, Argument);
                    end
                else
                    if Argument["IsRare"] then
                        table.insert(self.ListOfRareBox, Argument);
                    else
                        table.insert(self.ListOfNormalBox, Argument);
                    end
                end
            end;
            
            for _, Players in pairs(game.Players:GetChildren()) do
                if Players.Name ~= game.Players.LocalPlayer.Name then
                    for _, Item in pairs(Players.Backpack:GetChildren()) do
                        if Item.ClassName == "Tool" and string.match(Item.Name, "Fruit") then
                            Detector:Add({
                                ["FruitName"] = Item.Name;
                                ["Owner"] = Players.Name;
                                ["Position"] = "Backpack";
                                ["IsRare"] = table.find(Cache.DevConfig["ListOfRareDveilFruit"], Item.Name);
                                ["IsFruit"] = true;
                                ["IsAura"] = (Item:FindFirstChild("Main") and Item.Main:FindFirstChild("AuraAttachment") and true) or false;
                            });
                        elseif Item.ClassName == "Tool" and table.find(Cache.DevConfig["ListOfBox"], Item.Name) then
                            Detector:Add({
                                ["FruitName"] = Item.Name;
                                ["Owner"] = Players.Name;
                                ["Position"] = "Backpack";
                                ["IsRare"] = table.find(Cache.DevConfig["ListOfRareBox"], Item.Name);
                                ["IsFruit"] = false;
                            });
                        end
                    end
            
                    for _, Item in pairs(Players.Character:GetChildren()) do
                        if Item.ClassName == "Tool" and string.match(Item.Name, "Fruit") then
                            Detector:Add({
                                ["FruitName"] = Item.Name;
                                ["Owner"] = Players.Name;
                                ["Position"] = "Hand";
                                ["IsRare"] = table.find(Cache.DevConfig["ListOfRareDveilFruit"], Item.Name);
                                ["IsFruit"] = true;
                                ["IsAura"] = (Item:FindFirstChild("Main") and Item.Main:FindFirstChild("AuraAttachment") and true) or false;
                            });
                        elseif Item.ClassName == "Tool" and table.find(Cache.DevConfig["ListOfBox"], Item.Name) then
                            Detector:Add({
                                ["FruitName"] = Item.Name;
                                ["Owner"] = Players.Name;
                                ["Position"] = "Hand";
                                ["IsRare"] = table.find(Cache.DevConfig["ListOfRareBox"], Item.Name);
                                ["IsFruit"] = false;
                            });
                        end
                    end
                end
            end
            
            for _, Item in pairs(game.Workspace:GetChildren()) do
                if Item.ClassName == "Tool" and table.find(Cache.DevConfig["ListOfBox"], Item.Name) then
                    Detector:Add({
                        ["FruitName"] = Item.Name;
                        ["Owner"] = "None";
                        ["Position"] = "Ground";
                        ["IsRare"] = table.find(Cache.DevConfig["ListOfRareBox"], Item.Name);
                        ["IsFruit"] = false;
                    });
                end
            end
            
            for _, Item in pairs(game.Workspace.Trees.Tree.Model:GetChildren()) do
                if Item.ClassName == "Tool" and string.match(Item.Name, "Fruit") then
                    Detector:Add({
                        ["FruitName"] = Item.Name;
                        ["Position"] = "Ground";
                        ["IsRare"] = table.find(Cache.DevConfig["ListOfRareDveilFruit"], Item.Name);
                        ["IsFruit"] = true;
                        ["IsAura"] = (Item:FindFirstChild("Main") and Item.Main:FindFirstChild("AuraAttachment") and true) or false;
                    });
                end
            end
            
            for _, Path in pairs(game:GetService("Workspace").Island14.PedestalSpots:GetChildren()) do
                for _, Item in pairs(Path:GetChildren()) do
                    if Item.ClassName == "Tool" and string.match(Item.Name, "Fruit") then
                        Detector:Add({
                            ["FruitName"] = Item.Name;
                            ["Position"] = "Pedestal";
                            ["IsRare"] = table.find(Cache.DevConfig["ListOfRareDveilFruit"], Item.Name);
                            ["IsFruit"] = true;
                            ["IsAura"] = (Item:FindFirstChild("Main") and Item.Main:FindFirstChild("AuraAttachment") and true) or false;
                        });
                    end
                end
            end
            
            for _, Item in pairs(game:GetService("Workspace").Altar.FruitReceptical:GetChildren()) do
                if Item.ClassName == "Tool" and string.match(Item.Name, "Fruit") then
                    Detector:Add({
                        ["FruitName"] = Item.Name;
                        ["Position"] = "Rebirth Pedestal";
                        ["IsRare"] = table.find(Cache.DevConfig["ListOfRareDveilFruit"], Item.Name);
                        ["IsFruit"] = true;
                        ["IsAura"] = (Item:FindFirstChild("Main") and Item.Main:FindFirstChild("AuraAttachment") and true) or false;
                    });
                end
            end
            
            local Webhook = { Data = {} };
            function Webhook:Add(Values)
                table.insert(self.Data, {
                    Type = (Values.IsRare and "Rare") or "Normal";
                    Value = string.format("Item Name : %s, Owner Name : %s, Position : %s, IsRare : %s, IsAura : %s, JobId : %s", Values.FruitName or "None", Values.Owner or "None", Values.Position or "None", (Values.IsRare and "Yes") or "No", (Values.IsAura and "Yes") or "No", game.JobId);
                });
            end;

            function Webhook:Getfields()
                local Content = {};
                local NewTable = {};
                for Index, Value in pairs(self.Data) do
                    if Index <= 15 then
                        table.insert(Content, {
                            ["name"] = Value.Type .. " :";
                            ["value"] = Value.Value;
                            ["inline"] = false;
                        });
                    else
                        table.insert(NewTable, Value);
                    end
                end
                self.Data = NewTable;
                return Content;
            end

            function Webhook:Send(Url)
                if not Url then return end;
                if not string.find(Url, "https://discord.com/api/webhooks") then return end;
                local My_Request = (syn and syn.request) or (http and http.request) or request;
                My_Request({
                    Url = Url;
                    Method = "POST";
                    Headers = {["Content-Type"] = "application/json"};
                    Body = game:GetService("HttpService"):JSONEncode({
                        ["embeds"] = {{
                            ["color"] = tonumber(0xffffff);
                            ["title"] = "Krypton Notification";
                            ["fields"] = self:Getfields();
                            ["footer"] = {
                                ["text"] = "Time : " .. os.date("%c", os.time());
                            };
                            ["image"] = {
                                ["url"] = "https://i.stack.imgur.com/Fzh0w.png"
                            };
                        }}
                    });
                });
            end

            for _, Argument in pairs(Detector.ListOfNormalFruit) do
                if NotificationFruit and SelectNotificationFruit[Argument.FruitName] then
                    Webhook:Add(Argument);
                elseif NotificationAuraFruit and Argument.IsAura then
                    Webhook:Add(Argument);
                end
            end
            for _, Argument in pairs(Detector.ListOfRareFruit) do
                if NotificationFruite and SelectNotificationFruit[Argument.FruitName] then
                    Webhook:Add(Argument);
                elseif NotificationAuraFruit and Argument.IsAura then
                    Webhook:Add(Argument);
                end
            end
            for _, Argument in pairs(Detector.ListOfNormalBox) do
                if NotificationBox and SelecNotificationBox[Argument.FruitName] then
                    Webhook:Add(Argument);
                end
            end
            for _, Argument in pairs(Detector.ListOfRareBox) do
                if NotificationBox and SelecNotificationBox[Argument.FruitName] then
                    Webhook:Add(Argument);
                end
            end

            repeat
                if #Webhook.Data > 0 then
                    Local_Found_DF_BOX = true;
                    Webhook:Send(YourWebhookUrl);
                    wait();
                end
            until #Webhook.Data == 0;

            wait(DalayTime);
        end)
    end
end);


spawn(function()
        while wait() do
            pcall(function()
                if not _G.Start then return end;
                wait(DalayTime);
                if StopHopServerWhenFoundSelectDFBox and Local_Found_DF_BOX then
                    _G.Start:SetValue(false);
                    Library:Notify("I Found!, So i Stop");
                    return;
                end;
                local NotSameServer;
                xpcall(function()
                    NotSameServer = game:GetService("HttpService"):JSONDecode(readfile("NotSameServer.json"))
                    if #NotSameServer >= tonumber(NotSameServerListMax or 5) then
                        NotSameServer = {};
                    end
                    writefile("NotSameServer.json", game:GetService("HttpService"):JSONEncode(NotSameServer));
                end, function()
                    NotSameServer = {};
                    writefile("NotSameServer.json", game:GetService("HttpService"):JSONEncode(NotSameServer));
                end);
                
                local ServerList, Server = {};
                pcall(function()
                    Server = game:GetService('HttpService'):JSONDecode(game:HttpGet(string.format('https://games.roblox.com/v1/games/%s/servers/Public?sortOrder=Asc&limit=100', game.PlaceId)));
                end);
    
                pcall(function()
                    for _, v in pairs(Server.data) do
                        if not table.find(NotSameServer, v.id) and v.maxPlayers ~= v.playing then
                            table.insert(ServerList, v.id);
                        end
                    end
                end);
    
                if #ServerList == 0 then
                    game:GetService('TeleportService'):Teleport(game.PlaceId);
                end
    
                repeat
                    game.StarterGui:SetCore('SendNotification', {
                        Title = "README UWU",
                        Text = "Rejoining!",
                        Icon = "rbxassetid://4918719521",
                        Duration = 5,
                    });
                    local ServerID = ServerList[math.random(1, #ServerList)];
                    table.insert(NotSameServer, ServerID);
                    writefile('NotSameServer.json', game:GetService('HttpService'):JSONEncode(NotSameServer));
                    wait(3);
                    game:GetService('TeleportService'):TeleportToPlaceInstance(game.PlaceId, ServerID);
                    wait(10);
                until not game.Players;
            end)
        end
end);
