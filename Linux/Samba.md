# Samba

Steps to set up Samba  
Samba is like file sharing system  
It uses the `SMB` protocol 

## Installation  

```bash
sudo apt update
sudo apt install samba samba-common samba-client
```

## Configuration

```bash
sudo nvim /etc/samba/smb.conf
```

```
[MySharedDrive]
	comment = My Server Shared Drive
	path = /mount/drive
	browseable = yes
	read only = no
	guest ok = no
	valid users = cyprich
	create mask = 0644
	directory mask = 0755
```

> Note - change `cyprich` with your actual username

Meaning of properties 

| Property          | Meaning                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------ |
| `[MySharedDrive]` | Name of the share. This is what you'll see on Windows and Linux when Browse network shares |
| `comment`         | A descriptive text for the share                                                           |
| `path`            | The actual directory on server that you want to share                                      |
| `browseable`      | Allows clients to see this share when they browse the network                              |
| `read only`       | If it's read-only. `yes` - only read, `no` - read and write                                |
| `guest ok`        | If you have to authenticate. `yes` - no auth, `no` - auth needed                           |
| `valid users`     | List of *Samba users* who have access, separated by comma. Use `@users` for all users      |
| `create mask`     | Permissions for new files created on share                                                 |
| `directory mask`  | Permissions for new directories created on share                                           |

You can add more shares by adding another section like `[FamilyShare]` or something  

If you want to create share for everyone without password, you need to change/add configuration like this  

```
	guest ok = yes
	force user = nobody
	force group = nobody
```

### Samba users

Samba use it's own database for user passwords, independently on the actual Linux system  
Meaning that users can have one password to log in to device, and another to log in to Samba  

To add user change your Samba password, enter this command  

```bash
sudo smbpasswd -a cyprich  
```

> Note - change `cyprich` with your actual username

### Adjust permissions  

Universal, less secure version  

```bash
sudo chmod -R 777 /mount/drive
```

More secure version  

```bash
sudo chown -R cyprich:cyprich /mount/drive
sudo chmod -R 755 /mount/drive
```

### Adjust firewall 

If you don't use firewall, you don't have to do this  

```bash
sudo ufw allow samba
sudo ufw enable
sudo ufw status verbose  
```

### Restart Samba service 

```bash
sudo systemctl restart smbd nmbd
```

## Connecting 

How to access the Samba drive

In file explorer, or file manager or whatever you want to name it, go to this location 
- On Windows - `\\server_ip_address`, for example `\\192.168.1.1`   
	- You should see share(s) you created earlier,  double click to open it  
- On Linux - `smb:/server_ip_address/ShareName`, for example `smb:/192.168.1.1/MySharedDrive`   

You might be asked for credentials, based on your settings  

### Permanent mounting

#### Windows

Right click on the share  
Select `Map network drive...`  
This way you can assign it a letter for easier access in the future  

#### Linux

On client machine

**Create credentials**

```bash
nvim ~/.smbcredentials
```

```txt
username=cyprich
password=samba_password
```

```bash
chmod 600 ~/.smbcredentials 
```

**Mounting**

```bash
sudo mkdir /mnt/myservershare

sudo apt install cifs-utils

# you will need your UID and GID, so remember these two numbers
id -u
id -g

sudo nvim /etc/fstab
```

```bash
# replace the uid and gid with actual values you remembered
//your_server_ip/MySharedDrive /mnt/myservershare cifs credentials=/home/cyprich/.smbcredentials,uid=1000,gid=1000,iocharset=utf-8,defaults 0 0
```