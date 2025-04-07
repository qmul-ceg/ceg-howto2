### crontab
```bash
crontab -l # list crontab
crontab -e # edit crontab
```

```bash
crontab -r # delete crontab
sudo crontab -l > crontab.save # save
more crontab.save # check saved
crontab < crontab.save #restore
```

```bash
./crond stop   # stops cron daemon for all cron jobs for ALL users.
./crond start
```