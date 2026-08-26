# AWS SAA-C03 Real Exam Questions & Answers — Part 12 (Q276–Q300)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 12](https://www.youtube.com/watch/dy_PlPrxSfQ)

---

## Question 276

**Full Question:** A company is building a mobile app on AWS. The company wants to expand its reach to millions of users. The company needs to build a platform so that authorized users can watch the company's content on their mobile devices. What should a solutions architect recommend to meet these requirements?

**Short Question:** What's the best scalable, secure way to stream private video content to millions of authorized mobile app users?

**Options:**
- A. Publish content to a public Amazon S3 bucket. Use AWS Key Management Service (AWS KMS) keys to stream content.
- B. Set up an IPsec VPN between the mobile app and the AWS environment to stream content.
- ✅ C. Use Amazon CloudFront. Provide signed URLs to stream content.
- D. Set up AWS Client VPN between the mobile app and the AWS environment to stream content.

**Reason:** Amazon CloudFront is a global CDN that delivers content with high performance at scale, and CloudFront signed URLs give temporary, authenticated access so only authorized users can stream the content. The other options either expose content publicly (A), rely on KMS for the wrong purpose, or try to use VPN technologies that don't scale to millions of individual mobile users (B, D).

---

## Question 277

**Full Question:** A company runs a fleet of web servers using an Amazon RDS for PostgreSQL DB instance. After a routine compliance check, the company sets a standard that requires a recovery point objective (RPO) of less than 1 second for all its production databases. Which solution meets these requirements?

**Short Question:** Which RDS setup gives a sub-1-second RPO for a PostgreSQL database?

**Options:**
- ✅ A. Enable a Multi-AZ deployment for the DB instance.
- B. Enable autoscaling for the DB instance in one Availability Zone.
- C. Configure the DB instance in one Availability Zone and create multiple read replicas in a separate Availability Zone.
- D. Configure the DB instance in one Availability Zone and configure AWS Database Migration Service (AWS DMS) change data capture (CDC) tasks.

**Reason:** RDS Multi-AZ uses synchronous replication to a standby in another Availability Zone, so a transaction isn't committed until it's written to both, giving a near-zero RPO. Storage autoscaling (B), read replicas (C), and DMS CDC (D) don't affect or rely on asynchronous replication, which can't guarantee sub-second data loss protection.

---

## Question 278

**Full Question:** A company uses a payment processing system that requires messages for a particular payment ID to be received in the same order that they were sent. Otherwise, the payments might be processed incorrectly. Which actions should a solutions architect take to meet this requirement? (Choose two.)

**Short Question:** Which two AWS services/configurations guarantee strict message ordering per payment ID?

**Options:**
- A. Write the messages to an Amazon DynamoDB table with the payment ID as the partition key.
- ✅ B. Write the messages to an Amazon Kinesis data stream with the payment ID as the partition key.
- C. Write the messages to an Amazon ElastiCache for Memcached cluster with the payment ID as the key.
- D. Write the messages to an Amazon Simple Queue Service (Amazon SQS) queue. Set the message attribute to use the payment ID.
- ✅ E. Write the messages to an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Set the message group ID to use the payment ID.

**Reason:** Kinesis Data Streams preserves order within a shard, and using the payment ID as the partition key routes all related messages to the same shard in order; SQS FIFO queues guarantee strict ordering within a message group when the group ID is set to the payment ID. DynamoDB isn't a queue (A), ElastiCache provides no ordering guarantees (C), and standard SQS queues only offer best-effort ordering (D).

---

## Question 279

**Full Question:** A company is deploying a two-tier web application in a VPC. The web tier is using an Amazon EC2 Auto Scaling group with public subnets that span multiple Availability Zones. The database tier consists of an Amazon RDS for MySQL DB instance in separate private subnets. The web tier requires access to the database to retrieve product information. The web application is not working as intended. The web application reports that it cannot connect to the database. The database is confirmed to be up and running. All configurations for the network ACLs, security groups, and route tables are still in their default states. What should a solutions architect recommend to fix the application?

**Short Question:** With everything left at default settings, what's blocking the web tier EC2 instances from reaching the RDS database?

