local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Plovas Hub",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = ":P",
   LoadingSubtitle = "Prepare",
   ShowText = "", -- for mobile users to unhide Rayfield, change if you'd like
   Theme = "Amethyst", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   ToggleUIKeybind = "K", -- The keybind to toggle the UI visibility (string like "K" or Enum.KeyCode)

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from emitting warnings when the script has a version mismatch with the interface.

   -- ScriptID = "sid_xxxxxxxxxxxx", -- Your Script ID from developer.sirius.menu — enables analytics, managed keys, and script hosting

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Plovas Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include Discord.gg/. E.g. Discord.gg/ABCD would be ABCD
      RememberJoins = false -- Set this to false to make them join the Discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Plovas Hub",
      Subtitle = "Key",
      Note = "", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique, as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"notKeniauskas"} -- List of keys that the system will accept, can be RAW file links (pastebin, github, etc.) or simple strings ("hello", "key22")
   }
})

Rayfield:Notify({
   Title = "Plovas Hub Loaded",
   Content = ":P :P :P :P :P :P :P :P :P :P :P :P",
   Duration = 6.5,
   Image = 4483362458,
})

local Tab = Window:CreateTab("Universal Cheats", 4483362458) -- Title, Image
local SpecTab = Window:CreateTab("Specified Game Cheats", 4483362458) -- Title, Image

local Button = SpecTab:CreateButton({
   Name = "Project Delta",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/kriause2nd-sketch/ESP_kriause2nd/refs/heads/main/Delta%20Project%20ESP.md'))()
   end,
})

local Button = SpecTab:CreateButton({
   Name = "Bee Swarm Simulator (atlas)",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/kriause2nd-sketch/ESP_kriause2nd/refs/heads/main/atlasbss.md'))()
   end,
})

local Button = SpecTab:CreateButton({
   Name = "Combat Initiation",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/kriause2nd-sketch/ESP_kriause2nd/refs/heads/main/combat_ini.md'))()
   end,
})

local Button = Tab:CreateButton({
   Name = "ESP",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/kriause2nd-sketch/ESP_kriause2nd/refs/heads/main/ESP2.md'))()
   end,
})

local Button = Tab:CreateButton({
   Name = "Infinite Yield",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
   end,
})

local Button = Tab:CreateButton({
   Name = "Anti-AFK & Anti-LAG",
   Callback = function()
   -- The function that takes place when the button is pressed
   loadstring(game:HttpGet('https://raw.githubusercontent.com/kriause2nd-sketch/ESP_kriause2nd/refs/heads/main/Anti-AFK.md'))()
   end,
})

local TabM = Window:CreateTab("Misc", 4483362458) -- Title, Image

local Label = TabM:CreateLabel("Credits to Plovas", 4483362458, Color3.fromRGB(255, 255, 255), false) -- Title, Icon, Color, IgnoreTheme

local Button = TabM:CreateButton({
   Name = "Unload Rayfield",
   Callback = function()
   -- The function that takes place when the button is pressed
   Rayfield:Destroy()
   end,
})

Rayfield:LoadConfiguration()
