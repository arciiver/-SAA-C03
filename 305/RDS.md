# RDS

- Postgres
- Mysql
- MariaDB
- Oracle
- Micsoroft sql server
- IBM DB2
- Aurora 

## RDS – Storage Auto Scaling

- Helps you increase storage on your RDS DB instance dynamically.
- When RDS detects you are running out of free database storage, it scales automatically.
- Avoid manually scaling your database storage.
- You have to set **Maximum Storage Threshold** (maximum limit for DB storage).

### Automatically modify storage if:
- Free storage is less than 10% of allocated storage.
- Low-storage lasts at least 5 minutes.
- 6 hours have passed since last modification.


## RDS Read Replicas

- Up to 15 Read Replicas.
- Within AZ, Cross AZ, or Cross Region.
- Replication is **ASYNC**, so reads are eventually consistent.
- Replicas can be promoted to their own DB.
- Applications must update the connection string to use the read replicas.

## RDS NETWORK COST
#### Same region replication
In AWS there is a newtwork cost when data goes from one AZ to another. 
- ***For RDS Read Replicas within the same region, you dont pay that fee***
#### Cross region replication
You pay for the network trafic for cross region replication


## RDS MULTI AZ (Disaster recovery)

One DNS name, automatic app failover to standby
- **PS** Even read replicas can be setup as multi az for disaster recovery. But that will not happen auto like MULTI AZ SETUP


***QUESTION** How to enable rds from single az to multi-az
- No downtime
- Just click on modify and enable multi-AZ


## RDS Custom

- Managed Oracle and Microsoft SQL Server Database with OS and database customization.
- **RDS:** Automates setup, operation, and scaling of the database in AWS.
- **Custom:** Access to the underlying database and OS so you can:
  - Configure settings.
  - Install patches.
  - Enable native features.
  - Access the underlying EC2 instance using SSH or SSM Session Manager.

- **De-activate Automation Mode** to perform your customization, it’s better to take a DB snapshot before.

### RDS vs. RDS Custom
- **RDS:** Entire database and the OS to be managed by AWS.
- **RDS Custom:** Full admin access to the underlying OS and the database.



## Amazon Aurora

- Aurora is a proprietary technology from AWS (not open sourced).
- **Postgres and MySQL** are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database).
- Aurora is **AWS cloud optimized** and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS.
- Aurora storage automatically grows in increments of 10GB, up to 128TB.
- Aurora can have up to 15 replicas and the replication process is faster than MySQL (sub 10 ms replica lag).
- **Failover** in Aurora is instantaneous. It’s HA native.
- Aurora costs more than RDS (20% more) – but is more efficient.

- **Shared storage volume for all replicas, thats auto expanding from 10G to 128 TB**


## Aurora High Availability and Read Scaling

- **6 copies of your data across 3 AZs:**
  - 4 copies out of 6 needed for writes
  - 3 copies out of 6 needed for reads
  - Self-healing with peer-to-peer replication
  - Storage is striped across 100s of volumes
- **One Aurora Instance** takes writes (master)
- **Automated failover** for master in less than 30 seconds
- **Master + up to 15 Aurora Read Replicas** serve reads
- **Support for Cross Region Replication**
- **There is a lb callaed writer endpoint thats is pointing to the master. For clients that wants to write**
- **You can have asg for read replicas, and there  is a reader endpoint lb for read connection load balancing**

## Aurora auto scaling policy

- Based on average cpu utilization of aurora replicas
- average connections of aurora replicas
### Aurora Features

- Automatic fail-over
- Industry compliance
- Push-botton scaling
- Automated patching with Zero Downtime
- Backtrack: Restore data at any point of time without using backups 



## AUROA CUSTOM ENDPOINTS

Regarding the reader endpoint, instead of having only one reader endpoint for all your read replicas,
You can have custom endpoints for different subnet of aurora instances. Tex bigger instances for 
analytical queries
 

### SERVERLESS AURORA
You define min and max capacity and your instances gets autoscaled based on actual usage. 
- Good for infrequent intermittent or unpredictable workloads
- No capacity planing
- pay per second, can be more cost-effective



## Global Aurora

- **Aurora Cross Region Read Replicas:**
  - Useful for disaster recovery
  - Simple to put in place

- **Aurora Global Database (recommended):**
  - 1 Primary Region (read / write)
  - Up to **5 secondary** (read-only) regions, replication lag is less than 1 second
  - Up to **16 Read Replicas** per secondary region
  - Helps decrease latency
  - Promoting another region (for disaster recovery) has an RTO of < 1 minute
  - Typical cross-region replication lag less than 1 second


## Aurora Machine Learning

- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services

### Supported services:
- **Amazon SageMaker** (use with any ML model)
- **Amazon Comprehend** (for sentiment analysis)

- You don’t need to have ML experience

### Use cases:
- Fraud detection
- Ads targeting
- Sentiment analysis
- Product recommendations


## RDS Backups

### Automated backups:
- Daily full backup of the database (during the backup window)
- Transaction logs are backed-up by RDS every 5 minutes
- => ability to restore to any point in time (from oldest backup to 5 minutes ago)
- 1 to 35 days of retention, set 0 to disable automated backups

