## EC2 Purchasing Options:
- **On-Demand**
- **Reserved:**
  - Reserved Instances - Long time.
  - Convertible Reserved Instances - Long workloads with flexible instances.
- **Savings Plans:** 1-3 year commitment to an amount of usage, long workloads.
- **Spot Instances:** Short workloads, can lose instances, less reliable.
- **Dedicated Hosts:** Book an entire physical server, control instance placement.
- **Dedicated Instances:** No other customers will share your hardware.
- **Capacity Reservations:** Reserve capacity in a specific AZ for any duration.

## Billing:
- **On-Demand:** Billing per second, after the first minute.
- **Reserved:** Up to 72% discount. You reserve specific instance attributes (Instance type, Region, Tenancy, OS).
  - Payment Options: No Upfront + , Partial Upfront ++, All Upfront +++.
- **Convertible Reserved Instances:** Up to 66% discount. Can change instance attributes.
- **EC2 Savings Plans:**
  - Get a discount based on long-term usage (up to 72% - same as RIs).
  - Commit to a certain type of usage ($10/hour for 1 or 3 years).
  - Usage beyond EC2 Savings Plans is billed at the On-Demand price.
  - Locked to a specific instance family & AWS region (e.g., M5 in us-east-1).
- **Spot Instances:** Up to 90% discount compared to on-demand.
  - Most cost-efficient instances in AWS.
  - Not suitable for critical jobs or databases.

## EC2 Dedicated Hosts:
- A physical server with EC2 instance capacity fully dedicated to your use.
- Allows you to address compliance requirements and use your existing server-bound software licenses (per-socket, per-core, per-VM software licenses).
- **Purchasing Options:**
  - On-Demand: Pay per second for active Dedicated Host.
  - Reserved: 1 or 3 years (No Upfront, Partial Upfront, All Upfront).
- The most expensive option.
- Useful for software that has a complicated licensing model (BYOL – Bring Your Own License) or for companies that have strong regulatory or compliance needs.

### PS:
- Dedicated Instances mean that you have your own instance on your own hardware, whereas Dedicated Hosts give you access to the physical server itself and provide visibility into the lower-level hardware.
- **Dedicated Hosts:** You get full visibility into the underlying physical server. You have control over the physical server, including the number of cores and sockets. This makes it easier to address licensing compliance issues, as you can bring your own licenses (BYOL) for applications that require per-socket or per-core licensing.
- **Dedicated Instances:** You don’t have visibility into the underlying hardware (cores, sockets, etc.), but you are assured that your instances are running on dedicated physical servers that are not shared with other AWS customers.

## EC2 Capacity Reservations:
- Reserve On-Demand instances capacity in a specific AZ for any duration.
- You always have access to EC2 capacity when you need it.
- No time commitment (create/cancel anytime), no billing discounts.
- Combine with Regional Reserved Instances and Savings Plans to benefit from billing discounts.
- You’re charged at On-Demand rate whether you run instances or not.
- Suitable for short-term, uninterrupted workloads that need to be in a specific AZ.

### PS:
- Spot Fleet is a set of Spot Instances and optionally On-Demand Instances. It allows you to automatically request Spot Instances with the lowest price.

## Elastic IP:
- If you need to have a fixed public IP for your instance, you need to use an Elastic IP. Otherwise, your instances can change their public IP.
- Overall, try to avoid using Elastic IP.
  - Instead, use a random public IP and register a DNS name to it OR use a load balancer and don't use a public IP for EC2 instances at all.
- **Price:** Hourly charge for in-use and idle is the same.

## Placement Groups:
- Sometimes you want control over the EC2 Instance placement strategy.
- That strategy can be defined using placement groups.
- When you create a placement group, you specify one of the following strategies for the group:
  - **Cluster:** Clusters instances into a low-latency group in a single Availability Zone.
  - **Spread:** Spreads instances across underlying hardware (max 7 instances per group per AZ) – critical applications. Limited to 7 instances per AZ per placement group.
  - **Partition:** Spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to hundreds of EC2 instances per group (Hadoop, Cassandra, Kafka).
