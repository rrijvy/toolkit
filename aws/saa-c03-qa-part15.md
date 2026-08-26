# AWS SAA-C03 Real Exam Questions & Answers — Part 15 (Q351–Q375)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 15](https://www.youtube.com/watch/Cn-LlIawE7Y)

---

## Question 351

**Full Question:** A company has Amazon EC2 instances that run nightly batch jobs to process data. The EC2 instances run in an Auto Scaling group that uses on-demand billing. If a job fails on one instance, another instance will reprocess the job. The batch jobs run between 12:00 a.m. and 6:00 a.m. local time every day. Which solution will provide EC2 instances to meet these requirements most cost-effectively?

**Short Question:** Cheapest way to provision EC2 instances for a fault-tolerant, 6-hour nightly batch job?

**Options:**
- A. Purchase a one-year Savings Plan for Amazon EC2 that covers the instance family of the Auto Scaling group that the batch job uses
- B. Purchase a one-year Reserved Instance for the specific instance type and operating system of the instances in the Auto Scaling group that the batch job uses
- ✅ C. Create a new launch template for the Auto Scaling group. Set the instances to Spot Instances. Set a policy to scale out based on CPU usage
- D. Create a new launch template for the Auto Scaling group. Increase the instance size. Set a policy to scale out based on CPU usage

**Reason:** Because the job runs only 6 hours a day and can tolerate interruption (failed jobs are simply reprocessed), Spot Instances give the deepest discount (up to 90% off on-demand); Savings Plans and Reserved Instances only pay off for long-term, continuous usage, and increasing instance size only raises cost.

---

## Question 352

**Full Question:** A company needs the ability to analyze the log files of its proprietary application. The logs are stored in JSON format in an Amazon S3 bucket. Queries will be simple and will run on demand. A solutions architect needs to perform the analysis with minimal changes to the existing architecture. What should the solutions architect do to meet these requirements with the least amount of operational overhead?

**Short Question:** Lowest-overhead way to run occasional SQL queries on JSON logs already sitting in S3?

**Options:**
- A. Use Amazon Redshift to load all the content into one place and run the SQL queries as needed
- B. Use Amazon CloudWatch Logs to store the logs. Run SQL queries as needed from the Amazon CloudWatch console
- ✅ C. Use Amazon Athena directly with Amazon S3 to run the queries as needed
- D. Use AWS Glue to catalog the logs. Use a transient Apache Spark cluster on Amazon EMR to run the SQL queries as needed

**Reason:** Amazon Athena queries data directly in S3 with standard SQL and requires no infrastructure to provision or manage, satisfying both the "minimal changes" and "least operational overhead" requirements; Redshift and EMR both require moving data and managing clusters, and CloudWatch Logs would require re-architecting how logs are collected.

---

## Question 353

**Full Question:** A company uses a 100 GB Amazon RDS for Microsoft SQL Server Single-AZ DB instance in the us-east-1 Region to store customer transactions. The company needs high availability and automatic recovery for the DB instance. The company must also run reports on the RDS database several times a year. The report process causes transactions to take longer than usual to post to the customers' accounts. The company needs a solution that will improve the performance of the report process. Which combination of steps will meet these requirements? (Choose two.)

**Short Question:** Which two changes give an RDS SQL Server database both high availability and faster reporting without slowing transactions?

**Options:**
- ✅ A. Modify the DB instance from a Single-AZ DB instance to a Multi-AZ deployment
- B. Take a snapshot of the current DB instance. Restore the snapshot to a new RDS deployment in another Availability Zone
- ✅ C. Create a read replica of the DB instance in a different Availability Zone. Point all requests for reports to the read replica
- D. Migrate the database to RDS Custom
- E. Use RDS Proxy to limit reporting requests to the maintenance window

**Reason:** Multi-AZ gives automatic failover with a synchronous standby for high availability, while a read replica offloads the reporting queries so they no longer compete with transaction processing on the primary instance; a manual snapshot restore isn't a true HA solution, RDS Custom adds unneeded OS-level complexity, and RDS Proxy manages connections but can't schedule or restrict queries to a time window.

---

## Question 354

