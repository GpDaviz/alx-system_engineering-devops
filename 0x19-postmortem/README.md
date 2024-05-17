#### 0x19-postmortem

### Postmortem for Spy-cam Outage on May 16, 2024

**Issue Summary:**
- **Duration:** The outage occurred from 1:00 AM to 5:30 AM UTC on May 16, 2024.
- **Impact:** The Spy-cam video streaming service was completely unavailable during this time. Users were unable to access live video feeds or recorded footage. Approximately 80% of our user base was impacted.
- **Root Cause:** A configuration alteration to the primary database server resulted in it running out of disk space, causing a cascading failure across the system.

**Timeline:**
- **1:00 AM:** A scheduled database maintenance task overwrote a crucial configuration file, leading to the database rapidly consuming disk space.
- **1:30 AM:** A monitoring alert was triggered due to high disk usage on the primary database server.
- **2:00 AM:** An on-call engineer investigated the alert and observed the rapid decrease in disk space but couldn't pinpoint the cause.
- **2:30 AM:** The primary database server ran out of disk space and became unresponsive, causing a total outage of the Spy-cam service.
- **3:00 AM:** The incident was escalated to the database team for further investigation.
- **3:30 AM:** The team initially suspected a hardware failure and tried to fail over to the secondary database server, but the process stalled because the primary server was unresponsive.
- **4:00 AM:** The team identified the issue as a configuration problem and reversed the changes made by the maintenance task.
- **4:30 AM:** The primary database server was brought back online, and services began to recover.
- **5:30 AM:** The Spy-cam service was fully restored and operational.

**Root Cause and Resolution:**
The issue stemmed from a scheduled database maintenance task that overwrote a vital configuration file managing disk space usage. This caused the database to rapidly consume disk space until it filled up completely, making the primary database server unresponsive.

To fix the problem, the team reversed the configuration changes made by the maintenance task, allowing the database to release the occupied disk space and resume normal operations.

**Corrective and Preventative Measures:**

**Areas for Improvement:**
- **Monitoring and Alerting:** Enhance monitoring and alerting systems for critical configuration changes.
- **Failover and Recovery Procedures:** Improve procedures for database server failover and recovery.
- **Testing of Maintenance Jobs:** Implement more thorough testing of maintenance tasks before deployment.

**Tasks:**
1. **File Integrity Monitoring:** Implement file integrity monitoring for critical configuration files to detect unauthorized changes.
2. **Disk Space Monitoring:** Enhance disk space usage monitoring with stricter thresholds and better alerting mechanisms.
3. **Review Failover Procedures:** Review and update the database failover and recovery procedures to ensure smooth transitions.
4. **Testing Framework:** Develop a comprehensive testing framework for database maintenance tasks to identify potential issues pre-deployment.
5. **Manual Approval:** Introduce manual approval steps for maintenance tasks that modify critical configurations to prevent unauthorized changes.

This incident underscored the need for better monitoring, testing, and failover procedures, especially for critical components like the database servers. Addressing these areas will enhance the reliability and resilience of the Spy-cam service.
