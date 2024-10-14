# Cloudfront price classes

- Cloudfornt edge locations are all around the world
- Because of that data out per edge location varies
**You can reduce the number of edge locations for cost reduction**
    - Price Class All: all regions - best performance
    - Price Class 200: Most regions but excludes the most expensive regions
    - Price Class 100: only the least expensive regions
    
# Aws snow family

## Snowcone
8 TB HDD - 14 TB SSD

## Snowball Edge
80 TB - 210 TB

> If it takes more than a week to tranfer over the network, use snowball devices.
> - It can even run edge computing like ec2 or lambda at the edge, like AWS Lambda to perform local processing on the data stored on the device.
>- Showball cannot import to glacier directly, You must use s3 first, than lifecycle policy

**-**
## Amazon FSx
- **Launch 3rd party high-performance file systems on AWS**
- **Fully managed service**

## Amazon FSx Services:
- **FSx for Lustre**
    Lustre is derived from Linux and cluster
    **Machine learning: High Performance Computing HPC**
    **EXAM Seamless intregration with s3**
        - Can "read s3" as file system via FSx
        - Can write output of the computations back to s3 via FSx
        - Can be used from on-premises servers via VPN or Direct Connect
- **FSx for Windows File Server**
    - Can be mounted on Linux ec2 intances
    - Supports microsofts disributed file system (DFS) Namespaces
    - Can be used from on-premises servers via VPN or Direct Connect
- **FSx for NetApp ONTAP**
- **FSx for OpenZFS**


# FSx File System Deployment Options

## Scratch File System
- **Temporary storage**
- **Data is not replicated** (doesn’t persist if file server fails)
- High burst (6x faster, 200MBps per TiB)
- **Usage**: Short-term processing, optimize costs

## Persistent File System
- **Long-term storage**
- **Data is replicated within the same Availability Zone (AZ)**
- Replace failed files within minutes
- **Usage**: Long-term processing, sensitive data


# Amazon FSx for NetApp ONTAP

- **Managed NetApp ONTAP on AWS**
- **File System compatible with NFS, SMB, iSCSI protocol**
- Move workloads running on ONTAP or NAS to AWS

## Works with:
- Linux
- Windows
- MacOS
- VMware Cloud on AWS
- Amazon Workspaces & AppStream 2.0
- Amazon EC2, ECS, and EKS

## Additional Features:
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression, and data de-duplication
- **Point-in-time instantaneous cloning** (helpful for testing new workloads)

 # Hybrid Cloud for Storage

 How to expose the s3 data on-premises? 

 ## AWS STORAGE GATEWAY
 - **Bridge between on-premises data and cloud data**

## Use Cases:
- Disaster recovery
- Backup & restore
- Tiered storage
- On-premises cache & low-latency file access

## Types of Storage Gateway:
- S3 File Gateway
- FSx File Gateway
- Volume Gateway
- Tape Gateway


# Amazon S3 File Gateway

- **Configured S3 buckets** are accessible using the NFS and SMB protocol.
- Most recently used data is **cached** in the file gateway.
- Supports **S3 Standard, S3 Standard-IA, S3 One Zone-IA,** and **S3 Intelligent-Tiering**.
- **Transition to S3 Glacier** using a Lifecycle Policy.
- Bucket access using **IAM roles** for each File Gateway.
- **SMB Protocol** has integration with **Active Directory (AD)** for user authentication.

# Amazon FSx File Gateway

- **Native access** to Amazon FSx for Windows File Server.
- **Local cache** for frequently accessed data.
- Windows native compatibility (**SMB, NTFS, Active Directory**).
- Useful for group file shares and home directories.

# Volume Gateway
**used by application servers**
- **Block storage** using iSCSI protocol backed by S3.
- Backed by **EBS snapshots**, which can help restore on-premises volumes.
- 2 olika
- **Cached volumes**: Low-latency access to the most recent data.
- **Stored volumes**: Entire dataset is on-premises, with scheduled backups to S3.


# Tape Gateway
**used by backup applications**
- Some companies have backup processes using **physical tapes**.
- With **Tape Gateway**, companies use the same processes but in the cloud.
- **Virtual Tape Library (VTL)** backed by Amazon S3 and Glacier.
- Back up data using existing tape-based processes (and iSCSI interface).
- Works with leading backup software vendors.


## AWS Transfer Family

A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol.

### Supported Protocols
- **AWS Transfer for FTP** (File Transfer Protocol - FTP)
- **AWS Transfer for FTPS** (File Transfer Protocol over SSL - FTPS)
- **AWS Transfer for SFTP** (Secure File Transfer Protocol - SFTP)

### Features
- **Managed infrastructure**: Scalable, reliable, and highly available (multi-AZ).
- **Pricing**: Pay per provisioned endpoint per hour + data transfers in GB.
- **User management**: Store and manage users' credentials within the service.
- **Integration**: Integrate with existing authentication systems (e.g., Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom).
- **Usage**: Sharing files, public datasets, CRM, ERP, and more.



## AWS DataSync

- **Purpose**: Move large amounts of data to and from:
  - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API, etc.) – requires an agent.
  - AWS to AWS (different storage services) – no agent needed.

### Can Synchronize to:
- **Amazon S3** (any storage class, including Glacier).
- **Amazon EFS**.
- **Amazon FSx** (Windows, Lustre, NetApp, OpenZFS).

### Key Features:
- **Replication tasks** can be scheduled: hourly, daily, weekly.
- **File permissions and metadata** are preserved (NFS POSIX, SMB).
- **Performance**: One agent task can use up to 10 Gbps and can have a bandwidth limit configured.

> EXAM BAD NETWORK CAP, SO THAT AGENT CANT SYNC. AWS SNOWCONE HAS AGENT PRE-INSTALLED



## Storage Comparison

- **S3**: Object Storage
- **S3 Glacier**: Object Archival
- **EBS volumes**: Network storage for one EC2 instance at a time
- **Instance Storage**: Physical storage for your EC2 instance (high IOPS)
- **EFS**: Network File System for Linux instances, POSIX filesystem
- **FSx for Windows**: Network File System for Windows servers
- **FSx for Lustre**: High Performance Computing Linux file system
- **FSx for NetApp ONTAP**: High OS Compatibility
- **FSx for OpenZFS**: Managed ZFS file system
- **Storage Gateway**: S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
- **Transfer Family**: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS
- **DataSync**: Schedule data sync from on-premises to AWS, or AWS to AWS
- **Snowcone / Snowball / Snowmobile**: To move large amounts of data to the cloud, physically



fx netapp smb nfs isci
lustre hpc
fs windws file mount on linux

storage

s3: smb,nfs
fx smb ntfs, active directroy
tape backup isci
volume application isci

transfer familj, fpt ftps sftp 
datasync NFS, SMB, HDFS, S3 API, etc. efs s3 fsx (all, netapp windows, tape volume )