**Full Question:** A company is migrating a microservices application to an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. The company must configure the Amazon EKS control plane with endpoint private access set to true and endpoint public access set to false to maintain security compliance. The company must also put the data plane in private subnets. However, the company has received error notifications because the node cannot join the cluster. Which solution will allow the node to join the cluster?

**Short Question:** How do worker nodes in private subnets reach a fully private EKS control plane so they can join the cluster?

**Options:**
- A. Grant the required permission in AWS Identity and Access Management (IAM) to the Amazon EKS node IAM role
- ✅ B. Create interface VPC endpoints to allow nodes to access the control plane
- C. Recreate nodes in the public subnet. Restrict security groups for EC2 nodes
- D. Allow outbound traffic in the security group of the nodes

**Reason:** With both the control plane and worker nodes private, there is no network path between them until you create interface VPC endpoints (via AWS PrivateLink) for the EKS API and related services, giving nodes a private route to register with the cluster; the other options don't address the missing network path (IAM permissions and outbound security group rules aren't the problem, and public subnets violate the compliance requirement).

---

## Question 355

**Full Question:** A company is preparing to launch a public-facing web application in the AWS Cloud. The architecture consists of Amazon EC2 instances within a VPC behind an Elastic Load Balancer (ELB). A third-party service is used for the DNS. The company's solutions architect must recommend a solution to detect and protect against large-scale DDoS attacks. Which solution meets these requirements?

**Short Question:** Best way to detect and protect a load-balanced web app from large-scale DDoS attacks when DNS is handled by a third party?

**Options:**
- A. Enable Amazon GuardDuty on the account
- B. Enable Amazon Inspector on the EC2 instances
- C. Enable AWS Shield and assign Amazon Route 53 to it
- ✅ D. Enable AWS Shield Advanced and assign the ELB to it

**Reason:** AWS Shield Advanced provides enhanced protection against large-scale DDoS attacks and must be associated with the application's actual entry point, the ELB; GuardDuty and Inspector address threat detection and vulnerability scanning rather than DDoS, and Route 53 isn't usable here since DNS is handled by a third-party service.

---

## Question 356

**Full Question:** A company has a custom application with embedded credentials that retrieves information from an Amazon RDS for MySQL DB instance. Management says the application must be made more secure with the least amount of programming effort. What should a solutions architect do to meet these requirements?

**Short Question:** Most secure, lowest-effort way to remove embedded database credentials from an application and handle rotation automatically?

**Options:**
- A. Use AWS Key Management Service (AWS KMS) to create keys. Configure the application to load the database credentials from AWS KMS. Enable automatic key rotation.
- B. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Secrets Manager. Configure the application to load the database credentials from Secrets Manager. Create an AWS Lambda function that rotates the credentials in Secrets Manager.
- ✅ C. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Secrets Manager. Configure the application to load the database credentials from Secrets Manager. Set up a credentials rotation schedule for the application user in the RDS for MySQL database using Secrets Manager.
- D. Create credentials on the RDS for MySQL database for the application user and store the credentials in AWS Systems Manager Parameter Store. Configure the application to load the database credentials from Parameter Store. Set up a credentials rotation schedule for the application user in the RDS for MySQL database using Parameter Store.

**Reason:** AWS Secrets Manager is purpose-built for storing database credentials and has built-in automatic rotation for supported databases like RDS for MySQL, requiring no custom code. KMS manages encryption keys (not secrets), a custom Lambda function for rotation adds unnecessary programming effort, and Parameter Store lacks a built-in rotation feature.

---

## Question 357

**Full Question:** A solutions architect must migrate a Windows Internet Information Services (IIS) web application to AWS. The application currently relies on a fileshare hosted on the user's on-premises network attached storage (NAS). The solutions architect has proposed migrating the IIS web servers to Amazon EC2 instances in multiple Availability Zones that are connected to the storage solution, and configuring an Elastic Load Balancer attached to the instances. Which replacement to the on-premises fileshare is most resilient and durable?

**Short Question:** Which managed storage service best replaces an on-premises NAS fileshare for a multi-AZ Windows IIS app?

