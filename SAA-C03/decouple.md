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

