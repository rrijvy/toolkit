# AWS SAA-C03 Real Exam Questions & Answers — Part 16 (Q376–Q400)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 16](https://www.youtube.com/watch/tioox1E5qoM)

---

## Question 376

**Full Question:** A data analytics company wants to migrate its batch processing system to AWS. The company receives thousands of small data files periodically during the day through FTP. An on-premises batch job processes the data files overnight. However, the batch job takes hours to finish running. The company wants the AWS solution to process incoming data files as soon as possible with minimal changes to the FTP clients that send the files. The solution must delete the incoming data files after the files have been processed successfully. Processing for each file needs to take 3 to 8 minutes. Which solution will meet these requirements in the most operationally efficient way?

**Short Question:** What's the most operationally efficient AWS way to replace an on-prem FTP-plus-nightly-batch pipeline with near-real-time, per-file processing?

**Options:**
- A. Use an Amazon EC2 instance that runs an FTP server to store incoming files as objects in Amazon S3 Glacier Flexible Retrieval. Configure a job queue in AWS Batch. Use Amazon EventBridge rules to invoke the job to process the objects nightly from S3 Glacier Flexible Retrieval. Delete the objects after the job has processed the objects.
- B. Use an Amazon EC2 instance that runs an FTP server to store incoming files on an Amazon Elastic Block Store (Amazon EBS) volume. Configure a job queue in AWS Batch. Use Amazon EventBridge rules to invoke the job to process the files nightly from the EBS volume. Delete the files after the job has processed the files.
- C. Use AWS Transfer Family to create an FTP server to store incoming files on an Amazon Elastic Block Store (Amazon EBS) volume. Configure a job queue in AWS Batch. Use an Amazon S3 event notification when each file arrives to invoke the job in AWS Batch. Delete the files after the job has processed the files.
- ✅ D. Use AWS Transfer Family to create an FTP server to store incoming files in Amazon S3 Standard. Create an AWS Lambda function to process the files and to delete the files after they are processed. Use an S3 event notification to invoke the Lambda function when the files arrive.

**Reason:** AWS Transfer Family gives a fully managed FTP endpoint backed by S3 with minimal client changes, and an S3 event notification triggers a Lambda function per file (well within Lambda's 15-minute limit for the 3-8 minute jobs), giving near-instant, serverless processing. Options A and B use a self-managed EC2 FTP server with nightly batch processing (too slow and high overhead), and C contradicts itself by storing files on EBS while relying on an S3 event notification as the trigger.

---

## Question 377

**Full Question:** A company seeks a storage solution for its application. The solution must be highly available and scalable. The solution also must function as a file system, be mountable by multiple Linux instances in AWS and on premises through native protocols, and have no minimum size requirements. The company has set up a site-to-site VPN for access from its on-premises network to its VPC. Which storage solution meets these requirements?

**Short Question:** Which storage option gives a highly available, scalable, natively-mountable Linux file system shared between AWS and on-premises with no minimum size?

**Options:**
- A. Amazon FSx Multi-AZ deployments
- B. Amazon Elastic Block Store (Amazon EBS) Multi-Attach volumes
- ✅ C. Amazon Elastic File System (Amazon EFS) with multiple mount targets
- D. Amazon Elastic File System (Amazon EFS) with a single mount target and multiple access points

**Reason:** Amazon EFS is a serverless, regional NFS file system that stays highly available by having a mount target in each Availability Zone, and it can be reached from on-premises over the VPN. FSx variants target Windows or Lustre workloads rather than native Linux NFS, EBS Multi-Attach is block storage confined to a single AZ (not a real file system), and a single EFS mount target creates an AZ-level single point of failure regardless of access points.

---

## Question 378

**Full Question:** A company runs applications on Amazon EC2 instances in one AWS Region. The company wants to back up the EC2 instances to a second Region. The company also wants to provision EC2 resources in the second Region and manage the EC2 instances centrally from one AWS account. Which solution will meet these requirements most cost-effectively?

**Short Question:** What's the cheapest, centrally-managed way to back up whole EC2 instances to a second AWS Region?

**Options:**
- A. Create a disaster recovery (DR) plan that has a similar number of EC2 instances in the second Region. Configure data replication.
- B. Create point-in-time Amazon Elastic Block Store (Amazon EBS) snapshots of the EC2 instances. Copy the snapshots to the second Region periodically.
- ✅ C. Create a backup plan by using AWS Backup. Configure cross-Region backup to the second Region for the EC2 instances.
- D. Deploy a similar number of EC2 instances in the second Region. Use AWS DataSync to transfer the data from the source Region to the second Region.

**Reason:** AWS Backup is a centralized, fully managed service that can back up an entire EC2 instance (AMI plus EBS snapshots) with a single plan and handle cross-Region copying automatically, so you only pay for backup storage rather than running duplicate instances. Options A and D keep a parallel fleet of EC2 instances running (expensive), and option B only captures EBS data, missing the instance configuration needed for a full restore.

---

## Question 379

**Full Question:** A company containerized a Windows job that runs on .NET 6 framework under a Windows container. The company wants to run this job in the AWS Cloud. The job runs every 10 minutes. The job's runtime varies between 1 minute and 3 minutes. Which solution will meet these requirements most cost-effectively?

**Short Question:** What's the cheapest way to run a short, recurring Windows container job (every 10 minutes, 1-3 minute runtime) on AWS?

**Options:**
- A. Create an AWS Lambda function based on the container image of the job. Configure Amazon EventBridge to invoke the function every 10 minutes.
- B. Use AWS Batch to create a job that uses AWS Fargate resources. Configure the job scheduling to run every 10 minutes.
- ✅ C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate to run the job. Create a scheduled task based on the container image of the job to run every 10 minutes.
- D. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate to run the job. Create a standalone task based on the container image of the job. Use Windows Task Scheduler to run the job every 10 minutes.

**Reason:** Amazon ECS on AWS Fargate supports Windows containers and has a built-in scheduled tasks feature, making it a serverless, pay-per-use way to run the job on its 10-minute cadence. Lambda container images only support Linux, AWS Batch's Fargate support is Linux-only (Windows would need EC2 compute, which isn't serverless), and Windows Task Scheduler runs inside an OS instance and can't schedule a serverless Fargate task.

