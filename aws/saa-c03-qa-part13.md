# AWS SAA-C03 Real Exam Questions & Answers — Part 13 (Q301–Q325)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 13](https://www.youtube.com/watch/1brDfwMOEEs)

---

## Question 301

**Full Question:** A company is designing a new web application that will run on Amazon EC2 instances. The application will use Amazon DynamoDB for back-end data storage. The application traffic will be unpredictable. The company expects that the application read and write throughput to the database will be moderate to high. The company needs to scale in response to application traffic. Which DynamoDB table configuration will meet these requirements most cost-effectively?

**Short Question:** Which DynamoDB capacity mode and table class is the most cost-effective choice for unpredictable, moderate-to-high traffic?

**Options:**
- A. Configure DynamoDB with provisioned read and write by using the DynamoDB Standard table class. Set DynamoDB auto scaling to a maximum defined capacity.
- ✅ B. Configure DynamoDB in on-demand mode by using the DynamoDB Standard table class.
- C. Configure DynamoDB with provisioned read and write by using the DynamoDB Standard Infrequent Access (Standard-IA) table class. Set DynamoDB auto scaling to a maximum defined capacity.
- D. Configure DynamoDB in on-demand mode by using the DynamoDB Standard Infrequent Access (Standard-IA) table class.

**Reason:** On-demand mode automatically handles unpredictable traffic without manual capacity planning, and the Standard table class has a lower per-request cost than Standard-IA, which is only cheaper for infrequently accessed data — making it a poor fit for moderate-to-high throughput.

---

## Question 302

**Full Question:** A company wants to manage Amazon Machine Images (AMIs). The company currently copies AMIs to the same AWS Region where the AMIs were created. The company needs to design an application that captures AWS API calls and sends alerts whenever the Amazon EC2 CreateImage API operation is called within the company's account. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead way to get alerted whenever the EC2 CreateImage API is called?

**Options:**
- A. Create an AWS Lambda function to query AWS CloudTrail logs and to send an alert when a CreateImage API call is detected.
- B. Configure AWS CloudTrail with an Amazon SNS notification that occurs when updated logs are sent to Amazon S3. Use Amazon Athena to create a new table and to query on CreateImage when an API call is detected.
- ✅ C. Create an Amazon EventBridge (Amazon CloudWatch Events) rule for the CreateImage API call. Configure the target as an Amazon SNS topic to send an alert when a CreateImage API call is detected.
- D. Configure an Amazon SQS FIFO queue as a target for AWS CloudTrail logs. Create an AWS Lambda function to send an alert to an Amazon SNS topic when a CreateImage API call is detected.

**Reason:** All AWS API calls are automatically delivered as events to EventBridge, so a simple EventBridge rule filtering on the CreateImage event can directly trigger an SNS alert — a fully managed, real-time, serverless solution, unlike the polling (A), log-file-plus-Athena querying (B), or unsupported CloudTrail-to-SQS (D) approaches.

---

## Question 303

**Full Question:** A company is migrating applications to AWS. The applications are deployed in different accounts. The company manages the accounts centrally by using AWS Organizations. The company's security team needs a single sign-on (SSO) solution across all the company's accounts. The company must continue managing the users and groups in its on-premises self-managed Microsoft Active Directory. Which solution will meet these requirements?

**Short Question:** How do you set up centralized SSO across all AWS accounts while keeping users/groups managed in an on-premises Active Directory?

**Options:**
- A. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a one-way forest trust or a one-way domain trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- ✅ B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- C. Use AWS Directory Service. Create a two-way trust relationship with the company's self-managed Microsoft Active Directory.
- D. Deploy an identity provider (IdP) on premises. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console.

**Reason:** The standard architecture pairs AWS SSO (now IAM Identity Center) with AWS Directory Service for Microsoft AD using a two-way forest trust, which lets AWS properly query the on-premises AD for authentication and directory lookups — a one-way trust (A) is insufficient, and option C omits the SSO component needed for centralized cross-account access.

