---
layout: post
title: How to Restore a SQL Server Database
date: 2014-08-10 22:16:00
categories: SQL
---

We often follow these steps for creating a local copy of a production database.

Step 1
------------------
![Backup Database](/assets/backup-database.png)

Step 2
------------------
![Backup Settings](/assets/backup-settings.png)

Step 3
------------------
{% highlight sql %}
GO
EXEC sp_addumpdevice 'disk', 'WashAlertBackup', 'C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\Backup\WashAlert.bak' ;
GO

select * from sys.backup_devices

USE master;
GO
-- First determine the number and names of the files in the backup.
-- WashAlertBackup is the name of the backup device.
RESTORE FILELISTONLY
   FROM WashAlertBackup;

-- Restore the files for WashAlertBackup.
RESTORE DATABASE WashAlert
   FROM WashAlertBackup
   WITH REPLACE,
   MOVE 'WashAlert' TO 'C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\DATA\WashAlert.mdf', 
   MOVE 'WashAlert_log' TO 'C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\DATA\WashAlert_log.ldf';
GO
{% endhighlight %}