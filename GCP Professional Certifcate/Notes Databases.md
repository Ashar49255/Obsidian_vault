- **BigQuery** → Optimized for analytics, not transactional low-latency workloads.
- **Firestore** → Offers multi-region support but not the same level of strong global consistency as Spanner.
- **Cloud SQL** → Regional service; lacks global distribution and requires manual replication.
- **Spanner** -->   a **globally distributed, strongly consistent, low-latency database**, **Cloud Spanner** is the best choice.
# Google Cloud Databases – Quick Notes

## 1. Cloud Spanner

### Remember:
Global + strong consistency = Spanner
### Working:
- Distributed relational database across multiple regions
- Uses synchronous replication
- Provides strong consistency using TrueTime
- Scales horizontally automatically
### Use:
- Financial systems
- Global applications requiring high availability and consistency
### Cost:
Most expensive
- Charged per node and storage
- Always running, high operational cost
---
## 2. BigQuery
### Remember:
Analytics = BigQuery
### Working:
- Serverless data warehouse
- Uses columnar storage
- Runs SQL queries on large datasets
### Use:
- Reporting and analytics
- Business intelligence
### Cost:
Medium (usage-based)
- Pay per query (data scanned)
- Storage cost is relatively low

---
## 3. Firestore
### Remember:
Mobile apps and real-time apps = Firestore
### Working:
- NoSQL document database
- Stores data as collections and documents
- Supports real-time synchronization
- Automatically scales
### Use:
- Mobile and web applications
- Real-time features like chat
### Cost:
Cheap to medium
- Pay per read, write, and storage
- Low cost for small usage, increases with traffic

---
## 4. Cloud SQL
### Remember:
Traditional relational database = Cloud SQL
### Working:
- Managed MySQL, PostgreSQL, SQL Server
- Runs in a single region
- Supports read replicas (asynchronous replication)
### Use:
- Standard web applications
- Migration from on-premises databases
### Cost:
Medium
- Pay for instance uptime and storage
- Cheaper than Spanner, more predictable than Firestore

---
# Cost Ranking (Most Expensive to Cheapest)

1. Spanner (most expensive)
2. Cloud SQL (medium)
3. BigQuery (depends on usage)
4. Firestore (usually cheapest for small workloads)

---
# Final Memory Summary

- Global, strongly consistent applications: Spanner
- Analytics and large-scale queries: BigQuery
- Real-time mobile and web apps: Firestore
- Traditional SQL workloads: Cloud SQL
---------------

Cloud Interconnect provides high bandwidth and low latency. It does need encryption at the application level.