---

## Question 304

**Full Question:** A company uses Amazon API Gateway to run a private gateway with two REST APIs in the same VPC. The BuyStock RESTful web service calls the CheckFunds RESTful web service to ensure that enough funds are available before stock can be purchased. The company has noticed in the VPC flow logs that the BuyStock RESTful web service calls the CheckFunds RESTful web service over the internet instead of through the VPC. A solutions architect must implement a solution so that the APIs communicate through the VPC. Which solution will meet these requirements with the fewest changes to the code?

**Short Question:** How do you force two API Gateway REST APIs in the same VPC to talk to each other privately instead of over the internet, with minimal code changes?

**Options:**
- A. Add an X-API-Key header in the HTTP header for authorization.
- ✅ B. Use an interface endpoint.
- C. Use a gateway endpoint.
- D. Add an Amazon SQS queue between the two REST APIs.

**Reason:** An interface VPC endpoint (powered by AWS PrivateLink) for API Gateway's execute-api gives the API a private IP inside the VPC so calls resolve and stay on the private network with no code changes, whereas gateway endpoints only support S3 and DynamoDB, API keys don't affect network routing, and adding SQS would require a major architecture rewrite.

---

## Question 305

**Full Question:** A company has an aging network-attached storage (NAS) array in its data center. The NAS array presents SMB shares and NFS shares to client workstations. The company does not want to purchase a new NAS array. The company also does not want to incur the cost of renewing the NAS array support contract. Some of the data is accessed frequently, but much of the data is inactive. A solutions architect needs to implement a solution that migrates the data to Amazon S3, uses S3 lifecycle policies, and maintains the same look and feel for the client workstations. The solutions architect has identified AWS Storage Gateway as part of the solution. Which type of Storage Gateway should the solutions architect provision to meet these requirements?

**Short Question:** Which Storage Gateway type replaces an on-prem NAS with S3-backed SMB/NFS shares that support S3 lifecycle policies?

**Options:**
- A. Volume Gateway
- B. Tape Gateway
- C. Amazon FSx File Gateway
- ✅ D. Amazon S3 File Gateway

**Reason:** Amazon S3 File Gateway presents standard SMB and NFS shares to client workstations while storing the underlying data as S3 objects, enabling S3 lifecycle policies; Volume Gateway is block storage (iSCSI), Tape Gateway is for backup/archival (VTL), and FSx File Gateway is backed by Amazon FSx for Windows File Server rather than S3.

---

## Question 306

**Full Question:** A company runs a web application that is deployed on Amazon EC2 instances in the private subnet of a VPC. An Application Load Balancer (ALB) that extends across the public subnets directs web traffic to the EC2 instances. The company wants to implement new security measures to restrict inbound traffic from the ALB to the EC2 instances while preventing access from any other source inside or outside the private subnet of the EC2 instances. Which solution will meet these requirements?

**Short Question:** How do you restrict EC2 instances to only accept traffic from their ALB and nothing else?

**Options:**
- A. Configure a route in a route table to direct traffic from the internet to the private IP addresses of the EC2 instances
- ✅ B. Configure the security group for the EC2 instances to only allow traffic that comes from the security group for the ALB
- C. Move the EC2 instances into the public subnet. Give the EC2 instances a set of Elastic IP addresses
- D. Configure the security group for the ALB to allow any TCP traffic on any port

**Reason:** Referencing the ALB's security group as the allowed source in the EC2 instances' security group ensures only ALB traffic gets through, denying everything else by default. Route tables aren't firewalls (A), exposing instances in a public subnet is a security anti-pattern (C), and opening up the ALB's security group to all TCP traffic makes it overly permissive and doesn't even target the right resource (D).

---

## Question 307

