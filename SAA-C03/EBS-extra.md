## EBS – Volume Types Summary

### General Purpose SSD Volumes
- **Volume type:** gp3, gp2
- **Durability:** 99.8% - 99.9% (0.1% - 0.2% annual failure rate)
- **Use cases:**
  - Transactional workloads
  - Virtual desktops
  - Medium-sized, single-instance databases
  - Low-latency interactive applications
  - Boot volumes
  - Development and test environments
- **Volume size:** 1 GiB - 16 TiB
- **Max IOPS per volume (16 KiB I/O):** 16,000
- **Max throughput per volume:** 1,000 MiB/s
- **Amazon EBS Multi-attach:** Not supported
- **NVMe reservations:** Not supported
- **Boot volume:** Supported

### Provisioned IOPS SSD Volumes
- **Volume type:** io2, io2 Block Express, io1
- **Durability:**
  - io2 Block Express: 99.999% (0.001% annual failure rate)
  - io1, io2: 99.9% - 99.999% (0.1% - 0.2% annual failure rate)
- **Use cases:**
  - Workloads that require sub-millisecond latency
  - Sustained IOPS performance
  - More than 64,000 IOPS or 1,000 MiB/s throughput
  - I/O-intensive workloads
- **Volume size:**
  - io2 Block Express: 4 GiB - 64 TiB
  - io1, io2: 4 GiB - 16 TiB
- **Max IOPS per volume (16 KiB I/O):**
  - io2 Block Express: 256,000
  - io1, io2: 64,000
- **Max throughput per volume:**
  - io2 Block Express: 4,000 MiB/s
  - io1, io2: 1,000 MiB/s
- **Amazon EBS Multi-attach:** Supported
- **NVMe reservations:** Supported
- **Boot volume:** Supported

### Throughput Optimized HDD Volumes
- **Volume type:** st1
- **Durability:** 99.8% - 99.9%
- **Use cases:**
  - Big data
  - Data warehouses
  - Log processing
- **Volume size:** 125 GiB - 16 TiB
- **Max IOPS per volume (1 MiB I/O):** 500
- **Max throughput per volume:** 500 MiB/s
- **Amazon EBS Multi-attach:** Not supported
- **Boot volume:** Not supported

### Cold HDD Volumes
- **Volume type:** sc1
- **Durability:** 99.8% - 99.9%
- **Use cases:**
  - Throughput-oriented storage for data that is infrequently accessed
  - Scenarios where the lowest storage cost is important
- **Volume size:** 125 GiB - 16 TiB
- **Max IOPS per volume (1 MiB I/O):** 250
- **Max throughput per volume:** 250 MiB/s
- **Amazon EBS Multi-attach:** Not supported
- **Boot volume:** Not supported