**Options:**
- A. Migrate the fileshare to Amazon RDS
- B. Migrate the fileshare to AWS Storage Gateway
- ✅ C. Migrate the fileshare to Amazon FSx for Windows File Server
- D. Migrate the fileshare to Amazon Elastic File System (Amazon EFS)

**Reason:** Amazon FSx for Windows File Server is a fully managed native Windows file system using the SMB protocol that IIS expects, and it can be deployed multi-AZ for high resilience. RDS is a database (not a file system), Storage Gateway is meant for hybrid on-premises-to-cloud connectivity rather than native cloud storage, and EFS uses NFS which is Linux-native, not Windows-native.

---

## Question 358

**Full Question:** A company has an Amazon S3 data lake that is governed by AWS Lake Formation. The company wants to create a visualization in Amazon QuickSight by joining the data in the data lake with operational data that is stored in an Amazon Aurora MySQL database. The company wants to enforce column-level authorization so that the company's marketing team can access only a subset of columns in the database. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Lowest-overhead way to ingest database data into an S3 data lake and enforce column-level access for QuickSight users?

**Options:**
- A. Use Amazon EMR to ingest the data directly from the database to the QuickSight SPICE engine. Include only the required columns.
- B. Use AWS Glue Studio to ingest the data from the database to the S3 data lake. Attach an IAM policy to the QuickSight users to enforce column-level access control. Use Amazon S3 as the data source in QuickSight.
- C. Use AWS Glue Elastic Views to create a materialized view for the database in Amazon S3. Create an S3 bucket policy to enforce column-level access control for the QuickSight users. Use Amazon S3 as the data source in QuickSight.
- ✅ D. Use a Lake Formation blueprint to ingest the data from the database to the S3 data lake. Use Lake Formation to enforce column-level access control for the QuickSight users. Use Amazon Athena as the data source in QuickSight.

**Reason:** AWS Lake Formation is purpose-built to enforce fine-grained, column-level permissions and a Lake Formation blueprint provides low-overhead ingestion into the existing governed data lake, with Athena/QuickSight automatically respecting those permissions. EMR bypasses Lake Formation governance, IAM/S3 bucket policies cannot enforce column-level access, and AWS Glue Elastic Views has been discontinued.

---

## Question 359

**Full Question:** A company collects data from thousands of remote devices by using a RESTful web services application that runs on an Amazon EC2 instance. The EC2 instance receives the raw data, transforms the raw data, and stores all the data in an Amazon S3 bucket. The number of remote devices will increase into the millions soon. The company needs a highly scalable solution that minimizes operational overhead. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two steps build a highly scalable, low-overhead pipeline to ingest and transform data from millions of devices into S3?

**Options:**
- ✅ A. Use AWS Glue to process the raw data in Amazon S3.
- B. Use Amazon Route 53 to route traffic to different EC2 instances.
- C. Add more EC2 instances to accommodate the increasing amount of incoming data.
- D. Send the raw data to an Amazon SQS queue. Use EC2 instances to process the data.
- ✅ E. Use Amazon API Gateway to send the raw data to an Amazon Kinesis Data Stream. Configure Amazon Kinesis Data Firehose to use the data stream as a source to deliver the data to Amazon S3.

**Reason:** API Gateway plus Kinesis Data Streams and Kinesis Data Firehose forms a fully serverless, highly scalable ingestion pipeline for millions of devices, and AWS Glue is a serverless managed ETL service for transforming the data once it lands in S3 — both with minimal operational overhead. The EC2-based options (Route 53 routing, adding instances, or SQS with EC2 processing) all still require managing server fleets, which adds more operational overhead than the serverless approach.

---

## Question 360

**Full Question:** A company provides a Voice over Internet Protocol (VoIP) service that uses UDP connections. The service consists of Amazon EC2 instances that run in an Auto Scaling group. The company has deployments across multiple AWS Regions. The company needs to route users to the Region with the lowest latency. The company also needs automated failover between Regions. Which solution will meet these requirements?

**Short Question:** Best way to route VoIP (UDP) traffic to the lowest-latency AWS Region with automatic regional failover?

