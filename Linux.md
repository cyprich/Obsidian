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
| -C  | *-A* and *-B* combined                                                    |


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