**Options:**
- A. Add an explicit rule to the private subnet's network ACL to allow traffic from the web tier EC2 instances.
- B. Add a route in the VPC route table to allow traffic between the web tier's EC2 instances and the database tier.
- C. Deploy the web tier EC2 instances and the database tier's RDS instance into two separate VPCs and configure VPC peering.
- ✅ D. Add an inbound rule to the security group of the database tier's RDS instance to allow traffic from the web tier security group.

**Reason:** Security groups deny all inbound traffic by default, so since the web and database tiers use separate security groups, the database's security group is blocking the web servers until an inbound rule allows traffic from the web tier's security group. The default network ACL already allows all traffic (A), the default route table already routes between subnets in the same VPC (B), and splitting into separate VPCs (C) is an unnecessary architectural overhaul.

---

## Question 280

**Full Question:** A consulting company provides professional services to customers worldwide. The company provides solutions and tools for customers to expedite gathering and analyzing data on AWS. The company needs to centrally manage and deploy a common set of solutions and tools for customers to use for self-service purposes. Which solution will meet these requirements?

**Short Question:** Which AWS service lets a company centrally manage and offer a self-service catalog of approved solutions to customers?

**Options:**
- A. Create AWS CloudFormation templates for the customers.
- ✅ B. Create AWS Service Catalog products for the customers.
- C. Create AWS Systems Manager templates for the customers.
- D. Create AWS Config items for the customers.

**Reason:** AWS Service Catalog is purpose-built to let an organization publish a governed catalog of pre-approved products (often backed by CloudFormation templates) that customers can browse and deploy themselves, while the company retains control over versioning and permissions. Plain CloudFormation templates (A) lack the catalog/governance layer, Systems Manager (C) is for operational tasks like automation and patching, and AWS Config (D) is a compliance auditing tool, not a deployment catalog.

---

## Question 281

**Full Question:** A company runs an infrastructure monitoring service. The company is building a new feature that will enable the service to monitor data in customer AWS accounts. The new feature will call AWS APIs in customer accounts to describe Amazon EC2 instances and read Amazon CloudWatch metrics. What should the company do to obtain access to customer accounts in the most secure way?

**Short Question:** What's the most secure way for a third-party monitoring service to get read-only, cross-account access to customers' EC2 and CloudWatch data?

**Options:**
- ✅ A. Ensure that the customers create an IAM role in their account with read-only EC2 and CloudWatch permissions and a trust policy to the company's account
- B. Create a serverless API that implements a token vending machine to provide temporary AWS credentials for a role with read-only EC2 and CloudWatch permissions
- C. Ensure that the customers create an IAM user in their account with read-only EC2 and CloudWatch permissions. Encrypt and store customer access and secret keys in a secrets management system
- D. Ensure that the customers create an Amazon Cognito user in their account to use an IAM role with read-only EC2 and CloudWatch permissions. Encrypt and store the Amazon Cognito user and password in a secrets management system

**Reason:** A customer-owned IAM role with a trust policy to the company's account is the standard cross-account access pattern, letting the company assume the role for temporary, short-lived credentials with no long-term keys shared. IAM users with stored access keys (C) are a security risk, Cognito (D) is meant for end-user identity in apps rather than server-to-server access, and a token vending machine (B) is designed for end-user devices, not trusted server-to-server integrations.

---

## Question 282

**Full Question:** A company is storing 700 terabytes of data on a large network-attached storage (NAS) system in its corporate data center. The company has a hybrid environment with a 10 Gbps AWS Direct Connect connection. After an audit from a regulator, the company has 90 days to move the data to the cloud. The company needs to move the data efficiently and without disruption. The company still needs to be able to access and update the data during the transfer window. Which solution will meet these requirements?

**Short Question:** What's the best way to migrate 700 TB to AWS within 90 days over a 10 Gbps Direct Connect link while keeping the data live and updatable on-premises during the transfer?