---

## Question 380

**Full Question:** A company experienced a breach that affected several applications in its on-premises data center. The attacker took advantage of vulnerabilities in the custom applications that were running on the servers. The company is now migrating its applications to run on Amazon EC2 instances. The company wants to implement a solution that actively scans for vulnerabilities on the EC2 instances and sends a report that details the findings. Which solution will meet these requirements?

**Short Question:** Which AWS service actively scans EC2 instances for software vulnerabilities and can generate findings reports?

**Options:**
- A. Deploy AWS Shield to scan the EC2 instances for vulnerabilities. Create an AWS Lambda function to log any findings to AWS CloudTrail.
- B. Deploy Amazon Macie and AWS Lambda functions to scan the EC2 instances for vulnerabilities. Log any findings to AWS CloudTrail.
- C. Turn on Amazon GuardDuty. Deploy the GuardDuty agents to the EC2 instances. Configure an AWS Lambda function to automate the generation and distribution of reports that detail the findings.
- ✅ D. Turn on Amazon Inspector. Deploy the Amazon Inspector agent to the EC2 instances. Configure an AWS Lambda function to automate the generation and distribution of reports that detail the findings.

**Reason:** Amazon Inspector is purpose-built for automated vulnerability management, actively scanning EC2 instances for software vulnerabilities and unintended network exposure, with findings that can feed an automated reporting pipeline via Lambda. AWS Shield is DDoS protection, Amazon Macie protects sensitive data in S3, and Amazon GuardDuty is threat detection for malicious activity — none of them perform vulnerability scanning.

---

## Question 381

**Full Question:** A company needs to ingest and handle large amounts of streaming data that its application generates. The application runs on Amazon EC2 instances and sends data to Amazon Kinesis Data Streams, which is configured with default settings. Every other day, the application consumes the data and writes it to an Amazon S3 bucket for business intelligence (BI) processing. The company observes that Amazon S3 is not receiving all the data that the application sends to Kinesis Data Streams. What should a solutions architect do to resolve this issue?

**Short Question:** Why is streaming data going missing when a Kinesis consumer only reads once every 48 hours, and what setting fixes it?

**Options:**
- ✅ A. Update the Kinesis Data Streams default settings by modifying the data retention period
- B. Update the application to use the Kinesis Producer Library (KPL) to send the data to Kinesis Data Streams
- C. Update the number of Kinesis shards to handle the throughput of the data that is sent to Kinesis Data Streams
- D. Turn on S3 versioning within the S3 bucket to preserve every version of every object that is ingested in the S3 bucket