### Manual DB Snapshots:
- Manually triggered by the user
- Retention of backup for as long as you want

### Trick:
- In a stopped RDS database, you will still pay for storage. 
- If you plan on stopping it for a long time, you should snapshot & restore instead


## AURORA BACKUPS

- AUTOMATED 1- to 35 days (cannot be disables like RDS)
- Point-in-time recovery in that timeframe
- Manually triggered backup by the user
- Retention of backup for as long as you want


## RDS & Aurora Restore Options

- Restoring a RDS / Aurora backup or a snapshot creates a new database.

### Restoring MySQL RDS database from S3:
- Create a backup of your on-premises database.
- Store it on Amazon S3 (object storage).
- Restore the backup file onto a new RDS instance running MySQL.

### Restoring MySQL Aurora cluster from S3:
- Create a backup of your on-premises database using Percona XtraBackup.
- Store the backup file on Amazon S3.
- Restore the backup file onto a new Aurora cluster running MySQL.



## Aurora Database Cloning

- Create a new Aurora DB Cluster from an existing one.
- Faster than snapshot & restore.
- Uses **copy-on-write** protocol:
    - Initially, the new DB cluster uses the same data volume as the original DB cluster (fast and efficient – no copying is needed).
    - When updates are made to the new DB cluster data, then additional storage is allocated and data is copied to be separated.
- Very fast & cost-effective.
- Useful to create a "staging" database from a "production" database without impacting the production database.

## RDS & Aurora Security

- **At-rest encryption**:
  - Database master & replicas encryption using AWS KMS – must be defined at launch time.
  - If the master is not encrypted, the read replicas cannot be encrypted.
  - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted.

- **In-flight encryption**:
  - TLS-ready by default, use the AWS TLS root certificates client-side.

- **IAM Authentication**:
  - IAM roles to connect to your database (instead of username/password).

- **Security Groups**:
  - Control Network access to your RDS / Aurora DB.

- **No SSH available** except on RDS Custom.

- **Audit Logs** can be enabled and sent to CloudWatch Logs for longer retention.



## Amazon RDS Proxy

- Fully managed database proxy for RDS.
- Allows apps to pool and share DB connections established with the database.
- Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts).
- Serverless, autoscaling, highly available (multi-AZ).
- Reduced RDS & Aurora failover time by up to 66%.
- Supports RDS (MySQL, PostgreSQL, MariaDB, MS SQL Server) and Aurora (MySQL, PostgreSQL).
- No code changes required for most apps.
- Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager.
- RDS PROXY is never publicly accessible, only from the vpc

**USE CASE TEX: IF u have several lambdas thats connects to to your rds instances, it will be a mess with openconnections and timeouts. sol? rds proxy**



## ELASTICACHE
    Purpose: ElastiCache is to caching (Redis/Memcached) what RDS is to relational databases.
    Caching Benefits: Caches are in-memory databases, which provide fast access and are excellent for read-heavy workloads.
    Database Offloading: Helps reduce the load on your databases, particularly for frequently accessed data, thereby improving performance.
    Stateless Applications: It also helps in making applications stateless by storing session data or other temporary data in the cache.
    Managed Service: AWS manages maintenance, patching, optimizations, and monitoring of ElastiCache, ensuring high availability and reliability.
    Development Impact: Using ElastiCache involves changes in application code, as you need to implement caching mechanisms in your applications for leveraging in-memory storage.

This makes ElastiCache ideal for scenarios like web session stores, real-time analytics, and caching frequently accessed database queries. However, the key takeaway is that integrating ElastiCache requires adapting your application to use the caching layer effectively.



### ElastiCache – Redis vs Memcached

| **Redis**                               | **Memcached**                         |
|-----------------------------------------|---------------------------------------|
| Multi-AZ with Auto-Failover             | Multi-node for partitioning of data (sharding) |
| Read Replicas to scale reads and have high availability | No high availability (replication) |
| Data Durability using AOF persistence   | Non-persistent                        |
| Backup and restore features             | No backup and restore                 |
| Supports Sets and Sorted Sets           | Multi-threaded architecture           |



> **ELASTICACHE supports IAM authentication only for Redis**
> **Memcached supports SASL-based authentication**

### Patterns for ElastiCache

- **Lazy Loading**: All the read data is cached, data can become stale in cache.
- **Write Through**: Adds or updates data in the cache when written to a DB (no stale data).
- **Session Store**: Store temporary session data in a cache (using TTL features).

### ElastiCache – Redis Use Case

- **Gaming Leaderboards** are computationally complex.
- **Redis Sorted Sets** guarantee both uniqueness and element ordering.
- Each time a new element is added, it's ranked in real-time, then added in the correct order.





Important ports:

    FTP: 21

    SSH: 22

    SFTP: 22 (same as SSH)

    HTTP: 80

    HTTPS: 443 

vs RDS Databases ports:

    PostgreSQL: 5432

    MySQL: 3306

    Oracle RDS: 1521

    MSSQL Server: 1433

    MariaDB: 3306 (same as MySQL)

    Aurora: 5432 (if PostgreSQL compatible) or 3306 (if MySQL compatible)