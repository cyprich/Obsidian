# NixOS  

Installing, configuring and whatever more of NixOS  

## Basics

NixOS is Declarative  

|                                | Declarative                                                                                     | Imperative                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------- | ------------------------------------ |
| OS example                     | NixOS                                                                                           | Ubuntu                               |
| Paradigm                       | "What needs to be done"                                                                         | "How to do it"                       |
| Example of installing software | */etc/nixos/configuration.nix*<br>`environment.systemPackages = with pkgs; [neofetch vim htop]` | `sudo apt install neofetch vim htop` |

## Installation

Installation is done pretty easily with graphical install  
You can also follow [official documentation](https://nixos.org/manual/nixos/stable/#sec-installation-manual) for manual installation  

There are some stuff that might help  

### Generate NixOS config

```bash
nixos-generate-config --root /mnt
```

### Modify NixOS config  

```bash
vim /mnt/etc/nixos/configuration.nix
```

Uncomment or modify or add these lines  
Don't forget semicolons (`;`) at the end  

```nix
networking.hostName = "nixos";

networking.wireless.enable = true;  # if you use wifi
networking.networkmanager.enable = true;  

time.timeZone = "Europe/Bratislava";

services.xserver = {
	enable = true;
	windowManager.qtile.enable = true;  # qtile window manager
};

users.users.peter = {
	isNormalUser = true;
	extraGroups = [ "wheel" ];  # enable sudo
	packages = with pkgs; [
		tree
	];
};

programs.firefox.enable;

environment.systemPackages = with pkgs; [
	vim
	neovim
	wget
	btop
]
```

> Note: to search packages, you can go to [search.nixos.org](https://search.nixos.org/packages)  

## Usage

You can do configuration in `/etc/nixos/configuration.nix`   

```bash
vim /etc/nixos/configuration.nix
```

After any configuration is done, you need to rebuild it to apply changes with this command  

```bash
sudo nixos-rebuild switch
```

### Installing programs

You can add packages like this  

```nix
environment.systemPackages = with pkgs; [
	# you current packages 
	btop
];
```

Then rebuild (`nixos-rebuild switch`)  

