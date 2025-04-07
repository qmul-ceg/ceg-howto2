### switch to root user
```bash
$ sudo su
# and back again
$ su ubuntu
```


# adduser
high-level interactive utility - creates username + usergroup + `/home`, requests password input
```
sudo adduser <options> <username> <group>
```
- `--disabled-password` = login only by SSH key.
- `--gecos "" ` = set GECOS info as "" - won't be prompted for finger info.

create sudo user by adding user to sudo group
```
sudo adduser danield
sudo adduser danield sudo
```

check user details
```
sudo id danield
```

# passwd
modify user password
```
sudo passwd <options> <username>
```
- `-d` = *delete* - delete password (make it empty), sets the account as passwordless.
- `-e` = *expire* - immediately expire password. This in effect can force a user to change their password at the user's next login.
- `-l` = *lock* - locks password (user may still be able to log in using an SSH key)
- `-u` = *unlock* - unlocks password - returns it to previous value
- `-S` = *Status* - displays
	1. username
	2. password status - locked (L), no password (NP), usable password (P)
	3. date of last password change
	4. minimum age (days) allowed for password
	5. maximum age (days) allowed for password
	6. warning period (days) 
	7. inactivity period (days) 

Check user password
```
sudo chage -l danield
```

# useradd + usermod
low-level utilities to create and modify user account - needs flags or additional commands to create `/home`, password, shell etc
```bash
sudo useradd <options> <username>
sudo usermod <options> <username>
```
- `-r` = *raised system access* - account has `sudo` access
- `-m` = *make home* - creates a `/home/<username>` directory
- `-d` = *directory for home* - specifics a directory to use as home
- `-g <groupname>` = *primary group* - add to specified primary group
- `-G <groupname>,<groupname>` = *secondary groups* - add to specified groups `-aG` for usermod
- `-e <yyyy-mm-dd>` = *expiry* - user account expires on specified date. `-e 1` = *expiry 02/01/1970* - disables account
- `-E` = *RegEx* - use RegEx pattern searching
- `-a` = *append* - in usermod - add to user account

