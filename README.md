# HardcoreLife

A comprehensive hardcore survival plugin featuring grace periods, death management, item limiting, and customizable gameplay mechanics for ultimate survival challenges.

## Features

- 🕐 **Grace Period System** - New players receive a grace period where they can die without permanent consequences
- ⚰️ **Hardcore Death Management** - Players who die outside grace period are spectated and banned
- 🪽 **Elytra Prevention** - Block elytras from the game for balanced hardcore gameplay
- 📊 **Item Limiting** - Set maximum limits for specific items per player
- 💾 **Database Storage** - All grace times and player data persist across server restarts
- 🎨 **ItemsAdder Integration** - Support for custom death titles and chat messages
- ⚠️ **Warning System** - Automated warnings when grace time is running out

## Commands

### `/grace [add/remove/set] [player] [minutes]`
**Aliases:** `/gracetime`, `/gr`  
**Permission:** `hardcorelife.grace` (default: all players), `hardcorelife.grace.others` (manage others)

Manage grace time for yourself or other players.

**Examples:**
```
/grace                           - Check your remaining grace time
/grace add Steve 30              - Add 30 minutes to Steve's grace
/grace remove Steve 15           - Remove 15 minutes from Steve's grace
/grace set Steve 120             - Set Steve's grace to exactly 120 minutes
```

---

### `/revive <player>`
**Permission:** `hardcorelife.revive` (default: op)

Revive a dead player and restore them to survival mode.

**Examples:**
```
/revive Steve                    - Revive Steve and teleport him to spawn
```

---

### `/hardcore <setspawn|reload>`
**Aliases:** `/hc`, `/hardcorelife`  
**Permission:** `hardcorelife.admin` (default: op)

Main administrative command for HardcoreLife.

**Subcommands:**
- `setspawn` - Set the respawn location for revived players
- `reload` - Reload all plugin configurations

**Examples:**
```
/hardcore setspawn               - Set current location as spawn
/hardcore reload                 - Reload config files
```

## Permissions

| Permission | Description | Default |
|------------|-------------|---------|
| `hardcorelife.grace` | Use `/grace` command | All players |
| `hardcorelife.grace.others` | Manage other players' grace time | OP |
| `hardcorelife.revive` | Revive dead players | OP |
| `hardcorelife.admin` | Use admin commands | OP |
| `hardcorelife.bypass` | Bypass all hardcore restrictions | False |

## Configuration

### config.yml

```yaml
# Grace period settings
grace:
  # Initial grace time in seconds (7200 = 2 hours)
  initial-time: 7200
  
  warnings:
    hour-warning:
      enabled: true
    thirty-minute-warning:
      enabled: true
    ten-minute-warning:
      enabled: true
    five-minute-warning:
      enabled: true
    one-minute-warning:
      enabled: true
    countdown:
      enabled: true
    sound:
      enabled: true
      type: "ENTITY_EXPERIENCE_ORB_PICKUP"

# Death system settings
death:
  # Time before ban in seconds (300 = 5 minutes)
  spectator-time: 300
  ban-reason: "Deathban HARDCORE"
  
  sound:
    enabled: true
    type: "ENTITY_LIGHTNING_BOLT_THUNDER"

# Death titles with ItemsAdder support
death-titles:
  itemsadder:
    enabled: true
    titles:
      - "death_skull"
      - "death_cross"
      - "death_warning"

# Elytra prevention
elytra:
  prevent-usage: true

# Auto-save interval in minutes
auto-save-interval: 5

# Debug mode
debug: false
```

---

### itemlimit.yml

Configure maximum item limits per player.

```yaml
# Item Limiting System
# Set maximum amounts for specific items
# Format: MATERIAL_NAME: limit

limits:
  GOLDEN_APPLE: 16
  ENCHANTED_GOLDEN_APPLE: 3
  TOTEM_OF_UNDYING: 1
  ENDER_PEARL: 16
  TNT: 64
```

**Adding More Limits:**
1. Find the [Minecraft material name](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Material.html)
2. Add a new line: `MATERIAL_NAME: limit`
3. Run `/hardcore reload`

---

### messages.yml

All messages support legacy color codes (`&a`, `&c`, `&e`, etc.) and placeholders.

**Common Placeholders:**
- `{player}` - Player name
- `{time}` - Time amount
- `{seconds}` - Seconds remaining
- `{limit}` - Item limit
- `{item}` - Item name
- `{world}`, `{x}`, `{y}`, `{z}` - Location data

---

### deathmessages.yml

