
# RDS Backup & Restore  
Requires S3 bucket with access policy linked to db.
As root user (ceguser / cegadmin)
```sql
exec msdb.dbo.rds_backup_database  
@source_db_name='analysis_ceg',  
@s3_arn_to_backup_to='arn:aws:s3:::eastlondondb/bak_files/analysis_ceg.bak',  
@overwrite_s3_backup_file=1;
```
   @source_db_name           : Source database name to create a full backup of.  
   @s3_arn_to_backup_to      : S3 key ARN to store the backup file at.  
   @kms_master_key_arn       : KMS customer master key ARN to encrypt the backup file with.  
   @overwrite_s3_backup_file : Indicates whether to overwrite the specified file in S3 or not, if one exists. 1=overwrite 0=no overwrite  
   @type                     : Type of backup to create. Valid options are: FULL and DIFFERENTIAL. Defaults to FULL.  
   @number_of_files          : (Multifile backups only) Number of files to create for this backup.  

RDS cannot overwrite db, therefore need to drop db and restore. As root user under Master:  

exec msdb.dbo.rds_restore_database  
@restore_db_name='eldb2019',  
@s3_arn_to_restore_from='arn:aws:s3:::eastlondondb/bak_files/eldb2019_PREP.bak'

Output provides <task_id> that can be used to check progress:  
exec msdb.dbo.rds_task_status @task_id= <task_id>  
   
Restore will take a few minutes. Once complete Refresh  

[https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html)














# ELDB Pre-Production Checks and Prep

Take back up of DEV database [AWS RDS](https://qmulprod.sharepoint.com/sites/ClinicalEffectivenessGroupIPHS/IT%20%20Data%20Systems/AWS%20RDS.aspx)


Check for constraints and indexes  
USE eldb2020_DEV;  
SELECT *  
FROM   INFORMATION_SCHEMA.KEY_COLUMN_USAGE;  

Run **1- Make [Anonymised Identifier] NOT NULL.sql** -- approx. 2mins  

Run **2- Add Primary Key.sql** -- approx. 4mins  

If needed individual tables can be altered (PK column must be defined as NOT NULL)  
ALTER TABLE  [Thyroid disorders] ALTER COLUMN [Anonymised Identifier] varchar(255) NOT NULL  
GO  
ALTER TABLE  [Thyroid disorders] ADD CONSTRAINT [PK_Thyroid_disorders] PRIMARY KEY ([Anonymised Identifier])  

Check for Readcode collation - needs to be Latin1_General_CS_AI

If needed, run **9- Make Clinical Code Column Search Case Sensitiv****e.sql** -- makes Readcode fields case sensitive  

Run **INDEX_CORE.sql** -- adds indexs on CORE for Age, Gender, Ethnicitiy and Locality/Organisation

Run **PK and INDEX lookuptables.sql** -- adds Primary Keys and Indexes to lookup tables  

Shrink database to minimum size [AWS RDS](https://qmulprod.sharepoint.com/sites/ClinicalEffectivenessGroupIPHS/IT%20%20Data%20Systems/AWS%20RDS.aspx)  

Run DBCC CHECKDB -- checks db integrity  

Create new database

Import Data using wizard.  In Options select Schema and Data and Script Indexes to True, in order to import indexes etc.  Or rerun sql scripts on new db.

Take back up of production database [AWS RDS](https://qmulprod.sharepoint.com/sites/ClinicalEffectivenessGroupIPHS/IT%20%20Data%20Systems/AWS%20RDS.aspx)  

Create mapped access for Logins.

USE eldb2020;  
CREATE USER ceg_kelvins FROM LOGIN ceg_kelvins;  
CREATE USER ceguser FROM LOGIN ceguser;  
GO  
ALTER ROLE db_datareader ADD MEMBER ceg_kelvins;  
ALTER ROLE db_datareader ADD MEMBER ceguser;  
GO