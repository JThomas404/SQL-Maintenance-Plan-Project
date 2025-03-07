# Investigation of SQL Maintenance Plan Failure

## Overview
The SQL Maintenance Plan was failing due to insufficient disk space on the C: drive, which resulted in backups not being deleted, causing rapid disk space consumption. The investigation led to identifying several issues with the configuration and environment, which were then addressed.

One day on the disk space flagged at 16 GB of storage. I had cleared disk space temp files, cleared the bin, etc. and there was 16 gb of free space. 

![Low Disk Space](low_disk_space_investigation.png)

i had escalated the matter to the SAP Team to remove any other uneeded files to further clear up space, however, the next day the space decreased to 4GB of free space. 

![Low Disk Space](4gb_low_disk_space_investigation.png)

I was not aware of this at the time but this was due to the backup files frrom the previous day not deleting. This required me to manually clear up space and remove the backups.

![Backup Files](SQL_backups_investigation.png)
---

## **1. Identifying the Core Issue:**

The investigation began by reviewing the SQL Maintenance Plan and the job history. The primary issue was that the maintenance plan kept failing because there was **not enough disk space** to perform backups. The plan could not delete old backups, causing the disk to quickly fill up.
![SQL Maintenance Plan](SQL_maintenance_plan_investigation.png)

I investigated the issue by viewing the job history  on **SQL Server Management Studio (SSMS)**.
![Job History](SQL_mp_job_history_investigation.png)
![Job History Detailed](SQL_mp_job_history_detailed_investigation.png)

The Job history shows that the Maintenance plan failed today at 2 am.
---

## **2. Understanding the Configuration Problem:**

Upon further analysis, it was discovered that the company was using a **single disk (C: drive)** for both SQL Server data and backups. This practice is problematic because:
- **No redundancy:** Backups should be stored on a separate disk for better availability and disaster recovery.
- **Disk contention:** Storing both databases and backups on the same disk leads to performance issues and space-related failures.

---

## **3. Reviewing SQL Server Maintenance Plan:**

The maintenance plan was set up in **SQL Server Management Studio (SSMS)**. Despite the correct configuration, it was found that:
- The **Backup Cleanup Task** was not executed before the backup task.
- As a result, old backup files were not deleted, causing disk space to be consumed rapidly.

---

## **4. Investigating the Error:**

The error logs showed that the backup task failed due to a lack of space on the **C:\SQL\Backups** directory. The error code **112** was recorded, indicating that the disk was full or nearly full. The lack of space was preventing SQL Server from completing the backup operation.

![Maintenance Plane Execution Error](SQL_mp_execution_error_code_investigation.png)

---

## **5. Impact Analysis:**

The failure to execute the backups had the following consequences:
- Backup tasks for databases like **[BPAStagingLive]** failed, leading to **incomplete or missing backups**.
- This posed a risk of **data loss or inconsistency** within the system.

![Error Analysis](execution_error_code_analysis_investigation.png)

---

## **6. Hands-On Investigation:**

To resolve the issue, I logged onto the server to inspect the backup directory. It was confirmed that old backup files were not being deleted, causing the disk to fill up. I also noted that the maintenance plan had failed on several occasions.

---

## **7. Space Recovery and Collaboration:**

The server was running out of space, with only **16 GB** available. To resolve this:
- I collaborated with **Mr. Eames** (senior resource) to identify files that could be deleted to free up space.
- We removed **duplicate .dmp files** that were not essential for operations, which cleared up space for the backups to proceed.

![Removed Duplicate .dmp Files](deleted_duplicatedmp_files_investigation.png)

---

## **8. Monitoring and Final Resolution:**

After freeing up sufficient space, I monitored the SQL Maintenance Plan's execution. The subsequent plan execution was successful, and the backups were completed without errors.

---

## **Conclusion:**

The issue was caused by a combination of disk space limitations and the configuration of the maintenance plan. By adjusting the order of operations and freeing up disk space, the plan was able to run successfully. Going forward, I recommend implementing a more robust storage solution with backups on a separate disk or off-site to prevent similar issues.

For more information on the solutions implemented, see the [solutions.md](solutions.md).
