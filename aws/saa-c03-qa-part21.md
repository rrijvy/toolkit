# AWS SAA-C03 Real Exam Questions & Answers — Part 21 (Q481–Q500)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 21](https://www.youtube.com/watch/QqTdSvP0ldQ)

---

## Question 481

**Full Question:** A serverless application uses Amazon API Gateway, AWS Lambda, and Amazon DynamoDB. The Lambda function needs permissions to read and write to the DynamoDB table. Which solution will give the Lambda function access to the DynamoDB table most securely?

**Short Question:** What's the most secure way to give a Lambda function read/write access to a DynamoDB table?

**Options:**
- A. Create an IAM user with programmatic access for the Lambda function. Attach a policy to the user that allows read and write access to the DynamoDB table. Store the access_key_id and secret_access_key as Lambda environment variables. Ensure other AWS users do not have read/write access to the Lambda function configuration.
- ✅ B. Create an IAM role that includes Lambda as a trusted service. Attach a policy to the role that allows read and write access to the DynamoDB table. Update the configuration of the Lambda function to use the new role as the execution role.
- C. Create an IAM user with programmatic access for the Lambda function. Attach a policy to the user that allows read and write access to the DynamoDB table. Store the access_key_id and secret_access_key parameters in AWS Systems Manager Parameter Store as SecureString parameters. Update the Lambda function code to retrieve the SecureString parameters before connecting to the DynamoDB table.
- D. Create an IAM role that includes DynamoDB as a trusted service. Attach a policy to the role that allows read and write access from the Lambda function. Update the code of the Lambda function to attach to the new role as an execution role.

**Reason:** An IAM execution role trusted by the Lambda service supplies automatically-rotated temporary credentials, which is the AWS best practice; options A and C rely on long-term IAM user access keys (a security anti-pattern), and D configures the trust relationship with the wrong service (DynamoDB instead of Lambda).

---

## Question 482

**Full Question:** A company wants to share accounting data with an external auditor. The data is stored in an Amazon RDS DB instance that resides in a private subnet. The auditor has its own AWS account and requires its own copy of the database. What is the most secure way for the company to share the database with the auditor?

**Short Question:** What's the most secure way to give an external auditor, in their own AWS account, their own copy of an RDS database?

**Options:**
- A. Create a read replica of the database. Configure IAM database authentication to grant the auditor access.
- B. Export the database contents to text files. Store the files in an Amazon S3 bucket. Create a new IAM user for the auditor. Grant the user access to the S3 bucket.
- C. Copy a snapshot of the database to an Amazon S3 bucket. Create an IAM user. Share the user's keys with the auditor to grant access to the object in the S3 bucket.
- ✅ D. Create an encrypted snapshot of the database. Share the snapshot with the auditor. Allow access to the AWS Key Management Service (AWS KMS) encryption key.

**Reason:** Sharing an encrypted RDS snapshot directly with the auditor's account and granting them permission on the KMS key lets them restore their own independent copy without any long-term IAM credentials being created or shared; the other options either give ongoing access instead of a separate copy, rely on insecure shared IAM user keys, or attempt an unsupported RDS-snapshot-to-S3 copy.

---

## Question 483

**Full Question:** A company moved its on-premises PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. The company successfully launched a new product. The workload on the database has increased. The company wants to accommodate the larger workload without adding infrastructure. Which solution will meet these requirements most cost-effectively?

**Short Question:** How do you cost-effectively handle a sustained increase in RDS PostgreSQL workload without adding more infrastructure?

**Options:**
- ✅ A. Buy reserved DB instances for the total workload. Make the Amazon RDS for PostgreSQL DB instance larger.
- B. Make the Amazon RDS for PostgreSQL DB instance a Multi-AZ DB instance.
- C. Buy reserved DB instances for the total workload. Add another Amazon RDS for PostgreSQL DB instance.
- D. Make the Amazon RDS for PostgreSQL DB instance an on-demand DB instance.

**Reason:** Scaling the existing instance up (vertical scaling) handles the larger workload without adding infrastructure, and since the higher load is now the steady-state baseline, buying reserved instances is the cheapest pricing model; Multi-AZ only adds availability (not capacity), adding a second instance requires unwanted rearchitecture, and on-demand pricing is the most expensive option for sustained use.

---

## Question 484

**Full Question:** A company hosts its application in the AWS Cloud. The application runs on Amazon EC2 instances behind an Elastic Load Balancer in an Auto Scaling group, and with an Amazon DynamoDB table. The company wants to ensure the application can be made available in another AWS Region with minimal downtime. What should a solutions architect do to meet these requirements with the least amount of downtime?

