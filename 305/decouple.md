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


> Om din kö får för många meddelanden, så kan du med hjälp av Cloudwatch metric - queue Lenght sätta auto scaling policy for asg så att man får fler instancer som bearbar datan i sqs


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