**Full Question:** A company runs a Java-based job on an Amazon EC2 instance. The job runs every hour and takes 10 seconds to run. The job runs on a scheduled interval and consumes 1 GB of memory. The CPU utilization of the instance is low except for short surges during which the job uses the maximum CPU available. The company wants to optimize the costs to run the job. Which solution will meet these requirements?

**Short Question:** What's the cheapest way to run a tiny 10-second job that fires once an hour?

**Options:**
- A. Use AWS App2Container (A2C) to containerize the job. Run the job as an Amazon ECS task on AWS Fargate with 0.5 vCPU and 1 GB of memory
- ✅ B. Copy the code into an AWS Lambda function that has 1 GB of memory. Create an Amazon EventBridge scheduled rule to run the code each hour
- C. Use AWS App2Container (A2C) to containerize the job. Install the container in the existing Amazon Machine Image (AMI). Ensure that the schedule stops the container when the task finishes
- D. Configure the existing schedule to stop the EC2 instance at the completion of the job and restart the EC2 instance when the next job starts

**Reason:** AWS Lambda bills by actual execution duration (rounded to the nearest millisecond), so a 10-second hourly job costs almost nothing with zero idle charges. Fargate (A) has a roughly 1-minute minimum billing duration, wasting most of the charge on idle time, while installing on an AMI (C) or stopping/starting the EC2 instance (D) still leaves compute running or adds startup delay and operational complexity.

---

## Question 308

**Full Question:** A company is experiencing sudden increases in demand. The company needs to provision large Amazon EC2 instances from an Amazon Machine Image (AMI). The instances will run in an Auto Scaling group. The company needs a solution that provides minimum initialization latency to meet the demand. Which solution meets these requirements?

**Short Question:** How do you make new EC2 instances launched from an AMI fully performant immediately, with no warm-up lag?

**Options:**
- A. Use the AWS EC2 Register Image command to create an AMI from a snapshot. Use AWS Step Functions to replace the AMI in the Auto Scaling group
- ✅ B. Enable Amazon EBS fast snapshot restore on a snapshot. Provision an AMI by using the snapshot. Replace the AMI in the Auto Scaling group with the new AMI
- C. Enable AMI creation and define lifecycle rules in Amazon Data Lifecycle Manager (Amazon DLM). Create an AWS Lambda function that modifies the AMI in the Auto Scaling group
- D. Use Amazon EventBridge to invoke AWS Backup lifecycle policies that provision AMIs. Configure Auto Scaling group capacity limits as an event source in EventBridge

**Reason:** EBS fast snapshot restore pre-initializes volumes created from a snapshot, eliminating the lazy-loading I/O latency that normally causes reduced performance right after launch. The other options just automate AMI creation or replacement workflows (Step Functions, DLM, EventBridge/Backup) but none of them actually address the root cause of slow EBS volume initialization.

---

## Question 309

**Full Question:** A company has an API that receives real-time data from a fleet of monitoring devices. The API stores this data in an Amazon RDS DB instance for later analysis. The amount of data that the monitoring devices send to the API fluctuates. During periods of heavy traffic, the API often returns timeout errors. After an inspection of the logs, the company determines that the database is not capable of processing the volume of write traffic that comes from the API. A solutions architect must minimize the number of connections to the database and must ensure that data is not lost during periods of heavy traffic. Which solution will meet these requirements?

**Short Question:** How do you protect a database from a bursty write-heavy API without losing data or overloading DB connections?

**Options:**
- A. Increase the size of the DB instance to an instance type that has more available memory
- B. Modify the DB instance to be a Multi-AZ DB instance. Configure the application to write to all active RDS DB instances
- ✅ C. Modify the API to write incoming data to an Amazon SQS queue. Use an AWS Lambda function that Amazon SQS invokes to write data from the queue to the database
- D. Modify the API to write incoming data to an Amazon SNS topic. Use an AWS Lambda function that Amazon SNS invokes to write data from the topic to the database

