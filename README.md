-- Script to display the top 5 players' bounty in a leaderboard

local Players = game:GetService("Players")
local leaderboard = Instance.new("ScreenGui")
local frame = Instance.new("Frame")

leaderboard.Name = "BountyLeaderboard"
leaderboard.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
frame.Parent = leaderboard
frame.Size = UDim2.new(0, 200, 0, 300)

-- Function to update the leaderboard
local function updateLeaderboard()
    -- Sort players by their bounty (assuming bounty is stored in leaderstats)
    local playerStats = {}
    for _, player in pairs(Players:GetPlayers()) do
        if player:FindFirstChild("leaderstats") and player.leaderstats:FindFirstChild("Bounty") then
            table.insert(playerStats, {player = player, bounty = player.leaderstats.Bounty.Value})
        end
    end
    table.sort(playerStats, function(a, b) return a.bounty > b.bounty end)

    -- Display top 5 players
    for i = 1, math.min(5, #playerStats) do
        local textLabel = Instance.new("TextLabel")
        textLabel.Parent = frame
        textLabel.Text = playerStats[i].player.Name .. ": " .. playerStats[i].bounty
        textLabel.Size = UDim2.new(0, 200, 0, 30)
        textLabel.Position = UDim2.new(0, 0, 0, (i - 1) * 30)
    end
end

-- Update the leaderboard every 30 seconds
while true do
    updateLeaderboard()
    wait(30)
end
