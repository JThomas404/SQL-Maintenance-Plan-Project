# SQL Server Backup & Maintenance Project

## Project Overview

As a Cloud Engineer at Seidor Network, I worked on a project for a client experiencing SQL Server backup failures due to disk space constraints. This documentation outlines the issues, investigation, and solutions, highlighting critical best practices for SQL Server maintenance and backup strategies.

## The Problem

The SQL Server maintenance plan was configured to back up databases daily. However, the backups repeatedly failed. After investigation, the root cause was insufficient disk space, compounded by poor infrastructure decisions:

- **Single Disk Usage:** The server only had a single C: drive, which housed both the SQL Server and its backups.
- **Local Backup Storage:** Backups were stored on the same server, eliminating redundancy and high availability.
- **Backup Cleanup Failure:** The maintenance plan failed to delete older backups, causing the disk to fill up rapidly.

These issues led to a cycle of failed backups and a critical risk of data loss.

## Investigation

1. **Maintenance Plan Review:** Examined the SQL Server maintenance plan configuration.
2. **Job History Analysis:** Checked SQL Server Agent job logs, which showed backup tasks failing at 2 AM daily.
3. **Error Logs:** The backup task failed with error code **112** ("There is not enough space on the disk").
4. **Manual Disk Check:** Logged into the server and confirmed the C: drive was full, with no automatic cleanup occurring.

## Solution

1. **Immediate Space Clearance:** Manually deleted redundant **.dmp** files and other unnecessary data to free up space.
2. **Maintenance Plan Fix:** Reordered the maintenance tasks to run the **Backup Cleanup Task** before the **Backup Task**.
3. **Disk Space Advisory:** Advised the client to add separate D: and E: drives for backups, quoting options for scalable storage.
4. **High Availability Migration:** Initiated a migration project to enable off-server backup storage and ensure data redundancy.

## Best Practices Implemented

- **Separation of Concerns:** Isolated backups on a separate drive to prevent disk contention with live databases.
- **Backup Retention Policy:** Configured automated deletion of backups older than 7 days.
- **Off-Site Storage:** Recommended cloud-based backup solutions for disaster recovery.

## Results

After implementing these changes, the backups ran successfully, and disk space issues were resolved. The client gained a more resilient infrastructure with reduced risk of data loss.

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