**Reason:** Putting an SQS queue between the API and the database decouples the two and acts as a durable buffer, so a Lambda function can drain the queue and write to the database at a controlled, steady rate without losing data during spikes. Scaling up the DB instance (A) is not truly elastic, Multi-AZ standby instances can't be written to (B), and SNS is built for one-to-many fanout rather than the one-to-one work-queue pattern needed here (D).

---

## Question 310

**Full Question:** A company recently migrated its entire IT environment to the AWS Cloud. The company discovers that users are provisioning oversized Amazon EC2 instances and modifying security group rules without using the appropriate change control process. A solutions architect must devise a strategy to track and audit these inventory and configuration changes. Which actions should the solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two AWS services give you an audit trail of API calls and resource configuration changes?

**Options:**
- ✅ A. Enable AWS CloudTrail and use it for auditing
- B. Use data lifecycle policies for the Amazon EC2 instances
- C. Enable AWS Trusted Advisor and reference the security dashboard
- ✅ D. Enable AWS Config and create rules for auditing and compliance purposes
- E. Restore previous resource configurations with an AWS CloudFormation template

**Reason:** AWS CloudTrail records who did what, when, and from where for every API call (like launching an oversized instance or changing a security group), while AWS Config continuously tracks resource configuration history and can enforce compliance rules — together they cover both the "actions taken" and "configuration state" auditing needs. Data Lifecycle Manager (B) only manages snapshot/AMI backups, Trusted Advisor (C) gives best-practice recommendations rather than a full change history, and CloudFormation (E) is a remediation tool, not an auditing one.

---

## Question 311

**Full Question:** A company is planning to move its data to an Amazon S3 bucket. The data must be encrypted when it is stored in the S3 bucket. Additionally, the encryption key must be automatically rotated every year. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Simplest, lowest-maintenance way to encrypt S3 data with encryption keys that auto-rotate yearly?

**Options:**
- ✅ A. Move the data to the S3 bucket. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use the built-in key rotation behavior of SSE-S3 encryption keys
- B. Create an AWS KMS customer-managed key. Enable automatic key rotation. Set the S3 bucket's default encryption behavior to use the customer-managed KMS key. Move the data to the S3 bucket
- C. Create an AWS KMS customer-managed key. Set the S3 bucket's default encryption behavior to use the customer-managed KMS key. Move the data to the S3 bucket. Manually rotate the KMS key every year
- D. Encrypt the data with customer key material before moving the data to the S3 bucket. Create an AWS KMS key without key material. Import the customer key material into the KMS key. Enable automatic key rotation

**Reason:** SSE-S3 is fully managed by AWS, requiring only a single setting with no key management or rotation work by the user, making it the lowest-overhead option. KMS customer-managed keys (B, C) require the user to manage the key lifecycle, and imported key material (D) cannot use automatic rotation at all, requiring manual rotation instead.

---

## Question 312

**Full Question:** A company has an AWS Lambda function that needs read access to an Amazon S3 bucket that is located in the same AWS account. Which solution will meet these requirements in the most secure manner?

**Short Question:** Most secure way to give a Lambda function read access to a specific S3 bucket in the same account?

**Options:**
- A. Apply an S3 bucket policy that grants read access to the S3 bucket
- ✅ B. Apply an IAM role to the Lambda function. Apply an IAM policy to the role to grant read access to the S3 bucket
- C. Embed an access key and a secret key in the Lambda function's code to grant the required IAM permissions for read access to the S3 bucket
- D. Apply an IAM role to the Lambda function. Apply an IAM policy to the role to grant read access to all S3 buckets in the account

**Reason:** Attaching an execution role scoped to just the one bucket follows least privilege and gives Lambda temporary, secure credentials automatically. A bucket policy alone doesn't specify a principal (A), hardcoded keys are a major security anti-pattern (C), and granting access to all buckets is overly permissive (D).

---

## Question 313

