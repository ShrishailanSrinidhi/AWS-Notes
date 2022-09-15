# AWS EC2
Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud.

## valid EC2 lifecycle instance state

**1. Pending** - The instance is preparing to enter the running state. An instance enters the pending state when it launches for the first time, or when it is restarted after being in the stopped state.

**2. Running** - The instance is running and ready for use.

**3. Stopping** - The instance is preparing to be stopped. Take note that you will not billed if it is preparing to stop; however, you will still be billed if it is just preparing to hibernate.

**4. Stopped** - The instance is shut down and cannot be used. The instance can be restarted at any time.

**5. Shutting-down** - The instance is preparing to be terminated.
Terminated - The instance has been permanently deleted and cannot be restarted. Take note that Reserved Instances that are applied to terminated instances are still billed until the end of their term according to their payment option.

## Metadata and User Data:

- User data is data that is supplied by the user at instance launch in the form of a script.

- Instance metadata is data about your instance that you can use to configure or manage the running instance.

- Instance metadata is available at http://169.254.169.254/latest/meta-data/ (the trailing “/” is required).

- Instance user data is available at: http://169.254.169.254/latest/user-data.

## Billing and provisioning
**On demand**

- Pay for hours used with no commitment.

- Low cost and flexibility with no upfront cost.

- Ideal for auto scaling groups and unpredictable workloads.

- Good for dev/test.

**Spot**

- Spot Instances are available at up to a 90% discount compared to On-Demand prices.

- You can use Spot Instances for various stateless, fault-tolerant, or flexible applications such as big data, containerized workloads, CI/CD, web servers, high-performance computing (HPC), and other test & development workloads.

- You don’t have to bid for Spot Instances in the new pricing model, and you just pay the Spot price that’s in effect for the current hour for the instances that you launch.

- Spot Instances receive a two-minute interruption notice when these instances are about to be reclaimed by EC2, because EC2 needs the capacity back.

- Instances are not interrupted because of higher competing bids.

**Reserved**

- Purchase (or agree to purchase) usage of EC2 instances in advance for significant discounts over On-Demand pricing.

- Capacity is reserved for a term of 1 or 3 years.
- Can switch to AZ within the same region.
- Can change the instance size within the same instance type.
- Cannot change the instance size of Windows RIs.
- Can sell reservations on the AWS marketplace.
- If you don’t need your RI’s, you can try to sell them on the Reserved Instance Marketplace.

## Billing of EC2

- Billing commences when Amazon EC2 initiates the boot sequence of an AMI instance.

- Billing ends when the instance terminates, which could occur through a web services command, by running "shutdown -h", or through instance failure.
- AWS does charge for the storage of any Amazon EBS volumes.

- When you stop an instance, AWS shuts it down but doesn't charge hourly usage for a stopped instance or data transfer fees.

## IP Addresses

- Public IPv4 addresses are lost when the instance is stopped but private addresses (IPv4 and IPv6) are retained.

- Public IPv4 addresses are retained if you restart the instance.
Elastic IPs are retained when the instance is stopped.
- Elastic IP addresses are static public IP addresses that can be remapped (moved) between instances.
- All accounts are limited to 5 elastic IPs per region by default; however this is a soft limit which can be raised by a service limit increase to AWS Support.
- AWS charges for elastic IP’s when they’re not being used.
- An Elastic IP address is for use in a specific region only.
- When you stop and start an EC2 instance, it will generally be moved to different underlying hardware.

## Placement Group
**Cluster:** cluster instances into low latency groups in single Availability zone. Cluster placement groups are recommended for applications that benefit from low network latency, high network throughput, or both. They are also recommended when the majority of the network traffic is between the instances in the group.

**Spread:** spreads instances across underlying hardware (max 7 instances per group per AZ). Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other.

**Partition:** Partition placement groups help reduce the likelihood of correlated hardware failures for your application. When using partition placement groups, Amazon EC2 divides each group into logical segments called partitions. Amazon EC2 ensures that each partition within a placement group has its own set of racks. Scales to 100s of EC2 instances per group. Partition placement groups can be used to deploy large distributed and replicated workloads, such as HDFS, HBase, and Cassandra, across distinct racks.

