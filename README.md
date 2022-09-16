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

# Amazon RDS
- Amazon RDS is an Online Transaction Processing (OLTP) type of database.
- It is best suited to structured, relational data store requirements.
- Automated backups and patching are applied in customer-defined maintenance windows.
- RDS is a managed service and you do not have access to the underlying EC2 instance.
- Database instances are accessed via endpoints.
- Endpoints can be retrieved via the DB instance description in the AWS Management Console, DescribeDBInstances API or describe-db-instances command.

**Amazon RDS supports the following database engines:**
1. Amazon Aurora.
2. MySQL.
3. MariaDB.
4. Oracle.
5. SQL Server.
6. PostgreSQL.

**The Amazon RDS managed service includes the following:**
1. Security and patching of the DB instances.
2. Automated backup for the DB instances.
3. Software updates for the DB engine.
4. Easy scaling for storage and compute.
5. Multi-AZ option with synchronous replication.
6. Automatic failover for Multi-AZ option.
7. Read replicas option for read heavy workloads.

![0863418b56b4063ec9793a082ee5a786.png](../_resources/0863418b56b4063ec9793a082ee5a786.png)

You can have up to 40 Amazon RDS DB instances, with the following limitations:
1. 10 for each SQL Server edition (Enterprise, Standard, Web, and Express) under the "license-included" model
2. 10 for Oracle under the "license-included" model
3. 40 for MySQL, MariaDB, or PostgreSQL
4. 40 for Oracle under the "bring-your-own-license" (BYOL) licensing model.


**IAM DB authentication**
We can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MariaDB, MySQL, and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.
***Advantages:***
1. Network traffic to and from the database is encrypted using Secure Socket Layer (SSL) or Transport Layer Security (TLS). 
2. You can use IAM to centrally manage access to your database resources, instead of managing access individually on each DB instance.
3. For applications running on Amazon EC2, you can use profile credentials specific to your EC2 instance to access your database instead of a password, for greater security.

# Encryption
- Encryption at rest is supported for all DB types and uses AWS KMS.
- You cannot encrypt an existing DB, you need to create a snapshot, copy it, encrypt the copy, then build an encrypted DB from the snapshot.
- A Read Replica of an Amazon RDS encrypted instance is also encrypted using the same key as the master instance when both are in the same region.
- If the master and Read Replica are in different regions, you encrypt using the encryption key for that region.
- You can’t have an encrypted Read Replica of an unencrypted DB instance or an unencrypted Read Replica of an encrypted DB instance.

**When using encryption at rest the following elements are also encrypted:**
1. All DB snapshots.
2. Backups.
3. DB instance storage.
4. Read Replicas.

## Billing and Provisioning
1. DB instance hours (partial hours are charged as full hours).
2. Storage GB/month.
3. I/O requests/month – for magnetic storage.
4. Provisioned IOPS/month – for RDS provisioned IOPS SSD.
5. Egress data transfer.
6. Backup storage (DB backups and manual snapshots).

## Multi AZ and Read Replicas
**Failover**
1. Loss of primary AZ or primary DB instance failure.
2. Loss of network connectivity on primary.
3. Compute (EC2) unit failure on primary.
4. Storage (EBS) unit failure on primary.
5. The primary DB instance is changed.
6. Patching of the OS on the primary DB instance.
7. Manual failover (reboot with failover selected on primary)

**Read Replicas**
- Read replicas are used for read-heavy DBs and replication is asynchronous.
- Read replicas are for workload sharing and offloading.
- Read replicas provide read-only DR.
- Read replicas are created from a snapshot of the master instance.
- Must have automated backups enabled on the primary (retention period > 0).
- Read replicas are available for MySQL, PostgreSQL, MariaDB, Oracle, Aurora, and SQL Server.
- Up to 5 Read Replicas.
- Replication is ASYNC, so reads re eventually consistent.
- Replicas  can be promoted to their own DB.
- Applications must update the connection string to leverage read replicas.
- If data goes from one AZ to another then it is charged. Data transfer within AZ no charge.

