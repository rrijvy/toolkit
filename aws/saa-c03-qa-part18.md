# AWS SAA-C03 Real Exam Questions & Answers — Part 18 (Q421–Q440)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 18](https://www.youtube.com/watch/VqOiwbj1ark)

---

## Question 421

**Full Question:** A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume. For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer. After completing this change, users reported that each time they refreshed the website, they could see one subset of their documents or the other, but never all of the documents at the same time. What should a solutions architect propose to ensure users see all of their documents at once?

**Short Question:** How do you fix an ALB-fronted two-instance app where each EC2 instance's separate EBS volume only has half the user's files?

**Options:**
- A. Copy the data so both EBS volumes contain all the documents
- B. Configure the Application Load Balancer to direct a user to the server with the documents
- ✅ C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS
- D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server

**Reason:** The instances need a shared storage layer instead of isolated, per-instance EBS volumes; Amazon EFS provides a common file system that both instances can mount and read/write simultaneously, so every request sees the complete document set regardless of which server handles it.

---

## Question 422

**Full Question:** A company has multiple AWS accounts that use consolidated billing. The company runs several active, high-performance Amazon RDS for Oracle On-Demand DB instances for 90 days. The company's finance team has access to AWS Trusted Advisor in the consolidated billing account and all other AWS accounts. The finance team needs to use the appropriate AWS account to access the Trusted Advisor check recommendations for RDS. The finance team must review the appropriate Trusted Advisor check to reduce RDS costs. Which combination of steps should the finance team take to meet these requirements? (Choose two.)

**Short Question:** Which account and which Trusted Advisor check should the finance team use to find RDS cost savings across a multi-account organization?

**Options:**
- A. Use the Trusted Advisor recommendations from the account where the RDS instances are running
- ✅ B. Use the Trusted Advisor recommendations from the consolidated billing account to see all RDS instance checks at the same time
- C. Review the Trusted Advisor check for Amazon RDS reserved instance optimization
- ✅ D. Review the Trusted Advisor check for Amazon RDS idle DB instances
- E. Review the Trusted Advisor check for Amazon Redshift reserved node optimization

**Reason:** The management (consolidated billing) account gives a single centralized view of Trusted Advisor checks across all member accounts, and the "idle DB instances" check directly identifies RDS instances generating cost with no active connections — a more fundamental savings opportunity than reserved instance optimization, and unlike the Redshift check, it actually applies to RDS.

---

## Question 423

**Full Question:** A company has a business system that generates hundreds of reports each day. The business system saves the reports to a network share in CSV format. The company needs to store this data in the AWS Cloud in near real time for analysis. Which solution will meet these requirements with the least administrative overhead?

**Short Question:** What's the lowest-overhead way to get an on-premises network share's files into Amazon S3 in near real time?

**Options:**
- A. Use AWS DataSync to transfer the files to Amazon S3. Create a scheduled task that runs at the end of each day
- ✅ B. Create an Amazon S3 File Gateway. Update the business system to use a new network share from the S3 File Gateway
- C. Use AWS DataSync to transfer the files to Amazon S3. Create an application that uses the DataSync API in an automation workflow
- D. Deploy an AWS Transfer Family SFTP endpoint. Create a script that checks for new files on the network share and uploads the new files by using SFTP

**Reason:** An Amazon S3 File Gateway presents a standard NFS/SMB share that automatically and continuously uploads written files to S3 as objects, requiring only a simple share reconfiguration on the business system — unlike the daily batch DataSync job, custom DataSync automation, or a self-managed upload script, all of which add operational overhead or fail the near-real-time requirement.

---

## Question 424

**Full Question:** A company is designing a containerized application that will use Amazon Elastic Container Service (Amazon ECS). The application needs to access a shared file system that is highly durable and can recover data to another AWS Region with a recovery point objective (RPO) of 8 hours. The file system needs to provide a mount target in each Availability Zone within a Region. A solutions architect wants to use AWS Backup to manage the replication to another Region. Which solution will meet these requirements?

**Short Question:** Which shared file system for an ECS app gives multi-AZ mount targets and cross-region recovery via AWS Backup with an 8-hour RPO?

**Options:**
- A. Amazon FSx for Windows File Server with a Multi-AZ deployment
- B. Amazon FSx for NetApp ONTAP with a Multi-AZ deployment
- ✅ C. Amazon Elastic File System (Amazon EFS) with the Standard storage class
- D. Amazon FSx for OpenZFS

