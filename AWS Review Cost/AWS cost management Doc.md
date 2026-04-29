|  Legacy PHP Application  |  Region: us-east-2
**Date:** March 25, 2026  **Period:** Dec 2025 – Feb 2026  **Type:** Read-only · No changes made
```
Key Metrics — Cost Overview
Dec 2025 Total
$411.76
Baseline month
Feb 2026 Total
$552.02
▲ $140.26 vs Dec
MoM Increase
34%
Primarily RDS
Est. Annual Run Rate
~$6,624
At Feb 2026 pace

```
### Monthly cost by service (Dec 2025 → Jan 2026 → Feb 2026)

``RDSEC2 InstancesEC2 - OtherWorkMailELBVPCSupport
### RDS cost breakdown — Feb 2026 (major driver)

``db.m7g.large (new)MySQL 5.7 Extended Supportdb.m5.large (new)Existing RDS (Dec)

### Service cost breakdown

| Service                         | Dec 2025    | Jan 2026  | Feb 2026    | Change       | Status    |
| ------------------------------- | ----------- | --------- | ----------- | ------------ | --------- |
| **Relational Database Service** | $204.67     | ~$205     | $343.54     | +$138.87     | Critical  |
| EC2 - Instances                 | $69.04      | $69.04    | $62.36      | -$6.68       | Improving |
| WorkMail                        | $60.00      | $60.00    | $60.00      | $0.00        | Review    |
| Support (Developer)             | $29.00      | $29.00    | $29.00      | $0.00        | Review    |
| VPC                             | $19.43      | $19.42    | $20.19      | +$0.76       | Stable    |
| Elastic Load Balancing          | $17.09      | $16.99    | $15.33      | -$1.76       | Stable    |
| EC2 - Other                     | $12.00      | $13.01    | $21.00      | +$9.00       | Monitor   |
| Route 53                        | $0.52       | $0.53     | $0.61       | +$0.09       | Stable    |
| **Total**                       | **$411.76** | **~$413** | **$552.02** | **+$140.26** |           |

---

**Findings & Recommendations**
#### ``FinOps — Cost Reduction
--
**``F1 — RDS: Two new instances appeared in Feb 2026 (db.m7g.large + db.m5.large)**

Usage jumped from 0 hrs to 227.52 hrs and 213.40 hrs respectively in a single month. These may be test/staging instances left running. The db.m7g.large alone added $76.67 in one month. Verify if both are needed in production simultaneously.

Potential saving: $80–$115/month if one instance is unused or can be stopped

**``F2 — MySQL 5.7 Extended Support: $42.68/month and growing**

MySQL 5.7 reached end-of-life in Oct 2023. AWS charges Extended Support for Yr1–Yr2 at per-vCPU-hour rates (426.80 vCPU-hours in Feb). This cost will increase further in Year 2 support. Upgrading to MySQL 8.0 eliminates this cost entirely.

Potential saving: $42–$85+/month (doubles in Yr2 extended support)

**``F3 — RDS Instance Right-Sizing Opportunity**

Running db.m7g.large and db.m5.large simultaneously for a legacy PHP app is likely over-provisioned. Without CloudWatch CPU/memory metrics it's hard to confirm, but for most PHP web apps a db.t4g.medium or db.t3.medium is sufficient at a fraction of the cost.

Potential saving: $40–$80/month by downsizing to burstable instance class

**``F4 — WorkMail: $60/month flat — review active mailboxes**

AWS WorkMail costs $4/user/month. At $60/month this implies 15 active mailboxes. For a PHP project, this may be unnecessary. Consider consolidating to 1–2 technical mailboxes, or migrating to an external email provider to eliminate this cost.

Potential saving: $20–$50/month by reducing mailbox count

**``F5 — AWS Support Plan: $29/month Developer tier — consider downgrading**

Developer support at $29/month provides business-hours email support. If the team has sufficient in-house AWS expertise, downgrading to Basic (free) or retaining only for critical go-live periods could reduce cost.

