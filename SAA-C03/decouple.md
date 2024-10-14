## Amazon SQS – Standard Queue

- **Oldest offering**: Over 10 years old
- **Purpose**: Fully managed service, used to decouple applications

### Attributes:
- **Unlimited throughput**, unlimited number of messages in the queue
- **Default retention** of messages: 4 days, maximum of 14 days
- **Low latency**: Less than 10 ms on publish and receive
- **Message size limitation**: 256 KB per message sent

### Behavior:
- Can have **duplicate messages** (at least once delivery, occasionally)
- Can have **out-of-order messages** (best effort ordering)


> Om din kö får för många meddelanden, så kan du med hjälp av Cloudwatch metric - queue Lenght sätta auto scaling policy for asg så att man får fler instanser som bearbetar datan i sqs


## Amazon SQS – Security

### Encryption:
- **In-flight encryption** using HTTPS API
- **At-rest encryption** using KMS keys
- **Client-side encryption** if the client wants to perform encryption/decryption itself

### Access Controls:
- **IAM policies** to regulate access to the SQS API

### SQS Access Policies:
- Similar to S3 bucket policies
- Useful for cross-account access to SQS queues
- Useful for allowing other services (e.g., SNS, S3) to write to an SQS queue



## SQS – Message Visibility Timeout

- After a message is polled by a consumer, it becomes **invisible** to other consumers.
- By default, the "message visibility timeout" is **30 seconds**.
- This means the message has **30 seconds to be processed**.
- After the message visibility timeout is over, the message becomes **visible** again in SQS.
- If a message is not processed within the visibility timeout, it will be processed **twice**.
- A consumer could call the **ChangeMessageVisibility** API to get more time.
- If visibility timeout is high (hours), and the consumer crashes, re-processing will take time.
- If visibility timeout is too low (seconds), we may get **duplicates**.


## Amazon SQS – Long Polling

- When a consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are none in the queue. This is called **Long Polling**.
- **Long Polling** decreases the number of API calls made to SQS while increasing the efficiency and latency of your application.
- The wait time can be between **1 second to 20 seconds** (20 seconds preferable).
- Long Polling is preferable to **Short Polling**.
- Long Polling can be enabled at the **queue level** or at the **API level** using `WaitTimeSeconds`.

> So anytime at the exam you see a way to optimize the number of API calls made to an SQS queue and decrease of latency, think about long polling.

## SQS - FIFO
- Downsight: Limited throughput: 300 msg/s without batching, 3000 msg/s with
- Ups: remowing dublicates and meesages are processed in order by the consumer
> The name of the fifa sqs has to end with "fifo"


# SNS
```markdown
## Subscribers
- Kinesis
- SQS
- Lambda
- Email
- Sms
- Email-Json
- HTTP and HTTPS
- SMS
```

## SNS + SQS: Fan Out

## SNS + SQS: Fan Out

- Push once in SNS, receive in all SQS queues that are subscribers.
- Fully decoupled, no data loss.
- SQS allows for:
  - Data persistence
  - Delayed processing
  - Retries of work
- Ability to add more SQS subscribers over time.
- Make sure your SQS queue access policy allows for SNS to write.

- Cross region delivery, works with sqs queues in other regions 

-TEX pathers
    - S3 event to multiple queues
        - Tex for object create and prefix images/ u can only have one s3 Event rule
        - Send s3 event to sns -> many sqs queues - and lanbdas tex

    - Sns to s3 via Kinesis Data firehouse
    Buying service -> sns topic -> Kenesis Data firehouse -> s3, and more

>Sns can have both sqs standard and FIFO queues as subscripbers


```markdown
## SNS – Message Filtering

- **JSON policy** is used to filter messages sent to SNS topic's subscriptions.
- If a subscription doesn’t have a filter policy, it receives **every message**.
```
```

                               +------------------------+
                               |      SNS Topic         |
                               +------------------------+
                                         |
         ----------------------------------------------------------------------
        |                    |                      |                        |
+-------------------+  +------------------+  +------------------+  +--------------------+
|  SQS Queue        |  |  SQS Queue        |  |  SQS Queue        |  |  SQS Queue         |
|  (Placed orders)  |  |  (Cancelled       |  |  (Declined        |  |  (All)             |
|  Filter: Placed   |  |  orders)          |  |  orders)          |  |  (No filter)       |
+-------------------+  +------------------+  +------------------+  +--------------------+
                         |                   |
                     +----------------+     +----------------+
                     | Email           |     | Email          |
                     | (Cancelled      |     | Subscription   |
                     | orders)         |     | (Cancelled     |
                     |                 |     | orders)        |
                     +----------------+     +----------------+
```

## Kinesis Overview

- Makes it easy to **collect**, **process**, and **analyze** streaming data in real-time.
- Ingest real-time data such as: 
  - Application logs
  - Metrics
  - Website clickstreams
  - IoT telemetry data

### Kinesis Services:
1. **Kinesis Data Streams**: Capture, process, and store data streams.
2. **Kinesis Data Firehose**: Load data streams into AWS data stores.
3. **Kinesis Data Analytics**: Analyze data streams with SQL or Apache Flink.
4. **Kinesis Video Streams**: Capture, process, and store video streams.


```markdown
## Kinesis Data Streams

- **Retention**: Between 1 day to 365 days
- **Ability to reprocess** (replay) data
- Once data is inserted into Kinesis, it **cannot be deleted** (immutability)
- Data that shares the same partition goes to the same shard (ordering)
- **Producers**: 
  - AWS SDK
  - Kinesis Producer Library (KPL)
  - Kinesis Agent
- **Consumers**:
  - Write your own: Kinesis Client Library (KCL), AWS SDK
  - Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics

```

```markdown
## Kinesis Data Streams – Capacity Modes

### Provisioned Mode:
- You choose the number of shards provisioned, scale manually or using API.
- Each shard gets **1 MB/s in** (or 1000 records per second).
- Each shard gets **2 MB/s out** (classic or enhanced fan-out consumer).
- You pay per shard provisioned per hour.

### On-Demand Mode:
- No need to provision or manage the capacity.
- Default capacity provisioned: **4 MB/s in** or **4000 records per second**.
- Scales automatically based on observed throughput peak during the last 30 days.
- Pay per stream per hour & data in/out per GB.

```

> Kinesis vpc endpoints is available. 