**Options:**
- ✅ A. Create an AWS DataSync agent in the corporate data center. Create a data transfer task. Start the transfer to an Amazon S3 bucket
- B. Back up the data to AWS Snowball Edge Storage Optimized devices. Ship the devices to an AWS data center. Mount a target Amazon S3 bucket on the on-premises file system
- C. Use rsync to copy the data directly from local storage to a designated Amazon S3 bucket over the Direct Connect connection
- D. Back up the data on tapes. Ship the tapes to an AWS data center. Mount a target Amazon S3 bucket on the on-premises file system

**Reason:** AWS DataSync is a managed online transfer service that can fully use the 10 Gbps link, performing an initial copy and then syncing ongoing changes so the data stays live and updatable during migration. Snowball (B) and tape shipping (D) are offline methods that block live access, and rsync (C) is single-threaded and can't efficiently saturate a high-bandwidth link for this data volume.

---

## Question 283

**Full Question:** A company uses an Amazon EC2 instance to run a script to poll for and process messages in an Amazon Simple Queue Service (Amazon SQS) queue. The company wants to reduce operational costs while maintaining its ability to process a growing number of messages that are added to the queue. What should a solutions architect recommend to meet these requirements?

**Short Question:** How can a company cut costs and keep up with a growing SQS queue instead of running a script on an always-on EC2 instance?

**Options:**
- A. Increase the size of the EC2 instance to process messages faster
- B. Use Amazon EventBridge to turn off the EC2 instance when the instance is underutilized
- ✅ C. Migrate the script on the EC2 instance to an AWS Lambda function with the appropriate runtime
- D. Use AWS Systems Manager Run Command to run the script on demand

**Reason:** Migrating to an AWS Lambda function triggered by an SQS event source lets AWS handle polling, invokes the function only when messages arrive, and scales automatically, cutting costs versus a 24/7 EC2 instance. Turning off the instance (B) would stop message processing entirely, a bigger instance (A) raises cost without solving scalability, and Systems Manager Run Command (D) isn't built for continuous, event-driven queue processing.

---

## Question 284

**Full Question:** A security audit reveals that Amazon EC2 instances are not being patched regularly. A solutions architect needs to provide a solution that will run regular security scans across a large fleet of EC2 instances. The solution should also patch the EC2 instances on a regular schedule and provide a report of each instance's patch status. Which solution will meet these requirements?

**Short Question:** What combination of AWS services can regularly scan EC2 instances for vulnerabilities, patch them on a schedule, and report patch compliance status?

**Options:**
- A. Set up Amazon Macie to scan the EC2 instances for software vulnerabilities. Set up a cron job on each EC2 instance to patch the instance on a regular schedule
- B. Turn on Amazon GuardDuty in the account. Configure GuardDuty to scan the EC2 instances for software vulnerabilities. Set up AWS Systems Manager Session Manager to patch the EC2 instances on a regular schedule
- C. Set up Amazon Detective to scan the EC2 instances for software vulnerabilities. Set up an Amazon EventBridge scheduled rule to patch the EC2 instances on a regular schedule
- ✅ D. Turn on Amazon Inspector in the account. Configure Amazon Inspector to scan the EC2 instances for software vulnerabilities. Set up AWS Systems Manager Patch Manager to patch the EC2 instances on a regular schedule

**Reason:** Amazon Inspector is the purpose-built vulnerability scanner for EC2 instances, and AWS Systems Manager Patch Manager automates scheduled patching with compliance reporting, together covering both requirements. Macie (A) scans S3 for sensitive data (not EC2 vulnerabilities), GuardDuty (B) is a threat detection service rather than a vulnerability scanner, and Detective (C) is for security investigation, not scanning — none of these pair with a proper patch-reporting tool the way Inspector and Patch Manager do.

---

## Question 285

**Full Question:** A rapidly growing global e-commerce company is hosting its web application on AWS. The web application includes static content and dynamic content. The website stores online transaction processing (OLTP) data in an Amazon RDS database. The website's users are experiencing slow page loads. Which combination of actions should a solutions architect take to resolve this issue? (Choose two.)

**Short Question:** Which two actions best fix slow page loads for a global e-commerce site with static content, dynamic content, and an RDS-backed OLTP database?

**Options:**
- A. Configure an Amazon Redshift cluster
- ✅ B. Set up an Amazon CloudFront distribution
- C. Host the dynamic web content in Amazon S3
- ✅ D. Create a read replica for the RDS DB instance
- E. Configure a multi-AZ deployment for the RDS DB instance

