## 🛠️ Setup Guide

### 1. Custom Command #1 — Initial Trigger (`!mysterybox`)
* **Trigger Type**: Command / Prefix
* **Trigger**: `mysterybox`

```gotemplate
{{/* --- GameCentral Loot Drop CC (20% Trigger) --- */}}
{{ $cooldownSecs := 86400 }}
{{ $staffChannelID := 1379145901318344844 }}
{{ $staffRoleID := 1400716156930621480 }}
{{ $adminID := 1349194216169013248 }}
{{ $vipRoleID := 1394846017396015181 }}
{{ $removeVIPCCID := 2 }}
{{ $animCCID := 3 }}

{{ $isAdmin := or (eq .User.ID $adminID) (hasPermissions .Permissions.Administrator) }}

{{ $subcommand := "" }}
{{ if gt (len .Args) 1 }}
	{{ $subcommand = index .Args 1 | lower }}
{{ end }}

{{ if eq $subcommand "chances" }}
	{{ $chancesEmbed := cembed
		"title" "🎮 GameCentral Loot Drop Rates:"
		"description" "👑 **1% (Legendary)** — 1 Month Nitro\n💜 **9% (Epic)** — VIP Role (7 Days)\n🎨 **15% (Rare)** — World Boss Game boosts (ATK + DEF)\n🪙 **25% (Uncommon)** — 5000 GC Coins\n◽ **50% (Common)** — Better luck next drop!"
		"color" 0x5865F2
	}}
	{{ sendMessage nil (complexMessage "content" .User.Mention "embed" $chancesEmbed) }}
{{ else }}
	{{ $cd := dbGet .User.ID "mysterybox_cooldown" }}
	
	{{ if and $cd (not $isAdmin) }}
⏳ **GameCentral Vault Recharging!** Try opening another Loot Drop <t:{{ $cd.ExpiresAt.Unix }}:R>.
	{{ else }}
		{{ if not $isAdmin }}
			{{ dbSetExpire .User.ID "mysterybox_cooldown" "active" $cooldownSecs }}
		{{ end }}

		{{ $loadingEmbed := cembed
			"title" "🎮 GameCentral Vault Opening..."
			"description" "▓▓░░░░░░░░ 20%\n📡 *Connecting to GameCentral Mainframe...*"
			"color" 0x3498DB
			"footer" (sdict "text" "GameCentral Supply Drop • Unlocking...")
			"timestamp" currentTime
		}}
		
		{{ $msgID := sendMessageRetID nil (complexMessage "content" .User.Mention "embed" $loadingEmbed) }}
		{{ execCC $animCCID nil 2 (sdict "msgID" $msgID "userID" .User.ID "channelID" .Channel.ID "step" 1) }}
	{{ end }}
{{ end }}