**Reason:** Amazon EFS is a regional, serverless NFS file system with mount targets in every Availability Zone, and AWS Backup natively supports scheduled EFS backups replicated to another Region to meet the 8-hour RPO; FSx for Windows uses SMB (not suited to Linux containers), FSx for ONTAP is unnecessarily complex/costly, and FSx for OpenZFS is single-AZ only.

---

## Question 425

**Full Question:** A company has launched an Amazon RDS for MySQL DB instance. Most of the connections to the database come from serverless applications. Application traffic to the database changes significantly at random intervals. At times of high demand, users report that their applications experience database connection rejection errors. Which solution will resolve this issue with the least operational overhead?

**Short Question:** How do you stop serverless apps from overwhelming an RDS MySQL database with too many connections, with minimal ongoing effort?

**Options:**
- ✅ A. Create a proxy in RDS Proxy. Configure the users' applications to use the DB instance through RDS Proxy
- B. Deploy Amazon ElastiCache for Memcached between the users' applications and the DB instance
- C. Migrate the DB instance to a different instance class that has higher I/O capacity. Configure the users' applications to use the new DB instance
- D. Configure Multi-AZ for the DB instance. Configure the users' applications to switch between the DB instances

**Reason:** Amazon RDS Proxy is a fully managed connection pooler that lets many application connections share a small pool of actual database connections, directly solving connection exhaustion from bursty serverless traffic; caching, a bigger instance, or Multi-AZ failover don't manage or pool connections and so don't fix the root cause.

---

## Question 426

**Full Question:** A gaming company has a web application that displays scores. The application runs on Amazon EC2 instances behind an Application Load Balancer. The application stores data in an Amazon RDS for MySQL database. Users are starting to experience long delays and interruptions that are caused by database read performance. The company wants to improve the user experience while minimizing changes to the application's architecture. What should a solutions architect do to meet these requirements?

**Short Question:** What's the least-disruptive way to fix slow database reads that are causing delays for users?

**Options:**
- ✅ A. Use Amazon ElastiCache in front of the database
- B. Use RDS Proxy between the application and the database
- C. Migrate the application from EC2 instances to AWS Lambda
- D. Migrate the database from Amazon RDS for MySQL to Amazon DynamoDB

**Reason:** Amazon ElastiCache is a managed in-memory cache that can store the results of frequent queries, offloading read traffic from RDS with minimal architectural change. RDS Proxy only helps with connection pooling and failover, not caching, and the Lambda/DynamoDB migrations are major architecture changes that don't directly target the read-performance bottleneck.

---

## Question 427

**Full Question:** A solutions architect must secure a VPC network that hosts Amazon EC2 instances. The EC2 instances contain highly sensitive data and run in a private subnet. According to company policy, the EC2 instances that run in the VPC can access only approved third-party software repositories on the internet for software product updates, using the third party's URL. Other internet traffic must be blocked. Which solution meets these requirements?

**Short Question:** How do you restrict EC2 instances in a private subnet to only reach specific approved URLs on the internet?

**Options:**
- ✅ A. Update the route table for the private subnet to route the outbound traffic to an AWS Network Firewall firewall. Configure domain list rule groups.
- B. Set up an AWS WAF web ACL. Create a custom set of rules that filter traffic requests based on source and destination IP address range sets.
- C. Implement strict inbound security group rules. Configure an outbound rule that allows traffic only to the authorized software repositories on the internet by specifying the URLs.
- D. Configure an Application Load Balancer (ALB) in front of the EC2 instances. Direct all outbound traffic to the ALB. Use a URL-based rule listener in the ALB's target group for outbound access to the internet.

**Reason:** AWS Network Firewall supports domain list rule groups that filter outbound traffic by destination domain/URL, which is exactly what's needed here. Security groups can't filter by URL (only IPs/CIDRs/prefix lists), WAF protects inbound web traffic, and an ALB is not a forward proxy for outbound traffic.

---

## Question 428

**Full Question:** A company runs a website that uses a content management system (CMS) on Amazon EC2. The CMS runs on a single EC2 instance and uses an Amazon Aurora MySQL Multi-AZ DB instance for the data tier. Website images are stored on an Amazon Elastic Block Store (Amazon EBS) volume that is mounted inside the EC2 instance. Which combination of actions should a solutions architect take to improve the performance and resilience of the website? (Choose two.)

