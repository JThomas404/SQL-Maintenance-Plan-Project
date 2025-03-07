# SQL Server Backup & Maintenance Project

## Project Overview

As a Cloud Engineer at Seidor Networks, I worked on a project for our client, **Butternut Box**, who were experiencing disk alert failures. After investigating, I identified that the issue was caused by SQL Server backup failures. The backups were not being cleared from the previous day, causing the disk to fill up and preventing the `live_db_backup` from completing. This caused the SQL maintenance plan to fail and the backup cleanup task to not execute. 

This documentation outlines the issue, my investigation, and the solutions implemented, highlighting best practices for SQL Server maintenance and backup strategies.

## The Problem

The SQL Server maintenance plan was configured to back up databases daily. However, the backups repeatedly failed. After investigation, the root cause was identified as insufficient disk space, combined with poor infrastructure decisions:

- **Single Disk Usage:** The server only had a single C: drive, which stored both the SQL Server and its backups.
- **Local Backup Storage:** Backups were stored on the same server, eliminating redundancy and high availability.
- **Backup Cleanup Failure:** The maintenance plan failed to delete older backups, causing the disk to fill up rapidly.

These issues led to a cycle of failed backups and a critical risk of data loss.

## Investigation

1. **Maintenance Plan Review:** I reviewed the SQL Server maintenance plan configuration.
2. **Job History Analysis:** I checked SQL Server Agent job logs, which showed backup tasks failing daily at 2 AM.
3. **Error Logs:** The backup task failed with error code **112** ("There is not enough space on the disk").
4. **Manual Disk Check:** I logged into the server and confirmed that the C: drive was full, with no automatic cleanup occurring.

## Solution

1. **Immediate Space Clearance:** I manually deleted redundant **.dmp** files and other unnecessary data to free up space.
2. **Maintenance Plan Fix:** I reordered the maintenance tasks to run the **Backup Cleanup Task** before the **Backup Task**.
3. **Disk Space Advisory:** I recommended that the client add separate D: and E: drives for backups, quoting scalable storage options.
4. **High Availability Migration:** I initiated a migration project to enable off-server backup storage, ensuring data redundancy.

## Best Practices Implemented

- **Separation of Concerns:** Isolated backups on a separate drive to prevent disk contention with live databases.
- **Backup Retention Policy:** Configured automated deletion of backups older than 7 days.
- **Off-Site Storage:** Recommended cloud-based backup solutions for disaster recovery.

## Results

After implementing these changes, the backups ran successfully, and disk space issues were resolved. The client gained a more resilient infrastructure with reduced risk of data loss.

## Folder Structure



## Folder Structure

```
├── README.md
├── images
│   ├── sql_maintenance.png
│   ├── backup_error.png
│   └── disk_space_issue.png
└── pages
    ├── investigation.md
    ├── issues.md
    └── solution.md
```