## Elastic Network Interfaces

- An elastic network interface (referred to as a network interface) is a logical networking component in a VPC that represents a virtual network card.

- You cannot increase the network bandwidth of an instance by teaming multiple ENIs.

- You can only add one extra ENI when launching but more can be attached later.

- eth0 is the primary network interface and cannot be moved or detached.

- By default, eth0 is the only Elastic Network Interface (ENI) created with an EC2 instance when launched.

**A network interface can include the following attributes:**

- A primary private IPv4 address from the IPv4 address range of your VPC.

- One or more secondary private IPv4 addresses from the IPv4 address range of your VPC.

- One Elastic IP address (IPv4) per private IPv4 address.

- One public IPv4 address.

- One or more IPv6 addresses.

-One or more security groups.

- A MAC address.

- A source/destination check flag.

- A description.

## Attaching ENIs


- ENIs can be “hot attached” to running instances.

- ENIs can be “warm-attached” when the instance is stopped.

- ENIs can be “cold-attached” when the instance is launched.

- Default interfaces are terminated with instance termination.

- Manually added interfaces are not terminated by default.

- You can change the termination behavior.

## Interface endpoints
An interface endpoint is an elastic network interface with a private IP address from the IP address range of your subnet. It serves as an entry point for traffic destined to a service that is owned by AWS or owned by an AWS customer or partner.

## Enhanced Networking – Elastic Network Adapter (ENA)
Enhanced networking provides higher bandwidth, higher packet-per-second (PPS) performance, and consistently lower inter-instance latencies. Enhanced networking is enabled using an Elastic Network Adapter (ENA).

- If your packets-per-second rate appears to have reached its ceiling, you should consider moving to enhanced networking because you have likely reached the upper thresholds of the VIF driver.

- AWS currently supports enhanced networking capabilities using SR-IOV

- SR-IOV provides direct access to network adapters, provides higher performance (packets-per-second) and lower latency.

- Must launch an HVM AMI with the appropriate drivers.

- Only available for certain instance types.

- Only supported in an Amazon VPC.

## Elastic Fabric Adapter (EFA)
An Elastic Fabric Adapter is an AWS Elastic Network Adapter (ENA) with added capabilities. An EFA can still handle IP traffic, but also supports an important access model commonly called OS bypass.

This model allows the application (most commonly through some user-space middleware) to access the network interface without having to get the operating system involved with each message.

**Use cases for EFAs include**

- High Performance Computing (HPC) applications using the Message Passing Interface (MPI).

- Machine Learning (ML) applications using NVIDIA Collective Communications Library (NCCL).

- With EFA you get the application performance of on-premises HPC clusters with the on-demand elasticity and flexibility of the AWS cloud.


- EFA is available as an optional EC2 networking feature that you can enable on any supported EC2 instance at no additional cost.

## ENI vs ENA vs EFA
**When to use ENI:**

- This is the basic adapter type for when you don’t have any high-performance requirements.

- Can be used with all instance types.

**When to use ENA:**

- Good for use cases that require higher bandwidth and lower inter-instance latency.

- Supported for limited instance types (HVM only).

**When to use EFA:**

- High Performance Computing.

- MPI and ML use cases.

- Tightly coupled applications.

- Can be used with all instance types.

# API Gateway 
Together with Lambda, API Gateway forms the app-facing part of the AWS serverless infrastructure.

- Creating, deploying, and managing a REST application programming interface (API) to expose backend HTTP endpoints, AWS Lambda functions, or other AWS services.

- Creating, deploying, and managing a WebSocket API to expose AWS Lambda functions or other AWS services.

- Invoking exposed API methods through the frontend HTTP and WebSocket endpoints.

- Back-end services include Amazon EC2, AWS Lambda or any web application (public or private endpoints).

## Edge-Optimized Endpoint

