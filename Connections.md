# connection info
## Server Information
```SQL
SELECT  
   CONNECTIONPROPERTY('net_transport') AS net_transport,
   CONNECTIONPROPERTY('protocol_type') AS protocol_type,
   CONNECTIONPROPERTY('auth_scheme') AS auth_scheme,
   CONNECTIONPROPERTY('local_net_address') AS local_net_address,
   CONNECTIONPROPERTY('local_tcp_port') AS local_tcp_port,
   CONNECTIONPROPERTY('client_net_address') AS client_net_address 
    
 EXEC db_lookup.dbo.sp_WhoIsActive
 	@show_sleeping_spids = 
```

## Current Connections
```SQL
SELECT
DEC.session_id
,DES.login_time
,DES.login_name
,DES.host_name
,DES.program_name
,DES.client_interface_name
,DES.status
,DES.last_request_end_time
,DES.last_request_end_time
,DEC.protocol_type
,DEC.auth_scheme
FROM sys.dm_exec_sessions AS DES
JOIN sys.dm_exec_connections AS DEC ON DEC.session_id = DES.session_id
WHERE login_name <> 'NT AUTHORITY\SYSTEM'
AND login_name <> 'rdsa'
ORDER BY login_time;
```


## Connections - past 44 days
```SQL
SELECT
CONVERT(date, event_time, 108) as date_in
,MIN(CAST(CONVERT(char(16), event_time, 20) as time(0))) as time_in
,action_id as login_attempt
,MAX(CAST(CONVERT(char(16), event_time, 20) as time(0))) as time_out
,server_principal_name as login_name
,client_ip
FROM msdb.dbo.rds_fn_get_audit_file
('D:\rdsdbdata\SQLAudit\Audit-Logins*.sqlaudit', default, default )
WHERE server_principal_name <> 'NT AUTHORITY\SYSTEM'
GROUP BY 
	CONVERT(date, event_time, 108)
	,server_principal_name
	,action_id
	,client_ip
ORDER BY date_in DESC, time_in DESC
```

Aggregated Successful Logins
```SQL
SELECT
	login_name
	,COUNT(*) as connections
	,MAX(date_in) as most_recent_d
	,MAX(time_in) as most_recent_t
FROM (
SELECT
CONVERT(date, event_time, 108) as date_in
,MIN(CAST(CONVERT(char(16), event_time, 20) as time(0))) as time_in
,action_id as login_attempt
,MAX(CAST(CONVERT(char(16), event_time, 20) as time(0))) as time_out
,server_principal_name as login_name
,client_ip
FROM msdb.dbo.rds_fn_get_audit_file
('D:\rdsdbdata\SQLAudit\Audit-Logins*.sqlaudit', default, default )
WHERE server_principal_name <> 'NT AUTHORITY\SYSTEM'
GROUP BY 
	CONVERT(date, event_time, 108)
	,server_principal_name
	,action_id
	,client_ip
) as t
WHERE t.login_attempt = 'LGIS'
GROUP BY login_name
ORDER BY most_recent_d DESC, most_recent_t DESC
```

[https://cprovolt.wordpress.com/2013/08/02/sql-server-audit-action_id-list/](https://cprovolt.wordpress.com/2013/08/02/sql-server-audit-action_id-list/)

[https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.Audit.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.Audit.html)


http://whoisactive.com/