**Options:**
- ✅ A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.
- B. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Use the ALB as an AWS Global Accelerator endpoint in each Region.
- C. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 latency record that points to aliases for each NLB. Create an Amazon CloudFront distribution that uses the latency record as an origin.
- D. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 weighted record that points to aliases for each ALB. Deploy an Amazon CloudFront distribution that uses the weighted record as an origin.

**Reason:** A Network Load Balancer supports UDP traffic, and AWS Global Accelerator routes users over the AWS global network to the lowest-latency healthy regional NLB endpoint with fast automatic failover. ALBs only support HTTP/HTTPS (not UDP), and CloudFront is an HTTP/HTTPS CDN that does not support generic UDP traffic, ruling out the other options.

---

## Question 361

**Full Question:** A company has hired an external vendor to perform work in the company's AWS account. The vendor uses an automated tool that is hosted in an AWS account that the vendor owns. The vendor does not have IAM access to the company's AWS account. How should a solutions architect grant this access to the vendor?

**Short Question:** What's the standard, secure way to let an external vendor's tool (in its own AWS account) access resources in your AWS account?

**Options:**
- ✅ A. Create an IAM role in the company's account to delegate access to the vendor's IAM role. Attach the appropriate IAM policies to the role for the permissions that the vendor requires.
- B. Create an IAM user in the company's account with a password that meets the password complexity requirements. Attach the appropriate IAM policies to the user for the permissions that the vendor requires.
- C. Create an IAM group in the company's account. Add the vendor's tool IAM user from the vendor account to the group. Attach the appropriate IAM policies to the group for the permissions that the vendor requires.
- D. Create a new identity provider by choosing AWS account as the provider type in the IAM console. Supply the vendor's AWS account ID and username. Attach the appropriate IAM policies to the new provider for the permissions that the vendor requires.

**Reason:** Creating a cross-account IAM role with a trust policy that allows the vendor's AWS account to assume it is the AWS best practice, since it grants temporary credentials without sharing long-term secrets. A user with a password means long-term static credentials (a security anti-pattern), IAM groups can't contain users from another account, and "AWS account" isn't a valid identity provider type in IAM.

---

## Question 362

**Full Question:** A company is running a multi-tier e-commerce web application in the AWS Cloud. The application runs on Amazon EC2 instances with an Amazon RDS for MySQL Multi-AZ DB instance. Amazon RDS is configured with the latest generation DB instance with 2,000 GB of storage in a General Purpose SSD (gp3) Amazon EBS volume. The database performance affects the application during periods of high demand. A database administrator analyzes the logs in Amazon CloudWatch Logs and discovers that the application performance always degrades when the number of read and write IOPS is higher than 20,000. What should a solutions architect do to improve the application performance?

**Short Question:** Which storage change fixes RDS performance degradation once IOPS demand exceeds 20,000 on a gp3 volume?

**Options:**
- A. Replace the volume with a magnetic volume.
- B. Increase the number of IOPS on the gp3 volume.
- ✅ C. Replace the volume with a Provisioned IOPS SSD (io2) volume.
- D. Replace the 2,000 GB gp3 volume with two 1,000 GB gp3 volumes.

**Reason:** gp3 volumes max out at 16,000 IOPS, so they cannot meet a 20,000+ IOPS requirement, while io2 volumes support up to 64,000 IOPS and are built for I/O-intensive workloads. Magnetic volumes would make performance far worse, and RDS does not support striping multiple EBS volumes for a single instance.

---

## Question 363

**Full Question:** A company operates a two-tier application for image processing. The application uses two Availability Zones, each with one public subnet and one private subnet. An Application Load Balancer (ALB) for the web tier uses the public subnets. Amazon EC2 instances for the application tier use the private subnets. Users report that the application is running more slowly than expected. A security audit of the web server log files shows that the application is receiving millions of illegitimate requests from a small number of IP addresses. A solutions architect needs to resolve the immediate performance problem while the company investigates a more permanent solution. What should the solutions architect recommend to meet this requirement?

**Short Question:** What's the quickest way to block traffic from a few known malicious IP addresses hitting a load balancer in public subnets?

