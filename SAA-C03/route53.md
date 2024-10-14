## Amazon Route 53

- A highly available, scalable, fully managed, and **Authoritative** DNS.
  - **Authoritative** = the customer (you) can update the DNS records.
- Route 53 is also a Domain Registrar.
- Ability to check the health of your resources.
- The only AWS service that provides **100% availability SLA**.
- **Why Route 53?** 53 is a reference to the traditional DNS port.



### Route 53 - Records

- How you want to route traffic for a domain.
- Each record contains:
  - **Domain/subdomain Name** – e.g., example.com
  - **Record Type** – e.g., A or AAAA
  - **Value** – e.g., 12.34.56.78
  - **Routing Policy** – how Route 53 responds to queries
  - **TTL** – amount of time the record is cached at DNS Resolvers
- Route 53 supports the following DNS record types:
  - (must know) A / AAAA / CNAME / NS

### HOSTED ZONES

> - Public hosted zones
> - Private Hosted Zones (VPC)

> You pay 0,50 dollar per month per hosted zone

# CNAME vs Alias

- **AWS Resources** (Load Balancer, CloudFront, etc.) expose an AWS hostname:
  - Example: `lb1-1234.us-east-2.elb.amazonaws.com`
  - And you want: `myapp.mydomain.com`

## CNAME:
- Points a hostname to any other hostname.
  - Example: `app.mydomain.com => blabla.anything.com`
- **ONLY FOR NON-ROOT DOMAIN** (aka `something.mydomain.com`)

## Alias:
- Points a hostname to an AWS Resource.
  - Example: `app.mydomain.com => blabla.amazonaws.com`
- **Works for ROOT DOMAIN and NON-ROOT DOMAIN** (aka `mydomain.com`)



# Route 53 – Alias Records

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resource’s IP addresses
> **- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: `example.com`**
- Alias Record is always of type A/AAAA for AWS resources (IPv4/IPv6)
- You can’t set the TTL


# Route 53 – Alias Records Targets

- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone

>**Note:** You cannot set an ALIAS record for an EC2 DNS name.

Key Differences:
- Feature	CNAME	Alias
- Works at root domain	No (can’t use for apex/root domain)	Yes (can be used for apex/root domain)
- AWS-specific integration	No	Yes (for ELB, CloudFront, S3, etc.)
- Can coexist with other records	No (cannot coexist with other records like A)	Yes (can coexist with other records)
- DNS queries cost	Normal DNS query charges	No additional query charges for AWS resources
- Response time	Standard DNS resolution	Faster response (Route 53 handles AWS resource mapping)
- Use case	General-purpose domain aliasing	AWS service-specific aliasing (e.g., ELB, CloudFront)
# Route 53 – Routing Policies

- Define how Route 53 responds to DNS queries
- Don’t get confused by the word **"Routing"**
  - It’s not the same as Load Balancer routing, which routes the traffic
  - DNS does not route any traffic, it only responds to DNS queries
- Route 53 supports the following Routing Policies:
  - Simple
  - Weighted
  - Failover
  - Latency-based
  - Geolocation
  - Multi-Value Answer
  - Geoproximity (using Route 53 Traffic Flow feature)


# Routing Policies – Simple

- Typically, route traffic to a single resource
- Can specify multiple values in the same record
- If multiple values are returned, a random one is chosen by the **client** Du kan ange flera IPS eftter varannn
- When Alias is enabled, specify only one AWS resource
- Can’t be associated with Health Checks


# Routing Policies – Weighted

- Control the % of the requests that go to each specific resource.
- Assign each record a relative weight:
- Weights don’t need to sum up to 100.
- DNS records must have the same name and type.
- Can be associated with Health Checks.
**- Use cases: load balancing between regions, testing new application versions, etc.**
- Assign a weight of 0 to a record to stop sending traffic to a resource.
- If all records have a weight of 0, then all records will be returned equally.

> For arif.test.com you send 10 of the trafic to one ec2 and 90% to other one. 



# Routing Policies – Latency-based

- Redirects to the resource that has the least latency closest to the user.
- Very useful when minimizing latency is a priority for users.
- Latency is based on traffic between users and AWS Regions.
- Example: Germany users may be directed to the US if that region has the lowest latency.
- Can be associated with Health Checks to improve capacity and reliability.