**Reason:** Kinesis Data Streams retains data for only 24 hours by default, but the consumer only reads every 48 hours, so older records expire and are lost before they can be consumed; raising the retention period beyond 48 hours keeps the data available long enough to be read. KPL, shard count, and S3 versioning don't address the root cause, which is data expiring in the stream itself.

---

## Question 382

**Full Question:** An IAM user made several configuration changes to AWS resources in their company's account during a production deployment last week. A solutions architect learned that a couple of security group rules are not configured as desired. The solutions architect wants to confirm which IAM user was responsible for making the changes. Which service should the solutions architect use to find the desired information?

**Short Question:** Which AWS service can show exactly which IAM user made a specific configuration change?

**Options:**
- A. Amazon GuardDuty
- B. Amazon Inspector
- ✅ C. AWS CloudTrail
- D. AWS Config

**Reason:** AWS CloudTrail logs every API call made in the account, including the identity of the user or role that made it, so searching the event history reveals exactly which IAM user changed the security group rules. GuardDuty detects threats, Inspector scans for vulnerabilities, and AWS Config tracks how resource configurations changed over time but is not the primary tool for attributing an action to a specific user.

---

## Question 383

**Full Question:** A company wants to restrict access to the content of one of its main web applications and to protect the content by using authorization techniques available on AWS. The company wants to implement a serverless architecture and an authentication solution for fewer than 100 users. The solution needs to integrate with the main web application and serve web content globally. The solution must also scale as the company's user base grows while providing the lowest login latency possible. Which solution will meet these requirements most cost-effectively?

**Short Question:** Which combination of services gives a serverless, globally distributed, low-latency login for under 100 users?

**Options:**
- ✅ A. Use Amazon Cognito for authentication. Use Lambda@Edge for authorization. Use Amazon CloudFront to serve the web application globally
- B. Use AWS Directory Service for Microsoft Active Directory for authentication. Use AWS Lambda for authorization. Use an Application Load Balancer to serve the web application globally
- C. Use Amazon Cognito for authentication. Use AWS Lambda for authorization. Use Amazon S3 Transfer Acceleration to serve the web application globally
- D. Use AWS Directory Service for Microsoft Active Directory for authentication. Use Lambda@Edge for authorization. Use AWS Elastic Beanstalk to serve the web application globally

**Reason:** Amazon Cognito is a cost-effective, serverless identity service, Lambda@Edge runs the authorization logic right at CloudFront's edge locations for the lowest possible login latency, and CloudFront is purpose-built for global content delivery. The other options fail because Directory Service is overkill/less cost-effective for this user base, an Application Load Balancer and Elastic Beanstalk are regional (not global) services, and S3 Transfer Acceleration only speeds up uploads, not content delivery to users.

---

## Question 384

**Full Question:** A company runs a global web application on Amazon EC2 instances behind an Application Load Balancer. The application stores data in Amazon Aurora. The company needs to create a disaster recovery solution and can tolerate up to 30 minutes of downtime and potential data loss. The solution does not need to handle the load when the primary infrastructure is healthy. What should a solutions architect do to meet these requirements?

**Short Question:** What's the right warm-standby disaster recovery setup for a 30-minute recovery window where the backup site doesn't need to serve live traffic?

**Options:**
- ✅ A. Deploy the application with the required infrastructure elements in place. Use Amazon Route 53 to configure active-passive failover. Create an Aurora replica in a second AWS region
- B. Host a scaled-down deployment of the application in a second AWS region. Use Amazon Route 53 to configure active-active failover. Create an Aurora replica in the second region
- C. Replicate the primary infrastructure in a second AWS region. Use Amazon Route 53 to configure active-active failover. Create an Aurora database that is restored from the latest snapshot
- D. Back up data with AWS Backup. Use the backup to create the required infrastructure in a second AWS region. Use Amazon Route 53 to configure active-passive failover. Create an Aurora second primary instance in the second region

**Reason:** A warm-standby setup with a standing Aurora cross-region replica (kept current through asynchronous replication, which is acceptable given the tolerance for data loss) can be promoted quickly to meet the 30-minute RTO, and Route 53 active-passive failover only routes traffic to the secondary region when the primary becomes unhealthy, matching the requirement that the DR site not serve live traffic normally. The other options incorrectly use active-active failover (which sends live traffic to both regions) or rely on a slower snapshot restore.

