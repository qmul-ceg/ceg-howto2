## Process info
```SQL
SELECT
spid
,kpid
,login_time
,last_batch
,status
,hostname
,nt_username
,loginame
,cmd
,hostprocess
,cpu
,memusage
,physical_io
,dbid
,uid
FROM sys.sysprocesses
```

## Kill process
```SQL
EXEC sp_who2 -- show all processes

KILL <spid> -- KILL process <spid>

KILL <spid> with STATUSONLY
```

https://www.sqlshack.com/kill-spid-command-in-sql-server/