**Options:**
- A. Modify the inbound security group for the web tier. Add a deny rule for the IP addresses that are consuming resources.
- ✅ B. Modify the network ACL for the web tier subnets. Add an inbound deny rule for the IP addresses that are consuming resources.
- C. Modify the inbound security group for the application tier. Add a deny rule for the IP addresses that are consuming resources.
- D. Modify the network ACL for the application tier subnets. Add an inbound deny rule for the IP addresses that are consuming resources.

**Reason:** Network ACLs are stateless and support explicit deny rules, so adding a deny rule on the web tier's public subnet NACL blocks the malicious IPs before they reach the ALB. Security groups only support allow rules (no deny), and the application tier's private subnets only see traffic from the ALB, not the original attacker IPs, so a NACL rule there would be ineffective.

---

## Question 364

**Full Question:** A solutions architect is designing an asynchronous application to process credit card data validation requests for a bank. The application must be secure and be able to process each request at least once. Which solution will meet these requirements most cost-effectively?

**Short Question:** Most cost-effective, secure way to build an async Lambda-based system that guarantees at-least-once processing of encrypted requests?

**Options:**
- ✅ A. Use AWS Lambda event source mapping. Set an Amazon SQS standard queue as the event source. Use AWS Key Management Service (AWS KMS) for encryption. Add the kms:Decrypt permission for the Lambda execution role.
- B. Use AWS Lambda event source mapping. Use an Amazon SQS FIFO queue as the event source. Use SQS-managed encryption keys (SSE-SQS) for encryption. Add the encryption key invocation permission for the Lambda function.
- C. Use AWS Lambda event source mapping. Set an Amazon SQS FIFO queue as the event source. Use AWS KMS keys (SSE-KMS). Add the kms:Decrypt permission for the Lambda execution role.
- D. Use AWS Lambda event source mapping. Set an Amazon SQS standard queue as the event source. Use AWS KMS keys (SSE-KMS) for encryption. Add the "encryption key invocation" permission for the Lambda function.

**Reason:** An SQS standard queue already guarantees at-least-once delivery and is cheaper than FIFO (which adds unneeded ordering/exactly-once guarantees), and reading a KMS-encrypted message requires the Lambda execution role to have the specific kms:Decrypt permission, which only option A states correctly. Options B and D use FIFO or a vaguely-named permission instead of the correct kms:Decrypt.

---

## Question 365

**Full Question:** A company has an application that processes customer orders. The company hosts the application on an Amazon EC2 instance that saves the orders to an Amazon Aurora database. Occasionally, when traffic is high, the workload does not process orders fast enough. What should a solutions architect do to write the orders reliably to the database as quickly as possible?

**Short Question:** How should a single-EC2-instance order system be redesigned so a traffic spike doesn't cause orders to be lost or delayed?

**Options:**
- A. Increase the instance size of the EC2 instance when traffic is high. Write orders to Amazon SNS. Subscribe the database endpoint to the SNS topic.
- ✅ B. Write orders to an Amazon SQS queue. Use EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SQS queue and process orders into the database.
- C. Write orders to Amazon SNS. Subscribe the database endpoint to the SNS topic. Use EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SNS topic.
- D. Write orders to an Amazon SQS queue. When the EC2 instance reaches CPU threshold limits, use scheduled scaling of EC2 instances in an Auto Scaling group behind an Application Load Balancer to read from the SQS queue and process orders into the database.

**Reason:** Writing orders to an SQS queue decouples ingestion from processing, and an Auto Scaling group of EC2 instances can scale based on queue depth to reliably drain the backlog even under bursty traffic. A database endpoint cannot subscribe to an SNS topic, and scheduled scaling only fits predictable traffic, not sudden unpredictable spikes.

---

## Question 366

**Full Question:** A company wants to use the AWS Cloud to make an existing application highly available and resilient. The current version of the application resides in the company's data center. The application recently experienced data loss after a database server crashed because of an unexpected power outage. The company needs a solution that avoids any single points of failure. The solution must give the application the ability to scale to meet user demand. Which solution will meet these requirements?

**Short Question:** Best AWS architecture to make a two-tier application highly available, resilient, and scalable while avoiding single points of failure?