**Short Question:** Which two changes fix the single points of failure in the web/compute tier and image storage of this CMS website?

**Options:**
- A. Move the website images into an Amazon S3 bucket that is mounted on every EC2 instance
- B. Share the website images by using an NFS share from the primary EC2 instance. Mount this share on the other EC2 instances.
- ✅ C. Move the website images onto an Amazon Elastic File System (Amazon EFS) file system that is mounted on every EC2 instance
- D. Create an Amazon Machine Image (AMI) from the existing EC2 instance. Use the AMI to provision new instances behind an Application Load Balancer as part of an Auto Scaling group. Configure the Auto Scaling group to maintain a minimum of two instances. Configure an accelerator in AWS Global Accelerator for the website.
- ✅ E. Create an Amazon Machine Image (AMI) from the existing EC2 instance. Use the AMI to provision new instances behind an Application Load Balancer as part of an Auto Scaling group. Configure the Auto Scaling group to maintain a minimum of two instances. Configure an Amazon CloudFront distribution for the website.

**Reason:** Amazon EFS provides shared, highly available storage that multiple EC2 instances can mount simultaneously, removing the single EBS volume as a bottleneck; combined with an AMI-based Auto Scaling group behind an ALB plus CloudFront (rather than Global Accelerator, which is meant for non-HTTP global traffic routing, not typical website caching), the compute tier becomes resilient and performant. S3 can't be natively mounted as a filesystem, and using one EC2 instance as an NFS source just recreates the single point of failure.

---

## Question 429

**Full Question:** A company has a three-tier stateless web application. The web application runs on Amazon EC2 instances in an Auto Scaling group with a dynamic scaling policy that is configured to respond to scaling events. The database tier runs on Amazon RDS for PostgreSQL. The web application does not require temporary local storage on the EC2 instances. The company's recovery point objective (RPO) is 2 hours. The backup strategy must maximize scalability and optimize resource utilization for this environment. Which solution will meet these requirements?

**Short Question:** What's the most efficient backup strategy for a stateless EC2 web tier plus an RDS database, given a 2-hour RPO?

**Options:**
- A. Take snapshots of Amazon EBS volumes of the EC2 instances and database every 2 hours to meet the RPO
- B. Configure a snapshot lifecycle policy to take Amazon EBS snapshots. Enable automated backups in Amazon RDS to meet the RPO.
- ✅ C. Retain the latest Amazon Machine Images (AMIs) of the web and application tiers. Enable automated backups in Amazon RDS and use point-in-time recovery to meet the RPO.
- D. Take snapshots of Amazon EBS volumes of the EC2 instances every 2 hours. Enable automated backups in Amazon RDS and use point-in-time recovery to meet the RPO.

**Reason:** Since the EC2 tier is stateless, there's no unique data on its EBS volumes worth snapshotting — keeping an up-to-date AMI as a "golden image" for Auto Scaling to relaunch instances is sufficient; RDS automated backups with point-in-time recovery easily satisfy the 2-hour RPO for the stateful database. Taking periodic EBS snapshots of stateless instances (options A, B, D) is unnecessary overhead that doesn't optimize resource utilization.

---

## Question 430

**Full Question:** A company stores data in PDF format in an Amazon S3 bucket. The company must follow a legal requirement to retain all new and existing data in Amazon S3 for 7 years. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-effort way to enforce a strict 7-year, tamper-proof retention policy on both new and existing S3 objects?

**Options:**
- A. Turn on the S3 Versioning feature for the S3 bucket. Configure S3 Lifecycle to delete the data after seven years. Configure multi-factor authentication (MFA) Delete for all S3 objects.
- B. Turn on S3 Object Lock with governance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Recopy all existing objects to bring the existing data into compliance.
- C. Turn on S3 Object Lock with compliance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Recopy all existing objects to bring the existing data into compliance.
- ✅ D. Turn on S3 Object Lock with compliance retention mode for the S3 bucket. Set the retention period to expire after 7 years. Use S3 Batch Operations to bring the existing data into compliance.

**Reason:** S3 Object Lock in compliance mode is the only option that creates a true WORM (write-once-read-many) lock that even the root user can't override, satisfying the strict legal requirement; using S3 Batch Operations applies the lock to existing objects at scale without the massive overhead of downloading and re-uploading every file (as recopying would require). Versioning with MFA Delete (option A) doesn't provide a guaranteed unchangeable lock, and governance mode (option B) can still be overridden by privileged users.