**Reason:** CloudFront caches static content at edge locations closer to global users to cut latency, while an RDS read replica offloads heavy read traffic (like product browsing) from the primary database, both directly reducing page load times. Redshift (A) is for analytics not OLTP, S3 (C) can't serve dynamic content since it lacks a compute layer, and multi-AZ (E) only provides failover standby, not additional read capacity.

---

## Question 286

**Full Question:** A hospital needs to store patient records in an Amazon S3 bucket. The hospital's compliance team must ensure that all protected health information (PHI) is encrypted in transit and at rest. The compliance team must administer the encryption key for data at rest. Which solution will meet these requirements?

**Short Question:** How do you enforce encrypted S3 access plus give the compliance team control of the data-at-rest encryption key?

**Options:**
- A. Create a public SSL/TLS certificate in AWS Certificate Manager (ACM). Associate the certificate with Amazon S3. Configure default encryption for each S3 bucket to use server-side encryption with AWS KMS keys (SSE-KMS). Assign the compliance team to manage the KMS keys.
- B. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Configure default encryption for each S3 bucket to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Assign the compliance team to manage the SSE-S3 keys.
- ✅ C. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Configure default encryption for each S3 bucket to use server-side encryption with AWS KMS keys (SSE-KMS). Assign the compliance team to manage the KMS keys.
- D. Use the aws:SecureTransport condition on S3 bucket policies to allow only encrypted connections over HTTPS (TLS). Use Amazon Macie to protect the sensitive data that is stored in Amazon S3. Assign the compliance team to manage Macie.

**Reason:** You cannot attach an ACM certificate directly to an S3 endpoint (rules out A), and SSE-S3 keys are fully AWS-managed so the compliance team can't administer them (rules out B); Amazon Macie only discovers/classifies sensitive data, it doesn't encrypt it (rules out D). Option C correctly pairs a bucket policy requiring TLS (encryption in transit) with SSE-KMS using a customer-managed key the compliance team can administer (encryption at rest with key control).

---

## Question 287

**Full Question:** A company is developing a marketing communication service that targets mobile app users. The company needs to send confirmation messages with Short Message Service (SMS) to its users. The users must be able to reply to the SMS messages. The company must store the responses for a year for analysis. What should a solutions architect do to meet these requirements?

**Short Question:** Which service supports two-way SMS to mobile users plus long-term storage of replies for analysis?

**Options:**
- A. Create an Amazon Connect contact flow to send the SMS messages. Use AWS Lambda to process the responses.
- ✅ B. Build an Amazon Pinpoint journey. Configure Amazon Pinpoint to send events to an Amazon Kinesis data stream for analysis and archiving.
- C. Use Amazon Simple Queue Service (Amazon SQS) to distribute the SMS messages. Use AWS Lambda to process the responses.
- D. Create an Amazon Simple Notification Service (Amazon SNS) FIFO topic. Subscribe an Amazon Kinesis data stream to the SNS topic for analysis and archiving.

**Reason:** Amazon Pinpoint is a managed marketing communication service built for two-way SMS and can stream engagement events (like replies) to Kinesis for durable archiving/analysis, satisfying both requirements. Amazon Connect is meant for contact-center interactions rather than mass marketing, SQS cannot send SMS at all, and SNS is one-way and cannot receive user replies.

---

## Question 288

**Full Question:** A company needs to move data from an Amazon EC2 instance to an Amazon S3 bucket. The company must ensure that no API calls and no data are routed through public internet routes. Only the EC2 instance can have access to upload data to the S3 bucket. Which solution will meet these requirements?

**Short Question:** How do you give one EC2 instance private, internet-free, exclusive upload access to an S3 bucket?

**Options:**
- ✅ A. Create an interface VPC endpoint for Amazon S3 in the subnet where the EC2 instance is located. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- B. Create a gateway VPC endpoint for Amazon S3 in the availability zone where the EC2 instance is located. Attach appropriate security groups to the endpoint. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- C. Run the nslookup tool from inside the EC2 instance to obtain the private IP address of the S3 bucket service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.
- D. Use the AWS-provided publicly available ip-ranges.json file to obtain the private IP address of the S3 bucket service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance's IAM role for access.

