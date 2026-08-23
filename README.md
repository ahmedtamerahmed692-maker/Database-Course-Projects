Database Course Projects
This repository contains my practical projects for the Database course. Each practical project is kept in its own numbered folder so that the course work remains organized and easy to extend.
Repository organization
Every project should keep its ERD files, schema files, screenshots, requirements, database backups, SQL scripts, and documentation inside that project's folder only.
The current organization is:
```text
Database-Course-Projects/
├── 01-Simple-Clinic/
│   ├── ERD/
│   ├── SCHEMA/
│   ├── Requirements/
│   └── Database/
└── README.md
```
New practical projects should be added as separate numbered folders, such as `02-Library-System/` or `03-Hospital-System/`. No project files are included unless they have been provided for the course work.
General Database Restore Guide
The following guide applies to SQL Server database backup files used by projects in this repository. Project-specific requirements remain inside their corresponding project folder.
Prerequisites
Before starting, make sure that SQL Server and SQL Server Management Studio (SSMS) are installed on your computer.
Step 1: Download the backup file
Download the `.bak` file from the relevant project folder and verify its filename. If the filename contains random characters, spaces, or parentheses, rename it to a simple name such as:
```text
Clinic_Database.bak
```
Step 2: Move the backup to SQL Server's Backup folder
Copy the backup file to SQL Server's official Backup folder. The exact path can vary depending on the SQL Server version and instance:
```text
C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup
```
If you do not know the correct backup path for your SQL Server installation, run this query in SSMS:
```sql
SELECT SERVERPROPERTY('InstanceDefaultBackupPath');
```
Step 3: Inspect the backup file
Open New Query in SSMS and run the following command. Replace the path and filename with the actual values on your computer:
```sql
RESTORE FILELISTONLY
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak';
```
The result contains a `LogicalName` column. Save both logical names shown in the result: one for the data file and one for the log file.
Step 4: Restore the database
Run the following command in SSMS. Replace every placeholder with the correct value from your environment and the logical names returned in Step 3:
```sql
RESTORE DATABASE Clinic_Database
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak'
WITH MOVE 'DataLogicalNameFromStep3'
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database.mdf',
     MOVE 'LogLogicalNameFromStep3'
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database_log.ldf';
```
The logical names must match the values returned by `RESTORE FILELISTONLY` exactly. They may contain hyphens or other characters, so copy them exactly as displayed.
Step 5: Verify the restore
Run the following query to check whether the database exists:
```sql
SELECT *
FROM sys.databases
WHERE name = 'Clinic_Database';
```
If the query returns the database, the restore was successful.
Step 6: Optional fix for the `dbo` error
If you open Database Diagrams or another administrative tool and receive this error:
```text
Cannot execute as the database principal because the principal "dbo" does not exist
```
Run the following commands, replacing the database name if necessary:
```sql
USE Clinic_Database;
GO

ALTER AUTHORIZATION ON DATABASE::Clinic_Database TO sa;
```
Common errors and troubleshooting
Error	Likely cause	Suggested solution
`Access is denied`	The backup or restore operation uses a location where SQL Server does not have permission, such as the root of `C:\`.	Use SQL Server's official Backup folder and the appropriate DATA folder.
`Operating system error 2` or `Cannot open backup device`	The backup path or filename is incorrect, or the filename contains unexpected spaces or characters.	Verify the complete path and filename exactly. Rename the backup to a simple filename if necessary.
`Directory lookup...cannot find the path`	The backup came from a different SQL Server installation or version, such as `MSSQL16` instead of `MSSQL17`.	Use the `WITH MOVE` clauses from Step 4 and replace the paths with valid folders on your computer.
`dbo does not exist`	The owner of the original database is not available on the current computer.	Run the `ALTER AUTHORIZATION` command in Step 6.
Important notes
Do not commit passwords, connection strings, or other secrets to the repository. Upload a database backup only if it does not contain sensitive personal or production data.
Source: ProgrammingAdvices.com, © Copyright 2023, Project 1 – Simple Clinic requirements and restore instructions provided for this project.