- **LIMIT:** Up to 7 partitions per AZ and up to hundreds of EC2 instances.

** 
Spread 
Goal: Maximum isolation between instances.
Use Case: Designed for small groups of critical instances (up to 7 per Availability Zone) where high availability is crucial, and you want to reduce the risk of hardware failure impacting multiple instances at once.
Limit: Supports a maximum of 7 running instances per Availability Zone
Partition 
Goal: Fault tolerance across partitions of instances.
Use Case: Designed for large distributed workloads like Hadoop, HDFS, Cassandra, or other distributed applications where fault domains (partitions) are important.
Limit: You can have up to 7 partitions per Availability Zone. There is no limit on the number of instances within each partition.
**
## EC2 Hibernate:
- **Introducing EC2 Hibernate:**
  - The in-memory (RAM) state is preserved.
  - The instance boot is much faster! (The OS is not stopped/restarted).
  - Under the hood: The RAM state is written to a file in the root EBS volume.
  - The root EBS volume must be encrypted.

### Use Cases:
- Long-running processing.
- Saving the RAM state.
- Services that take time to initialize.

## Storage Options:

## EBS:
- Elastic Block Store (EBS) volume is a network drive you can attach to your instances while they run.
- They are bound to a specific availability zone.
- There is a multi-attach feature (several EC2 instances) for some EBS.
- You have to do a snapshot of the existing volume in order to move it to other regions.
- **Billing:** You get billed for all the provisioned capacity.

### Controls the EBS behavior when an EC2 instance terminates:
- By default, the root EBS volume is deleted (attribute enabled).
- By default, any other attached EBS volume is not deleted (attribute disabled).
- This can be controlled by the AWS console / AWS CLI.

### Use Case:
- Preserve root volume when the instance is terminated.
***

### EC2 Instance Store

- EBS volumes are network drives with good but **"limited"** performance.
- **If you need a high-performance hardware disk, use EC2 Instance Store**.

### Key Points:
- Better I/O performance.
- EC2 Instance Store loses their storage if they're stopped (ephemeral).
- Good for buffer / cache / scratch data / temporary content.
- Risk of data loss if hardware fails.
- Backups and Replication are your responsibility.


***
### EBS Volume Types

- **EBS Volumes come in 6 types:**
  - **gp2 / gp3 (SSD):** General purpose SSD volume that balances price and performance for a wide variety of workloads.
  - **io1 / io2 Block Express (SSD):** Highest-performance SSD volume for mission-critical, low-latency, or high-throughput workloads. Provision ios
  - **st1 (HDD):** Low-cost HDD volume designed for frequently accessed, throughput-intensive workloads.
  - **sc1 (HDD):** Lowest-cost HDD volume designed for less frequently accessed workloads.

- **EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)**

- **ONLY gp2/gp3 and io 1/io 2 block extpress can be used as boot volumes**



### EBS Volume Types Use Cases

#### General Purpose SSD GP2/GP3

- **Cost effective storage, low-latency**
- **Use Cases:**
  - System boot volumes
  - Virtual desktops
  - Development and test environments
  - Low latency interactive applications
  - Boot volumes
  - Transactional workloads

- **Volume size:** 1 GiB - 16 TiB

#### gp3:
- Baseline of 3,000 IOPS and throughput of 125 MiB/s.
- Can increase IOPS up to 16,000 and throughput up to 1,000 MiB/s independently.

#### gp2:
- Small gp2 volumes can burst IOPS to 3,000.
- Size of the volume and IOPS are linked, max IOPS is 16,000.
- 3 IOPS per GB, means at 5,334 GiB you reach the max IOPS.

#### Provisioned IOPS (PIOPS) SSD
 **Use Cases:**
