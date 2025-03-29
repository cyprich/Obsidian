# Linux

Linux commands that I keep forgetting :)

# Compressing and archiving

## Compressing

```shell
tar -czf newfile.tar.gz oldfiles
```

## Extracting

```shell
tar -xzf file.tar.gz
```

## Listing

Shows the file size before and after compressing

```shell
tar -l file.tar.gz
```

## Extra compression

Requires additional package(s) but has a lot better compression ratio

```shell
7z a -t7z -m0=lzma2 -mx=9 -md=64m output.7z input
```

```shell
zstd -19 --ultra input -o output.zst
```

# Working with text

## Cut

```shell
cut -d: -f1,5-7 /etc/passwd
ls -l | cut -c1-11,50- > output.txt
```

|     | Description |
| --- | ----------- |
| -d  | Delimiter   |
| -f  | Field       |
| -c  | Characters  |

## Sort

```shell
sort -t ":" -k "3" -n -r /etc/passwd
sort -t, -k2 -k1 -k3n file.csv
```

|     | Description  |
| --- | ------------ |
| -t  | Delimiter    |
| -k  | Field number |
| -n  | Numeric sort |
| -r  | Reverse sort |

## Grep

```shell
grep "bash" /etc/passwd
grep -A3 -B3 "ahoj" file.txt
```

|     | Description                                                               |
| --- | ------------------------------------------------------------------------- |
| -c  | Counts the number of occurences                                           |
| -n  | Shows the number of line of occurence                                     |
| -v  | **Inverts** search, showing lines without the word                        |
| -w  | Whole word - if searched word is part of another word, it will be ignored |
| -q  | Quiet - returns 1 if it was found, return 0 if it wasn't found            |
| -A  | After - shows result along with 3 lines after result                      |
| -B  | Before - shows result along with 3 lines before result                    |
| -C  | _-A_ and _-B_ combined                                                    |

# Users

## Adding

```shell
useradd
```

|     | Description                                                    |
| --- | -------------------------------------------------------------- |
| -D  | Shows default values for creating new users                    |
| -u  | User ID (UID)                                                  |
| -g  | Primary group (name or GID)                                    |
| -G  | Supplementary group                                            |
| -m  | Make new home directory                                        |
| -b  | Base directory, under which will be the home directory created |
| -d  | Set home directory, either new or already existing             |
| -s  | Default shell                                                  |
| -c  | Comment                                                        |

For example...  
`useradd -mb /test ferko` - user named `ferko` with home directory `/test/ferko`  
`useradd -u 1009 -g users -G sales,research -m -c 'i really dont know much about this user...' ferko`

## Changing User Password

User doesn't have password by default  
`passwd jozko` - more interactive way to set password
`echo "ferko:newPassword | sudo chpasswd` - more suitable for scripts

## Modifying User

```shell
usermod
```

|     | Description             |
| --- | ----------------------- |
| -c  | Comment                 |
| -d  | Home directory          |
| -g  | Primary group           |
| -G  | Supplementary group     |
| -aG | Add supplementary group |
| -l  | Change login            |
| -s  | Change shell            |
| -u  | Change UID              |
| -L  | Lock account            |
| -U  | Unlock account          |

## Deleting user

```shell
userdel jozko
```

|     | Description                            |
| --- | -------------------------------------- |
| -r  | Delete user along with his home folder |

Filles that belong to user and are not in home folder will become **orphaned**

# Groups

## Creating group

```shell
groupadd -g 1006 name
groupadd -r name
```

|     | Description                    |
| --- | ------------------------------ |
| -g  | Group ID (GID) of new group    |
| -r  | Assign GID from reserved space |

If GID is not provided while creating new group, the system will assign the next available GID, for example 1007

## Modifying group

```shell
groupmod -n oldName newName
groupmod -g 1008 name
```

|     | Description              |
| --- | ------------------------ |
| -n  | Change name of the group |
| -g  | Change GID\*             |

\*When changing GID, files that belong to the group will become **orphaned**!

## Deleting group

```shell
groupdel name
```

When removing group, files that belong to the group will become **orphaned**!

## Group info

| Command               | Description                                |
| --------------------- | ------------------------------------------ |
| `cat /etc/group`      | Shows all groups on system                 |
| `getent group <name>` | Shows info about group with given \<name\> |
| `find / --nogroup`    | Shows orphaned files                       |

# Ownership and permissions

Change group

```shell
chgrp group file
```

Change ownership

```shell
chown user file
chown user:group file
chown :group file
```

Change permissions

```shell
chmod permissions file
```

# System log

```shell
journalctl -xe
```

# Network

These commands are for Ubuntu (not sure about others)

## Basic commands

```bash
ping <ip | domain>  # ping
traceroute <ip | domain>  # traceroute
tracepath <ip | domain>  # traceroute
nslookup <ip | domain>  # nslookup

ip a  # show ip addresses + more
ip r  # show default route
```

## WiFi