---

## Question 385

**Full Question:** A company runs an application on Amazon EC2 instances. The company needs to implement a disaster recovery (DR) solution for the application. The DR solution needs to have a recovery time objective (RTO) of less than 4 hours. The DR solution also needs to use the fewest possible AWS resources during normal operations. Which solution will meet these requirements in the most operationally efficient way?

**Short Question:** Which disaster recovery approach uses the fewest standing resources day-to-day while still recovering within 4 hours?

**Options:**
- A. Create Amazon Machine Images (AMIs) to back up the EC2 instances. Copy the AMIs to a secondary AWS region. Automate infrastructure deployment in the secondary region by using AWS Lambda and custom scripts
- ✅ B. Create Amazon Machine Images (AMIs) to back up the EC2 instances. Copy the AMIs to a secondary AWS region. Automate infrastructure deployment in the secondary region by using AWS CloudFormation
- C. Launch EC2 instances in a secondary AWS region. Keep the EC2 instances in the secondary region active at all times
- D. Launch EC2 instances in a secondary availability zone. Keep the EC2 instances in the secondary availability zone active at all times

**Reason:** A backup-and-restore strategy using AMIs copied to a secondary region and deployed with AWS CloudFormation keeps normal-operation costs to just storage, while still provisioning the full stack quickly and reliably within the 4-hour RTO. Option A uses less efficient custom Lambda scripts instead of infrastructure-as-code, and options C and D keep EC2 instances running continuously (violating the "fewest resources" requirement) or only protect against an availability zone failure rather than a full regional outage.

---

## Question 386

**Full Question:** A company is migrating a Linux-based web server group to AWS. The web servers must access files in a shared file store for some content. The company must not make any changes to the application. What should a solutions architect do to meet these requirements?

**Short Question:** Best shared storage option for a group of Linux web servers that needs zero application changes?

**Options:**
- A. Create an Amazon S3 standard bucket with access to the web servers
- B. Configure an Amazon CloudFront distribution with an Amazon S3 bucket as the origin
- ✅ C. Create an Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system on all web servers
- D. Configure a general purpose SSD (GP3) Amazon Elastic Block Store (Amazon EBS) volume. Mount the EBS volume to all web servers

**Reason:** Amazon EFS is a fully managed NFS file system that multiple Linux instances can mount concurrently and access like a normal local directory, requiring no code changes; S3 needs API calls (not a real file system), CloudFront only serves content to end users, and EBS volumes can only attach to one instance at a time.

---

## Question 387

**Full Question:** A company uses AWS Organizations with resources tagged by account. The company also uses AWS Backup to back up its AWS infrastructure resources. The company needs to back up all AWS resources. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Least-effort way to make sure every resource across an AWS Organization gets tagged and included in backups?

**Options:**
- ✅ A. Use AWS Config to identify all untagged resources. Tag the identified resources programmatically. Use tags in the backup plan
- B. Use AWS Config to identify all resources that are not running. Add those resources to the backup vault
- C. Require all AWS account owners to review their resources to identify the resources that need to be backed up
- D. Use Amazon Inspector to identify all non-compliant resources

**Reason:** AWS Config can continuously detect resources missing the required backup tag and trigger automatic remediation (e.g., via Lambda) to tag them, so the AWS Backup plan (which selects resources by tag) picks them up automatically with minimal manual effort; the other options either target the wrong resources, add to a vault instead of a plan, rely on error-prone manual review, or use Inspector, which scans for vulnerabilities, not tags.

---

## Question 388

**Full Question:** A company wants to deploy a new public web application on AWS. The application includes a web server tier that uses Amazon EC2 instances. The application also includes a database tier that uses an Amazon RDS for MySQL DB instance. The application must be secure and accessible for global customers that have dynamic IP addresses. How should a solutions architect configure the security groups to meet these requirements?

**Short Question:** Correct security group setup for a public web tier plus a private database tier when customers have dynamic IPs?

**Options:**
- ✅ A. Configure the security group for the web servers to allow inbound traffic on port 443 from 0.0.0.0/0. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the security group of the web servers
- B. Configure the security group for the web servers to allow inbound traffic on port 443 from the IP addresses of the customers. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the security group of the web servers
- C. Configure the security group for the web servers to allow inbound traffic on port 443 from the IP addresses of the customers. Configure the security group for the DB instance to allow inbound traffic on port 3306 from the IP addresses of the customers
- D. Configure the security group for the web servers to allow inbound traffic on port 443 from 0.0.0.0/0. Configure the security group for the DB instance to allow inbound traffic on port 3306 from 0.0.0.0/0

