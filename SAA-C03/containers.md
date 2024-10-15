

```markdown
## Amazon ECS – IAM Roles for Tasks

### EC2 Instance Profile (EC2 Launch Type only):
- Used by the **ECS agent**.
- Makes API calls to ECS service.
- Sends container logs to CloudWatch Logs.
- Pulls Docker images from ECR.
- References sensitive data in Secrets Manager or SSM Parameter Store.

### ECS Task Role:
- Allows each task to have a specific role.
- Use different roles for the different ECS services you run. Allows make API calls to AWS services
- Task Role is defined in the **task definition**.
```

> EFS can be used of ec2 instance type containers and fargate containers at the same type. 
> EFS USECASE: Persistent multi-az shared storage for your containers

```markdown
## Task auto-scaling - How many containers for your task
## ECS Service Auto Scaling - Launch TYPE

- Automatically increase/decrease the desired number of ECS tasks.

- Amazon ECS Auto Scaling uses **AWS Application Auto Scaling**:
  - ECS Service Average CPU Utilization
  - ECS Service Average Memory Utilization – Scale on RAM
  - ALB Request Count Per Target – Metric coming from the ALB

- **Target Tracking** – Scale based on target value for a specific CloudWatch metric.
- **Step Scaling** – Scale based on a specified CloudWatch Alarm.
- **Scheduled Scaling** – Scale based on a specified date/time (predictable changes).

- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level).
- Fargate Auto Scaling is much easier to setup (because **Serverless**).


```
```markdown
## EC2 Launch type - Auto Scaling EC2 Instances
- USE EC2 Cluster Capacity Provider instead of Auto scaling group Scaling
EC2 instances gets added when cpu or ram capacity is missing
```
```markdown
## Solutions
The usual.
Event bridge can trigger ecs task (like lambda) event based
Event bridge schedule based triggers to ecs task
ECS task can pull messages from SQS 
**Event bridge can intercept stopped task and can send out notifications using sns**

```



```markdown
## EKS - Data Volumes
Support for:
    - EBS
    - EFS (Works with fargate)
    - FSx for Lustre
    - FSx for Netapp ONTAP
```


```markdown```


```markdown```