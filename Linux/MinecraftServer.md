# Minecraft Server

Step to configure Minecraft Server on Ubuntu Server OS

## Prerequisites

### Java

Make sure Java is installed by typing `java --version`  
If it's not, install it with `sudo apt install openjdk-21-jdk-headless`

### `server.jar`

Command to download stuff: `wget <link>`  
Download `server.jar` from [this link](https://piston-data.mojang.com/v1/objects/450698d1863ab5180c25d7c804ef0fe6369dd1ba/server.jar)`

> If link doesn't work, see [this tutorial](https://www.minecraft.net/en-us/download/server)

## Running server

The problem with running the server is that by default if you close your terminal, the server shuts down  
There are two ways of solving this problem

### `screen`

#### Create a screen session

`screen -S <name>` for example `screen -S minecraft`  
Now you are in a screen session  
If you run the server inside screen session, it doesn't shut down when you leave  
You can run the server with following command: `java -Xmx7G -Xms7G -jar server.jar`

#### Exiting a screen session

`ctrl+a` and `ctrl+d`

#### Listing screen sessions

`screen -ls`

#### Rejoin screen session

`screen -r <name>` for example `screen -r minecraft`

#### Delete screen session

`sreen -XS <name> quit` for example `screen -XS minecraft quit`

### `nohup`

You just run this command: `nohup java -Xmx7G -Xms7G -jar server.jar nogui &`  
While this method is easier, you can not go back to the server

## Connecting to the server

### LAN

With `ip add` command, you can see an IP Address of your server

### WAN

#### Port forwarding

Add Port Forwarding option on your router  
It could look something like this

| Service Name     | Source Target | Port Range | Local IP      | Local Port | Protocol           |
| ---------------- | ------------- | ---------- | ------------- | ---------- | ------------------ |
| Minecraft Server |               | 25565      | 192.168.1.231 | 25565      | BOTH (TCP and UDP) |

You can find your Public IP Address on your router settings, or via sites like [whatsmyipaddress.com](https://whatismyipaddress.com/)

#### Playit

If you can't open a port for some reason, check out [Playit tutorial](./Playit.md)

## Update server

If you want to update server to newer version, it's as simple as this

1. Download new `server.jar` file from [official Minecraft website](https://piston-data.mojang.com/v1/objects/45810d238246d90e811d896f87b14695b7fb6839/server.jar)
2. Stop the server
3. _(Optional, recommended)_ create a backup of the server
4. Replace old `server.jar` with newly downloaded `server.jar`
5. Run the server

After running the server for the first time after update, you will see some extra info

## Plugins

Some notes on Paper plugins, I'm kinda new to this

### Worlds with `Multiverse`

Required plugins: `Multiverse`

Create worlds

```bash
mv create <name> <environment> [--seed <seed> --generator <generator[:id]> --world-type <worldtype> --adjust-spawn --no-structures --biome <biome>]

mv create lobby normal --world-type flat
mv create survival normal

# if you already have existing one
mv import <name> <environment>
```

Setting spawn point

```bash
mv setspawn [worldname:x,y,z[,pitch,yaw]] [--unsafe]

mv setspawn lobby:0,-60,0
```

Moving between worlds

```bash
mv tp <player> <destination>
mv tp YungCypo lobby
mv tp YungCypo survival
```

Additional config

```bash
mv modify <world> set difficulty peaceful
mv modify <world> set hunger false

mv entity-spawn-config modify lobby monster set spawn false
```

### Regions

Required plugins: `WorldEdit`, `WorldGuard`

If you have multiple worlds, select one

> Note: The symbol `//` is not comment, and is required for the command

```bash
//world <name>

# to reset, leave empty
//world
```

Define position/area

```bash
//pos1 a,b,c
//pos2 x,y,z

# or in one command
//pos a,b,c x,y,z


# to reset, leave empty
//pos1
//pos2
//pos
```

Define region based on area

```bash
rg define <name>

rg define spawn
```

List regions

```bash
rg list
```

Restrict stuff in region

```bash
rg flag <name> build deny
rg flag <name> pvp deny
rg flag <name> mob-spawning deny
rg flag <name> chest-access deny
rg flag <name> use deny
rg flag <name> item-drop deny
rg flag <name> item-pickup deny
rg flag <name> food deny


Available flags: allowed-cmds, block-break, block-place, block-trampling, blocked-cmds, breeze-charge-explosion, build, chest-access, chorus-fruit-teleport, copper-fade, coral-fade, creeper-explosion, crop-growth, damage-animals, deny-message, deny-spawn, enderdragon-block-damage, enderman-grief, enderpearl, entity-item-frame-destroy, entity-painting-destroy, entry, entry-deny-message, exit, exit-deny-message, exit-override, exit-via-teleport, exp-drops, fall-damage, farewell, farewell-title, feed-amount, feed-delay, feed-max-hunger, feed-min-hunger, fire-spread, firework-damage, frosted-ice-form, frosted-ice-melt, game-mode, ghast-fireball, grass-growth, greeting, greeting-title, heal-amount, heal-delay, heal-max-health, heal-min-health, ice-form, ice-melt, interact, invincible, item-drop, item-frame-rotation, item-pickup, lava-fire, lava-flow, leaf-decay, lighter, lightning, mob-damage, mob-spawning, moisture-change, mushroom-growth, mycelium-spread, natural-health-regen, natural-hunger-drain, nonplayer-protection-domains, notify-enter, notify-leave, other-explosion, passthrough, pistons, potion-splash, pvp, ravager-grief, receive-chat, respawn-anchors, ride, rock-growth, sculk-growth, send-chat, sleep, snow-fall, snow-melt, snowman-trails, soil-dry, spawn, teleport, teleport-message, time-lock, tnt, use, use-anvil, use-dripleaf, vehicle-destroy, vehicle-place, vine-growth, water-flow, weather-lock, wind-charge-burst, wither-damage,
```

## Authentication with `AuthMe`

Required plugins: `AuthMe`

### When logged in as player

Register (first time only)

```bash
register <password> <confirmPassword>
```

Login (every time you log in to server)

```bash
login <password>
```

Change password

```bash
changepassword <oldPassword> <newPassword>
```

### When logged in server

Register player

```bash
register <username> <password>
```

You might also want to change this in `config.yml`

```yml
AllowUnregisteredLogin: false
kickNonRegistered: true
ForceSingleSession: true
```