# DBsnapshots
- DB Snapshots are user-initiated and enable you to back up your DB instance in a known state as frequently as you wish, and then restore to that specific state.
- Cannot be used for point-in-time recovery.
- Snapshots are stored on S3.
- Snapshots remain on S3 until manually deleted.
- Can restore up to the last 5 minutes.
- Only default DB parameters and security groups are restored – you must manually associate all other DB parameters and SGs.
- Snapshots can be shared with other AWS accounts.

# High Availability Approaches for Databases
1. If possible, choose DynamoDB over RDS because of inherent fault tolerance.
2. If DynamoDB can’t be used, choose Aurora because of redundancy and automatic recovery features.
3. If Aurora can’t be used, choose Multi-AZ RDS.

# Migration
- AWS Database Migration Service helps you migrate databases to AWS quickly and securely.
- Use along with the Schema Conversion Tool (SCT) to migrate databases to AWS RDS or EC2-based databases.
- The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database.
- Schema Conversion Tool can copy database schemas for homogenous migrations (same database) and convert schemas for heterogeneous migrations (different database).

# RDS Conversion from Single AZ to Multi AZ
![ffc002770543960740d3040276f63e09.png](../_resources/ffc002770543960740d3040276f63e09.png)

# Monitoring, Logging and Reporting
**1. Amazon RDS Events** – Subscribe to Amazon RDS events to be notified when changes occur with a DB instance, DB snapshot, DB parameter group, or DB security group.
**2. Database log files** – View, download, or watch database log files using the Amazon RDS console or Amazon RDS API operations. You can also query some database log files that are loaded into database tables.
**3. Amazon RDS Enhanced Monitoring** — Look at metrics in real time for the operating system.
**4. Amazon RDS Performance Insights** — Assess the load on your database and determine when and where to act.
**5. Amazon RDS Recommendations** — Look at automated recommendations for database resources, such as DB instances, read replicas, and DB parameter groups.

# Amazon Aurora
- Aurora is an AWS proprietary database.
- 2 copies of data are kept in each AZ with a minimum of 3 AZ’s (6 copies).
- Can handle the loss of up to two copies of data without affecting DB write availability and up to three copies without affecting read availability.

**Aurora Replicas**
1. Aurora replica (up to 15)
2. MySQL Read Replica (up to 5)

![f359d3f1bbec4184d58819de90dfce9c.png](../_resources/f359d3f1bbec4184d58819de90dfce9c.png)

**Global Database**
For globally distributed applications you can use Global Database, where a single Aurora database can span multiple AWS regions to enable fast local reads and quick disaster recovery. Global Database uses storage-based replication to replicate a database across multiple AWS Regions, with typical latency of less than 1 second.

**Architecture**
An Aurora cluster consists of a set of **compute (database)** nodes and a shared **storage volume**.
The storage volume consists of **six storage nodes** placed in **three Availability Zones** for high availability and durability of user data.

# Aurora Serverless
- Available for MySQL-compatible and PostgreSQL-compatible editions.
- With Aurora Serverless, you only pay for database storage and the database capacity and I/O your database consumes while it is active.
- Can migrate between standard and serverless configurations with a few clicks in the Amazon RDS Management Console.

# Backup and Restore
1. Amazon Aurora’s backup capability enables point-in-time recovery for your instance.
2. Your automatic backup retention period can be configured up to thirty-five days.
3. Automated backups are enabled by default and data is stored on S3 and is equal to the size of the DB.
4. When automated backups are turned on for your DB Instance, Amazon RDS automatically performs a full daily snapshot of your data (during your preferred backup window).
5. Amazon RDS retains backups of a DB Instance for a limited, default is 7 days but can be up to 35 days.
6. The DB instance must be in an Active state for automated backups to happen.
7. The granularity of point-in-time recovery is 5 minutes.

**Aurora DB failover**
1. If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica, which in turn is promoted to become the new primary. Start-to-finish, failover typically completes within 30 seconds.
2. If you are running Aurora Serverless and the DB instance or AZ becomes unavailable, Aurora will automatically recreate the DB instance in a different AZ.
3. If you do not have an Amazon Aurora Replica (i.e. single instance) and are not running Aurora Serverless, Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance. This replacement of the original instance is done on a best-effort basis and may not succeed.

