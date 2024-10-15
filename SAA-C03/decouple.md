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
- Kinesis FIREHOUSE (NOT KINESIS DATA STREAMS)
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
- Each shard gets **1 MB/s in** (or 1000 records per second). Encanced fan out 2mbit in
- Each shard gets **2 MB/s out** (classic or enhanced fan-out consumer).
- You pay per shard provisioned per hour.

### On-Demand Mode:
- No need to provision or manage the capacity.
- Default capacity provisioned: **4 MB/s in** or **4000 records per second**.
- Scales automatically based on observed throughput peak during the last 30 days.
- Pay per stream per hour & data in/out per GB.

```

> Kinesis vpc endpoints is available. 

```markdown
## Kinesis Data Firehose

- **Fully Managed Service**: No administration, automatic scaling, serverless.
   - **AWS Producers**: Kinesis Data Stream / Kinesis Agent / Cloudwatch / Eventbridge, etc
   - **AWS Destinations**: Redshift / Amazon S3 / OpenSearch
   - **3rd Party Partners**: Splunk / MongoDB / DataDog / NewRelic / ...
   - **Custom**: Send to any HTTP endpoint.

- **Pricing**: Pay for data going through Firehose.

- **Near Real-Time**:
   - Buffer interval: 0 seconds (no buffering) to 900 seconds.
   - Buffer size: Minimum 1MB - Max 128MB. - Hur stor datan kan vara innan den måste levereras till destinations. Den väntar inon interval tiden och sizen innan den levererar. 

- **Supported Features**:
   - Supports many data formats, conversions, transformations, and compression.
   - Supports custom data transformations using AWS Lambda.
   - Can send failed or all data to a backup S3 bucket.

* After ingesting, you can optionaly transform your data with aws lambda
```

```markdown
## Kinesis partition - partition key

The partition key is important for ensuring ordering of data within a shard. All records with the same partition key will be written to the same shard, thus ensuring their order is preserved.A Partition Key is a key associated with each data record when it is written to a Kinesis stream.
It determines which shard in the stream the data will be sent to.

Let’s say you are ingesting streaming data of clickstreams from a website. You can use the user ID as the partition key. This ensures that all events (clicks, page views, etc.) for a specific user are stored in the same shard, maintaining the order of the events for that user.
In Summary:

    Partition: The shards in the stream where data is ingested.
    Partition Key: A string assigned to each record that determines which shard the record is stored in. Helps in distributing data across shards and preserving order within a shard.
```

```markdown
## Ordering Data into SQS

- For **SQS Standard**, there is no ordering.
- For **SQS FIFO**, if you don’t use a Group ID, messages are consumed in the order they are sent, with only one consumer.

- You want to scale the number of consumers, but you want messages to be "grouped" when they are related to each other.
- Then you use a **Group ID** (similar to **Partition Key** in Kinesis).
```

```markdown

## Kinesis vs SQS Ordering

Let’s assume 100 trucks, 5 Kinesis shards, 1 SQS FIFO.

### Kinesis Data Streams:
- On average, you'll have 20 trucks per shard.
- Trucks will have their data ordered within each shard.
- The maximum amount of consumers in parallel we can have is 5.
- Can receive up to 5 MB/s of data.

### SQS FIFO:
- You only have one SQS FIFO queue.
- You will have 100 Group IDs.
- You can have up to 100 consumers (due to the 100 Group IDs).
- You have up to 300 messages per second (or 3000 if using batching).
```


```markdown
## SQS vs SNS vs Kinesis

### SQS:
- Consumer "pull data".
- Data is deleted after being consumed.
- Can have as many workers (consumers) as we want.
- No need to provision throughput.
- Ordering guarantees only on FIFO queues.
- Individual message delay capability.

### SNS:
- Push data to many subscribers.
- Up to 12,500,000 subscribers.
- Data is not persisted (lost if not delivered).
- Pub/Sub model.
- Up to 100,000 topics.
- No need to provision throughput.
- Integrates with SQS for fan-out architecture pattern.
- FIFO capability for SQS FIFO.

### Kinesis:
- **Standard**: Pull data (2 MB per shard).
- **Enhanced fan-out**: Push data (2 MB per shard per consumer).
- Possibility to replay data.
- Meant for real-time big data, analytics, and ETL.
- Ordering at the shard level.
- Data expires after X days (configurable).
- Provisioned mode or on-demand capacity mode.

```

## Amazon MQ

- **SQS, SNS** are "cloud-native" services: proprietary protocols from AWS.
- Traditional applications running from on-premises may use open protocols such as: **MQTT, AMQP, STOMP, Openwire, WSS**.
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use **Amazon MQ**.
- Amazon MQ is a managed message broker service for:
  - **RabbitMQ**
  - **ActiveMQ**

- **Amazon MQ** doesn’t "scale" as much as SQS / SNS.
- **Amazon MQ** runs on servers, can run in Multi-AZ with failover.
- **Amazon MQ** has both **queue features** (~SQS) and **topic features** (~SNS).