**Reason:** An interface VPC endpoint puts a private-IP network interface for S3 inside the VPC so traffic never leaves the AWS network, and the bucket resource policy locks access down to just the EC2 instance's IAM role, satisfying both requirements. Security groups cannot be attached to a gateway endpoint (rules out B), and S3 has no discoverable private IP via nslookup or the public ip-ranges.json file, so C and D would not actually provide private connectivity.

---

## Question 289

**Full Question:** A company has an application that is backed by an Amazon DynamoDB table. The company's compliance requirements specify that database backups must be taken every month, must be available for 6 months, and must be retained for 7 years. Which solution will meet these requirements?

**Short Question:** What's the simplest managed way to schedule monthly DynamoDB backups with a 6-month-to-cold-storage, 7-year-total-retention lifecycle?

**Options:**
- ✅ A. Create an AWS Backup plan to back up the DynamoDB table on the first day of each month. Specify a lifecycle policy that transitions the backup to cold storage after 6 months. Set the retention period for each backup to 7 years.
- B. Create a DynamoDB on-demand backup of the DynamoDB table on the first day of each month. Transition the backup to Amazon S3 Glacier Flexible Retrieval after 6 months. Create an S3 lifecycle policy to delete backups that are older than 7 years.
- C. Use the AWS SDK to develop a script that creates an on-demand backup of the DynamoDB table. Set up an Amazon EventBridge rule that runs the script on the first day of each month. Create a second script that will run on the second day of each month to transition DynamoDB backups that are older than 6 months to cold storage and to delete backups that are older than 7 years.
- D. Use the AWS CLI to create an on-demand backup of the DynamoDB table. Set up an Amazon EventBridge rule that runs the command on the first day of each month with a cron expression. Specify in the command to transition the backups to cold storage after 6 months and to delete the backups after 7 years.

**Reason:** AWS Backup is a fully managed service that lets one backup plan handle scheduling, cold-storage transition, and retention all in a single lifecycle policy, meeting every requirement with no custom code. Native DynamoDB on-demand backups can't be moved into an S3 storage class like Glacier (rules out B), and neither the AWS SDK/custom scripts nor the AWS CLI backup command support built-in lifecycle transition/retention parameters (rules out C and D).

---

## Question 290

**Full Question:** A company has a static website that is hosted on Amazon CloudFront in front of Amazon S3. The static website uses a database backend. The company notices that the website does not reflect updates that have been made in the website's Git repository. The company checks the continuous integration and continuous delivery (CI/CD) pipeline between the Git repository and Amazon S3. The company verifies that the webhooks are configured properly and that the CI/CD pipeline is sending messages that indicate successful deployments. A solutions architect needs to implement a solution that displays the updates on the website. Which solution will meet these requirements?

**Short Question:** The CI/CD pipeline successfully deploys new files to S3, but users still see the old version through CloudFront — what fixes that?

**Options:**
- A. Add an Application Load Balancer.
- B. Add Amazon ElastiCache for Redis or Memcached to the database layer of the web application.
- ✅ C. Invalidate the CloudFront cache.
- D. Use AWS Certificate Manager (ACM) to validate the website's SSL certificate.

**Reason:** CloudFront was still serving the old cached files from its edge locations even though S3 had the new ones, and a CloudFront invalidation forces it to fetch the latest version from the origin on the next request. A load balancer is for routing traffic to compute back ends, ElastiCache addresses database performance not static file caching, and an SSL certificate issue would cause connection errors rather than stale content — none of them fix the caching problem.

---

## Question 291

**Full Question:** A company manages its own Amazon EC2 instances that run MySQL databases. The company is manually managing replication and scaling as demand increases or decreases. The company needs a new solution that simplifies the process of adding or removing compute capacity to or from its database tier as needed. The solution also must offer improved performance, scaling, and durability with minimal effort from operations. Which solution meets these requirements?

**Short Question:** Which AWS database solution removes the need to manually manage replication and scaling for a MySQL database on EC2?