**Options:**
- ✅ A. Deploy the application servers by using Amazon EC2 instances in an autoscaling group across multiple availability zones. Use an Amazon RDS DB instance in a Multi-AZ configuration.
- B. Deploy the application servers by using Amazon EC2 instances in an autoscaling group in a single availability zone. Deploy the database on an EC2 instance. Enable EC2 auto recovery.
- C. Deploy the application servers by using Amazon EC2 instances in an autoscaling group across multiple availability zones. Use an Amazon RDS DB instance with a read replica in a single availability zone. Promote the read replica to replace the primary DB instance if the primary DB instance fails.
- D. Deploy the application servers by using Amazon EC2 instances in an autoscaling group across multiple availability zones. Deploy the primary and secondary database servers on EC2 instances across multiple availability zones. Use Amazon Elastic Block Store (Amazon EBS) Multi-Attach to create shared storage between the instances.

**Reason:** An autoscaling group spanning multiple AZs plus RDS Multi-AZ gives synchronous replication and automatic failover, eliminating single points of failure with minimal operational overhead. A single-AZ app tier or self-managed EC2 database (options B and D) reintroduces single points of failure and heavy management burden, and a read replica (option C) uses asynchronous replication with manual failover, which can lose data.

---

## Question 367

**Full Question:** A company is planning to migrate a commercial off-the-shelf application from its on-premises data center to AWS. The software has a software licensing model using sockets and cores, with predictable capacity and uptime requirements. The company wants to use its existing licenses, which were purchased earlier this year. Which Amazon EC2 pricing option is the most cost-effective?

**Short Question:** Most cost-effective EC2 option for a predictable, bring-your-own-license workload billed by physical sockets and cores?

**Options:**
- ✅ A. Dedicated Reserved Hosts
- B. Dedicated On-Demand Hosts
- C. Dedicated Reserved Instances
- D. Dedicated On-Demand Instances

**Reason:** Dedicated Hosts give visibility into physical sockets and cores needed to comply with the licensing model, and reserving one (Dedicated Reserved Host) gives the biggest discount for a predictable, long-term workload; Dedicated Instances lack that socket/core visibility, and On-Demand pricing is not the cheapest choice for steady, predictable usage.

---

## Question 368

**Full Question:** A company has implemented a self-managed DNS service on AWS. The solution consists of Amazon EC2 instances in different AWS Regions that are configured as endpoints of a standard accelerator in AWS Global Accelerator. The company wants to protect the solution against DDoS attacks. What should a solutions architect do to meet this requirement?

**Short Question:** How to add DDoS protection to a DNS service that uses AWS Global Accelerator with EC2 endpoints across regions?

**Options:**
- ✅ A. Subscribe to AWS Shield Advanced. Add the accelerator as a resource to protect.
- B. Subscribe to AWS Shield Advanced. Add the EC2 instances as resources to protect.
- C. Create an AWS WAF web ACL that includes a rate-based rule. Associate the web ACL with the accelerator.
- D. Create an AWS WAF web ACL that includes a rate-based rule. Associate the web ACL with the EC2 instances.

**Reason:** DDoS protection must be applied at the application's public entry point, and AWS Shield Advanced supports AWS Global Accelerator as a protected resource type; individual EC2 instances cannot be added to Shield Advanced directly, and AWS WAF only handles layer-7 HTTP/S threats and cannot attach to a Global Accelerator or EC2 instances.

---

## Question 369

**Full Question:** A company has deployed a Java Spring Boot application as a pod that runs on Amazon Elastic Kubernetes Service (Amazon EKS) in private subnets. The application needs to write data to an Amazon DynamoDB table. A solutions architect must ensure that the application can interact with the DynamoDB table without exposing traffic to the internet. Which combination of steps should the solutions architect take to accomplish this goal? (Choose two.)

**Short Question:** Which two steps let an EKS pod in a private subnet securely write to DynamoDB without going over the internet?

