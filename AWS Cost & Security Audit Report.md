**Project:** RealKnu (6297-4555-4024)  
**Date:** March 25, 2026

---

## **1. Executive Summary**

This report analyzes AWS usage, cost trends, and security posture for the RealKnu project between **Dec 2025 – Feb 2026**, focusing on **cost optimization (FinOps) and security best practices**.

**Key Highlights:**

- Total AWS cost increased **$411.76 → $552.02 (34%)**.
- Major cost drivers: **RDS instances and MySQL extended support**.
- Minor fluctuations in EC2, ELB, VPC, Route 53.
- Security gaps: **RDS encryption disabled, Multi-AZ disabled, IAM MFA not enforced**.
![[Screenshot from 2026-03-25 16-30-56.png]]
    Cost Comparison Graph (Dec 2025 → Feb 2026).

---

## **2. Scope & Objectives**

**Scope:**

- AWS services analyzed: RDS, EC2, ELB, VPC, Route 53, WorkMail.
- Focus: **month-over-month cost and usage**.
- Identify **opportunities for cost reduction and security improvements**.

**Objectives:**

- Identify high-cost drivers.
- Recommend **FinOps strategies** to reduce cost.
- Recommend **security improvements** without making changes.

---

## **3. Methodology**

- Data sources: **AWS Cost Explorer, RDS Dashboard, IAM Console**
- Comparison: **Dec 2025 → Jan 2026 → Feb 2026**
- Only **read-only analysis**, no changes made.

---

## **4. Observations & Evidence**

### **4.1 Cost Observations**

|Service / Instance|Dec 2025|Jan 2026|Feb 2026|Observation / Evidence|
|---|---|---|---|---|
|**RDS - db.m7g.large**|$0.00|$0.12|$76.67|Usage spike: 0 → 227.52 Hrs → Major cost driver|
|**RDS - db.m5.large**|$0.00|$0.00|$36.49|Usage spike: 0 → 213.40 Hrs|
|**RDS - Extended Support MySQL5.7**|$0.00|$0.00|$42.68|Legacy support; vCPU-hour: 0 → 426.80|
|EC2 - Other|$12.00|$13.01|$21.00|Minor increase|
|EC2-Instances|$69.04|$69.04|$62.36|Minor decrease|
|ELB|$17.09|$16.99|$15.33|Slight decrease|
|VPC|$19.43|$19.42|$20.19|Stable|
|Route 53|$0.52|$0.53|$0.61|Stable|
|WorkMail|$60.00|$60.00|$60.00|Stable|
![[Screenshot from 2026-03-25 16-58-02.png]]

RDS Cost Explorer / Month-over-Month cost trend.

**Observation:**

- **RDS instances** caused **$138.87 increase (67.85%)**, major contributor.
- Other services stable.

---

### **4.2 Security Observations**

|Area|Observation|Evidence|Risk Level|
|---|---|---|---|
|RDS Encryption|Disabled|RDS Dashboard → Encryption: Disabled|🔴 High|
|RDS Multi-AZ|Disabled|RDS Dashboard → Multi-AZ: No|🔴 High|
|IAM Users|MFA not enforced|IAM Console → MFA: Not enabled|🟡 Medium|
|Root/Admin usage|High privileges|Root account exists|🔴 High|
![[Screenshot from 2026-03-25 16-36-46.png]]

RDS Dashboard showing encryption & Multi-AZ settings.

---

## **5. Analysis / Interpretation**

- **RDS spikes and extended support** drove cost **$138.87 (67.85%)** increase.
- Minor services stable; optimization focus: **RDS instance management**.
- Security gaps could cause operational/compliance risks.

---

## **6. Recommendations**

### **6.1 Cost Optimization (FinOps)**

1. **Right-size RDS instances** → reduce over-provisioning.
2. **Terminate idle/temporary instances** (db.m7g.large, db.m5.large).
3. **Review extended support costs** → upgrade DB engine.
4. **Enable RDS auto-scaling** → lower costs during low usage.
5. **Reserved Instances / Savings Plans** for predictable workloads.
6. **AWS Budgets / Cost Anomaly Detection** → alert on unexpected spikes.

### **6.2 Security Best Practices**

1. Enable **RDS encryption at rest**.
2. Enable **Multi-AZ** for production DBs.
3. Enforce **MFA for all IAM users**.
4. Limit **root/admin usage**, use IAM roles for daily operations.
5. Enable **CloudTrail & CloudWatch logging** for auditing.

---

## **7. Conclusion**

- AWS total cost increased **34% (Dec → Feb)** mainly due to **RDS usage spikes and legacy support**.
- **FinOps opportunities:** right-size instances, terminate idle resources, use reserved capacity, monitor anomalies.
- **Security improvements:** encryption, Multi-AZ, IAM MFA, root account management.
