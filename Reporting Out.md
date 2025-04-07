## RAISERROR
```sql
RAISERROR({message},{severity},{state}[,{argument}]) [WITH NOWAIT]
```
- {message} = varchar()
	- can contain variables from {argument}
		- %s = string
		- %d or %i = integer
		- %I64d =  bigint
- {severity} = int 
	- 0-9 = information
	- 10 = not severe - outputs in black
	- 11+ = output in red
- {state} = int 0-255
	- user-defined. Can be used to indicate place in script
- {argument} = variables to report in message
```sql
RAISERROR('completed',10,0) WITH NOWAIT

RAISERROR('completed %s %d',10,0,'this',8) WITH NOWAIT
-- returns: completed this 8

RAISERROR('completed %d %I64d',10,0,8,2424245124153) WITH NOWAIT
-- returns: completed 8 2424245124153

```