**Options:**
- ✅ A. Migrate the databases to Amazon Aurora Serverless for Aurora MySQL
- B. Migrate the databases to Amazon Aurora Serverless for Aurora PostgreSQL
- C. Combine the databases into one larger MySQL database. Run the larger database on larger EC2 instances
- D. Create an EC2 Auto Scaling group for the database tier. Migrate the existing databases to the new environment

**Reason:** Aurora Serverless for Aurora MySQL is a fully managed, MySQL-compatible database that automatically scales compute capacity up and down, giving better performance and durability with minimal operational effort. Migrating to PostgreSQL means an unnecessary complex engine change, combining into a bigger instance is just manual vertical scaling, and running a stateful database on an EC2 Auto Scaling group is impractical to manage.

---

## Question 292

**Full Question:** A research company runs experiments that are powered by a simulation application and a visualization application. The simulation application runs on Linux and outputs intermediate data to an NFS share every 5 minutes. The visualization application is a Windows desktop application that displays the simulation output and requires an SMB file system. The company maintains two synchronized file systems. This strategy is causing data duplication and inefficient resource usage. The company needs to migrate the applications to AWS without making code changes to either application. Which solution will meet these requirements?

**Short Question:** What AWS storage lets a Linux app (needing NFS) and a Windows app (needing SMB) share the same data without duplicating it or changing code?

**Options:**
- A. Migrate both applications to AWS Lambda. Create an Amazon S3 bucket to exchange data between the applications
- B. Migrate both applications to Amazon Elastic Container Service (Amazon ECS). Configure Amazon FSx File Gateway for storage
- C. Migrate the simulation application to Linux Amazon EC2 instances. Migrate the visualization application to Windows EC2 instances. Configure Amazon Simple Queue Service (Amazon SQS) to exchange data between the applications
- ✅ D. Migrate the simulation application to Linux Amazon EC2 instances. Migrate the visualization application to Windows EC2 instances. Configure Amazon FSx for NetApp ONTAP for storage

**Reason:** Amazon FSx for NetApp ONTAP is a fully managed multi-protocol file system that can serve the same data volume concurrently over both NFS and SMB, letting each application connect the way it already does with no code changes and no duplicated data. S3 is object storage (not mountable as NFS/SMB), FSx File Gateway is meant for on-premises hybrid access rather than as the primary in-cloud store, and SQS is a message queue, not a shared file system.

---

## Question 293

**Full Question:** A company stores its data objects in Amazon S3 Standard storage. A solutions architect has found that 75% of the data is rarely accessed after 30 days. The company needs all the data to remain immediately accessible with the same high availability and resiliency, but the company wants to minimize storage costs. Which storage solution will meet these requirements?

**Short Question:** What's the cheapest S3 storage class that still keeps data instantly accessible and just as highly available as S3 Standard, for data that goes cold after 30 days?

**Options:**
- A. Move the data objects to S3 Glacier Deep Archive after 30 days
- ✅ B. Move the data objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days
- C. Move the data objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days
- D. Move the data objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) immediately

**Reason:** S3 Standard-IA costs less than S3 Standard while still offering immediate access and the same multi-AZ high availability and resiliency, matching the requirement exactly once data turns infrequently accessed at 30 days. Glacier Deep Archive isn't immediately accessible, and both One Zone-IA options fail because that class only stores data in a single AZ (less resilient), and moving immediately would also be less cost-effective since the data is still frequently accessed in the first 30 days.

---

## Question 294

**Full Question:** An e-commerce company is experiencing an increase in user traffic. The company's store is deployed on Amazon EC2 instances as a two-tier web application consisting of a web tier and a separate database tier. As traffic increases, the company notices that the architecture is causing significant delays in sending timely marketing and order confirmation email to users. The company wants to reduce the time it spends resolving complex email delivery issues and minimize operational overhead. What should a solutions architect do to meet these requirements?

**Short Question:** How should the company offload sending marketing and order-confirmation emails so it stops dealing with delivery problems and server overhead?