**Short Question:** How do you set up cross-region disaster recovery for this EC2/DynamoDB app with the least downtime?

**Options:**
- ✅ A. Create an Auto Scaling group and a load balancer in the disaster recovery Region. Configure the DynamoDB table as a global table. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- B. Create an AWS CloudFormation template to create EC2 instances, load balancers, and DynamoDB tables to be launched when needed. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- C. Create an AWS CloudFormation template to create EC2 instances and a load balancer to be launched when needed. Configure the DynamoDB table as a global table. Configure DNS failover to point to the new disaster recovery Region's load balancer.
- D. Create an Auto Scaling group and load balancer in the disaster recovery Region. Configure the DynamoDB table as a global table. Create an Amazon CloudWatch alarm to trigger an AWS Lambda function that updates Amazon Route 53 to point to the disaster recovery load balancer.

**Reason:** A warm standby with the Auto Scaling group and load balancer already running in the DR region, combined with DynamoDB global tables for low-latency replication and native Route 53 DNS failover, gives the fastest recovery; the CloudFormation options launch infrastructure only after a disaster (pilot light/backup-restore, too slow), and the CloudWatch/Lambda option adds unnecessary custom complexity compared to built-in Route 53 health-check failover.

---

## Question 485

**Full Question:** A company runs a highly available image processing application on Amazon EC2 instances in a single VPC. The EC2 instances run inside several subnets across multiple Availability Zones. The EC2 instances do not communicate with each other. However, the EC2 instances download images from Amazon S3 and upload images to Amazon S3 through a single NAT gateway. The company is concerned about data transfer charges. What is the most cost-effective way for the company to avoid regional data transfer charges?

**Short Question:** What's the cheapest way to stop paying data transfer/NAT charges for EC2-to-S3 traffic in the same region?

**Options:**
- A. Launch the NAT gateway in each Availability Zone.
- B. Replace the NAT gateway with a NAT instance.
- ✅ C. Deploy a gateway VPC endpoint for Amazon S3.
- D. Provision an EC2 dedicated host to run the EC2 instances.

**Reason:** A gateway VPC endpoint routes S3 traffic privately within the AWS network, eliminating both the NAT gateway's per-GB processing fee and cross-AZ data transfer charges at no additional cost; a NAT gateway per AZ still incurs processing fees, a NAT instance still incurs data transfer costs and adds operational overhead, and a dedicated host only affects compute tenancy, not networking costs.

---

## Question 486

**Full Question:** A company uses high block storage capacity to run its workloads on premises. The company's daily peak input and output transactions per second are not more than 15,000 IOPS. The company wants to migrate the workloads to Amazon EC2 and to provision disk performance independent of storage capacity. Which Amazon Elastic Block Store (Amazon EBS) volume type will meet these requirements most cost-effectively?

**Short Question:** Which EBS volume type gives up to 15,000 IOPS provisioned independently of volume size, most cheaply?

**Options:**
- A. GP2 volume type
- B. IO2 volume type
- ✅ C. GP3 volume type
- D. IO1 volume type

**Reason:** GP3 lets you provision IOPS (up to 16,000) and throughput independently of storage size and can meet the 15,000 IOPS requirement at a lower cost than the provisioned-IOPS types. GP2 ties IOPS to volume size (3 IOPS/GiB, requiring a huge and wasteful volume), while IO1/IO2 can meet the requirement but are more expensive than necessary.

---

## Question 487

**Full Question:** A company operates an e-commerce website on Amazon EC2 instances behind an Application Load Balancer (ALB) in an Auto Scaling group. The site is experiencing performance issues related to a high request rate from illegitimate external systems with changing IP addresses. The security team is worried about potential DoS attacks against the website. The company must block the illegitimate incoming requests in a way that has a minimal impact on legitimate users. What should a solutions architect recommend?

**Short Question:** How do you block a high-volume, changing-IP DoS-style attack on an ALB-fronted site without hurting real users?

**Options:**
- A. Deploy Amazon Inspector and associate it with the ALB
- ✅ B. Deploy AWS WAF, associate it with the ALB, and configure a rate-based rule
- C. Deploy rules to the network ACLs associated with the ALB to block the incoming traffic
- D. Deploy Amazon GuardDuty and enable rate limiting protection when configuring GuardDuty