- An edge-optimized API endpoint is best for geographically distributed clients. API requests are routed to the nearest CloudFront Point of Presence (POP). This is the default endpoint type for API Gateway REST APIs.

- Edge-optimized APIs capitalize the names of HTTP headers (for example, Cookie).

- CloudFront sorts HTTP cookies in natural order by cookie name before forwarding the request to your origin. For more information about the way CloudFront processes cookies, see Caching Content Based on Cookies.

- Any custom domain name that you use for an edge-optimized API applies across all regions.

## Identity and Access Management

- Resource-based policies.

- Standard IAM Roles and Policies (identity-based policies).

- IAM Tags.

- Endpoint policies for interface VPC endpoints. Lambda authorizers.

- Amazon Cognito user pools.

## Charges

- With Amazon API Gateway, you only pay when your APIs are in use.

- There are no minimum fees or upfront commitments.

- You pay only for the API calls you receive, and the amount of data transferred out.

- There are no data transfer out charges for Private APIs (however, AWS PrivateLink charges apply when using Private APIs in Amazon API Gateway).

- Amazon API Gateway also provides optional data caching charged at an hourly rate that varies based on the cache size you select.

- The API Gateway free tier includes one million API calls per month for up to 12 months.

# AWS Lambda
- To enable your Lambda function to access resources inside your private VPC, you must provide additional VPC-specific configuration information that includes VPC subnet IDs and security group IDs.

- Max is 15 minutes (900 seconds), default is 3 seconds.

- You pay for the time it runs.

- Lambda terminates the function at the timeout.

- Lambda assumes an IAM role when it executes the function.

- Lambda scales concurrently executing functions up to your default limit (1000).

- Application Load Balancers (ALBs) support AWS Lambda functions as targets.

**Exam tip:** If a Lambda function needs to connect to a VPC and needs Internet access, make sure you connect to a private subnet that has a route to a NAT Gateway (the NAT Gateway will be in a public subnet).

## Supported AWS event sources include

1. Amazon S3.
2. Amazon DynamoDB.
3. Amazon Kinesis Data Streams.
4. Amazon Simple Notification Service.
5. Amazon Simple Email Service.
6. Amazon Simple Queue Service.
7. Amazon Cognito.
8. AWS CloudFormation.
9. Amazon CloudWatch Logs.
10. Amazon CloudWatch Events.
11. AWS CodeCommit.
12. AWS Config.
13. Amazon Alexa.
14. Amazon Lex.
15. Amazon API Gateway.
16. AWS IoT Button.
17. Amazon CloudFront.
18. Amazon Kinesis Data Firehose.

## Services that Lambda reads events from:

1. Amazon Kinesis
2. Amazon DynamoDB
3. Amazon Simple Queue Service

## Dead Letter Queue (DLQ)

A dead-letter queue saves discarded events for further processing. A dead-letter queue acts the same as an on-failure destination in that it is used when an event fails all processing attempts or expires without being processed.

## Lambda@Edge

Lambda@Edge allows you to run code across AWS locations globally without provisioning or managing servers, responding to end users at the lowest network latency.

The functions run in response to CloudFront events, without provisioning or managing servers.

## ELB

- The Lambda function and target group must be in the same account and in the same Region.

- The maximum size of the request body that you can send to a Lambda function is 1 MB.

- The maximum size of the response JSON that the Lambda function can send is 1 MB.

- WebSockets are not supported. Upgrade requests are rejected with an HTTP 400 code.

- By default, health checks are disabled for target groups of type lambda.

- You can enable health checks to implement DNS failover with Amazon Route 53. The Lambda function can check the health of a downstream service before responding to the health check request.

## Operations and Monitoring

- Lambda automatically monitors Lambda functions and reports metrics through CloudWatch.

- You can use AWS X-Ray to visualize the components of your application, identify performance bottlenecks, and troubleshoot requests that resulted in an error.

- Must have permissions to write to X-Ray in the execution role.

## Charges

- Number of requests.
- Duration of the request calculated from the time your code begins execution until it returns or terminates.
- The amount of memory allocated to the function.