Potential saving: $29/month if downgraded to Basic

**``F6 — VPC: NAT Gateway charges ($19–$20/month) — review data transfer**

NAT Gateway charges include an hourly fee plus per-GB data processing. For a PHP app, check if all subnets truly need NAT Gateway. Private resources that only need outbound internet access could use NAT Instance (cheaper) or VPC Endpoints for AWS services.

Potential saving: $5–$12/month via VPC endpoint optimization

**``F7 — Reserved Instances / Savings Plans for predictable workloads**

EC2 ($62/month) and any stable RDS instance are good candidates for 1-year Reserved Instances. EC2 RI for a t3.medium (if that's the instance type) saves ~38% vs on-demand. For RDS, Reserved Instances save 30–40% over on-demand.

Potential saving: $25–$50/month on compute via 1yr Reserved Instances

**``F8 — Enable Cost Anomaly Detection & AWS Budgets**

The $140 spike in Feb went undetected until billing review. Set a Budget alert at $450/month with SNS email notifications. Enable Cost Anomaly Detection for RDS specifically — this would have flagged the new instances immediately.

Preventive: catches future spikes before month-end billing surprise


Security Findings
--
**S1 — RDS Encryption at Rest: DISABLED [HIGH RISK]**

RDS instances are running without encryption at rest. This means database files on disk are unprotected. Enabling encryption requires creating an encrypted snapshot and restoring to a new instance (cannot be enabled in-place). This is a compliance and data protection issue.

Risk: Data exposure if storage is accessed or copied. Required for PCI-DSS, HIPAA, ISO 27001.

**S2 — RDS Multi-AZ: DISABLED [HIGH RISK]**

Single-AZ RDS means a hardware failure or AZ outage brings down the database with no automatic failover. For a production application, this is a significant availability risk. Multi-AZ standby replica provides automatic failover in ~60–120 seconds.

Risk: Complete database downtime during AZ failures. No automatic recovery.

 **S3 — IAM MFA Not Enforced [HIGH RISK]**

IAM users can log in without Multi-Factor Authentication. This means a compromised password = full account access. Apply an IAM policy DenyWithoutMFA to force all users to enroll MFA before accessing console resources. Also check for any access keys without rotation policy.

Risk: Account takeover via credential stuffing or phishing. No second factor stops attackers.

**S4 — Root Account Usage [HIGH RISK]**

Root account has unrestricted access to everything including billing, IAM deletion, and account closure. Root should NEVER be used for daily operations. Enable MFA on root immediately, delete root access keys if they exist, and lock root credentials in a secure vault.

Risk: Root account compromise = total account loss. No recovery controls.

**S5 — CloudTrail Not Confirmed Enabled [MEDIUM]**

No CloudTrail activity was visible in the cost data (CloudTrail storage would show in S3 costs, which are $0.00). Without CloudTrail, there is no audit log of who made changes, when, and from where. This is critical for incident response and compliance.

Risk: No forensic trail for security incidents or unauthorized changes.

** S6 — IAM Access Key Age Unknown [MEDIUM]**

AWS best practice requires access keys to be rotated every 90 days. Review IAM → Users → Security Credentials for last rotation date. Keys older than 90 days should be rotated. Keys older than 1 year should be considered potentially compromised and replaced immediately.

Risk: Long-lived credentials increase exposure window if leaked in code or config files.

**S7 — S3 Public Access Status Unknown [MEDIUM]**

S3 shows $0.00 cost which may mean S3 is used minimally or accessed via CloudFront. Confirm that Block Public Access is enabled at the account level in S3 settings. Even if no buckets exist today, this prevents accidental public bucket creation in the future.

Risk: Accidental data exposure if public access block is not enforced at account level.

**S8 — Security Groups: Review 0.0.0.0/0 Rules [LOW-MEDIUM]**

For a legacy PHP app, check that Security Groups only allow necessary ports. Port 22 (SSH) should NOT be open to 0.0.0.0/0 — restrict to specific IPs or use AWS Systems Manager Session Manager. Port 3306 (MySQL) should never be publicly accessible.

Recommendation: Use least-privilege Security Group rules and enable VPC Flow Logs.
Action Roadmap
--
Prioritized action plan — implement in this order for maximum impact and minimum risk.

**1  Immediate (This Week) — Security**

Enable MFA on root account and all IAM users. Review and delete root access keys if they exist. Set up CloudTrail in all regions. Rotate any access keys older than 90 days. Cost: $0.

**2  Identify the new RDS instances — Are they needed?**

Determine why db.m7g.large and db.m5.large appeared in Feb. If they are test/staging instances, stop or terminate them. If only one is needed, terminate the other. Expected saving: $80–$115/month. Timeline: 1–2 days.

**3 Upgrade MySQL 5.7 → MySQL 8.0**

Schedule a maintenance window for the MySQL upgrade. Test application compatibility with MySQL 8.0 in staging first. This eliminates the $42/month Extended Support charge (which doubles in Year 2). Timeline: 1–2 weeks with testing.

**4 Right-size RDS instance after load analysis**

Enable CloudWatch Enhanced Monitoring on RDS for 2 weeks. If CPU stays below 20–30%, downgrade to db.t4g.medium. For a typical PHP app this saves $40–$80/month. Timeline: 3–4 weeks (monitoring period first).

**5  Enable RDS Encryption at Rest**

Create encrypted snapshot of current RDS instance → Restore to new encrypted instance → Update app connection string → Terminate unencrypted instance. Must be planned with downtime window. Timeline: 1 weekend.

**6 Audit WorkMail and reduce mailbox count**

List all WorkMail users. Disable or remove inactive ones. $4/user/month — reducing from 15 to 5 users saves $40/month. Consider migrating to external provider entirely. Timeline: 1 week.

**7 Set up AWS Budgets + Cost Anomaly Detection**

Create a budget at $450/month with email alerts at 80% and 100%. Enable Cost Anomaly Detection for RDS, EC2. This prevents future surprise bills. Cost: Free tier. Timeline: 30 minutes.

**8 Purchase Reserved Instances for stable workloads**

Once instance types are right-sized and stable, purchase 1-year Reserved Instances for EC2 and RDS. Saves 30–40% vs on-demand. Do this AFTER right-sizing — buying RI for wrong-sized instances wastes savings. Timeline: 30 days after step 4.

### Projected monthly cost after optimization
**Current (Feb 2026)**
$552
**After Quick Wins (Steps 1-4)**
$320–$370
==Save ~$180–$230/mo==
**After Full Optimization**
$240–$290
==Save ~$260–$310/mo==

------
**1. Cost Increase**

- Total cost increased from **$412 → $552 (+34%)**
- Main reason: **RDS (database cost increased by ~$139)**

**2. Root Cause**

- Two **new RDS instances** were added in Feb (likely test/staging)
- **MySQL 5.7** is outdated → extra **$42/month support cost**
- Database size/config likely **overpowered for our PHP app**

**3. Key Savings Opportunities**

- Remove unused RDS instance → save **$80–$115/month**
- Upgrade MySQL → save **$42+/month**
- Right-size database → save **$40–$80/month**
- Reduce WorkMail users → save **$20–$50/month**

👉 **Total possible savings: ~$260–$300/month**

**4. Security Risks (Important)**

- No **MFA enabled** → high risk
- **RDS not encrypted**
- **No Multi-AZ** → downtime risk
- **CloudTrail not enabled** → no activity logs

**5. Action Plan**

- Enable MFA + security fixes (immediate)
- Remove unnecessary RDS instances
- Upgrade MySQL to 8.0
- Optimize database size
- Set budget alerts to avoid future spikes