# Route 53 – Health Checks

- **HTTP Health Checks** are only for public resources.

- **Health Checks** enable Automated DNS Failover:
  1. Health checks that monitor an endpoint (application, server, or other AWS resource).
  2. Health checks that monitor other health checks (Calculated Health Checks).
  3. Health checks that monitor CloudWatch Alarms (provides full control):
     - Example: throttles of DynamoDB, alarms on RDS, custom metrics, etc.
     - Useful for private resources.

- Health Checks are integrated with **CloudWatch (CW)** metrics.



# Health Checks – Monitor an Endpoint

- About **15 global health checkers** will check the endpoint's health:
  - **Healthy/Unhealthy Threshold**: 3 (default).
  - **Interval**: 30 seconds (can be set to 10 seconds with higher cost).
  - **Supported protocols**: HTTP, HTTPS, and TCP.
  - If more than 18% of health checkers report the endpoint is healthy, Route 53 considers it **Healthy**. Otherwise, it’s **Unhealthy**.
  - You can choose which locations you want Route 53 to use for health checks.

- Health Checks pass only when the endpoint responds with 2xx or 3xx status codes.
- Health Checks can be set up to pass/fail based on the text in the first **5120 bytes** of the response.
- Configure your router/firewall to allow incoming request from Route 53 Health Checkers IP. 


# Health Checks – Monitor an Endpoint

- About **15 global health checkers** will check the endpoint's health:
  - **Healthy/Unhealthy Threshold**: 3 (default).
  - **Interval**: 30 seconds (can be set to 10 seconds with higher cost).
  - **Supported protocols**: HTTP, HTTPS, and TCP.
  - If more than 18% of health checkers report the endpoint is healthy, Route 53 considers it **Healthy**. Otherwise, it’s **Unhealthy**.
  - You can choose which locations you want Route 53 to use for health checks.

- Health Checks pass only when the endpoint responds with 2xx or 3xx status codes.
- Health Checks can be set up to pass/fail based on the text in the first **5120 bytes** of the response.
# Health Checks – Private Hosted Zones
< Workround to create healthchecks for resources inside vpc

- Route 53 health checkers are **outside** the VPC.
- They can’t access **private endpoints** (such as private VPC or on-premises resources).
- You can create a **CloudWatch Metric** and associate it with a **CloudWatch Alarm**, then create a Health Check that monitors the alarm itself.



# Routing Policies – Failover

You create healthcheck **mandatory** to primary instance, if the healthcheck fails the trafic gets routed to the secondary - Disaster Recovery instance


# Routing Policies – Geolocation

- Different from Latency-based routing!
- This routing is based on the **user's location**.
- Specify location by Continent, Country, or US State (most precise location is selected if there's overlap).
- Should create a **"Default"** record in case there’s no match on location.
- Use cases:
  - Website localization
  - Restrict content distribution
  - Load balancing
- Can be associated with **Health Checks**.



# Geoproximity Routing Policy

- Routes traffic to your resources based on the geographic location of users and resources.
- Ability to **shift more traffic** to resources based on a defined bias.
- To change the size of the geographic region, specify **bias values**:
  - To expand (1 to 99) – more traffic to the resource.
  - To shrink (-1 to -99) – less traffic to the resource.

- Resources can be:
  - AWS resources (specify AWS region).
  - Non-AWS resources (specify Latitude and Longitude).


# Routing Policies – Multi-Value

- Use when routing traffic to multiple resources.
- Route 53 returns multiple values/resources.
- Can be associated with **Health Checks** (returns only values for healthy resources).
- Up to **8 healthy records** are returned for each Multi-Value query.
- **Multi-Value** is not a substitute for having an ELB (Elastic Load Balancer).
| Name              | Type     | Value        | TTL | Set ID | Health Check |
|-------------------|----------|--------------|-----|--------|--------------|
| www.example.com    | A Record | 192.0.2.2    | 60  | Web1   | A            |
| www.example.com    | A Record | 198.51.100.2 | 60  | Web2   | B            |
| www.example.com    | A Record | 203.0.113.2  | 60  | Web3   | C            |

> Client side loadbalancing 