**Options:**
- A. Create a separate application tier using EC2 instances dedicated to email processing
- ✅ B. Configure the web instance to send email through Amazon Simple Email Service (Amazon SES)
- C. Configure the web instance to send email through Amazon Simple Notification Service (Amazon SNS)
- D. Create a separate application tier using EC2 instances dedicated to email processing. Place the instances in an Auto Scaling group

**Reason:** Amazon SES is a fully managed, scalable email service built for sending marketing and transactional email, and it handles deliverability concerns like IP reputation, bounces, and complaints, which minimizes operational overhead. Building a dedicated EC2 email tier (with or without Auto Scaling) just recreates the same management burden, and SNS is a notification service not designed as a full-featured email delivery platform.

---

## Question 295

**Full Question:** A company hosts a marketing website in an on-premises data center. The website consists of static documents and runs on a single server. An administrator updates the website content infrequently and uses an SFTP client to upload new documents. The company decides to host its website on AWS and to use Amazon CloudFront. The company's solutions architect creates a CloudFront distribution. The solutions architect must design the most cost-effective and resilient architecture for website hosting to serve as the CloudFront origin. Which solution will meet these requirements?

**Short Question:** What's the cheapest, most resilient CloudFront origin setup for hosting a rarely-updated static marketing website?

**Options:**
- A. Create a virtual server by using Amazon Lightsail. Configure the web server in the Lightsail instance. Upload website content by using an SFTP client
- B. Create an AWS Auto Scaling group for Amazon EC2 instances. Use an Application Load Balancer. Upload website content by using an SFTP client
- ✅ C. Create a private Amazon S3 bucket. Use an S3 bucket policy to allow access from a CloudFront origin access identity (OAI). Upload website content by using the AWS CLI
- D. Create a public Amazon S3 bucket. Configure AWS Transfer Family for SFTP. Configure the S3 bucket for website hosting. Upload website content by using the SFTP client

**Reason:** A private S3 bucket accessed only through a CloudFront origin access identity is the most cost-effective and resilient option for static content, and keeping the bucket private (forcing all traffic through CloudFront) follows AWS security best practices. Lightsail and the EC2/ALB setup both require running a server continuously at higher cost, and making the bucket public plus paying for AWS Transfer Family's hourly SFTP cost is both a security risk and less cost-effective than uploading via the CLI.

---

## Question 296

**Full Question:** A company is looking for a solution that can store video archives in AWS from old news footage. The company needs to minimize costs and will rarely need to restore these files. When the files are needed, they must be available in a maximum of 5 minutes. What is the most cost-effective solution?

**Short Question:** Cheapest way to archive rarely-accessed video files while still retrieving them within 5 minutes when needed?

**Options:**
- ✅ A. Store the video archives in Amazon S3 Glacier and use expedited retrievals
- B. Store the video archives in Amazon S3 Glacier and use standard retrievals
- C. Store the video archives in Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
- D. Store the video archives in Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)

**Reason:** S3 Glacier (Flexible Retrieval) offers the lowest storage cost for rarely-accessed archives, and its expedited retrieval option returns data in 1-5 minutes, meeting the deadline. Standard retrieval from Glacier takes 3-5 hours (too slow), while S3 Standard-IA and One Zone-IA give fast retrieval but at a much higher storage cost, and One Zone-IA also lacks multi-AZ resilience.

---

## Question 297

**Full Question:** A company is building a three-tier application on AWS. The presentation tier will serve a static website. The logic tier is a containerized application. This application will store data in a relational database. The company wants to simplify deployment and reduce operational costs. Which solution will meet these requirements?

**Short Question:** Which combination of services gives the simplest, lowest-overhead three-tier setup (static site + containers + relational DB)?

**Options:**
- ✅ A. Use Amazon S3 to host static content. Use Amazon ECS with AWS Fargate for compute power. Use a managed Amazon RDS cluster for the database.
- B. Use Amazon CloudFront to host static content. Use Amazon ECS with Amazon EC2 for compute power. Use a managed Amazon RDS cluster for the database.
- C. Use Amazon S3 to host static content. Use Amazon EKS with AWS Fargate for compute power. Use a managed Amazon RDS cluster for the database.
- D. Use Amazon EC2 reserved instances to host static content. Use Amazon EKS with Amazon EC2 for compute power. Use a managed Amazon RDS cluster for the database.