```bash
nmcli d  # list devices
nmcli r wifi on  # turn on wifi adapter
nmcli d wifi on  # list available networks
nmcli d wifi connect <ssid> password <password>  # connects to wifi. replace <ssid> and <password> with the actual values

nmcli r wifi off  # disable wifi completely
```

### Release/Renew IP address

```bash
# you might need to install this package
sudo apt install isc-dhcp-client

# release and renew
sudo dhclient -r
sudo dhclient

# chatgpt said it was suppposed to be like this, but for me it worked anyway
sudo dhclient -r wlp2s0
sudo dhclient wlp2s0

# check with this command
ip a
```

## Local DNS

Add DNS entry for your PC
You need to edit `/etc/hosts` file and add like something like this

```
192.168.88.3   www.segedin.sk
```

> On Windows it's located in `C:\Windows\System32\drivers\etc\hosts`

## Set static IP

You need to edit `/etc/netplan/50-cloud-init.yaml` or something similar, so it looks something like this

```bash
network:
 version: 2
 renderer: NetworkManager
 ethernets:
   eth0:
     dhcp4: no
     addresses: [172.23.207.254/20]
     gateway4: 192.168.1.1
     nameservers:
         addresses: [8.8.8.8,8.8.8.4]
```

# Grub

Rebuild grub or something?  
Not really what it actually does  
When Grub does not behave as it supposed to, try this :)

```
sudo grub-install --terget=x86_64-efi --efi-directory=/boot/efi --bootloader-id=EndeavourOS
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

# Various

Commands that do not deserve full section, but deserves to be there

`hostnamectl` - find info about OS and more

`lspci`

Disable sleep on lid close

```bash
# open this file
sudo nvim /etc/systemd/logind.conf

# write these lines
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
# save and quit

# restart this process
sudo systemctl restart systemd-logind
```

```bash
# stress testing
sudo apt install stress
stress --cpu 8 --io 4 --vm 4 --vm-bytes 1024M --timeout 10s
```

## Fingerprint

You can log in into your device with fingerprint  
First of all, make sure you have physical fingerprint sensor  
Make sure you have installed `fprintd` package (`yay -S fprintd`)  
To scan finger, use the command `fprintd-enroll`  
Follow instructions on screen and you should be ready to go

# Cool programs

Mostly useless programs, just for fun

## cowsay

Cow says whatever you told it to

```text
______
< ahoj >
 ------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

More characters can be specified with `-f`  
Cow files are located at `/usr/share/cow`

### xcowsay

Some kind of graphic alternative to `cowsay`

### ponysay

Alternative to `cowsay`, displays My Little Pony character

### charasay

Alternative to `cowsay`, displays various characters

## btop

Alternative to `top`, but a lot better

## neofetch

Shows info about your system

```
                     ./o.                  cyprich@HP-Victus
                   ./sssso-                -----------------
                 `:osssssss+-              OS: EndeavourOS Linux x86_64
               `:+sssssssssso/.            Host: Victus by HP Gaming Laptop 16-s0xxx
             `-/ossssssssssssso/.          Kernel: 6.9.9-arch1-1
           `-/+sssssssssssssssso+:`        Uptime: 14 mins
         `-:/+sssssssssssssssssso+/.       Packages: 1008 (pacman)
       `.://osssssssssssssssssssso++-      Shell: zsh 5.9
      .://+ssssssssssssssssssssssso++:     Resolution: 1920x1080
    .:///ossssssssssssssssssssssssso++:    DE: GNOME 46.3.1
  `:////ssssssssssssssssssssssssssso+++.   WM: Mutter
`-////+ssssssssssssssssssssssssssso++++-   WM Theme: Adwaita
 `..-+oosssssssssssssssssssssssso+++++/`   Theme: Adwaita [GTK2/3]
   ./++++++++++++++++++++++++++++++/:.     Icons: Adwaita [GTK2/3]
  `:::::::::::::::::::::::::------``       Terminal: kgx
                                           CPU: AMD Ryzen 7 7840HS w/ Radeon 780M Graphics (16) @ 5.137GHz
                                           GPU: NVIDIA GeForce RTX 4060 Max-Q / Mobile
                                           GPU: AMD ATI 05:00.0 Phoenix1
                                           Memory: 2305MiB / 15265MiB



```

### fastfetch

Alternative to `neofetch`, shows more info

### onefetch

Similar to `neofetch`, but it's made for Git Repositories

## cmatrix

## figlet

Displays given text in this fancy-pants-ahh style

```
       _           _
  __ _| |__   ___ (_)
 / _` | '_ \ / _ \| |
| (_| | | | | (_) | |
 \__,_|_| |_|\___// |
                |__/

```

With `-f` you can specify 'font'  
Fonts are located at `/usr/share/figlet/fonts`

## sl

Steam locomotive  
Displays a train on your screen, when you mistype `ls`

## cbonsai

Shows bonsai tree  
`-l` makes it live grow

## pipes.sh

Nice-looking screensaver  
`yay -S pipes.sh`

## asciiquarium

Ascii aquarium
`yay -S asciiquarium`
