### run filer
```bash
cd /data/filer_gp
sudo nohup sudo ./run_filer.sh &
```
nohup: no hangups, ie keep running
&: run in background. Terminal output to nohup.out
```bash
sudo tail -f nohup.out
```
tail: last 10 lines of text from file
\-f reload as file grows.

To truncate nohup.out file after manual filer run.
```bash
cd /data/filer_gp   #cd to filer directory
sudo cp /dev/null nohup.out    #copy null into nohup.out
```
alternative is to use > but this requires rw and does not take `sudo`. So need to use elavated user.
```bash
sudo su    #change to root user
>nohup.out   #copy nothing into nohup.out 
```
  