- **Critical business applications** with sustained IOPS performance.
- Or applications that need more than 16,000 IOPS.
- Great for **database workloads** (sensitive to storage performance and consistency).
- Workloads that requiere submilisecond latency

#### io1 (4 GiB - 16 TiB):
- Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other instances.
- Can increase PIOPS independently from storage size.

#### io2 Block Express (4 GiB - 64 TiB):
- Sub-millisecond latency.
- Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1.

- Supports **EBS Multi-attach**.
***


#### st1 HDD
- Big data
- Data warehouses
- Log proccessing

#### Sc1 Cold HDD
- Scenarios where the lowest storage cost is important
- Throughput-oriented storage for data that is infrequently accessed


### EBS Multi-Attach – io1/io2 family

- Attach the same EBS volume to multiple EC2 instances in the same AZ.
- Each instance has full read & write permissions to the high-performance volume.

#### Use cases:
- Achieve **higher application availability** in clustered Linux applications (e.g., Teradata).
- Applications must manage concurrent write operations.

- **Up to 16 EC2 Instances at a time**.

- **Must use a file system that’s cluster-aware (not XFS, EXT4, etc.).**


### EBS SNAPSHOTS FEATURES:

    EBS Snapshot Archive
        Move a Snapshot to an "archive tier" that is 75% cheaper.
        Takes within 24 to 72 hours for restoring the archive.

    Recycle Bin for EBS Snapshots
        Set up rules to retain deleted snapshots so you can recover them after an accidental deletion.
        Specify retention (from 1 day to 1 year).

    Fast Snapshot Restore (FSR)
        Force full initialization of snapshot to have no latency on the first use ($$$)


### AMI Process (from an EC2 instance)

- Start an EC2 instance and customize it.
- Stop the instance (for data integrity).
- Build an AMI – this will also create EBS snapshots.
- Launch instances from other AMIs.

---

**Illustration:**

- **US-EAST-1A** (Custom AMI creation)
  - Create AMI ➡ **Custom AMI**
  - Launch from AMI ➡ **US-EAST-1B**




### EBS ENCRYPTION

- LEVERAGES KEYS FROM KMS **AES-256**


### Encryption: Encrypt an unencrypted EBS volume

Copying snaphot enables to encryt unencrypted volume

1. Create an EBS snapshot of the volume.
2. Encrypt the EBS snapshot (using copy).
3. Create a new EBS volume from the snapshot (the volume will also be encrypted).
4. Now you can attach the encrypted volume to the original instance.



### Amazon EFS – Elastic File System

- Managed NFS (Network File System) that can be mounted on many EC2 instances.
- EFS works with EC2 instances in multi-AZ.
- Highly available, scalable, expensive (3x gp2), pay per use.

### EFS Performance Thoughput Mode
- Enhanges
  - Elastic (recomended) Use with unpredictable I/O
  - Provisioned - If u can estimate application Thoughput
- Bursting

### EFS Performance MODE
- General Purpose: Idial for high performance and latency application
- MAX I/O: Designed for highly parallelized workloads

### EFS Storage Classes 

### Storage Tiers (lifecycle management feature – move file after N days)
- **Standard:** For frequently accessed files.
- **Infrequent access (EFS-IA):** Cost to retrieve files, lower price to store.
- **Archive:** Rarely accessed data (few times each year), 50% cheaper.
- Implement **lifecycle policies** to move files between storage tiers.

### Availability and Durability
- **Standard:** Multi-AZ, great for production.
- **One Zone:** One AZ, great for development. Backup enabled by default, compatible with IA (EFS One Zone-IA).


EBS vs EFS
EBS:
 - One instance (except multi-attach io 1/io 2) Dock inom samma AZ
EFS:
 - Mounting 100s of intances across AZ
 - Only for linux instances posix


 **PS:** When creating EC2 instances, you can only use the following EBS volume types as boot volumes: gp2, gp3, io1, io2, and Magnetic (Standard).