Customize death messages with ItemsAdder support.

```yaml
# Death Messages Configuration
# Support for ItemsAdder custom font images

messages:
  # General death message
  - "&4&l☠ &c{player} &7has fallen in hardcore mode! &4&l☠"
  
  # With ItemsAdder custom images
  - ":death_skull: &c{player} &7has perished :death_skull:"
  - ":offset_-8::death_title: &c{player}"
```

## ItemsAdder Setup

### 1. Install ItemsAdder
Download and install [ItemsAdder](https://www.spigotmc.org/resources/73355/) on your server.

### 2. Create Custom Death Titles

Create a file in `plugins/ItemsAdder/data/resource_pack/assets/YOUR_NAMESPACE/font/death_titles.yml`:

```yaml
info:
  namespace: hardcorelife

font_images:
  death_skull:
    path: "font/death/skull.png"
    y_position: 0
    scale_ratio: 1.0
    symbol: "☠"
  
  death_cross:
    path: "font/death/cross.png"
    y_position: 0
    scale_ratio: 1.0
    symbol: "✖"
  
  death_warning:
    path: "font/death/warning.png"
    y_position: 0
    scale_ratio: 1.0
    symbol: "⚠"
  
  death_title:
    path: "font/death/death_title.png"
    y_position: -20
    scale_ratio: 2.0
    symbol: "💀"
```

### 3. Add Custom Images

Place your custom PNG images in:
```
plugins/ItemsAdder/data/resource_pack/assets/hardcorelife/textures/font/death/
```

**Image Requirements:**
- Format: PNG with transparency
- Recommended size: 256x256 pixels for titles
- Keep file sizes small for better performance

### 4. Configure HardcoreLife

Edit `config.yml` to enable ItemsAdder titles:

```yaml
death-titles:
  itemsadder:
    enabled: true
    titles:
      - "death_skull"
      - "death_cross"
      - "death_warning"
      - "death_title"
```

Edit `deathmessages.yml` to use custom titles:

```yaml
messages:
  # Using ItemsAdder font images
  - ":death_skull: &4&lHARDCORE DEATH &4&l:death_skull:"
  - ":offset_-8::death_title:"
  - "&c{player} &7has died permanently!"
  
  # Mix regular text with custom images
  - ":death_warning: &c{player} &7is no more :death_warning:"
  
  # Large centered title
  - ":offset_-100::death_title:"
  - "&c{player}"
```

### 5. Use in Chat Messages

You can also use ItemsAdder titles in `messages.yml`:

```yaml
grace:
  expired: ":death_warning: &4&lYour grace period has expired! :death_warning:"
  
death:
  hardcore-death: ":death_skull: &4&lYou have died outside of grace! :death_skull:"
```

### 6. Regenerate Resource Pack

After making changes:
```
/iazip
```

Then distribute the resource pack to your players.

## How It Works

### Grace Period System
1. New players receive initial grace time (default: 2 hours)
2. Grace time counts down while players are online
3. Players receive warnings as grace time runs out:
   - 1 hour remaining
   - 30 minutes remaining
   - 10 minutes remaining
   - 5 minutes remaining
   - 1 minute remaining
   - 10-second countdown
4. When grace expires, next death becomes permanent

### Death Mechanics
- **Death within grace:** Player respawns at safe location, no penalty
- **Death outside grace:** 
  1. Player enters spectator mode
  2. Server-wide death message broadcast
  3. After 5 minutes (configurable), player is banned
  4. Only admins can revive with `/revive`

### Database Storage
All grace times and player data are stored in SQLite database (`plugins/HardcoreLife/data.db`), ensuring no data loss during:
- Server restarts
- Plugin updates
- Unexpected crashes

## Installation

1. Download `HardcoreLife.jar`
2. Place in your `plugins/` folder
3. (Optional) Install [ItemsAdder](https://www.spigotmc.org/resources/73355/) for custom titles
4. Restart your server
5. Configure `config.yml`, `messages.yml`, `itemlimit.yml`, and `deathmessages.yml`
6. Set spawn location with `/hardcore setspawn`
7. Reload with `/hardcore reload`

## Requirements

- **Minecraft:** 1.21.1+
- **Server:** Paper, Spigot, or Purpur
- **Java:** 21+
- **Optional:** ItemsAdder for custom font images

## Support & Issues

If you encounter any issues or have suggestions, please create an issue on GitHub.

---

**Author:** Dutchwilco  
**Version:** 1.0.0  
**License:** All Rights Reserved
