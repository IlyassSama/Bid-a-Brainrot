local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local ProfileService = require(ReplicatedStorage.Modules.ProfileService)

local DATASTORE_NAME = "test3"

local TEMPLATE = {
	Cash = 100,
	Level = 1,
	Rebirths = 0,

	Inventory = {
		Brainrots = {
			["Noobini Pizzanini"] = 1
		},

		Buildings = {
			["Podium"] = 1
		},
	},

	PlacedBrainrots = {},
	PassedTutorial = false,
	PlotSize = 10,
}


local PlayerProfiles = {}
local PlayerDataHandler = {}

local ProfileStore = ProfileService.GetProfileStore(DATASTORE_NAME, TEMPLATE)

function PlayerDataHandler:GetProfile(player)
	return PlayerProfiles[player]
end

function PlayerDataHandler:GetData(player)
	local profile = PlayerProfiles[player]
	return profile and profile.Data or nil
end

Players.PlayerAdded:Connect(function(player)
	local profile = ProfileStore:LoadProfileAsync("Player_" .. player.UserId, "ForceLoad")

	if profile then
		profile:AddUserId(player.UserId)
		profile:Reconcile()

		PlayerProfiles[player] = profile

		profile:ListenToRelease(function()
			PlayerProfiles[player] = nil
			player:Kick("Your data has been released.")
		end)

		if not player:IsDescendantOf(Players) then
			profile:Release()
			return
		end

		print("✅ Data loaded for:", player.Name)
		print(profile.Data) -- 🔁 Prints the full data structure

	else
		player:Kick("Failed to load your data.")
	end
end)

Players.PlayerRemoving:Connect(function(player)
	local profile = PlayerProfiles[player]
	if profile then
		profile:Release()
	end
end)

return PlayerDataHandler
