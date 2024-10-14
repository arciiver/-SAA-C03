Global aurora Typical cross-region replication for aurora taskes less than 1 second

### Trick:
- In a stopped RDS database, you will still pay for storage. 
- If you plan on stopping it for a long time, you should snapshot & restore instead

- Creating a new Aurora DB Cluster from an existing one using aurora database cloning is faster tha´n taking snapshot and restore

- A way to enforce IAM authentication for your databases, its amazon rds proxy


-To ensure a replica of your database is available in another AWS Region for disaster recovery, Amazon Aurora Global Database is highly recommended. 
While Amazon RDS does offer cross-region read replicas for disaster recovery, Aurora Global Database provides a more robust and purpose-built solution for high availability and disaster recovery across multiple AWS regions


- RDS read replica uses Asynchronous replication and multi-az uses synchronous replication
Key Differences:

    RDS Read Replica (Asynchronous):
        Use case: Read scaling
        Replication: Asynchronous
        Data loss: There can be a small replication lag, so it’s possible that in the event of a failure, the replica might not have the latest changes.
    RDS Multi-AZ (Synchronous):
        Use case: High availability and disaster recovery
        Replication: Synchronous
        Data loss: No data loss because the replication is synchronous. Both primary and standby databases always have the same data.

These two replication mechanisms serve different purposes: RDS read replicas are for scaling reads, while Multi-AZ deployments are for ensuring high availability and durability of the database.


- You have an un-encrypted RDS DB instance and you want to create Read Replicas. Can you configure the RDS Read Replicas to be encrypted? NO

Aurora minimizing cost = Use aurora serverless

- Aurora only supports Mysql and postgresql

- Automated backups has only max retencion for 35 days


- Route 53 healthcheck can healthcheck other healtcheck

You have enabled versioning in your S3 bucket which already contains a lot of files. Which version will the existing files have? =  Null

You have enabled versioning in your S3 bucket which already contains a lot of files. Which version will the existing files have?


> S3 requester pays. Authenticated aws accounts pays for the network trafic when requesting objects your a bucket. 

> Using S3 batch operations u can encrypt un-encrypted data or restore objects from S3 glacier

> While you're uploading large files to an S3 bucket using Multi-part Upload, there are a lot of unfinished parts stored in the S3 bucket due to network issues. You are not using these unfinished parts and they cost you money. What is the best approach to remove these unfinished parts? = S3 lifecycle policy to automate old/unfinished parts deletion

> You are looking to get recommendations for S3 Lifecycle Rules. How can you analyze the optimal number of days to move objects between different storage tiers? S3 analytics

> You have a large dataset stored on-premises that you want to upload to the S3 bucket. The dataset is divided into 10 GB files. You have good bandwidth but your Internet connection isn't stable. What is the best way to upload this dataset to S3 and ensure that the process is fast and avoid any problems with the Internet connection? USE s3 multipart upload and s3 transfer acceleration

>S3 Transfer Acceleration is a feature of Amazon S3 that speeds up the upload and download of objects over long distances by utilizing Amazon CloudFront’s globally distributed network of edge locations. This feature is particularly useful if you have users or applications located far from your S3 bucket's region, as it significantly reduces the time it takes to transfer data to and from S3.

You would like to retrieve a subset of your dataset stored in S3 with the .csv format. You would like to retrieve a month of data and only 3 columns out of 10, to minimize compute and network costs. What should you use? S3 select


>Multi-Part Upload is recommended as soon as the file is over 100 MB


> Bucket policies are avaliated before "Default Encryption"

> You can analyze s3 access logs with athena

> Which of the following S3 Object Lock configuration allows you to prevent an object or its versions from being overwritten or deleted indefinitely and gives you the ability to remove it manually? Legal hold : Because there is a retention period for governance and compliance mode..

'


Differences Between EFS Multi-Attach and EBS io2 Multi-Attach:
1. Type of Storage and Use Case:

    EFS (Elastic File System):
        Type: Fully managed network file system.
        Use Case: Designed for shared file storage across multiple instances. Suitable for use cases requiring a shared, scalable file system, such as web servers, content management systems, or shared storage for containerized applications.

    EBS io2 (Multi-Attach):
        Type: Block storage device.
        Use Case: Intended for high-performance applications that require consistent low-latency access to raw block storage. Suitable for clustered databases, distributed file systems, and applications needing high IOPS and low latency.

- ebs io2 can be attached to instances from the same az but efs can be mounted to instances i hole region? YES

- Cloudfront can use s3,alb or ec2 as origin
