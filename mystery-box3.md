### 2. Custom Command #3
* **Trigger Type**: None (Executed via scheduleUniqueCC)

```gotemplate

{{/* --- GameCentral Animation Steps & Result (CC #40) --- */}}
{{ $staffChannelID := 1379145901318344844 }}
{{ $staffRoleID := 1400716156930621480 }} {{$vipRoleID := 1394846017396015181 }}
{{ $removeVIPCCID := 39 }} {{$animCCID := 40 }}

{{ $data := .ExecData }}
{{ $userMention := printf "<@\%d>" $data.userID }}
{{ $step :=$data.step }}

{{ if eq $step 1 }} 	{{$embed40 := cembed
		"title" "🎮 GameCentral Vault Opening..."
		"description" "▓▓▓▓░░░░░░ 40%\n🔍 *Scanning Crate Biometrics...*"
		"color" 0x3498DB
		"footer" (sdict "text" "GameCentral Supply Drop • Unlocking...")
		"timestamp" currentTime
	}}
	{{ editMessage $data.channelID $data.msgID (complexMessage "content" $userMention "embed" $embed40) }}
	{{ execCC $animCCID nil 2 (sdict "msgID" $data.msgID "userID" $data.userID "channelID" $data.channelID "step" 2) }}

{{ else if eq $step 2 }} 	{{$embed60 := cembed
		"title" "🎮 GameCentral Vault Opening..."
		"description" "▓▓▓▓▓▓░░░░ 60%\n⚙️ *Bypassing Security Locks...*"
		"color" 0x3498DB
		"footer" (sdict "text" "GameCentral Supply Drop • Unlocking...")
		"timestamp" currentTime
	}}
	{{ editMessage $data.channelID $data.msgID (complexMessage "content" $userMention "embed" $embed60) }}
	{{ execCC $animCCID nil 2 (sdict "msgID" $data.msgID "userID" $data.userID "channelID" $data.channelID "step" 3) }}

{{ else if eq $step 3 }} 	{{$embed80 := cembed
		"title" "🎮 GameCentral Vault Opening..."
		"description" "▓▓▓▓▓▓▓▓░░ 80%\n🔓 *Access Granted! Unfolding Loot Box...*"
		"color" 0xF1C40F
		"footer" (sdict "text" "GameCentral Supply Drop • Almost there...")
		"timestamp" currentTime
	}}
	{{ editMessage $data.channelID $data.msgID (complexMessage "content" $userMention "embed" $embed80) }}
	{{ execCC $animCCID nil 2 (sdict "msgID" $data.msgID "userID" $data.userID "channelID" $data.channelID "step" 4) }}

{{ else if eq $step 4 }}
	{{ $roll := randInt 1 101 }} 	{{$title := "" }}
	{{ $desc := "" }}
	{{ $color := 0x95A5A6 }} 	{{$rewardName := "" }}
	{{ $isWin := false }}

	{{ if le $roll 1 }} 		{{$title = "👑 LEGENDARY DROP!" }}
		{{ $desc = printf "%s, TRIPLE KILL! You unlocked a **Legendary Drop**: **1 Month Nitro**!" $userMention }}
		{{ $color = 0xFEE75C }} 		{{$rewardName = "LEGENDARY: 1 Month Nitro 👑" }}
		{{ $isWin = true }}
	{{ else if le $roll 10 }} 		{{$title = "💜 EPIC LOOT!" }}
		{{ $desc = printf "%s, NICE SHOT! You won an **Epic Perk**: **VIP Role (7 Days)**!\n\n✨ *Your VIP role has been automatically assigned for 7 days!*" $userMention }}
		{{ $color = 0x9B59B6 }} 		{{$rewardName = "EPIC: VIP Role (7 Days) 💜" }}
		{{ $isWin = true }}

		{{ giveRoleID $data.userID $vipRoleID }}
		{{ scheduleUniqueCC $removeVIPCCID nil 604800 (printf "remove_vip_%d" $data.userID) (sdict "userID" $data.userID "roleID" $vipRoleID) }}

	{{ else if le $roll 25 }} 		{{$title = "🎨 RARE DROP!" }}
		{{ $desc = printf "%s, You unlocked a **Rare Perk**: **World Boss Game boosts (ATK + DEF)**! Tag a Staff member to claim your boosts." $userMention }}
		{{ $color = 0x1ABC9C }} 		{{$rewardName = "RARE: World Boss Game boosts (ATK + DEF)" }}
		{{ $isWin = true }}
	{{ else if le $roll 50 }} 		{{$title = "🪙 UNCOMMON DROP!" }}
		{{ $desc = printf "%s, You found **5000 GameCentral Coins**! Tag a Staff member to claim it." $userMention }}
		{{ $color = 0x3498DB }} 		{{$rewardName = "UNCOMMON: 5000 GC Coins 🪙" }}
		{{ $isWin = true }}
	{{ else }}
		{{ $title = "◽ COMMON DROP (Empty)" }}
		{{ $desc = printf "%s, Out of ammo this time! The crate was empty, try again tomorrow." $userMention }}
		{{ $color = 0x95A5A6 }}
	{{ end }}

	{{ $finalEmbed := cembed
		"title" $title
		"description" (printf "██████████ 100%%\n✨ **Loot Box Opened!**\n\n%s" $desc)
		"color" $color
		"footer" (sdict "text" "GameCentral Supply Drop • Daily Reset")
		"timestamp" currentTime
	}}
	{{ editMessage $data.channelID $data.msgID (complexMessage "content" $userMention "embed" $finalEmbed) }}

	{{ if $isWin }}
		{{ $staffEmbed := cembed
			"title" "🎮 GameCentral Loot Alert!"
			"description" (printf "👤 **Gamer:** %s (`%d`)\n📍 **Channel:** <#%d>\n🎁 **Drop:** %s" $userMention$data.userID $data.channelID $rewardName)
			"color" $color
			"timestamp" currentTime
		}}
		{{ sendMessage $staffChannelID (complexMessage "content" (printf "<@&%d>" $staffRoleID) "embed" $staffEmbed "allowed_mentions" (sdict "parse" (cslice "roles" "users"))) }}
	{{ end }}
{{ end }}
