---
share: "true"
---

## Move to the Git repo folder
```bash
cd ~/Github/ELDB2023
# cd C:/Users/Kelvin/Github/ELDB2023
```
## Check files needing action
```shell
git status
# Changes and Untracked files will be listed
```
## Update local repo from Github
```shell
git pull
# info on update
```
## Add files to the staging environment
```shell
git add --all
# OR
git add -A
```
or to select a single file
```shell
git add <file>
```
## Commit changes to repo with a message
```shell
git commit -m "commit message"
```
The message at the end of the commit should be something related to what the commit contains.
## Push the local repo to the Github repo
```
git push origin 
```

