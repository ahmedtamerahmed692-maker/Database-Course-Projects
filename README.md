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
```

---

## 🗂️ Projects Overview

| # | Project | Key Domains & Entities |
|---|---|---|
| **01** | **Simple Clinic** | Patients, Doctors, Appointments, Medical Records, Payments |
| **02** | **Simple Library** | Books, Copies, Members, Borrowing Records, Fines, Reservations |
| **03** | **Karate Club** | Members, Instructors, Belt Tests, Belts, Subscriptions, Payments |
| **04** | **Car Rental** | Customers, Vehicles, Categories, Fuel Types, Bookings, Returns, Transactions |
| **05** | **Online Store** | Customers, Products, Orders, Order Items, Payments, Shippings, Reviews |

---

## 🛠️ General Database Restore Guide (SQL Server)

This guide walks through restoring any `.bak` database file from this repository using **SQL Server Management Studio (SSMS)**.

### Prerequisites
* Microsoft SQL Server installed
* SQL Server Management Studio (SSMS)

---

### Step 1: Download & Locate the Backup File
Download the desired `.bak` file from its project folder (`Database/`) and ensure its filename is clean (e.g., `Clinic_Database.bak`, `KarateClub_Database.bak`).

---

### Step 2: Copy to SQL Server Default Backup Directory
Copy the `.bak` file to your instance's default backup location, usually:

```text
C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\
```

To find your instance's exact backup path, run this in SSMS:

```sql
SELECT SERVERPROPERTY('InstanceDefaultBackupPath');
```

---

### Step 3: Inspect Logical File Names
Open a **New Query** window in SSMS and inspect the backup file headers:

```sql
RESTORE FILELISTONLY 
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak';
```

> **Note:** Copy the values from the `LogicalName` column for both the data file (`Type = 'D'`) and the log file (`Type = 'L'`).

---

### Step 4: Restore Database with Relocation (`WITH MOVE`)
Run the restore statement using the logical names identified in Step 3:

```sql
RESTORE DATABASE Clinic_Database 
FROM DISK = 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\Backup\Clinic_Database.bak'
WITH MOVE 'DataLogicalNameFromStep3' 
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database.mdf',
     MOVE 'LogLogicalNameFromStep3' 
         TO 'C:\Program Files\Microsoft SQL Server\MSSQLXX.MSSQLSERVER\MSSQL\DATA\Clinic_Database_log.ldf';
```

---

### Step 5: Verify Restoration
Verify that the database is online and accessible:

```sql
SELECT name, state_desc 
FROM sys.databases 
WHERE name = 'Clinic_Database';
```

---

### Step 6: Fix `dbo` Principal Error (If Diagram Fails)
If accessing database diagrams returns the error:
`Cannot execute as the database principal because the principal "dbo" does not exist`

Fix database ownership with:

```sql
USE Clinic_Database;
GO
ALTER AUTHORIZATION ON DATABASE::Clinic_Database TO sa;
GO
```

---

## ⚠️ Troubleshooting Common Errors

| Error Message | Probable Cause | Resolution |
|---|---|---|
| **Access is denied** | SQL Server service account lacks access to the file directory. | Place `.bak` inside the official SQL Server `Backup/` folder. |
| **Operating system error 2 / Cannot open backup device** | File path or name is mistyped or does not exist. | Check the full file path and verify extension `.bak`. |
| **Directory lookup... cannot find the path** | Source backup directory tree differs from target server. | Use the `WITH MOVE` syntax to route `.mdf` and `.ldf` to local `DATA/`. |
| **Principal "dbo" does not exist** | Original database owner SID is missing on the local machine. | Execute `ALTER AUTHORIZATION ON DATABASE::[DbName] TO sa;`. |

---

## 📌 Important Notes
* **Security:** Never commit production connection strings, credentials, or production data dumps to public repositories.
* **Course Work:** Schemas and requirements adapted from course curricula (ProgrammingAdvices.com).
