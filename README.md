# 🎮 YAGPDB GameCentral Loot Drop System

An advanced, fully-automated **Loot Drop / Mystery Box** system for Discord servers using **YAGPDB Custom Commands**.

---

## ✨ Features

- ⏳ **5-Stage Animated Unlocking**: Visual progress bar updating in real time (20% ➔ 40% ➔ 60% ➔ 80% ➔ 100%).
- 🎲 **Custom Drop Rates**:
  - 👑 **1% (Legendary)**: 1 Month Nitro
  - 💜 **9% (Epic)**: VIP Role (7 Days)
  - 🎨 **15% (Rare)**: World Boss Game boosts (ATK + DEF)
  - 🪙 **25% (Uncommon)**: 5,000 Coins
  - ◽ **50% (Common)**: Better luck next drop
- ⏱️ **24-Hour Cooldown**: Dynamic relative Discord timestamp display (`<t:...:R>`).
- 🛡️ **Admin Bypass**: Server Administrators & designated Admins ignore cooldowns for testing.
- 🎟️ **Automated VIP Expiry**: Automatically grants VIP role upon winning and removes it exactly after 7 days using scheduled custom commands.
- 📢 **Staff Alerts**: Automatically posts win alerts in a staff channel with guaranteed role mentions (`allowed_mentions`).
- 📊 **Chance Checker**: Command `!mysterybox chances` to view drop rates.

---

## ⚙️ Configuration Variables
Make sure to adjust the following variables at the top of CC #2 and CC #3:
- `$staffChannelID:` Channel ID for staff notifications.
- `$staffRoleID:` Role ID to ping staff members upon a win.
- `$vipRoleID:` Role ID assigned to VIP winners.
- `$adminID:` Specific Admin User ID for testing without cooldowns.
- `$removeVIPCCID:` ID of CC #2.
- `$animCCID:` ID of CC #3.
- `** MAKE SURE TO CHANGE ALL TEXTS FROM GAMECENTRAL TO YOUR SERVER **` 
