# SSIS-Task
<img width="636" height="329" alt="image" src="https://github.com/user-attachments/assets/e77f0c1c-4235-4dde-a8bd-bf56df5b6056" />
<img width="751" height="154" alt="image" src="https://github.com/user-attachments/assets/1a3bcf86-5daf-4161-a2b6-099bed4aa919" />
📌 Project Overview

This task demonstrates how to use SQL Server Integration Services (SSIS) to import employee data from an Excel file into a SQL Server database table with workflow control and error handling.

The package performs the following operations:

Import employee data from Excel into SQL Server.
If the import succeeds:
Automatically create a full database backup.
If the import fails:

Display an error message using C# Script Task:

"The task has been failed"

🛠 Technologies Used
SQL Server Integration Services
Microsoft SQL Server
Microsoft Visual Studio
Excel File Source
C# Script Task
Execute SQL Task
🎯 Task Requirements

Import the following employee data fields from Excel:

Column Name	Data Type
emp_id	        INT
emp_name	VARCHAR
Gender	        VARCHAR


🗂 Package Workflow
        +-------------------+
        | Data Flow Task    |
        | Import From Excel |
        +---------+---------+
                  |
        Success   |   Failure
          Green   |    Red
                  |
     +------------+------------+
     |                         |
+----v----+             +------v------+
| Execute |             | Script Task |
| SQL Task|             | C# Message  |
| Backup  |             | Failure Msg |
+---------+             +-------------+


📥 Step 1 - Create SQL Table
CREATE TABLE Employees
(
    emp_id INT,
    emp_name VARCHAR(100),
    Gender VARCHAR(20)
);


📊 Step 2 - Prepare Excel File

Create an Excel file named:

Employees.xlsx

Example data:

emp_id	emp_name	Gender
1	Ahmed	Male
2	Sara	Female
3	Omar	Male


🔄 Step 3 - Create SSIS Package
Inside Control Flow

Add:

✅ Data Flow Task

Purpose:
Import data from Excel into SQL Server table.


🔧 Step 4 - Configure Data Flow
Inside Data Flow

Add:

1️⃣ Excel Source
Select Excel file
Choose worksheet
2️⃣ OLE DB Destination
Connect to SQL Server
Select Employees table
💾 Step 5 - Create Full Database Backup

After successful import:

Add:

✅ Execute SQL Task

Connect it with:

Green precedence constraint (Success)
SQL Command:
BACKUP DATABASE SchoolDB
TO DISK = 'D:\Backup\SchoolDB.bak'
WITH FORMAT,
MEDIANAME = 'SQLServerBackups',
NAME = 'Full Backup of SchoolDB';


❌ Step 6 - Handle Failure Using C#
Add:
✅ Script Task

Connect it with:

Red precedence constraint (Failure)

Language:

C#
Script Code
using System;
using Microsoft.SqlServer.Dts.Runtime;

public void Main()
{
MessageBox.Show("The task has been failed");
Dts.TaskResult = (int)ScriptResults.Success;
}

✅ Expected Result
If Import Succeeds
Data inserted into SQL Server table.
Full database backup created automatically.
If Import Fails
C# message box appears:
The task has been failed


📌 Concepts Learned

This task demonstrates:

Excel Source in SSIS
OLE DB Destination
Control Flow
Data Flow
Execute SQL Task
Database Backup Automation
Error Handling
Script Task with C#
🚀 Final Outcome

A complete ETL workflow that:

✔ Imports employee data
✔ Stores data in SQL Server
✔ Creates automatic database backup
✔ Handles failures professionally using C#