**Full Question:** An online retail company has more than 50 million active customers and receives more than 25,000 orders each day. The company collects purchase data for customers and stores this data in Amazon S3. Additional customer data is stored in Amazon RDS. The company wants to make all the data available to various teams so that the teams can perform analytics. The solution must provide the ability to manage fine-grained permissions for the data and must minimize operational overhead. Which solution will meet these requirements?

**Short Question:** Best low-overhead way to unify S3 and RDS data for analytics with fine-grained (table/column-level) access control?

**Options:**
- A. Migrate the purchase data to write directly to Amazon RDS. Use RDS access controls to limit access
- B. Schedule an AWS Lambda function to periodically copy data from Amazon RDS to Amazon S3. Create an AWS Glue Crawler. Use Amazon Athena to query the data. Use S3 policies to limit access
- ✅ C. Create a data lake by using AWS Lake Formation. Create an AWS Glue JDBC connection to Amazon RDS. Register the S3 bucket in Lake Formation. Use Lake Formation access controls to limit access
- D. Create an Amazon Redshift cluster. Schedule an AWS Lambda function to periodically copy data from Amazon S3 and Amazon RDS to Amazon Redshift. Use Amazon Redshift access controls to limit access

**Reason:** AWS Lake Formation is a managed service that can register both S3 and RDS data sources and provides centralized, fine-grained (database/table/column-level) permission management with minimal effort. RDS access controls (A) and S3 bucket policies (B) aren't granular enough, and building a Redshift warehouse with custom ETL (D) adds significant operational overhead.

---

## Question 314

**Full Question:** A company has hundreds of Amazon EC2 Linux-based instances in the AWS Cloud. Systems administrators have used shared SSH keys to manage the instances. After a recent audit, the company's security team is mandating the removal of all shared keys. A solutions architect must design a solution that provides secure access to the EC2 instances. Which solution will meet this requirement with the least amount of administrative overhead?

**Short Question:** Lowest-overhead way to eliminate shared SSH keys while keeping secure access to hundreds of EC2 instances?

**Options:**
- ✅ A. Use AWS Systems Manager Session Manager to connect to the EC2 instances
- B. Use AWS Security Token Service (AWS STS) to generate one-time SSH keys on demand
- C. Allow shared SSH access to a set of bastion instances. Configure all other instances to allow only SSH access from the bastion instances
- D. Use an Amazon Cognito custom authorizer to authenticate users. Invoke an AWS Lambda function to generate a temporary SSH key

**Reason:** Systems Manager Session Manager gives secure, IAM-controlled, browser/CLI-based access without any SSH keys, open inbound ports, or bastion hosts, eliminating shared keys entirely with minimal admin work. STS issues AWS credentials, not SSH keys (B), bastion hosts still require SSH key management and host maintenance (C), and a custom Cognito/Lambda key-generation system is complex to build and maintain (D).

---

## Question 315

**Full Question:** A company wants to implement a backup strategy for Amazon EC2 data and multiple Amazon S3 buckets. Because of regulatory requirements, the company must retain backup files for a specific time period. The company must not alter the files for the duration of the retention period. Which solution will meet these requirements?

**Short Question:** Best way to enforce immutable (WORM), regulatory-compliant backups for both EC2 and S3?

**Options:**
- A. Use AWS Backup to create a backup vault that has a vault lock in governance mode. Create the required backup plan
- B. Use Amazon Data Lifecycle Manager to create the required automated snapshot policy
- C. Use Amazon S3 File Gateway to create the backup. Configure the appropriate S3 lifecycle management
- ✅ D. Use AWS Backup to create a backup vault that has a vault lock in compliance mode. Create the required backup plan

**Reason:** AWS Backup centrally backs up both EC2 and S3, and Vault Lock in compliance mode enforces true write-once-read-many (WORM) immutability that cannot be altered or deleted by anyone, including the root user. Governance mode (A) still allows privileged users to change settings, Data Lifecycle Manager doesn't support S3 (B), and S3 File Gateway is a hybrid file-access service, not a backup solution for EC2 (C).

