# Database Course Projects

This repository contains practical relational database projects designed and documented for the Database Course. Each practical project is structured within its own numbered directory to maintain clean separation, reproducibility, and modularity.

---

## 📁 Repository Structure

```text
Database-Course-Projects/
├── 01-Simple-Clinic/
│   ├── Requirements/
│   │   └── Project-Requirements.md
│   ├── SCHEMA/
│   │   └── SimpleClinic-RelationalSchema.png
│   └── Database/
│       └── Clinic_Database.bak
├── 02-Simple-Library/
│   ├── Requirements/
│   │   └── Project-Requirements.md
│   ├── SCHEMA/
│   │   └── SimpleLibrary-RelationalSchema.png
│   └── Database/
│       └── Library_Database.bak
├── 03-Karate-Club/
│   ├── Requirements/
│   │   └── Project-Requirements.md
│   ├── SCHEMA/
│   │   └── KarateClub-RelationalSchema.png
│   └── Database/
│       └── KarateClub_Database.bak
├── 04-Car-Rental/
│   ├── Requirements/
│   │   └── Project-Requirements.md
│   ├── SCHEMA/
│   │   └── CarRental-RelationalSchema.png
│   └── Database/
│       └── CarRental_Database.bak
├── 05-Online-Store/
│   ├── Requirements/
│   │   └── Project-Requirements.md
│   ├── SCHEMA/
│   │   └── OnlineStore-RelationalSchema.png
│   └── Database/
│       └── OnlineStore_Database.bak
└── README.md
🗂️ Projects Overview#ProjectKey Domains & Entities01Simple ClinicPatients, Doctors, Appointments, Medical Records, Payments02Simple LibraryBooks, Copies, Members, Borrowing Records, Fines, Reservations03Karate ClubMembers, Instructors, Belt Tests, Belts, Subscriptions, Payments04Car RentalCustomers, Vehicles, Categories, Fuel Types, Bookings, Returns, Transactions05Online StoreCustomers, Products, Orders, Order Items, Payments, Shippings, Reviews
🛠️ General Database Restore Guide (SQL Server)This guide walks through restoring any .bak database file from this repository using SQL Server Management Studio (SSMS).PrerequisitesMicrosoft SQL Server installedSQL Server Management Studio (SSMS)Step 1: Download & Locate the Backup FileDownload the desired .bak file from its project folder (Database/) and ensure its filename is clean (e.g., Clinic_Database.bak, KarateClub_Database.bak).Step 2: Copy to SQL Server Default Backup DirectoryCopy the .bak file to your instance's default backup location, usually:PlaintextC:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\
To find your instance's exact backup path, run this in SSMS:SQLSELECT SERVERPROPERTY('InstanceDefaultBackupPath');
Step 3: Inspect Logical File NamesOpen a New Query window in SSMS and inspect the backup file headers:SQLRESTORE FILELISTONLY 
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak';
Note: Copy the values from the LogicalName column for both the data file (Type = 'D') and the log file (Type = 'L').Step 4: Restore Database with Relocation (WITH MOVE)Run the restore statement using the logical names identified in Step 3:SQLRESTORE DATABASE Clinic_Database 
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak'
WITH MOVE 'DataLogicalNameFromStep3' 
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database.mdf',
     MOVE 'LogLogicalNameFromStep3' 
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database_log.ldf';
Step 5: Verify RestorationVerify that the database is online and accessible:SQLSELECT name, state_desc 
FROM sys.databases 
WHERE name = 'Clinic_Database';
Step 6: Fix dbo Principal Error (If Diagram Fails)If accessing database diagrams returns the error:Cannot execute as the database principal because the principal "dbo" does not existFix database ownership with:SQLUSE Clinic_Database;
GO
ALTER AUTHORIZATION ON DATABASE::Clinic_Database TO sa;
GO
⚠️ Troubleshooting Common ErrorsError MessageProbable CauseResolutionAccess is deniedSQL Server service account lacks access to the file directory.Place .bak inside the official SQL Server Backup/ folder.Operating system error 2 / Cannot open backup deviceFile path or name is mistyped or does not exist.Check the full file path and verify extension .bak.Directory lookup... cannot find the pathSource backup directory tree differs from target server.Use the WITH MOVE syntax to route .mdf and .ldf to local DATA/.Principal "dbo" does not existOriginal database owner SID is missing on the local machine.Execute ALTER AUTHORIZATION ON DATABASE::[DbName] TO sa;.