**Reason:** Since customers have dynamic IPs, the web tier must accept HTTPS from anywhere (0.0.0.0/0), but the database should only trust traffic from the web servers' security group so it is never directly exposed to the internet; option D fails by opening the database to the whole internet, while B and C try to whitelist unstable customer IPs, which is impractical and (in C) also exposes the database.

---

## Question 389

**Full Question:** A company runs an online marketplace web application on AWS. The application serves hundreds of thousands of users during peak hours. The company needs a scalable, near-real-time solution to share the details of millions of financial transactions with several other internal applications. Transactions also need to be processed to remove sensitive data before being stored in a document database for low-latency retrieval. What should a solutions architect recommend to meet these requirements?

**Short Question:** Best near-real-time architecture to stream, sanitize, store, and fan out millions of financial transactions?

**Options:**
- A. Store the transactions data into Amazon DynamoDB. Set up a rule in DynamoDB to remove sensitive data from every transaction upon write. Use DynamoDB Streams to share the transactions data with other applications
- B. Stream the transactions data into Amazon Kinesis Data Firehose to store data in Amazon DynamoDB and Amazon S3. Use AWS Lambda integration with Kinesis Data Firehose to remove sensitive data. Other applications can consume the data stored in Amazon S3
- ✅ C. Stream the transactions data into Amazon Kinesis Data Streams. Use AWS Lambda integration to remove sensitive data from every transaction and then store the transactions data in Amazon DynamoDB. Other applications can consume the transactions data off the Kinesis data stream
- D. Store the batch transactions data in Amazon S3 as files. Use AWS Lambda to process every file and remove sensitive data before updating the files in Amazon S3. The Lambda function then stores the data in Amazon DynamoDB. Other applications can consume transaction files stored in Amazon S3

**Reason:** Kinesis Data Streams gives a highly scalable real-time ingestion point where a Lambda function can strip sensitive data before writing sanitized records to DynamoDB, while other applications independently consume the same stream in near real time; DynamoDB has no built-in write-time transformation rule, Kinesis Data Firehose cannot deliver directly to DynamoDB, and the S3 batch-file approach introduces unwanted latency.

---

## Question 390

**Full Question:** A company serves a dynamic website from a fleet of Amazon EC2 instances behind an Application Load Balancer (ALB). The website needs to support multiple languages to serve customers around the world. The website's architecture is running in the US West 1 region and is exhibiting high request latency for users that are located in other parts of the world. The website needs to serve requests quickly and efficiently regardless of a user's location. However, the company does not want to recreate the existing architecture across multiple regions. What should a solutions architect do to meet these requirements?

**Short Question:** How to cut global latency for a dynamic, multilingual site in one region without deploying it to multiple regions?

**Options:**
- A. Replace the existing architecture with a website that is served from an Amazon S3 bucket. Configure an Amazon CloudFront distribution with the S3 bucket as the origin. Set the cache behavior settings to cache based on the accept-language request header
- ✅ B. Configure an Amazon CloudFront distribution with the ALB as the origin. Set the cache behavior settings to cache based on the accept-language request header
- C. Create an Amazon API Gateway API that is integrated with the ALB. Configure the API to use the HTTP integration type. Set up an API Gateway stage to enable the API cache based on the accept-language request header
- D. Launch an EC2 instance in each additional region and configure Nginx to act as a cache server for that region. Put all the EC2 instances and the ALB behind an Amazon Route 53 record set with a geolocation routing policy

**Reason:** Amazon CloudFront can use the existing ALB as its origin and cache edge content close to users worldwide, and configuring cache behavior on the accept-language header lets it serve the right language variant without touching the application or adding regions; S3 can't host a dynamic site, API Gateway isn't built for accelerating a full website, and deploying EC2 instances per region violates the no-multi-region-rearchitecture requirement.

---

## Question 391

**Full Question:** An application uses an Amazon RDS MySQL DB instance. The RDS database is becoming low on disk space. A solutions architect wants to increase the disk space without downtime. Which solution meets these requirements with the least amount of effort?

**Short Question:** Least-effort, no-downtime way to grow storage on a low-on-space RDS database?