---

## Question 431

**Full Question:** A company has resources across multiple AWS Regions and accounts. A newly hired solutions architect discovers that a previous employee did not provide details about the resource inventory. The solutions architect needs to build and map the relationship details of the various workloads across all accounts. Which solution will meet these requirements in the most operationally efficient way?

**Short Question:** Best automated way to discover and map AWS resource relationships across multiple accounts and Regions?

**Options:**
- A. Use AWS Systems Manager Inventory to generate a map view from the detailed view report
- B. Use AWS Step Functions to collect workload details, and build architecture diagrams of the workloads manually
- ✅ C. Use Workload Discovery on AWS to generate architecture diagrams of the workloads
- D. Use AWS X-Ray to view the workload details, and build architecture diagrams with relationships

**Reason:** Workload Discovery on AWS (formerly AWS Perspective) is purpose-built to import data from AWS Config and AWS CloudTrail across multiple accounts and Regions and automatically generate architecture diagrams; Systems Manager Inventory only gathers OS/software metadata, Step Functions with manual diagramming is high-effort, and X-Ray only traces application requests, not infrastructure inventory.

---

## Question 432

**Full Question:** A company has a large data set for its online advertising business stored in an Amazon RDS for MySQL DB instance in a single Availability Zone. The company wants business reporting queries to run without impacting the write operations to the production DB instance. Which solution meets these requirements?

**Short Question:** How to run heavy reporting queries without slowing down the primary RDS database's write operations?

**Options:**
- ✅ A. Deploy RDS read replicas to process the business reporting queries
- B. Scale out the DB instance horizontally by placing it behind an Elastic Load Balancer
- C. Scale up the DB instance to a larger instance type to handle write operations and queries
- D. Deploy the DB instance in multiple Availability Zones to process the business reporting queries

**Reason:** RDS read replicas offload read-heavy reporting workloads to a separate readable copy of the database, freeing the primary instance to handle writes; you cannot put RDS behind an ELB, vertical scaling doesn't fix resource contention between reads and writes, and a Multi-AZ standby cannot serve read traffic.

---

## Question 433

**Full Question:** A company recently deployed a new auditing system to centralize information about operating system versions, patching, and installed software for Amazon EC2 instances. A solutions architect must ensure all instances provisioned through EC2 Auto Scaling groups successfully send reports to the auditing system as soon as they are launched and terminated. Which solution achieves these goals most efficiently?

**Short Question:** Best way to trigger custom actions on both EC2 instance launch and termination within an Auto Scaling group?

**Options:**
- A. Use a scheduled AWS Lambda function to run a script remotely on all EC2 instances to send data to the audit system
- ✅ B. Use EC2 Auto Scaling lifecycle hooks to run a custom script to send data to the audit system when instances are launched and terminated
- C. Use an EC2 Auto Scaling launch configuration to run a custom script through user data to send data to the audit system when instances are launched and terminated
- D. Run a custom script on the instance operating system to send data to the audit system, and configure the script to be invoked by the EC2 Auto Scaling group when the instance starts and is terminated

**Reason:** EC2 Auto Scaling lifecycle hooks are specifically designed to pause an instance in a wait state during launch or termination so a custom script can run at exactly those two points; a scheduled Lambda is inefficient polling, and user data only runs once at launch, so it can't handle termination.

---

## Question 434

**Full Question:** A company hosts an online shopping application that stores all orders in an Amazon RDS for PostgreSQL single-AZ DB instance. Management wants to eliminate single points of failure and has asked a solutions architect to recommend an approach to minimize database downtime without requiring any changes to the application code. Which solution meets these requirements?

**Short Question:** Simplest way to make an existing single-AZ RDS database highly available with zero application code changes?

**Options:**
- ✅ A. Convert the existing database instance to a Multi-AZ deployment by modifying the database instance and specifying the Multi-AZ option
- B. Create a new RDS Multi-AZ deployment, take a snapshot of the current RDS instance, and restore the new Multi-AZ deployment with the snapshot
- C. Create a read replica of the PostgreSQL database in another Availability Zone, and use Amazon Route 53 weighted record sets to distribute requests across the databases
- D. Place the RDS for PostgreSQL database in an Amazon EC2 Auto Scaling group with a minimum group size of two, and use Amazon Route 53 weighted record sets to distribute requests across instances