**Retention periods:**
1. By default the retention period is 7 days if configured from the console for all DB engines except Aurora.
2. The default retention period is 1 day if configured from the API or CLI.
3. The retention period for Aurora is 1 day regardless of how it is configured.
4. You can increase the retention period up to 35 days.
5. During the backup window I/O may be suspended.
6. Automated backups are deleted when you delete the RDS DB instance.
7. Automated backups are only supported for InnoDB storage engine for MySQL (not for myISAM).

![2799fa8220a1543c48e60f7cc9017a73.png](../_resources/2799fa8220a1543c48e60f7cc9017a73.png)

![735e56a0baa72d19163f1de624578dca.png](../_resources/735e56a0baa72d19163f1de624578dca.png)

# Dynamo DB
Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.
- Amazon DynamoDB stores three geographically distributed replicas of each table to enable high availability and data durability.
- Data is synchronously replicated across 3 facilities (AZs) in a region.
- DynamoDB is a serverless service – there are no instances to provision or manage.
- DynamoDB can be used for storing session state data.

**DynamoDB is made up of:**
**1. Tables:** Tables are a collection of items and items are made up of attributes (columns).
**2. Items:** The aggregate size of an item cannot exceed 400KB including keys and all attributes.
**3. Attributes:** Attributes consists of a name and a value or set of values.

**DynamoDB Accelerator (DAX)**
Fully managed in-memory cache for DynamoDB that increases performance (microsecond latency). You do not need to modify application logic, since DAX is compatible with existing DynamoDB API calls.

**DAX is used to improve READ performance (not writes).**

**Working**
1. DAX is a write-through caching service – this means the data is written to the cache as well as the back-end store at the same time.
2. Allows you to point your DynamoDB API calls at the DAX cluster and if the item is in the cache (cache hit), DAX returns the result to the application.
3. If the item requested is not in the cache (cache miss) then DAX performs an Eventually Consistent GetItem operation against DynamoDB
4. Retrieval of data from DAX reduces the read load on DynamoDB tables.
5. This may result in being able to reduce the provisioned read capacity on the table.

**Dynamo DB streams**
DynamoDB Streams captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores this information in a log for up to 24 hours. Events are recorded in near real-time.

**Dynamo DB Global Tables**
![a270b13e897b529f28492de5454854e5.png](../_resources/a270b13e897b529f28492de5454854e5.png)

**Access Control**
All authentication and access control is managed using IAM.
DynamoDB supports identity-based policies.
DynamoDB doesn’t support resource-based policies.
In DynamoDB, the primary resources are tables. Additional resource types, indexes, and streams can be created.

# Elasticache
- Fully managed implementations of two popular in-memory data stores – Redis and Memcached.
- Best for scenarios where the DB load is based on Online Analytics Processing (OLAP) transactions.
- Billed by node size and hours of use.

**There are two types of ElastiCache engine:**
1. **Memcached** – simplest model, can run large nodes with multiple cores/threads, can be scaled in and out, can cache objects such as DBs.
2. **Redis** – complex model, supports encryption, master / slave replication, cross AZ (HA), automatic failover and backup/restore.

![94954fa441365b3228e2b8f81dcec4a0.png](../_resources/94954fa441365b3228e2b8f81dcec4a0.png)

# Patterns
**Lazy Loading**
1. Loads the data into the cache only when necessary (if a cache miss occurs).
2. Lazy loading avoids filling up the cache with data that won’t be requested.
3. If requested data is in the cache, ElastiCache returns the data to the application.
4. If the data is not in the cache or has expired, ElastiCache returns a null.
5. The application then fetches the data from the database and writes the data received into the cache so that it is available for next time.
6. Data in the cache can become stale if Lazy Loading is implemented without other strategies (such as TTL).

**Write Through**
1. When using a write-through strategy, the cache is updated whenever a new write or update is made to the underlying database.
2. Allows cache data to remain up to date.
3. This can add wait time to write operations in your application.
4. Without a TTL you can end up with a lot of cached data that is never read.

# Monitoring and Reporting
**Memcached Metrics**
1.  CloudWatch metrics:
2.  CPUUtilization
3.  SwapUsage
4.  Evictions
5.  CurrConnections 