---

## Question 316

**Full Question:** A company runs a microservice-based serverless web application. The application must be able to retrieve data from multiple Amazon DynamoDB tables. A solutions architect needs to give the application the ability to retrieve the data with no impact on the baseline performance of the application. Which solution will meet these requirements in the most operationally efficient way?

**Short Question:** What's the most operationally efficient way to query across multiple DynamoDB tables without hitting the production tables' read capacity?

**Options:**
- A. AWS AppSync pipeline resolvers
- B. Amazon CloudFront with Lambda@Edge functions
- C. Edge-optimized Amazon API Gateway with AWS Lambda functions
- ✅ D. Amazon Athena Federated Query with a DynamoDB connector

**Reason:** AppSync, Lambda@Edge, and API Gateway with Lambda would all query the live DynamoDB tables directly, consuming read capacity and impacting performance. Athena Federated Query provides a separate, serverless analytical query layer that decouples these lookups from the production tables.

---

## Question 317

**Full Question:** A company has a Java application that uses Amazon Simple Queue Service (Amazon SQS) to parse messages. The application cannot parse messages that are larger than 256 KB in size. The company wants to implement a solution to give the application the ability to parse messages as large as 50 MB. Which solution will meet these requirements with the fewest changes to the code?

**Short Question:** With minimal code changes, how do you let a Java app using SQS handle messages up to 50 MB instead of the 256 KB limit?

**Options:**
- ✅ A. Use the Amazon SQS Extended Client Library for Java to host messages that are larger than 256 KB in Amazon S3
- B. Use Amazon EventBridge to post large messages from the application instead of Amazon SQS
- C. Change the limit in Amazon SQS to handle messages that are larger than 256 KB
- D. Store messages that are larger than 256 KB in Amazon Elastic File System (Amazon EFS). Configure Amazon SQS to reference this location in the messages

**Reason:** The SQS Extended Client Library for Java is the purpose-built solution: it automatically stores large payloads in S3 and sends only a small pointer message through SQS, requiring minimal code changes. EventBridge has the same 256 KB limit, the SQS limit itself is fixed and cannot be changed, and manually building an EFS pointer pattern would require significant custom code.

---

## Question 318

**Full Question:** A company that hosts its web application on AWS wants to ensure all Amazon EC2 instances, Amazon RDS DB instances, and Amazon Redshift clusters are configured with tags. The company wants to minimize the effort of configuring and operating this check. What should a solutions architect do to accomplish this?

**Short Question:** What's the lowest-effort way to continuously check that EC2, RDS, and Redshift resources are properly tagged?

**Options:**
- ✅ A. Use AWS Config Rules to define and detect resources that are not properly tagged
- B. Use Cost Explorer to display resources that are not properly tagged. Tag those resources manually
- C. Write API calls to check all resources for proper tag allocation. Periodically run the code on an EC2 instance
- D. Write API calls to check all resources for proper tag allocation. Schedule an AWS Lambda function through Amazon CloudWatch to periodically run the code

**Reason:** AWS Config provides a managed "required tags" rule that continuously and automatically audits resource compliance with no custom code needed. Cost Explorer is only for cost analysis and requires manual follow-up, while the EC2- and Lambda-based options both require writing and maintaining custom scripts.

---

## Question 319

**Full Question:** A company has a regional subscription-based streaming service that runs in a single AWS Region. The architecture consists of web servers and application servers on Amazon EC2 instances. The EC2 instances are in Auto Scaling groups behind Elastic Load Balancers. The architecture includes an Amazon Aurora global database cluster that extends across multiple Availability Zones. The company wants to expand globally and to ensure that its application has minimal downtime. Which solution will provide the most fault tolerance?

