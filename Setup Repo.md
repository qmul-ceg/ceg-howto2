## Create a local git repo
##### Git Bash
## Navigate to local folder holding the repo files
```shell
cd ~/github/myproject
```
### *OR create a folder for the repo files*
```bash
cd ~/github
mkdir myproject
cd myproject
```
## Initialize a local git repository (repo)
(create a `.git` folder) in the root of the folder, with a 'main' branch
```shell
git init -b main
```
### *If git has been initialised as 'master'*
```shell
git branch -m master main
```
## Add all the existing files to the local repo + Commit them.
```
git add .
git commit -m "setup repo"
```
##### Github.com
## Create a github repository (repo)
Navigate to 'ceg-qmul' - click New
Add the repo name and description - create
##### Git Bash
## Add the github repo to the git repo
Add a remote repo with the name `origin`, found at SSH `git@github.com:ceg-qmul/ELDB2021.git` 
```shell
git remote add origin git@github.com:ceg-qmul/ELDB2021.git
```
## Ensure that the branch is called 'main'
Move & Force the branch 'main' (optional as it was init as main)
```
git branch -M main
```
## Push files to github
Push the committed files to the `main` branch of `origin` (github repo), and set this as the default upstream (tracking) for this branch
```bash
git push -u origin main
```
output:
```bash
Enumerating objects: 79, done.
Counting objects: 100% (79/79), done.
Delta compression using up to 8 threads
Compressing objects: 100% (79/79), done.
Writing objects: 100% (79/79), 129.17 KiB | 1.42 MiB/s, done.
Total 79 (delta 8), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (8/8), done.
To github.com:ceg-qmul/ELDB2022.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```









Need to have Github CLI to publish a repository from the terminal.  The standard method is to create a repo in github.com and then sync the local repo with git [[./Sync Repo|Sync Repo]] . The simpler option is to use Github Desktop.
## Github Desktop
- dropdown Current Repository >> Add >> Add existing repository >> Choose... >> {repo folder} >> Add Repository
	- navigate to added repo
	- Publish Repository
		- Select organization 
		- Publish