**Options:**
- ✅ A. Enable storage autoscaling in RDS
- B. Increase the RDS database instance size
- C. Change the RDS database instance storage type to Provisioned IOPS
- D. Back up the RDS database, increase the storage capacity, restore the database, and stop the previous instance

**Reason:** RDS storage autoscaling automatically raises allocated storage online once free space drops below a threshold, needing only a one-time setup and no downtime; instance size changes compute not storage, changing storage type addresses IOPS not automatic growth, and the backup/restore approach is manual and causes downtime.

---

## Question 392

**Full Question:** A company stores call transcript files on a monthly basis. Users access the files randomly within one year of the call, but users access the files infrequently after one year. The company wants to optimize its solution by giving users the ability to query and retrieve files that are less than one year old as quickly as possible. A delay in retrieving older files is acceptable. Which solution will meet these requirements most cost-effectively?

**Short Question:** Cheapest way to give fast queryable access to files under 1 year old and delay-tolerant access to older archived files?

**Options:**
- A. Store individual files with tags in Amazon S3 Glacier Instant Retrieval. Query the tags to retrieve the files from S3 Glacier Instant Retrieval
- ✅ B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 lifecycle policies to move the files to S3 Glacier Flexible Retrieval after one year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select
- C. Store individual files with tags in Amazon S3 Standard storage. Store search metadata for each archive in Amazon S3 Standard storage. Use S3 lifecycle policies to move the files to S3 Glacier Instant Retrieval after one year. Query and retrieve the files by searching for metadata from Amazon S3
- D. Store individual files in Amazon S3 Standard storage. Use S3 lifecycle policies to move the files to S3 Glacier Deep Archive after one year. Store search metadata in Amazon RDS. Query the files from Amazon RDS. Retrieve the files from S3 Glacier Deep Archive

**Reason:** S3 Intelligent-Tiering automatically optimizes cost for the unpredictable first-year access pattern while keeping instant retrieval, and a lifecycle policy cheaply moves data to S3 Glacier Flexible Retrieval afterward since delays are acceptable; Athena queries the hot S3 data and S3 Glacier Select queries the archived data, avoiding the extra cost or operational overhead of tags-as-database queries, S3 Standard for year one, or maintaining a separate RDS metadata store.

---

## Question 393

**Full Question:** A company wants to give a customer the ability to use on-premises Microsoft Active Directory to download files that are stored in Amazon S3. The customer's application uses an SFTP client to download the files. Which solution will meet these requirements with the least operational overhead and no changes to the customer's application?

**Short Question:** Least-overhead way to let an SFTP client authenticate via on-prem Active Directory to download files from S3, with no app changes?

**Options:**
- ✅ A. Set up AWS Transfer Family with SFTP for Amazon S3. Configure integrated Active Directory authentication
- B. Set up AWS Database Migration Service (AWS DMS) to synchronize the on-premises client with Amazon S3. Configure integrated Active Directory authentication
- C. Set up AWS DataSync to synchronize between the on-premises location and the S3 location by using AWS IAM Identity Center (AWS Single Sign-On)
- D. Set up a Windows Amazon EC2 instance with SFTP to connect the on-premises client with Amazon S3. Integrate AWS Identity and Access Management (IAM)

**Reason:** AWS Transfer Family provides a fully managed SFTP endpoint backed by S3 with built-in Active Directory integration for authentication, requiring no client-side changes; DMS is for database migration, DataSync has no SFTP endpoint, and a self-managed EC2 SFTP server carries far more operational overhead.

---

## Question 394

**Full Question:** A company has a service that reads and writes large amounts of data from an Amazon S3 bucket in the same AWS Region. The service is deployed on Amazon EC2 instances within the private subnet of a VPC. The service communicates with Amazon S3 over a NAT gateway in the public subnet. However, the company wants a solution that will reduce the data output costs. Which solution will meet these requirements most cost-effectively?

**Short Question:** Cheapest way to cut data transfer costs for a private-subnet EC2 service that talks to S3 in the same region via a NAT gateway?

**Options:**
- A. Provision a dedicated EC2 NAT instance in the public subnet. Configure the route table for the private subnet to use the elastic network interface of this instance as the destination for all S3 traffic
- B. Provision a dedicated EC2 NAT instance in the private subnet. Configure the route table for the public subnet to use the elastic network interface of this instance as the destination for all S3 traffic
- ✅ C. Provision a VPC gateway endpoint. Configure the route table for the private subnet to use the gateway endpoint as the route for all S3 traffic
- D. Provision a second NAT gateway. Configure the route table for the private subnet to use this NAT gateway as the destination for all S3 traffic