**Reason:** Modifying the existing instance to enable Multi-AZ keeps the same database endpoint (so no app changes are needed) while AWS handles the standby replication and automatic failover; the other options either require a new endpoint, misuse a read replica for failover, or aren't even valid (RDS instances can't run in an EC2 Auto Scaling group).

---

## Question 435

**Full Question:** A company has thousands of edge devices that collectively generate one terabyte of status alerts each day. Each alert is approximately 2 kilobytes in size. A solutions architect needs to implement a solution to ingest and store the alerts for future analysis. The company wants a highly available solution; however, the company needs to minimize costs and does not want to manage additional infrastructure. Additionally, the company wants to keep 14 days of data available for immediate analysis and archive any data older than 14 days. What is the most operationally efficient solution that meets these requirements?

**Short Question:** Most cost-effective, infrastructure-free way to ingest huge daily alert volumes, keep 14 days queryable, then archive the rest?

**Options:**
- ✅ A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts, configure it to deliver the alerts to an Amazon S3 bucket, and set up an S3 lifecycle configuration to transition data to Amazon S3 Glacier after 14 days
- B. Launch Amazon EC2 instances across two Availability Zones and place them behind an Elastic Load Balancer to ingest the alerts; create a script on the EC2 instances to store the alerts in an Amazon S3 bucket, and set up an S3 lifecycle configuration to transition data to Amazon S3 Glacier after 14 days
- C. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts, configure it to deliver the alerts to an Amazon OpenSearch Service cluster, and set up the cluster to take manual snapshots every day and delete data older than 14 days
- D. Create an Amazon SQS standard queue to ingest the alerts and set the message retention period to 14 days; configure consumers to poll the queue, check the age of each message, and analyze it as needed — if a message is 14 days old, the consumer should copy it to an Amazon S3 bucket and delete it from the queue

**Reason:** Kinesis Data Firehose to S3 is a fully managed, serverless pipeline that handles high-volume ingestion with no infrastructure to manage, and an S3 lifecycle rule automatically archives data to Glacier after 14 days at low cost; EC2-based ingestion adds operational overhead, OpenSearch is too expensive for a terabyte/day with manual snapshot management, and SQS isn't a suitable long-term analytics store and its retention tops out at 14 days.

---

## Question 436

**Full Question:** A company hosts a multi-tier web application on Amazon Linux Amazon EC2 instances behind an Application Load Balancer. The instances run in an Auto Scaling group across multiple Availability Zones. The company observes that the Auto Scaling group launches more On-Demand Instances when the application's end users access high volumes of static web content. The company wants to optimize cost. What should a solutions architect do to redesign the application most cost effectively?

**Short Question:** What's the cheapest fix when EC2 Auto Scaling keeps scaling out just to serve static web content?

**Options:**
- A. Update the Auto Scaling group to use Reserved Instances instead of On-Demand Instances
- B. Update the Auto Scaling group to scale by launching Spot Instances instead of On-Demand Instances
- ✅ C. Create an Amazon CloudFront distribution to host the static web content from an Amazon S3 bucket
- D. Create an AWS Lambda function behind an Amazon API Gateway API to host the static website content

**Reason:** Offloading static content (images, CSS, JavaScript) to S3 and serving it through CloudFront removes the unnecessary load from the EC2 instances entirely, stopping the wasteful scale-out and improving performance; Reserved or Spot Instances only make the inefficient scaling cheaper without fixing the root cause, and Lambda/API Gateway is meant for dynamic APIs, not static content hosting.

---

## Question 437

**Full Question:** A company wants to ingest customer payment data into the company's data lake in Amazon S3. The company receives payment data every minute on average. The company wants to analyze the payment data in real time, then ingest the data into the data lake. Which solution will meet these requirements with the most operational efficiency?

**Short Question:** What's the most hands-off way to ingest streaming payment data, analyze it in real time, and land it in an S3 data lake?

**Options:**
- A. Use Amazon Kinesis Data Streams to ingest data. Use AWS Lambda to analyze the data in real time.
- B. Use AWS Glue to ingest data. Use Amazon Kinesis Data Analytics to analyze the data in real time.
- ✅ C. Use Amazon Kinesis Data Firehose to ingest data. Use Amazon Kinesis Data Analytics to analyze the data in real time.
- D. Use Amazon API Gateway to ingest data. Use AWS Lambda to analyze the data in real time.