**Redis Metrics**
1. CloudWatch metrics:
2. EngineCPUUtilization
3. MemoryFragmentationRatio 
4. CacheHits
5. CacheMisses
6. CacheHitRate
7. CurrConnections

# Amazon RedShift
Amazon Redshift is a fast, fully managed data warehouse that makes it simple and cost-effective to analyze all your data using standard SQL and existing Business Intelligence (BI) tools.

RedShift is an Online Analytics Processing (OLAP) type of DB.

Multi-node consists of:
Leader node:
- Manages client connections and receives queries.
- Simple SQL endpoint.
- Stores metadata.
- Optimizes query plan.
- Coordinates query execution.

Compute nodes:
- Stores data and performs queries and computations.
- Local columnar storage.
- Parallel/distributed execution of all queries, loads, backups, restores, resizes.
- Up to 128 compute nodes.

**Availability and Durability**
Only available in one AZ but you can restore snapshots into another AZ.
Alternatively, you can run data warehouse clusters in multiple AZ’s by loading data into two Amazon Redshift data warehouse clusters in separate AZs from the same set of Amazon S3 input files.

RedShift provides continuous/incremental backups.

RedShift always keeps three copies of your data:
1. The original.
2. A replica on compute nodes (within the cluster).
3. A backup copy on S3.

# Amazon EMR
It is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data. 
- Runs in one Availability Zone within an Amazon VPC.
- Supports Apache Spark, HBase, Presto and Flink.
- Most used for log analysis, financial analysis, or extract, translate and loading (ETL) activities.

# Amazon Kinesis
Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information.
- Data is processed in “shards” – with each shard able to ingest 1000 records per second.
- There is a default limit of 500 shards, but you can request an increase to unlimited shards.

There are four types of Kinesis service:
1. Kinesis Video Streams
2. Kinesis Data Streams
3. Kinesis Data Firehose
4. Kinesis Data Analytics

# Kinesis Video Streams
Kinesis Video Streams makes it easy to securely stream video from connected devices to AWS for analytics, machine learning (ML), and other processing. It stores data in shards, stores data for 24 hours by default, up to 7 days.

# Kinesis Data Streams
Kinesis Data Streams enables real-time processing of streaming big data. Kinesis Data Streams stores data for later processing by applications (key difference with Firehose which delivers data directly to AWS services).

By default, records of a stream are accessible for up to 24 hours from the time they are added to the stream (can be raised to 7 days by enabling extended data retention).

# Kinesis Data Firehose
Kinesis Data Firehose is the easiest way to load streaming data into data stores and analytics tools. Enables near real-time analytics with existing business intelligence tools and dashboards. Kinesis Data Streams can be used as the source(s) to Kinesis Data Firehose.

**Firehose Destinations include:**
1. Amazon S3.
2. Amazon Redshift.
3. Amazon Elasticsearch Service.
4. Splunk.

# Kinesis Data Analytics
Amazon Kinesis Data Analytics is the easiest way to process and analyze real-time, streaming data. It can use standard SQL queries to process Kinesis data streams.
- Can ingest data from Kinesis Streams and Kinesis Firehose.
- Output to S3, RedShift, Elasticsearch and Kinesis Data Streams.

# Amazon Athena
1. Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL.
2. While Amazon Athena is ideal for quick, ad-hoc querying and integrates with Amazon QuickSight for easy visualization, it can also handle complex analysis, including large joins, window functions, and arrays.
3. Amazon Athena integrates directly with Identity and Access Management and you can leverage the use of bucket policies within S3 also. 

**Pricing**
- With Amazon Athena, you pay only for the queries that you run.
- You are charged based on the amount of data scanned by each query.
- You can get significant cost savings and performance gains by compressing, partitioning, or converting your data to a columnar format, because each of those operations reduces the amount of data that Athena needs to scan to execute a query.

# AWS Glue
AWS Glue is just a serverless ETL service that crawls your data, builds a data catalog, performs data preparation, data transformation, and data ingestion. It won't allow you to utilize different big data frameworks effectively, unlike Amazon EMR.