**Reason:** AWS WAF attaches directly to an ALB and its rate-based rules automatically block any individual IP that exceeds a request-rate threshold, handling constantly changing attacker IPs without affecting legitimate traffic. Inspector only scans for vulnerabilities, network ACLs can't scale to block a large, shifting set of IPs, and GuardDuty only detects threats — it doesn't block traffic and has no rate-limiting feature (that's a WAF capability).

---

## Question 488

**Full Question:** A manufacturing company has machine sensors that upload CSV files to an Amazon S3 bucket. These CSV files must be converted into images and must be made available as soon as possible for the automatic generation of graphical reports. The images become irrelevant after one month, but the CSV files must be kept to train machine learning (ML) models twice a year. The ML trainings and audits are planned weeks in advance. Which combination of steps will meet these requirements most cost-effectively? (Choose two.)

**Short Question:** What's the cheapest way to instantly process uploaded CSVs into short-lived images while cheaply archiving the CSVs for twice-yearly, planned-ahead ML training?

**Options:**
- A. Launch an Amazon EC2 Spot Instance that downloads the CSV files every hour, generates the image files, and uploads the images to the S3 bucket
- ✅ B. Design an AWS Lambda function that converts the CSV files into images and stores the images in the S3 bucket. Invoke the Lambda function when a CSV file is uploaded
- ✅ C. Create S3 lifecycle rules for CSV files and image files in the S3 bucket. Transition the CSV files from S3 Standard to S3 Glacier 1 day after they are uploaded. Expire the image files after 30 days
- D. Create S3 lifecycle rules for CSV files and image files in the S3 bucket. Transition the CSV files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 1 day after they are uploaded. Expire the image files after 30 days
- E. Create S3 lifecycle rules for CSV files and image files in the S3 bucket. Transition the CSV files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 1 day after they are uploaded. Keep the image files in Reduced Redundancy Storage (RRS)

**Reason:** An S3-event-triggered Lambda function processes each CSV into an image immediately (event-driven, no polling delay), and since CSVs are only needed a couple times a year with weeks of advance notice, transitioning them to S3 Glacier is the cheapest archival option while expiring images after 30 days removes them once irrelevant. Polling with EC2 (A) isn't "as soon as possible," One Zone-IA (D) isn't as cheap as Glacier for rarely-accessed archival data and lacks AZ resilience, and Standard-IA plus legacy RRS (E) is neither the cheapest nor does it delete the images as required.

---

## Question 489

**Full Question:** A solutions architect wants to use the following JSON text as an identity-based policy to grant specific permissions. Which IAM principals can the solutions architect attach this policy to? (Choose two.)

**Short Question:** Which two AWS IAM entity types can have a standard identity-based policy attached directly to them?

**Options:**
- ✅ A. Role
- ✅ B. Group
- C. Organization
- D. Amazon Elastic Container Service (Amazon ECS) resource
- E. Amazon EC2 resource

**Reason:** Identity-based policies can be attached to IAM identities — roles and groups (and users) — to define what actions they can perform. AWS Organizations uses Service Control Policies (SCPs) instead, and ECS/EC2 resources are not IAM principals — an EC2 instance gets permissions via an attached IAM role, not a directly attached policy.

---

## Question 490

**Full Question:** A company needs to migrate a MySQL database from its on-premises data center to AWS within 2 weeks. The database is 20 terabytes in size. The company wants to complete the migration with minimal downtime. Which solution will migrate the database most cost-effectively?

**Short Question:** What's the cheapest way to migrate a 20 TB on-premises MySQL database to AWS within 2 weeks with minimal downtime?

**Options:**
- ✅ A. Order an AWS Snowball Edge Storage Optimized device. Use AWS Database Migration Service (AWS DMS) with the AWS Schema Conversion Tool (AWS SCT) to migrate the database with replication of ongoing changes. Send the Snowball Edge device to AWS to finish the migration and continue the ongoing replication
- B. Order an AWS Snowmobile vehicle. Use AWS Database Migration Service (AWS DMS) with the AWS Schema Conversion Tool (AWS SCT) to migrate the database with ongoing changes. Send the Snowmobile vehicle back to AWS to finish the migration and continue the ongoing replication
- C. Order an AWS Snowball Edge Compute Optimized with GPU device. Use AWS Database Migration Service (AWS DMS) with the AWS Schema Conversion Tool (AWS SCT) to migrate the database with ongoing changes. Send the Snowball device to AWS to finish the migration and continue the ongoing replication
- D. Order a 1 Gbps dedicated AWS Direct Connect connection to establish a connection with the data center. Use AWS Database Migration Service (AWS DMS) with the AWS Schema Conversion Tool (AWS SCT) to migrate the database with replication of ongoing changes

**Reason:** A Snowball Edge Storage Optimized device handles the bulk 20 TB offline transfer quickly while AWS DMS captures and replicates ongoing changes during transit, enabling a low-downtime cutover once the data lands in AWS. Snowmobile is built for exabyte-scale transfers (overkill for 20 TB), the Compute Optimized with GPU device is meant for edge compute rather than simple bulk storage transfer, and provisioning a new Direct Connect connection typically takes weeks to months — missing the 2-week deadline and costing more for a one-time migration.

---

## Question 491

**Full Question:** An e-commerce company wants to use machine learning (ML) algorithms to build and train models. The company will use the models to visualize complex scenarios and to detect trends in customer data. The architecture team wants to integrate its ML models with a reporting platform to analyze the augmented data and use the data directly in its business intelligence dashboards. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Least-overhead way to build/train ML models and feed the results into BI dashboards?

**Options:**
- A. Use AWS Glue to create an ML transform to build and train models. Use Amazon OpenSearch Service to visualize the data.
- ✅ B. Use Amazon SageMaker to build and train models. Use Amazon QuickSight to visualize the data.
- C. Use a pre-built ML Amazon Machine Image (AMI) from the AWS Marketplace to build and train models. Use Amazon OpenSearch Service to visualize the data.
- D. Use Amazon QuickSight to build and train models by using calculated fields. Use Amazon QuickSight to visualize the data.

**Reason:** Amazon SageMaker is the fully managed service for the entire ML lifecycle, and Amazon QuickSight is a serverless BI service with native SageMaker integration for dashboards, so together they minimize operational overhead; the other options misuse Glue ML transforms, self-managed AMIs, or QuickSight's limited calculated fields for real model building.

---

## Question 492

**Full Question:** A company needs to store data from its healthcare application. The application's data frequently changes. A new regulation requires audit access at all levels of the stored data. The company hosts the application on an on-premises infrastructure that is running out of storage capacity. A solutions architect must securely migrate the existing data to AWS while satisfying the new regulation. Which solution will meet these requirements?

**Short Question:** Best way to migrate frequently-changing on-prem healthcare data to S3 while logging object-level access for compliance?

**Options:**
- ✅ A. Use AWS DataSync to move the existing data to Amazon S3. Use AWS CloudTrail to log data events.
- B. Use AWS Snowcone to move the existing data to Amazon S3. Use AWS CloudTrail to log management events.
- C. Use Amazon S3 Transfer Acceleration to move the existing data to Amazon S3. Use AWS CloudTrail to log data events.
- D. Use AWS Storage Gateway to move the existing data to Amazon S3. Use AWS CloudTrail to log management events.

**Reason:** AWS DataSync is a managed online transfer service well suited for continuously changing data, and enabling CloudTrail data events (not just management events) logs object-level access as the regulation requires; Snowcone is an offline method unsuited to changing data, Transfer Acceleration is just a speed feature, and Storage Gateway is a hybrid storage tool, not a migration service.

---

## Question 493

**Full Question:** A company uses AWS Organizations to run workloads within multiple AWS accounts. The company needs to design a solution that will prevent the modification of cost usage tags. Which solution will meet these requirements?

**Short Question:** How to block cost allocation tag changes across all accounts in an organization, except for authorized users?

**Options:**
- A. Create a custom AWS Config rule to prevent tag modification except by authorized principals.
- B. Create a custom trail in AWS CloudTrail to prevent tag modification.
- ✅ C. Create a service control policy (SCP) to prevent tag modification except by authorized principals.
- D. Create custom Amazon CloudWatch logs to prevent tag modification.

**Reason:** Service control policies are the preventive guardrail feature in AWS Organizations and can explicitly deny tagging actions org-wide with exceptions for authorized principals, whereas AWS Config and CloudTrail are detective/logging tools that can't stop an action, and CloudWatch is for monitoring, not enforcement.

---

## Question 494

**Full Question:** A company runs a highly available SFTP service. The SFTP service uses two Amazon EC2 instances that run with Elastic IP addresses to accept traffic from trusted IP sources on the internet. The SFTP service is backed by shared storage that is attached to the instances. User accounts are created and managed as Linux users on the SFTP servers. The company wants a serverless option that provides high IOPS performance and highly configurable security. The company also wants to maintain control over user permissions. Which solution will meet these requirements?

**Short Question:** Best serverless replacement for a self-managed high-IOPS SFTP service with IP filtering and user permission control?

**Options:**
- A. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume. Create an AWS Transfer Family SFTP service with a public endpoint that allows only trusted IP addresses. Attach the EBS volume to the SFTP service endpoint. Grant users access to the SFTP service.
- ✅ B. Create an encrypted Amazon Elastic File System (Amazon EFS) volume. Create an AWS Transfer Family SFTP service with Elastic IP addresses and a VPC endpoint that has internet-facing access. Attach a security group to the endpoint that allows only trusted IP addresses. Attach the EFS volume to the SFTP service endpoint. Grant users access to the SFTP service.
- C. Create an Amazon S3 bucket with default encryption enabled. Create an AWS Transfer Family SFTP service with a public endpoint that allows only trusted IP addresses. Attach the S3 bucket to the SFTP service endpoint. Grant users access to the SFTP service.
- D. Create an Amazon S3 bucket with default encryption enabled. Create an AWS Transfer Family SFTP service with a VPC endpoint that has internal access in a private subnet. Attach a security group that allows only trusted IP addresses. Attach the S3 bucket to the SFTP service endpoint. Grant users access to the SFTP service.

**Reason:** AWS Transfer Family is serverless, and pairing it with Amazon EFS delivers the needed high IOPS and POSIX permission control, while an internet-facing VPC endpoint with a security group gives more configurable, robust IP-based security than a public endpoint; EBS isn't a supported Transfer Family backend, and a private/internal VPC endpoint (option D) can't accept traffic from the internet as required.

---

## Question 495

**Full Question:** A company runs a custom application on Amazon EC2 On-Demand Instances. The application has front-end nodes that need to run 24 hours a day, 7 days a week, and backend nodes that need to run only for a short time based on workload. The number of backend nodes varies during the day. The company needs to scale out and scale in more instances based on workload. Which solution will meet these requirements most cost-effectively?

**Short Question:** Most cost-effective EC2 purchasing mix for an always-on front end plus a variable, short-lived backend?

**Options:**
- A. Use Reserved Instances for the front-end nodes. Use AWS Fargate for the backend nodes.
- ✅ B. Use Reserved Instances for the front-end nodes. Use Spot Instances for the backend nodes.
- C. Use Spot Instances for the front-end nodes. Use Reserved Instances for the backend nodes.
- D. Use Spot Instances for the front-end nodes. Use AWS Fargate for the backend nodes.

**Reason:** Reserved Instances give the best discount for the predictable 24/7 front end, while Spot Instances offer deep discounts well suited to the short-lived, interruption-tolerant backend nodes; Fargate is for containers rather than the EC2 instances described, and using Spot for the always-on front end (or Reserved for the short-lived backend) mismatches pricing model to workload pattern.

---

## Question 496

**Full Question:** A company has an on-premises application that generates a large amount of time-sensitive data that is backed up to Amazon S3. The application has grown, and there are user complaints about internet bandwidth limitations. A solutions architect needs to design a long-term solution that allows for both timely backups to Amazon S3 and minimal impact on internet connectivity for internal users. Which solution meets these requirements?

**Short Question:** Best long-term way to send large ongoing S3 backups from on-premises without eating the office's shared internet bandwidth?

**Options:**
- A. Establish AWS VPN connections and proxy all traffic through a VPC gateway endpoint
- ✅ B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection
- C. Order daily AWS Snowball devices, load the data onto the Snowball devices, and return the devices to AWS each day
- D. Submit a support ticket through the AWS Management Console to request the removal of S3 service limits from the account

**Reason:** AWS Direct Connect provides a dedicated private, high-bandwidth link between the data center and AWS, so backup traffic no longer competes with users for the shared internet connection; VPN still rides over the public internet, Snowball is meant for offline/one-time transfers not daily backups, and S3 has no relevant service limit causing this bottleneck.

---

## Question 497

**Full Question:** A company runs multiple Windows workloads on AWS. The company's employees use Windows file shares that are hosted on two Amazon EC2 instances. The file shares synchronize data between themselves and maintain duplicate copies. The company wants a highly available and durable storage solution that preserves how users currently access the files. What should a solutions architect do to meet these requirements?

**Short Question:** Best fully managed replacement for a self-hosted, syncing Windows file share on EC2 that keeps the same SMB access for users?

**Options:**
- A. Migrate all the data to Amazon S3, and set up IAM authentication for users to access files
- B. Set up an Amazon S3 File Gateway, and mount the S3 File Gateway on the existing EC2 instances
- ✅ C. Extend the file share environment to Amazon FSx for Windows File Server with a Multi-AZ configuration, and migrate all the data to FSx for Windows File Server
- D. Extend the file share environment to Amazon Elastic File System (Amazon EFS) with a Multi-AZ configuration, and migrate all the data to Amazon EFS

**Reason:** Amazon FSx for Windows File Server is a fully managed native Windows (SMB) file system, and a Multi-AZ deployment makes it highly available and durable while preserving the exact same file-share access users already use; S3 is object storage (not a mountable file share), the S3 File Gateway is meant for on-premises-to-cloud hybrid access, and EFS uses NFS, which is native to Linux, not Windows.

---

## Question 498

**Full Question:** A company wants to use the AWS Cloud to improve its on-premises disaster recovery (DR) configuration. The company's core production business application uses Microsoft SQL Server Standard, which runs on a virtual machine (VM). The application has a recovery point objective (RPO) of 30 seconds or fewer and a recovery time objective (RTO) of 60 minutes. The DR solution needs to minimize costs wherever possible. Which solution will meet these requirements?

**Short Question:** Cheapest DR setup that hits a 30-second RPO and 60-minute RTO for an on-premises SQL Server Standard database?

**Options:**
- A. Configure a multi-site active/active setup between the on-premises server and AWS by using Microsoft SQL Server Enterprise with Always On availability groups
- ✅ B. Configure a warm standby Amazon RDS for SQL Server database on AWS. Configure AWS Database Migration Service (AWS DMS) to use change data capture (CDC)
- C. Use AWS Elastic Disaster Recovery configured to replicate disk changes to AWS as a pilot light
- D. Use third-party backup software to capture backups every night. Store a secondary set of backups in Amazon S3

**Reason:** A warm standby Amazon RDS instance kept in sync by AWS DMS with change data capture gives continuous, low-latency replication that easily meets a 30-second RPO and a 60-minute RTO at low cost; option A forces an expensive SQL Server Enterprise upgrade, option C does whole-server block replication rather than database-aware replication, and nightly backups (option D) would leave an RPO of up to 24 hours.

---

## Question 499

**Full Question:** A global video streaming company uses Amazon CloudFront as a content distribution network (CDN). The company wants to roll out content in a phased manner across multiple countries. The company needs to ensure that viewers who are outside the countries to which the company rolls out content are not able to view the content. Which solution will meet these requirements?

**Short Question:** How do you block CloudFront viewers outside specific countries during a phased content rollout?

**Options:**
- ✅ A. Add geographic restrictions to the content in CloudFront by using an allow list. Set up a custom error message
- B. Set up a new URL for restricted content. Authorize access by using a signed URL and cookies. Set up a custom error message
- C. Encrypt the data for the content that the company distributes. Set up a custom error message
- D. Create a new URL for restricted content. Set up a time-restricted access policy for signed URLs

**Reason:** CloudFront's built-in geographic restriction (geoblocking) feature with a country allow list is purpose-built to permit access only from approved countries; signed URLs/cookies and encryption control who can access content and for how long, not where the viewer is located.

---

## Question 500

**Full Question:** A company wants to provide data scientists with near real-time, read-only access to the company's production Amazon RDS for PostgreSQL database. The database is currently configured as a Single-AZ database. The data scientists use complex queries that will not affect the production database. The company needs a solution that is highly available. Which solution will meet these requirements most cost-effectively?

**Short Question:** Cheapest highly-available way to give data scientists a separate read path into a production RDS for PostgreSQL database without slowing it down?

**Options:**
- A. Scale the existing production database in a maintenance window to provide enough power for the data scientists
- B. Change the setup from a Single-AZ to a Multi-AZ instance deployment with a larger secondary standby instance. Provide the data scientists access to the secondary instance
- C. Change the setup from a Single-AZ to a Multi-AZ instance deployment. Provide two additional read replicas for the data scientists
- ✅ D. Change the setup from a Single-AZ to a Multi-AZ cluster deployment with two readable standby instances. Provide read endpoints to the data scientists

**Reason:** An RDS Multi-AZ cluster deployment provides a primary writer plus two readable standby instances, so the standbys serve double duty as both failover targets and a read endpoint for the data scientists — delivering high availability and read isolation in one cost-effective package; scaling the production instance still burdens it directly, a standard Multi-AZ standby cannot serve reads, and Multi-AZ instance plus two separate read replicas requires paying for four instances instead of two extra ones.