**Reason:** A VPC gateway endpoint routes S3 traffic privately within the AWS network with no hourly or per-GB data processing charges, eliminating the NAT gateway cost entirely; a NAT instance still incurs EC2 and internet transfer costs (and can't function placed in a private subnet), and a second NAT gateway still charges the same per-GB processing fee.

---

## Question 395

**Full Question:** A company uses a legacy application to produce data in CSV format. The legacy application stores the output data in Amazon S3. The company is deploying a new commercial off-the-shelf application that can perform complex SQL queries to analyze data that is stored in Amazon Redshift and Amazon S3 only. However, the new application cannot process the CSV files that the legacy application produces. The company cannot update the legacy application to produce data in another format. The company needs to implement a solution so that the new application can use the data that the legacy application produces. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Lowest-overhead way to transform legacy CSV files in S3 so a new SQL-based app (which only reads Redshift/S3) can use them?

**Options:**
- ✅ A. Create an AWS Glue extract, transform, and load (ETL) job that runs on a schedule. Configure the ETL job to process the CSV files and store the processed data in Amazon Redshift
- B. Develop a Python script that runs on Amazon EC2 instances to convert the CSV files to SQL files. Invoke the Python script on a cron schedule to store the output files in Amazon S3
- C. Create an AWS Lambda function and an Amazon DynamoDB table. Use an S3 event to invoke the Lambda function. Configure the Lambda function to perform an ETL job to process the CSV files and store the processed data in the DynamoDB table
- D. Use Amazon EventBridge to launch an Amazon EMR cluster on a weekly schedule. Configure the EMR cluster to perform an ETL job to process the CSV files and store the processed data in an Amazon Redshift table

**Reason:** AWS Glue is a fully managed, serverless ETL service that can read the CSV files, transform them, and load them directly into Amazon Redshift on a schedule with no infrastructure to manage; the EC2 script requires self-managed servers, DynamoDB isn't a format the new app can query, and EMR needs cluster management, making it more overhead than Glue.

---

## Question 396

**Full Question:** A company is making a prototype of the infrastructure for its new website by manually provisioning the necessary infrastructure. This infrastructure includes an Auto Scaling group, an Application Load Balancer, and an Amazon RDS database. After the configuration has been thoroughly validated, the company wants the capability to immediately deploy the infrastructure for development and production use in two Availability Zones in an automated fashion. What should a solutions architect recommend to meet these requirements?

**Short Question:** What's the best way to turn a manually-built prototype infrastructure into a repeatable, automated deployment across two Availability Zones?

**Options:**
- A. Use AWS Systems Manager to replicate and provision the prototype infrastructure in two Availability Zones
- ✅ B. Define the infrastructure as a template by using the prototype infrastructure as a guide. Deploy the infrastructure with AWS CloudFormation
- C. Use AWS Config to record the inventory of resources that are used in the prototype infrastructure. Use AWS Config to deploy the prototype infrastructure into two Availability Zones
- D. Use AWS Elastic Beanstalk and configure it to use an automated reference to the prototype infrastructure to automatically deploy new environments in two Availability Zones

**Reason:** AWS CloudFormation is the native infrastructure-as-code service, letting the architect define the validated ALB, Auto Scaling group, and RDS setup as a template for reliable, repeatable deployments. Systems Manager only manages existing resources, AWS Config only audits/records configuration (it can't deploy), and Elastic Beanstalk has no built-in way to reference and replicate an existing manually-built stack.

---

## Question 397

**Full Question:** A company has a three-tier web application that is on a single server. The company wants to migrate the application to the AWS Cloud. The company also wants the application to align with the AWS Well-Architected Framework and to be consistent with AWS recommended best practices for security, scalability, and resiliency. Which combination of solutions will meet these requirements? (Choose three.)

**Short Question:** Which three actions correctly re-architect a single-server three-tier app into a secure, scalable, resilient AWS design?

**Options:**
- A. Create a VPC across two Availability Zones with the application's existing architecture. Host the application with existing architecture on an Amazon EC2 instance in a private subnet in each Availability Zone with EC2 Auto Scaling groups. Secure the EC2 instance with security groups and network access control lists (network ACL)
- B. Set up security groups and network access control lists (network ACL) to control access to the database layer. Set up a single Amazon RDS database in a private subnet
- ✅ C. Create a VPC across two Availability Zones. Refactor the application to host the web tier, application tier, and database tier. Host each tier on its own private subnet with Auto Scaling groups for the web tier and application tier
- D. Use a single Amazon RDS database. Allow database access only from the application tier security group
- ✅ E. Use Elastic Load Balancers in front of the web tier. Control access by using security groups containing references to each layer's security groups
- ✅ F. Use an Amazon RDS database Multi-AZ cluster deployment in private subnets. Allow database access only from application tier security groups

**Reason:** A true three-tier redesign needs a multi-AZ VPC with each tier refactored onto its own subnet with Auto Scaling (C), a load balancer in front of the web tier with security groups referencing each other for least-privilege access (E), and a Multi-AZ RDS deployment for automatic failover with access locked to the app tier (F). Options A, B, and D fail because they either keep the old monolith intact or rely on a single, non-resilient RDS instance.

---

## Question 398

**Full Question:** A solutions architect is designing a new API using Amazon API Gateway that will receive requests from users. The volume of requests is highly variable — several hours can pass without receiving a single request. The data processing will take place asynchronously, but should be completed within a few seconds after a request is made. Which compute service should the solutions architect have the API invoke to deliver the requirements at the lowest cost?

**Short Question:** What's the cheapest compute backend for an API with unpredictable traffic, long idle gaps, and short async processing tasks?

**Options:**
- A. An AWS Glue job
- ✅ B. An AWS Lambda function
- C. A containerized service hosted in Amazon Elastic Kubernetes Service (Amazon EKS)
- D. A containerized service hosted in Amazon ECS with Amazon EC2

**Reason:** AWS Lambda bills per invocation and execution time in milliseconds, so it costs nothing during idle hours and fits a task that finishes in a few seconds. AWS Glue has a long minimum billing duration that's wasteful for short jobs, and both EKS and ECS-on-EC2 require continuously running compute clusters that incur cost even when idle.

---

## Question 399

**Full Question:** A company has multiple VPCs across AWS Regions to support and run workloads that are isolated from workloads in other regions. Because of a recent application launch requirement, the company's VPCs must communicate with all other VPCs across all regions. Which solution will meet these requirements with the least amount of administrative effort?

**Short Question:** What's the least-effort way to let every VPC across multiple AWS Regions communicate with every other VPC?

**Options:**
- A. Use VPC peering to manage VPC communication in a single region. Use VPC peering across regions to manage VPC communications
- B. Use AWS Direct Connect gateways across all regions to connect VPCs across regions and manage VPC communications
- ✅ C. Use AWS Transit Gateway to manage VPC communication in a single region and Transit Gateway peering across regions to manage VPC communications
- D. Use AWS PrivateLink across all regions to connect VPCs across regions and manage VPC communications

**Reason:** AWS Transit Gateway acts as a regional hub connecting all VPCs in that region, and Transit Gateway peering links the regional hubs together, avoiding the exponential complexity of full-mesh VPC peering. Direct Connect is meant for on-premises-to-AWS connectivity, and PrivateLink only exposes access to a specific service, not full bidirectional VPC-to-VPC networking.

---

## Question 400

**Full Question:** A social media company is building a feature for its website. The feature will give users the ability to upload photos. The company expects significant increases in demand during large events and must ensure that the website can handle the upload traffic from users. Which solution meets these requirements with the most scalability?

**Short Question:** What's the most scalable way to handle a surge of user photo uploads on a website?

**Options:**
- A. Upload files from the user's browser to the application servers. Transfer the files to an Amazon S3 bucket
- B. Provision an AWS Storage Gateway file gateway. Upload files directly from the user's browser to the file gateway
- ✅ C. Generate Amazon S3 pre-signed URLs in the application. Upload files directly from the user's browser into an S3 bucket
- D. Provision an Amazon Elastic File System (Amazon EFS) file system. Upload files directly from the user's browser to the file system

**Reason:** Pre-signed URLs let the app server quickly authorize each upload while the browser sends the file straight to S3, offloading all the heavy transfer work from the application servers so it scales with S3 rather than being bottlenecked by compute capacity. Routing uploads through app servers (A) creates a bottleneck, Storage Gateway (B) is a hybrid on-premises service not meant for direct browser uploads, and EFS (D) is an internal VPC file system with no browser-facing HTTP upload endpoint.