**Reason:** S3 is the simplest, cheapest way to host a static site, and ECS with Fargate removes the need to manage servers, while RDS handles the relational database with minimal admin work. CloudFront is a CDN (not a hosting origin), EKS adds more operational complexity than ECS for this use case, and hosting static content on EC2 is inefficient and high-overhead.

---

## Question 298

**Full Question:** A company hosts a multiplayer gaming application on AWS. The company wants the application to read data with submillisecond latency and run ad hoc queries on historical data. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Best low-overhead way to get submillisecond reads for live game data plus ad hoc queries on historical data?

**Options:**
- A. Use Amazon RDS for data that is frequently accessed. Run a periodic custom script to export the data to an Amazon S3 bucket.
- B. Store the data directly in an Amazon S3 bucket. Implement an S3 lifecycle policy to move older data to S3 Glacier Deep Archive for long-term storage. Run one-time queries on the data in Amazon S3 by using Amazon Athena.
- ✅ C. Use Amazon DynamoDB with DynamoDB Accelerator (DAX) for data that is frequently accessed. Export the data to an Amazon S3 bucket by using DynamoDB table export. Run one-time queries on the data in Amazon S3 by using Amazon Athena.
- D. Use Amazon DynamoDB for data that is frequently accessed. Turn on streaming to Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to read the data from Kinesis Data Streams. Store the records in an Amazon S3 bucket.

**Reason:** DynamoDB with DAX provides the required in-memory, submillisecond read latency for the hot tier, and DynamoDB's built-in table export to S3 is a low-overhead managed way to move historical data for querying with Athena. RDS can't hit submillisecond latency, plain S3 storage isn't suited as a primary low-latency database, and building a Kinesis streaming pipeline (option D) is unnecessarily complex and still lacks DAX for the hot-data requirement.

---

## Question 299

**Full Question:** A company has a data ingestion workflow that consists of the following: an Amazon Simple Notification Service (Amazon SNS) topic for notifications about new data deliveries, and an AWS Lambda function to process the data and record metadata. The company observes that the ingestion workflow fails occasionally because of network connectivity issues. When such a failure occurs, the Lambda function does not ingest the corresponding data unless the company manually reruns the job. Which combination of actions should a solutions architect take to ensure that the Lambda function ingests all data in the future? (Choose two.)

**Short Question:** How to make an SNS-to-Lambda ingestion pipeline resilient so failed invocations don't silently drop data?

**Options:**
- A. Deploy the Lambda function in multiple Availability Zones
- ✅ B. Create an Amazon Simple Queue Service (Amazon SQS) queue and subscribe it to the SNS topic
- C. Increase the CPU and memory that are allocated to the Lambda function
- D. Increase provisioned concurrency for the Lambda function
- ✅ E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue

**Reason:** Adding an SQS queue between SNS and Lambda (subscribing the queue to the topic, then having Lambda poll the queue) creates a durable buffer, so a message stays queued and is retried if an invocation fails instead of being lost. Lambda is already multi-AZ by default, and neither more compute resources nor provisioned concurrency address the actual problem, which is lost messages from failed invocations, not slow starts or insufficient resources.

---

## Question 300

**Full Question:** A company is storing backup files by using Amazon S3 Standard storage. The files are accessed frequently for 1 month. However, the files are not accessed after 1 month. The company must keep the files indefinitely. Which storage solution will meet these requirements most cost-effectively?

**Short Question:** Cheapest S3 setup for files accessed heavily for 1 month, then never again but kept forever?

**Options:**
- A. Configure S3 Intelligent-Tiering to automatically migrate objects
- ✅ B. Create an S3 lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month
- C. Create an S3 lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month
- D. Create an S3 lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month

**Reason:** Since the access pattern is known and predictable (heavy use for a month, then none), a straightforward lifecycle rule moving data to S3 Glacier Deep Archive — the cheapest storage class for indefinite archival — is more cost-effective than paying Intelligent-Tiering's monitoring fee. S3 Standard-IA and One Zone-IA are both pricier than Deep Archive for long-term storage, and One Zone-IA also isn't resilient enough for backups that must be kept indefinitely.