**Options:**
- ✅ A. Attach an IAM role that has sufficient privileges to the EKS pod.
- B. Attach an IAM user that has sufficient privileges to the EKS pod.
- C. Allow outbound connectivity to the DynamoDB table through the private subnet's network ACLs.
- ✅ D. Create a VPC endpoint for DynamoDB.
- E. Embed the access keys in the Java Spring Boot code.

**Reason:** IAM roles for service accounts (IRSA) give the pod temporary, secure credentials instead of long-lived keys, and a gateway VPC endpoint for DynamoDB provides a private network path from the VPC to DynamoDB so traffic never touches the internet; attaching an IAM user or hardcoding access keys are insecure anti-patterns, and network ACL changes alone don't create the missing route to DynamoDB.

---

## Question 370

**Full Question:** A company runs an internal browser-based application. The application runs on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 autoscaling group across multiple availability zones. The autoscaling group scales up to 20 instances during work hours but scales down to two instances overnight. Staff are complaining that the application is very slow when the day begins, although it runs well by mid-morning. How should the scaling be changed to address the staff complaints and keep costs to a minimum?

**Short Question:** How to fix slow morning performance from a scaling group that reacts too slowly to the daily traffic spike, without overspending?

**Options:**
- A. Implement a scheduled action that sets the desired capacity to 20 shortly before the office opens.
- B. Implement a step scaling action triggered at a lower CPU threshold and decrease the cooldown period.
- ✅ C. Implement a target tracking action triggered at a lower CPU threshold and decrease the cooldown period.
- D. Implement a scheduled action that sets the minimum and maximum capacity to 20 shortly before the office opens.

**Reason:** Lowering the target tracking CPU threshold makes the autoscaling group add instances sooner as morning traffic ramps up, and a shorter cooldown lets it keep scaling out quickly, so capacity still tracks real demand and stays cost-efficient. Fixed scheduled actions (A and D) always jump to 20 instances regardless of actual traffic, wasting money on quiet days, and step scaling (B) is a valid fix but is generally more complex to manage than target tracking, which AWS recommends for typical use cases like this.

---

## Question 371

**Full Question:** A company wants to migrate a Windows-based application from on-premises to the AWS Cloud. The application has three tiers: an application tier, a business tier, and a database tier running Microsoft SQL Server. The company wants to use specific features of SQL Server, such as native backups and Data Quality Services. The company also needs to share files for processing between the tiers. How should a solutions architect design the architecture to meet these requirements?

**Short Question:** Best AWS hosting design for a 3-tier Windows/SQL Server app that needs native SQL Server features and shared file access between tiers?

**Options:**
- A. Host all three tiers on Amazon EC2 instances. Use Amazon FSx File Gateway for file sharing between the tiers.
- ✅ B. Host all three tiers on Amazon EC2 instances. Use Amazon FSx for Windows File Server for file sharing between the tiers.
- C. Host the application tier and the business tier on Amazon EC2 instances. Host the database tier on Amazon RDS. Use Amazon Elastic File System (Amazon EFS) for file sharing between the tiers.
- D. Host the application tier and the business tier on Amazon EC2 instances. Host the database tier on Amazon RDS. Use a Provisioned IOPS SSD (io2) Amazon Elastic Block Store (Amazon EBS) volume for file sharing between the tiers.

**Reason:** Because the company needs SQL Server features like native backups and Data Quality Services that Amazon RDS doesn't expose, the database must run on EC2 for full OS-level access, and Amazon FSx for Windows File Server provides the native SMB shared file system needed between Windows EC2 instances — EFS only supports NFS (Linux), EBS can't be shared across instances, and FSx File Gateway is meant for hybrid on-premises-to-cloud connectivity, not primary storage for an all-AWS deployment.

---

## Question 372

**Full Question:** A company wants to migrate its on-premises application to AWS. The application produces output files that vary in size from tens of gigabytes to hundreds of terabytes. The application data must be stored in a standard file system structure. The company wants a solution that scales automatically, is highly available, and requires minimum operational overhead. Which solution will meet these requirements?

**Short Question:** Most scalable, highly available, low-maintenance way to run an app needing a huge shared standard file system?

