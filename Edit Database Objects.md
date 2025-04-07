## Change schema
```SQL
ALTER SCHEMA eldb
TRANSFER dbo.[05_Deprivation];
```
  
## Change name of table, column, index etc
Any user-created object in database.
```SQL
USE eldb2021;
EXEC sp_rename 'dbo.05_Deprivation', 'deprivation'
EXEC sp_rename 'dbo.deprivation.IMD_11', 'IMD_2011', 'COLUMN'
EXEC sp_rename 'dbo.deprivation.IX_deprivation', 'cx_deprivation', 'INDEX'
```

## Find indexes on a temp table
```SQL
EXEC tempdb.dbo.sp_helpindex '#temp'
```

## Database size
```sql
SELECT 
	databases.name AS [Database Name], 
	materfiles.type_desc AS [File Type],
    CAST((materfiles.Size * 8) / 1024.0 AS DECIMAL(18, 1)) AS [Initial Size (MB)], 
    'By '+IIF(materfiles.is_percent_growth = 1, 
		CAST(materfiles.growth AS VARCHAR(10))+'%', 
		CONVERT(VARCHAR(30), CAST((materfiles.growth * 8) / 1024.0 AS DECIMAL(18, 1)))+' MB') AS [Autogrowth], 
    IIF(materfiles.max_size = 0, 'No growth is allowed', 
		IIF(materfiles.max_size = -1, 'Unlimited', 
		CAST((CAST(materfiles.max_size AS BIGINT) * 8) / 1024 AS VARCHAR(30))+' MB')) AS [MaximumSize]
FROM sys.master_files AS materfiles
INNER JOIN sys.databases AS databases ON databases.database_id = materfiles.database_id
where databases.name='eldb2021'
```

```SQL
USE eldb2021;
SELECT
name
,file_id
,file_guid
,physical_name
,size/128.0 FileSizeInMB
,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS EmptySpaceInMB
FROM sys.database_files;
```

## Indexes in Database
```SQL
USE eldb2021;
SELECT
	Tab.name as Table_Name 
	,IX.name as Index_Name
	,IX.type_desc as Index_Type
	,Col.name as Index_Column_Name
	,IXC.is_included_column as Is_Included_Column
FROM  sys.indexes IX 
INNER JOIN sys.index_columns IXC ON IX.object_id = IXC.object_id AND IX.index_id = IXC.index_id  
INNER JOIN sys.columns Col ON IX.object_id = Col.object_id AND IXC.column_id = Col.column_id     
INNER JOIN sys.tables Tab ON IX.object_id = Tab.object_id
ORDER BY Table_Name
```

### Delete Duplicates
```sql
WITH CTE(patient_id, dup) AS (
SELECT
	patient_id, 
	ROW_NUMBER() OVER(PARTITION BY patient_id ORDER BY earliest_date) as dup
FROM eldb2021.dbo.Cancer)

DELETE FROM CTE
WHERE dup >1;
```