**Reason:** Kinesis Data Firehose is a fully managed service that ingests streaming data and reliably loads it into S3 with no infrastructure to manage, while Kinesis Data Analytics can read directly from the Firehose stream for real-time analysis; options A and D never actually deliver the data into the S3 data lake, and Glue is a batch ETL tool, not a real-time ingestion service.

---

## Question 438

**Full Question:** A company's application integrates with multiple software as a service (SaaS) sources for data collection. The company runs Amazon EC2 instances to receive the data and to upload the data to an Amazon S3 bucket for analysis. The same EC2 instance that receives and uploads the data also sends a notification to the user when an upload is complete. The company has noticed slow application performance and wants to improve the performance as much as possible. Which solution will meet these requirements with the least operational overhead?

**Short Question:** How do you fix slow performance when one EC2 instance is doing data ingestion, S3 upload, and notifications all by itself?

**Options:**
- ✅ A. Create an Auto Scaling group so that EC2 instances can scale out. Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.
- B. Create an Amazon AppFlow flow to transfer data between each SaaS source and the S3 bucket. Configure an S3 event notification to send events to an Amazon SNS topic when the upload to the S3 bucket is complete.
- C. Create an Amazon EventBridge rule for each SaaS source to send output data. Configure the S3 bucket as the rule's target. Create a second EventBridge rule to send events when the upload to the S3 bucket is complete. Configure an Amazon SNS topic as the second rule's target.
- D. Create a Docker container to use instead of an EC2 instance. Host the containerized application on Amazon Elastic Container Service (Amazon ECS). Configure Amazon CloudWatch Container Insights to send events to an Amazon SNS topic when the upload to the S3 bucket is complete.

**Reason:** An Auto Scaling group lets the ingestion tier scale horizontally to handle load, and offloading notifications to a native S3 event notification triggering SNS decouples that work from the EC2 instances with minimal overhead; AppFlow changes the ingestion model unnecessarily, EventBridge can't take an S3 bucket as a rule target the way described, and CloudWatch Container Insights only monitors performance, it doesn't fire on S3 uploads.

---

## Question 439

**Full Question:** A company is migrating its on-premises workload to the AWS Cloud. The company already uses several Amazon EC2 instances and Amazon RDS DB instances. The company wants a solution that automatically starts and stops the EC2 instances and DB instances outside of business hours. The solution must minimize cost and infrastructure maintenance. Which solution will meet these requirements?

**Short Question:** What's the lowest-maintenance way to automatically start/stop EC2 and RDS instances on a schedule outside business hours?

**Options:**
- A. Scale the EC2 instances by using elastic resize. Scale the DB instances to zero outside of business hours.
- B. Explore AWS Marketplace for partner solutions that will automatically start and stop the EC2 instances and DB instances on a schedule.
- C. Launch another EC2 instance. Configure a cron schedule to run shell scripts that will start and stop the existing EC2 instances and DB instances on a schedule.
- ✅ D. Create an AWS Lambda function that will start and stop the EC2 instances and DB instances. Configure Amazon EventBridge to invoke the Lambda function on a schedule.

**Reason:** A Lambda function invoked on a schedule by Amazon EventBridge is fully serverless, requires no infrastructure to maintain, and only costs for the few seconds it runs; option A uses invalid terminology (you can't scale an RDS instance to zero), option B adds unnecessary third-party complexity and cost, and option C requires provisioning and running an extra EC2 instance 24/7 just to act as a scheduler.

---

## Question 440

**Full Question:** A company has 1 million users that use its mobile app. The company must analyze the data usage in near real time. The company also must encrypt the data in near real time and must store the data in a centralized location in Apache Parquet format for further processing. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead pipeline to ingest, encrypt, analyze near-real-time, and store mobile app usage data as Parquet in a central location?

**Options:**
- A. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data. Invoke an AWS Lambda function to send the data to the Kinesis Data Analytics application.
- B. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data. Invoke an AWS Lambda function to send the data to the EMR cluster.
- C. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data.
- ✅ D. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data.

**Reason:** Kinesis Data Firehose is fully managed and can automatically convert streaming data to Parquet format and deliver it straight to S3 with no servers to manage, while Kinesis Data Analytics can read directly from that Firehose stream for near-real-time analysis; options A and B rely on Kinesis Data Streams, which doesn't write to S3 directly and needs a separate consumer, and options B and C use an EMR cluster, which carries far more operational overhead than a serverless approach.