**Options:**
- A. Migrate the application to run as containers on Amazon Elastic Container Service (Amazon ECS). Use Amazon S3 for storage.
- B. Migrate the application to run as containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use Amazon Elastic Block Store (Amazon EBS) for storage.
- ✅ C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.
- D. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic Block Store (Amazon EBS) for storage.

**Reason:** A Multi-AZ Auto Scaling group of EC2 instances gives the required scalability and high availability, and Amazon EFS is a fully managed, serverless shared file system that scales automatically to petabytes and can be mounted concurrently by many instances — unlike S3 (object storage, not a mountable file system) or EBS (block storage that only attaches to one instance/pod at a time).

---

## Question 373

**Full Question:** A company uses Amazon EC2 instances to host its internal systems. As part of a deployment operation, an administrator tries to use the AWS CLI to terminate an EC2 instance. However, the administrator receives a 403 Access Denied error message. The administrator is using an IAM role that has a specific IAM policy attached (containing an Allow statement granting EC2 terminate permissions, and a Deny statement that applies unless the source IP falls within the CIDR blocks 192.0.2.0/24 or 203.0.113.0/24). What is the cause of the unsuccessful request?

**Short Question:** Why does an IAM user with an "allow terminate" policy still get Access Denied when terminating an EC2 instance?

**Options:**
- A. The EC2 instance has a resource-based policy with a deny statement.
- B. The principal has not been specified in the policy statement.
- C. The action field does not grant the actions that are required to terminate the EC2 instance.
- ✅ D. The request to terminate the EC2 instance does not originate from the CIDR blocks 192.0.2.0/24 or 203.0.113.0/24.

**Reason:** The IAM policy has an Allow statement plus a Deny statement with an IP address condition, and in IAM policy evaluation an explicit deny always overrides an allow — since the request was denied, the administrator's IP address must fall outside the allowed CIDR ranges, not because permissions or the principal are missing (EC2 also doesn't support resource-based policies like S3 does).

---

## Question 374

**Full Question:** A company wants to migrate 100 GB of historical data from an on-premises location to an Amazon S3 bucket. The company has a 100 megabits per second (Mbps) internet connection on premises. The company needs to encrypt the data in transit to the S3 bucket. The company will store new data directly in Amazon S3. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Lowest-overhead way to do a one-time 100 GB encrypted-in-transit migration to S3 over a 100 Mbps link?

**Options:**
- A. Use the `aws s3 sync` command in the AWS CLI to move the data directly to an S3 bucket.
- ✅ B. Use AWS DataSync to migrate the data from the on-premises location to an S3 bucket.
- C. Use AWS Snowball to move the data to an S3 bucket.
- D. Set up an IPsec VPN from the on-premises location to AWS. Use the `aws s3 cp` command in the AWS CLI to move the data directly to an S3 bucket.

**Reason:** With a 100 Mbps connection, 100 GB transfers over the network in just a few hours, so AWS DataSync — a fully managed service that automatically handles in-transit encryption, data integrity checks, scheduling, and monitoring — meets the requirement with the least operational effort; CLI-based sync/cp still work but require manual monitoring/scripting, a VPN adds unnecessary complexity since HTTPS already encrypts CLI transfers, and Snowball is meant for much larger (terabyte/petabyte-scale) transfers where shipping a physical device makes sense.

---

## Question 375

**Full Question:** A development team needs to host a website that will be accessed by other teams. The website contents consist of HTML, CSS, client-side JavaScript, and images. Which method is the most cost-effective for hosting the website?

**Short Question:** Cheapest way to host a purely static website (HTML/CSS/JS/images, no server-side processing)?

**Options:**
- A. Containerize the website and host it in AWS Fargate.
- ✅ B. Create an Amazon S3 bucket and host the website there.
- C. Deploy a web server on an Amazon EC2 instance to host the website.
- D. Configure an Application Load Balancer with an AWS Lambda target that uses the Express.js framework.

**Reason:** Amazon S3's built-in static website hosting feature is the cheapest option since it only charges for storage and (very inexpensive) data transfer with no servers to run or manage, whereas Fargate, EC2, and a Lambda-behind-ALB setup all involve running compute resources continuously or unnecessary complexity better suited to dynamic applications.
