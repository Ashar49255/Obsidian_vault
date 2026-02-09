# **AWS S3 (Simple Storage Service) – Quick Reference**

## **1. Overview**
- Object storage service in AWS for storing/retrieving any amount of data.
- Highly **durable (11 nines)**, **scalable**, and **secure**.
- Use cases: backups, media storage, static websites, data archiving, big data.

---
## **2. Key Concepts**

|Term|Description|
|---|---|
|**Bucket**|Container for objects, globally unique name.|
|**Object**|File stored in S3 (data + metadata + key).|
|**Key**|Unique identifier for an object inside a bucket.|
|**Region**|AWS region where bucket resides.|
|**Versioning**|Keep multiple versions of an object.|
|**Storage Class**|Determines cost & access frequency (Standard, IA, Glacier, etc.)|

---

## **3. Storage Classes**
- **Standard** → Frequent access, low latency
- **Intelligent-Tiering** → Auto-tier frequent/infrequent access
- **Standard-IA** → Infrequent access, cheaper
- **One Zone-IA** → Single AZ, cheaper
- **Glacier / Deep Glacier** → Archival, long-term, low cost

---

## **4. Features**
- **Durability:** 99.999999999%
- **Scalability:** Auto-scales storage
- **Security:** IAM, bucket policies, ACLs, encryption (SSE)
- **Lifecycle Policies:** Auto-transition/delete objects
- **Event Notifications:** Trigger Lambda, SQS, or SNS

---

## **5. Common Commands (AWS CLI)**

 Upload file aws s3 cp file.txt s3://mybucket/
List objects aws s3 ls s3://mybucket/
Delete object aws s3 rm s3://mybucket/file.txt 
Sync folder aws s3 sync ./local-folder s3://mybucket/`


---

## **6. Integration with AWS Services**
- **CloudFront** → CDN for S3 content
- **Lambda** → Triggered by object events
- **Glacier** → Archival storage
- **Athena** → Query data in S3 using SQL
- **Backup** → Automated backups to S3
- **RDS / Redshift** → Export snapshots to S3

---

## **7. Security Options**
- **Bucket Policies** → JSON access control
- **IAM Policies** → User/group permissions
- **ACLs** → Object-level permissions
- **Encryption** → SSE-S3, SSE-KMS, SSE-C
- **Public Access Block** → Prevent public exposure

---

✅ **Summary:**  
S3 is a **durable, scalable, secure object storage** service, central to most AWS storage and data workflows. Integrates with multiple AWS services for content delivery, analytics, and backup.