**Short Question:** How do you expand a single-region app to a second region with the highest fault tolerance and minimal downtime?

**Options:**
- A. Extend the Auto Scaling groups for the web tier and the application tier to deploy instances in Availability Zones in a second region. Use an Aurora global database to deploy the database in the primary region and the second region. Use Amazon Route 53 health checks with a failover routing policy to the second region
- B. Deploy the web tier and the application tier to a second region. Add an Aurora PostgreSQL cross-region Aurora replica in the second region. Use Amazon Route 53 health checks with a failover routing policy to the second region. Promote the secondary to primary as needed
- C. Deploy the web tier and the application tier to a second region. Create an Aurora PostgreSQL database in the second region. Use AWS Database Migration Service (AWS DMS) to replicate the primary database to the second region. Use Amazon Route 53 health checks with a failover routing policy to the second region
- ✅ D. Deploy the web tier and the application tier to a second region. Use an Amazon Aurora global database to deploy the database in the primary region and the second region. Use Amazon Route 53 health checks with a failover routing policy to the second region. Promote the secondary to primary as needed

**Reason:** Option D deploys a full independent stack in the second region and uses Aurora Global Database, which offers fast native cross-region replication (RPO of seconds, RTO under a minute) combined with Route 53 failover. Auto Scaling groups can't span regions (ruling out A), a plain cross-region read replica has higher RPO than a global database (ruling out B), and DMS-based replication has more lag and complexity than Aurora's native replication (ruling out C).

---

## Question 320

**Full Question:** A company wants to securely exchange data between its software as a service (SaaS) application Salesforce account and Amazon S3. The company must encrypt the data at rest by using AWS Key Management Service (AWS KMS) customer managed keys (CMKs). The company must also encrypt the data in transit. The company has enabled API access for the Salesforce account. Which solution will meet these requirements?

**Short Question:** What's the managed, low-effort way to securely transfer data between Salesforce and S3 with KMS customer managed key encryption at rest and encryption in transit?

**Options:**
- A. Create AWS Lambda functions to transfer the data securely from Salesforce to Amazon S3
- B. Create an AWS Step Functions workflow. Define the task to transfer the data securely from Salesforce to Amazon S3
- ✅ C. Create Amazon AppFlow flows to transfer the data securely from Salesforce to Amazon S3
- D. Create a custom connector for Salesforce to transfer the data securely from Salesforce to Amazon S3

**Reason:** Amazon AppFlow is a fully managed integration service with a built-in Salesforce connector that encrypts data in transit automatically and supports specifying a customer managed KMS key for encryption at rest in S3. Lambda and Step Functions would require writing and maintaining custom integration code, and building a custom connector is the highest-effort option of all.

---

## Question 321

**Full Question:** A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS region. The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes. Modifications to the clone data must not affect the production environment. The software that accesses this data requires consistently high IO performance. A solutions architect needs to minimize the time that is required to clone the production data into the test environment. Which solution will meet these requirements?

**Short Question:** What's the fastest way to clone EBS volumes into a test environment while keeping full IO performance and not touching production data?

**Options:**
- A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.
- B. Configure the production EBS volumes to use the EBS multi-attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.
- C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.
- ✅ D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.

