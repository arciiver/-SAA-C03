> Showball cannot import to glacier directly, You must use s3 first, than lifecycle policy

## AWS FSx for windows file server
- AWS FSx for windows file server, can be mounted on linux ec2 instances


## AWS FSx for Lustre

- **Machine learning: High Performance Computing (HPC)**

**EXAM Seamless integration with S3**

- Can "read S3" as file system via FSx
- Can write output of the computations back to S3 via FSx
- Can be used from on-premises servers via VPN or Direct Connect



# Datasync
Replication with aws datasync, file permission and metadata are preserved (NFS POSIX,SMB)
> EXAM BAD NETWORK CAP, SO THAT AGENT CANT SYNC. AWS SNOWCONE HAS AGENT PRE-INSTALLED


>Which AWS service is best suited to migrate a large amount of data from an S3 bucket to an EFS file system? AWS DATASYNC

> So anytime at the exam you see a way to optimize the number of API calls made to an SQS queue and decrease of latency, think about long polling.


>Sns can have both sqs standard and FIFO queues as subscripbers

Here is the content from the image in **Markdown** format:


```markdown
Amazon FSx for Lustre is designed for fast processing of workloads like machine learning and high-performance computing. It can be linked directly to data in Amazon S3.
```

```markdown
Which Amazon FSx service would be most appropriate for a Linux-based workload that requires advanced data management features like snapshots, cloning, and data compression?

A. Amazon FSx for OpenZFS
```


```markdown
Choose FSx when you need a fully managed file system with specific features and protocol support.
Use Storage Gateway when you need to integrate on-premises applications with AWS storage without significant changes to your existing infrastructure.
```


```markdown
Kinesis Data Firehouse - NEAR REAL-TIME
```
```markdown
ECS - Task Role is defined in the **task definition**.

```

> Amason s3 can NOT be mounted as a file system on ECS

> LAMBDA  upp to 10 GB RAM per function - Increasing RAM will also improve CPU and network