
wait(tonumber(Settings.Delay))
            spawn(function()
                local Old = os.time()
                repeat
                    wait()
                until Settings.EmergencyReJoin and Settings.EmergencyReJoinTime and not Settings.FoundWillStop and (os.time() - Old) > tonumber(Settings.EmergencyReJoinTime)
                game.StarterGui:SetCore('SendNotification', {
                    Title = "Emergency ReJoin Working",
                    Text = "Something Went Wrong \n Please Check Your Settings \n Or Call Kang <3",
                    Icon = "rbxassetid://4918719521",
                    Duration = 5,
                })
                wait(3)
                game:GetService('TeleportService'):Teleport(game.PlaceId)
            end)
            
            repeat
                wait()
            until game.Players and (game.Workspace:FindFirstChild('Trees') and game.Workspace.Trees:FindFirstChild('Tree') and  game.Workspace.Trees.Tree:FindFirstChild('Model')) and (game.Workspace:FindFirstChild('Island14') and game.Workspace.Island14:FindFirstChild('PedestalSpots'))
            
            spawn(function()
                repeat
                    wait()
                until not game.Workspace:FindFirstChild('Trees')
                wait(10)
                game:GetService('TeleportService'):Teleport(game.PlaceId)
            end)
            
            if Settings.It_Is_Latest_Version and not Settings.NotSameServerListMax then
                game.StarterGui:SetCore('SendNotification', {
                    Title = "README UWU",
                    Text = "Your script is't not last version \n you can got it on hunter v1 or ctrl + v on browser.",
                    Icon = "rbxassetid://4918719521",
                    Duration = 30,
                })
                setclipboard('https://cdn.discordapp.com/attachments/658642551804395541/786851251014991902/OPL_sub_Hunter.lua')
                return
            end
            
            
            
            local RATE_Fruit = loadstring(game:HttpGet'https://raw.githubusercontent.com/KangKung01/KangWinnerB/main/OPL/DevilFurit_Name.lua')()
            local RATE_BOX = loadstring(game:HttpGet'https://raw.githubusercontent.com/KangKung01/KangWinnerB/main/OPL/Box_Name.lua')()
            local STATS_STOP
            
            local Firewebhook = function(Type, Owner, Name, Stats, Where)
                local Request = (syn and syn.request) or request
                Request({
                    Url = tostring(Settings.WebHook),
                    Method = 'POST',
                    Headers = {['Content-Type'] = 'application/json'},
                    Body = game:GetService('HttpService'):JSONEncode({
                        embeds = {{
                            title = tostring(Settings.Title),
                            fields = {
                                {
                                    name = 'Type :',
                                    value = tostring(Type),
                                    inline = false
                                },
                                {
                                    name = 'Owner :',
                                    value = tostring(Owner),
                                    inline = false
                                },
                                {
                                    name = 'Name :',
                                    value = tostring(Name),
                                    inline = false
                                },
                                {
                                    name = 'Stats :',
                                    value = tostring((Stats == '' and 'None') or Stats),
                                    inline = false
                                },
                                {
                                    name = 'Where :',
                                    value = tostring(Where),
                                    inline = false
                                },
                                {
                                    name = 'Server :',
                                    value = tostring(game.JobId),
                                    inline = false
                                }
                            },
                            color = tonumber(Settings.Color)
                        }}
                    })
                })
                if Settings.FoundWillStop then
                    STATS_STOP = true
                end
            end
            local Stats_Check
            
            
            pcall(function()
                if Settings.RareFurits then
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Backpack:GetChildren()) do
                            if table.find(RATE_Fruit, Fruit.Name) then
                                Firewebhook('Fruit', Players.Name, Fruit.Name, Fruit.ToolTip, 'Backpack')
                            end
                        end
                    end
            
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Character:GetChildren()) do
                            if Fruit.ClassName == 'Tool' and table.find(RATE_Fruit, Fruit.Name) then
                                Firewebhook('Fruit', Players.Name, Fruit.Name, Fruit.ToolTip, 'Hand')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Trees.Tree.Model:GetChildren()) do
                        if table.find(RATE_Fruit, Fruit.Name) then
                            Firewebhook('Fruit', 'None', Fruit.Name, Fruit.ToolTip, 'Ground')
                        end
                    end
            
                    for _, Path in pairs(game.Workspace.Island14.PedestalSpots:GetChildren()) do
                        for _, Fruit in pairs(Path:GetChildren()) do
                            if table.find(RATE_Fruit, Fruit.Name) then
                                Firewebhook('Fruit', 'None', Fruit.Name, Fruit.ToolTip, 'Nomal Pedestal')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Altar.FruitReceptical:GetChildren()) do
                        if table.find(RATE_Fruit, Fruit.Name) then
                            Firewebhook('Fruit', 'None', Fruit.Name, Fruit.ToolTip, 'Rebirth Pedestal')
                        end
                    end
                end
            
                if Settings.RareBox then
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Backpack:GetChildren()) do
                            if table.find(RATE_BOX, Fruit.Name) then
                                Firewebhook('Box', Players.Name, Fruit.Name, Fruit.ToolTip, 'Backpack')
                            end
                        end
                    end
            
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Character:GetChildren()) do
                            if Fruit.ClassName == 'Tool' and table.find(RATE_BOX, Fruit.Name) then
                                Firewebhook('Box', Players.Name, Fruit.Name, Fruit.ToolTip, 'Hand')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Trees.Tree.Model:GetChildren()) do
                        if table.find(RATE_BOX, Fruit.Name) then
                            Firewebhook('Box', 'None', Fruit.Name, Fruit.ToolTip, 'Ground')
                        end
                    end
            
                    for _, Path in pairs(game.Workspace.Island14.PedestalSpots:GetChildren()) do
                        for _, Fruit in pairs(Path:GetChildren()) do
                            if table.find(RATE_BOX, Fruit.Name) then
                                Firewebhook('Box', 'None', Fruit.Name, Fruit.ToolTip, 'Nomal Pedestal')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Altar.FruitReceptical:GetChildren()) do
                        if table.find(RATE_BOX, Fruit.Name) then
                            Firewebhook('Box', 'None', Fruit.Name, Fruit.ToolTip, 'Rebirth Pedestal')
                        end
                    end
                end
            
                if Settings.AuraFurits then
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Backpack:GetChildren()) do
                            if Fruit.ClassName == 'Tool' and Fruit:FindFirstChild('Main') and Fruit.Main:FindFirstChild('AuraAttachment') then
                                Firewebhook('Fruit [Arua]', Players.Name, Fruit.Name, Fruit.ToolTip, 'Backpack')
                            end
                        end
                    end
            
                    for _, Players in pairs(game.Players:GetChildren()) do
                        for _, Fruit in pairs(Players.Character:GetChildren()) do
                            if Fruit.ClassName == 'Tool' and Fruit:FindFirstChild('Main') and Fruit.Main:FindFirstChild('AuraAttachment') then
                                Firewebhook('Fruit [Arua]', Players.Name, Fruit.Name, Fruit.ToolTip, 'Hand')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Trees.Tree.Model:GetChildren()) do
                        if Fruit.ClassName == 'Tool' and Fruit:FindFirstChild('Main') and Fruit.Main:FindFirstChild('AuraAttachment') then
                            Firewebhook('Fruit [Arua]', 'None', Fruit.Name, Fruit.ToolTip, 'Ground')
                        end
                    end
            
                    for _, Path in pairs(game.Workspace.Island14.PedestalSpots:GetChildren()) do
                        for _, Fruit in pairs(Path:GetChildren()) do
                            if Fruit.ClassName == 'Tool' and Fruit:FindFirstChild('Main') and Fruit.Main:FindFirstChild('AuraAttachment') then
                                Firewebhook('Fruit [Arua]', 'None', Fruit.Name, Fruit.ToolTip, 'Nomal Pedestal')
                            end
                        end
                    end
            
                    for _, Fruit in pairs(game.Workspace.Altar.FruitReceptical:GetChildren()) do
                        if Fruit.ClassName == 'Tool' and Fruit:FindFirstChild('Main') and Fruit.Main:FindFirstChild('AuraAttachment') then
                            Firewebhook('Fruit [Arua]', 'None', Fruit.Name, Fruit.ToolTip, 'Rebirth Pedestal')
                        end
                    end
                end
                Stats_Check = true
            end)
            
            repeat
                wait()
            until Stats_Check and Settings.ServerHoping and not STATS_STOP
            
            
            
            local NotSameServer
            xpcall(function()
                NotSameServer = game:GetService('HttpService'):JSONDecode(readfile('NotSameServer.json'))
                if #NotSameServer >= tonumber(Settings.NotSameServerListMax) then
                    NotSameServer = {}
                end
                writefile('NotSameServer.json', game:GetService('HttpService'):JSONEncode(NotSameServer))
            end, function()
                NotSameServer = {}
                writefile('NotSameServer.json', game:GetService('HttpService'):JSONEncode(NotSameServer))
            end)
            
            local ServerList, Server = {}
            pcall(function()
                Server = game:GetService('HttpService'):JSONDecode(game:HttpGet(string.format('https://games.roblox.com/v1/games/%s/servers/Public?sortOrder=Asc&limit=100', game.PlaceId)))
            end)
            
            
            pcall(function()
                for _, v in pairs(Server.data) do
                    if not table.find(NotSameServer, v.id) and v.maxPlayers ~= v.playing then
                        table.insert(ServerList, v.id)
                    end
                end
            end)
            
            if #ServerList == 0 then
                game:GetService('TeleportService'):Teleport(game.PlaceId)
            end
            
            repeat
                game.StarterGui:SetCore('SendNotification', {
                    Title = "README UWU",
                    Text = "Rejoining!",
                    Icon = "rbxassetid://4918719521",
                    Duration = 5,
                })
                local ServerID = ServerList[math.random(1, #ServerList)]
                table.insert(NotSameServer, ServerID)
                writefile('NotSameServer.json', game:GetService('HttpService'):JSONEncode(NotSameServer))
                wait(3)
                game:GetService('TeleportService'):TeleportToPlaceInstance(game.PlaceId, ServerID)
                wait(10)
            until not game.Players