**Reason:** EBS Fast Snapshot Restore (FSR) pre-initializes new volumes created from a snapshot so they deliver full IOPS immediately, avoiding the lazy-loading warm-up delay. The other options either use the wrong storage type (instance store can't be restored from a snapshot), dangerously reattach live production volumes, or describe an illogical restore sequence.

---

## Question 322

**Full Question:** An IoT company is releasing a mattress that has sensors to collect data about a user's sleep. The sensors will send data to an Amazon S3 bucket. The sensors collect approximately 2 MB of data every night for each mattress. The company must process and summarize the data for each mattress. The results need to be available as soon as possible. Data processing will require 1 GB of memory and will finish within 30 seconds. Which solution will meet these requirements most cost effectively?

**Short Question:** What's the cheapest way to run a small, 30-second, 1GB-memory data processing job triggered by an S3 upload?

**Options:**
- A. Use AWS Glue with a Scala job.
- B. Use Amazon EMR with an Apache Spark script.
- ✅ C. Use AWS Lambda with a Python script.
- D. Use AWS Glue with a PySpark job.

**Reason:** AWS Lambda is serverless and event-driven, can be triggered directly by an S3 upload, and bills per millisecond of actual use, making it ideal for this quick 30-second job. AWS Glue and Amazon EMR both carry minimum billing durations or cluster management overhead that make them far more expensive for such a small, short-running task.

---

## Question 323

**Full Question:** A solutions architect is reviewing the resilience of an application. The solutions architect notices that a database administrator recently failed over the application's Amazon Aurora PostgreSQL database writer instance as part of a scaling exercise. The failover resulted in 3 minutes of downtime for the application. Which solution will reduce the downtime for scaling exercises with the least operational overhead?

**Short Question:** How do you cut application downtime during Aurora PostgreSQL writer failovers with minimal extra management effort?

**Options:**
- A. Create more Aurora PostgreSQL read replicas in the cluster to handle the load during failover.
- B. Set up a secondary Aurora PostgreSQL cluster in the same AWS region. During failover, update the application to use the secondary cluster's writer endpoint.
- C. Create an Amazon ElastiCache for Memcached cluster to handle the load during failover.
- ✅ D. Set up an Amazon RDS Proxy for the database. Update the application to use the proxy endpoint.

**Reason:** Amazon RDS Proxy maintains a stable, persistent connection pool and transparently redirects traffic to the newly promoted writer during a failover, cutting reconnection time from minutes to seconds without manual intervention. Read replicas, a secondary manual cluster, and ElastiCache for Memcached don't address the connection-switching delay that actually causes the downtime.

---

## Question 324

**Full Question:** A business application is hosted on Amazon EC2 and uses Amazon S3 for encrypted object storage. The chief information security officer has directed that no application traffic between the two services should traverse the public internet. Which capability should the solutions architect use to meet the compliance requirements?

**Short Question:** How do you keep EC2-to-S3 traffic off the public internet entirely?

**Options:**
- A. AWS Key Management Service (AWS KMS)
- ✅ B. VPC endpoint
- C. Private subnet
- D. Virtual Private Gateway

**Reason:** A VPC endpoint (for S3) provides private connectivity so traffic stays on the AWS network instead of traversing the public internet. AWS KMS only handles encryption keys, a private subnet alone doesn't create connectivity to S3 without an endpoint or NAT gateway, and a Virtual Private Gateway is for connecting to on-premises networks via VPN/Direct Connect, not to AWS services like S3.

---

## Question 325

**Full Question:** A media company hosts its website on AWS. The website application's architecture includes a fleet of Amazon EC2 instances behind an Application Load Balancer (ALB), and a database that is hosted on Amazon Aurora. The company's cybersecurity team reports that the application is vulnerable to SQL injection. How should the company resolve this issue?

**Short Question:** What service should you use to protect a web application behind an ALB from SQL injection attacks?

**Options:**
- ✅ A. Use AWS WAF in front of the ALB. Associate the appropriate web ACLs with AWS WAF.
- B. Create an ALB listener rule to reply to SQL injections with a fixed response.
- C. Subscribe to AWS Shield Advanced to block all SQL injection attempts automatically.
- D. Set up Amazon Inspector to block all SQL injection attempts automatically.

**Reason:** AWS WAF is purpose-built to inspect web requests and block application-layer attacks like SQL injection using web ACL rules placed in front of the ALB. ALB listener rules only route traffic and can't inspect for attacks, AWS Shield defends against network/transport-layer DDoS attacks rather than SQL injection, and Amazon Inspector only scans for vulnerabilities—it can't actively block real-time attacks.
