Sync with only selected folders from a repo
```bash
git remote add -f origin git@github.com:qmul-ceg/{repo}
git sparse-checkout set {folder/subfolder}
git sparse-checkout add {folder2/subfolder2}
git sparse-checkout list
# folders in checkout
git pull origin main
```
If needed set permissions for new folder
```
chmod -R 774 ~/eldb2024_data/BUILD_DATA
```

```
cd {git repo folder}
touch .gitignore
echo '.env' > .gitignore
```