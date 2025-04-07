 ### navigation
```bash
cd OR cd ~ OR cd ~/   #change directory to home ()
cd /    #change directory to root
cd /data/filer_gp   #change directory to /data/filer_gp
cd data    #change to data directory within current directory
cd ./     # change to current directory
cd ../    # change to parent directory
cd -    # change to previous directory

ls -l   #list content of current folder with info
ls /var #list content of /var folder
ls -lR  #list content of current folder + subfolders with info 
ls -lS  #list content of current folder by size with info
```

### file permissions
```bash
sudo chmod <attribute> <file>
``` 
*rwx* permissions for *file owner group other* = -rwxrwxrwx

***numeric method***
 Read = 4,  Write = 2,  Execute = 1
 add up values for each user
 ```bash
sudo chmod 660 example.sh   #sets rw to file owner and group
sudo chmod 160 example.sh   #sets x to file owner and rw to group
sudo chmod 777 example.sh   #sets rwx to all
``` 

***symbolic method***
'u', 'g', 'o' and 'a' = file owner (user), group, other users, and all users
'+' and '-' = add or remove attributes
'r', 'w' and 'x' = read, write, and execute
```bash
sudo chmod g+rw example.sh   #adds rw to group
sudo chmod a-x example.sh    #removes execute for all users
``` 

https://chmod-calculator.com
https://ss64.com/bash/chmod.html