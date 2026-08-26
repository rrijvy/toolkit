# AWS SAA-C03 Real Exam Questions & Answers — Part 9 (Q201–Q225)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 9](https://www.youtube.com/watch/u3sI-CHb75k)

---

## Question 201

**Full Question:** A company needs to export its database once a day to Amazon S3 for other teams to access. The exported object size varies between 2 GB and 5 GB. The S3 access pattern for the data is variable and changes rapidly. The data must be immediately available and must remain accessible for up to 3 months. The company needs the most cost-effective solution that will not increase retrieval time. Which S3 storage class should the company use to meet these requirements?

**Short Question:** Which S3 storage class is most cost-effective for daily exports with unpredictable, rapidly changing access needing instant availability for 3 months?

**Options:**
- ✅ A. S3 Intelligent-Tiering
- B. S3 Glacier Instant Retrieval
- C. S3 Standard
- D. S3 Standard-Infrequent Access (S3 Standard-IA)

**Reason:** S3 Intelligent-Tiering automatically moves objects between frequent and infrequent access tiers based on actual usage, with no retrieval fees and no performance impact, making it ideal for unpredictable access patterns — whereas S3 Standard is too costly if data goes cold, and Glacier Instant Retrieval or Standard-IA risk high retrieval fees if data is accessed more often than expected.

---

## Question 202

**Full Question:** A company's security team requests that network traffic be captured in VPC flow logs. The logs will be frequently accessed for 90 days and then accessed intermittently. What should a solutions architect do to meet these requirements when configuring the logs?

**Short Question:** How should VPC flow logs be stored to support frequent access for 90 days, then infrequent access after that, cost-effectively?

**Options:**
- A. Use Amazon CloudWatch as the target. Set the CloudWatch log group with an expiration of 90 days.
- B. Use Amazon Kinesis as the target. Configure the Kinesis stream to always retain the logs for 90 days.
- C. Use AWS CloudTrail as the target. Configure CloudTrail to save to an Amazon S3 bucket and enable S3 Intelligent-Tiering.
- ✅ D. Use Amazon S3 as the target. Enable an S3 lifecycle policy to transition the logs from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 90 days.

**Reason:** VPC flow logs can be sent to Amazon S3, and a lifecycle policy can automatically move them from S3 Standard (frequent access) to S3 Standard-IA (infrequent but still immediately available) after 90 days; CloudWatch with an expiration would delete the logs, Kinesis isn't a valid VPC flow logs destination, and CloudTrail only logs API calls, not network traffic.

---

## Question 203

**Full Question:** A company needs to store contract documents. A contract lasts for 5 years. During the 5-year period, the company must ensure that the documents cannot be overwritten or deleted. The company needs to encrypt the documents at rest and rotate the encryption keys automatically every year. Which combination of steps should a solutions architect take to meet these requirements with the least operational overhead? (Choose two.)

**Short Question:** Which two steps provide immutable, encrypted storage for 5-year contracts with automatic yearly key rotation and minimal admin effort?

**Options:**
- A. Store the documents in Amazon S3. Use S3 Object Lock in governance mode.
- ✅ B. Store the documents in Amazon S3. Use S3 Object Lock in compliance mode.
- C. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure key rotation.
- ✅ D. Use server-side encryption with AWS Key Management Service (AWS KMS) customer managed keys. Configure key rotation.
- E. Use server-side encryption with AWS Key Management Service (AWS KMS) customer provided (imported) keys. Configure key rotation.

**Reason:** S3 Object Lock in compliance mode prevents deletion or overwriting by anyone, including the root user, until retention expires (governance mode can be overridden by privileged users, so it fails the strict requirement); and KMS customer managed keys support a simple one-time setting to enable automatic annual key rotation, while SSE-S3 gives no control over rotation schedule and imported key material doesn't support automatic rotation at all.

---

## Question 204

**Full Question:** A company is using a content management system that runs on a single Amazon EC2 instance. The EC2 instance contains both the web server and the database software. The company must make its website platform highly available and must enable the website to scale to meet user demand. What should a solutions architect recommend to meet these requirements?

**Short Question:** How do you make a single-EC2-instance web+database app both highly available and scalable?

**Options:**
- A. Move the database to Amazon RDS and enable automatic backups. Manually launch another EC2 instance in the same availability zone. Configure an application load balancer in the availability zone and set the two instances as targets.
- B. Migrate the database to an Amazon Aurora instance with a read replica in the same availability zone as the existing EC2 instance. Manually launch another EC2 instance in the same availability zone. Configure an application load balancer and set the two EC2 instances as targets.
- ✅ C. Move the database to Amazon Aurora with a read replica in another availability zone. Create an Amazon Machine Image (AMI) from the EC2 instance. Configure an application load balancer in two availability zones. Attach an Auto Scaling group that uses the AMI across two availability zones.
- D. Move the database to a separate EC2 instance and schedule backups to Amazon S3. Create an Amazon Machine Image (AMI) from the original EC2 instance. Configure an application load balancer in two availability zones. Attach an Auto Scaling group that uses the AMI across two availability zones.

**Reason:** Option C separates the database onto a managed, multi-AZ Aurora database and makes the web tier scalable and highly available with an AMI-based Auto Scaling group behind a multi-AZ load balancer; the other options either keep everything in a single availability zone (a single point of failure), rely on manual instance management with no autoscaling, or use a self-managed EC2 database instead of a managed highly available service.

---

## Question 205

**Full Question:** A company has a mobile chat application with a data store based in Amazon DynamoDB. Users would like new messages to be read with as little latency as possible. A solutions architect needs to design an optimal solution that requires minimal application changes. Which method should the solutions architect select?

**Short Question:** How do you get the lowest possible read latency for a DynamoDB-backed chat app with minimal code changes?

**Options:**
- ✅ A. Configure Amazon DynamoDB Accelerator (DAX) for the new messages table. Update the code to use the DAX endpoint.
- B. Add DynamoDB read replicas to handle the increased read load. Update the application to point to the read endpoint for the read replicas.
- C. Double the number of read capacity units for the new messages table in DynamoDB. Continue to use the existing DynamoDB endpoint.
- D. Add an Amazon ElastiCache for Redis cache to the application stack. Update the application to point to the Redis cache endpoint instead of DynamoDB.

**Reason:** DAX is a fully managed, API-compatible in-memory cache for DynamoDB that delivers microsecond read latency, so only the endpoint needs to change; read replicas describe DynamoDB global tables (meant for multi-region latency, not in-region microsecond caching), raising read capacity units improves throughput but not per-request latency, and ElastiCache for Redis isn't API-compatible with DynamoDB so it would require significant application logic changes.

---

## Question 206

**Full Question:** A company is developing a new mobile app. The company must implement proper traffic filtering to protect its Application Load Balancer (ALB) against common application-level attacks such as cross-site scripting (XSS) or SQL injection. The company has minimal infrastructure and operational staff. The company needs to reduce its share of the responsibility in managing, updating, and securing servers for its AWS environment. What should a solutions architect recommend to meet these requirements?

**Short Question:** What's the managed, low-overhead way to protect an ALB from application-layer attacks like XSS and SQL injection?

**Options:**
- ✅ A. Configure AWS WAF rules and associate them with the ALB.
- B. Deploy the application using Amazon S3 with public hosting enabled.
- C. Deploy AWS Shield Advanced and add the ALB as a protected resource.
- D. Create a new ALB that directs traffic to an Amazon EC2 instance running a third-party firewall, which then passes the traffic to the current ALB.

**Reason:** AWS WAF is a managed web application firewall purpose-built to block Layer 7 exploits like XSS and SQL injection, and it attaches directly to an ALB with minimal operational effort. S3 static hosting doesn't apply to a dynamic app behind an ALB, Shield Advanced protects against DDoS (Layer 3/4) rather than application-layer attacks, and routing through a self-managed EC2 firewall adds significant operational burden the company is trying to avoid.

---

## Question 207

**Full Question:** A university research laboratory needs to migrate 30 terabytes of data from an on-premises Windows file server to Amazon FSx for Windows File Server. The laboratory has a 1 Gbit per second network link that many other departments in the university share. The laboratory wants to implement a data migration service that will maximize the performance of the data transfer. However, the laboratory needs to be able to control the amount of bandwidth that the service uses to minimize the impact on other departments. The data migration must take place within the next 5 days. Which AWS solution will meet these requirements?

**Short Question:** Which service can migrate 30TB to FSx for Windows over a shared 1 Gbps link within 5 days while letting you throttle bandwidth?

**Options:**
- A. AWS Snowcone
- B. Amazon FSx File Gateway
- ✅ C. AWS DataSync
- D. AWS Transfer Family

**Reason:** AWS DataSync is a fully managed service that maximizes transfer performance over the network and includes built-in bandwidth throttling, and it supports Amazon FSx for Windows File Server as a destination. Snowcone is unnecessary since the network transfer is feasible within the deadline, FSx File Gateway is for ongoing hybrid access rather than a one-time bulk migration, and Transfer Family only supports S3 and EFS as backends, not FSx for Windows File Server.

---

## Question 208

**Full Question:** A company runs a web application on Amazon EC2 instances in multiple Availability Zones. The EC2 instances are in private subnets. A solutions architect implements an internet-facing Application Load Balancer (ALB) and specifies the EC2 instances as the target group. However, the internet traffic is not reaching the EC2 instances. How should the solutions architect reconfigure the architecture to resolve this issue?

**Short Question:** Why isn't internet traffic reaching EC2 instances behind an internet-facing ALB, and how do you fix it?

**Options:**
- A. Replace the ALB with a Network Load Balancer. Configure a NAT gateway in a public subnet to allow internet traffic.
- B. Move the EC2 instances to public subnets. Add a rule to the EC2 instances' security groups to allow outbound traffic to 0.0.0.0/0.
- C. Update the route tables for the EC2 instances' subnets to send 0.0.0.0/0 traffic through the internet gateway route. Add a rule to the EC2 instances' security groups to allow outbound traffic to 0.0.0.0/0.
- ✅ D. Create public subnets in each Availability Zone. Associate the public subnets with the ALB. Update the route tables for the public subnets with a route to the private subnets.

**Reason:** An internet-facing ALB must itself sit in public subnets (with a route to an internet gateway) to receive traffic from the internet, and it can then forward that traffic to the EC2 instances safely kept in private subnets. Options A and the NAT gateway don't enable inbound traffic, while B and C effectively turn the private subnets public, violating the security best practice of keeping application servers off the internet directly.

---

## Question 209

**Full Question:** A company hosts a multi-tier web application that uses an Amazon Aurora MySQL DB cluster for storage. The application tier is hosted on Amazon EC2 instances. The company's IT security guidelines mandate that the database credentials be encrypted and rotated every 14 days. What should a solutions architect do to meet this requirement with the least operational effort?

**Short Question:** What's the lowest-effort way to store and auto-rotate Aurora MySQL database credentials every 14 days?

**Options:**
- ✅ A. Create a new AWS KMS encryption key. Use AWS Secrets Manager to create a new secret that uses the KMS key with the appropriate credentials. Associate the secret with the Aurora DB cluster. Configure a custom rotation period of 14 days.
- B. Create two parameters in AWS Systems Manager Parameter Store — one for the username as a string parameter and one using the SecureString type for the password, with AWS KMS encryption for the password parameter, loaded by the application tier. Implement an AWS Lambda function that rotates the password every 14 days.
- C. Store a file containing the credentials in an AWS KMS–encrypted Amazon EFS file system. Mount the EFS file system on all EC2 instances of the application tier, restrict file access so the application can read it and only super users can modify it, and implement an AWS Lambda function that rotates the credentials in Aurora every 14 days and writes new credentials into the file.
- D. Store a file containing the credentials in an AWS KMS–encrypted Amazon S3 bucket that the application uses to load credentials, downloading the file regularly. Implement an AWS Lambda function that rotates the Aurora credentials every 14 days and uploads the new credentials to the file in the S3 bucket.

**Reason:** AWS Secrets Manager natively integrates with Aurora and has a built-in automatic rotation feature, so you just configure a 14-day rotation schedule instead of building anything custom. The other options (Parameter Store, EFS, or S3) can store encrypted credentials but all require you to write and maintain a custom Lambda rotation function yourself, which is significantly more operational effort.

---

## Question 210

**Full Question:** A company wants to analyze and troubleshoot access denied errors and unauthorized errors that are related to IAM permissions. The company has AWS CloudTrail turned on. Which solution will meet these requirements with the least effort?

**Short Question:** What's the easiest way to search CloudTrail logs for IAM access-denied/unauthorized errors?

**Options:**
- A. Use AWS Glue and write custom scripts to query CloudTrail logs for the errors.
- B. Use AWS Batch and write custom scripts to query CloudTrail logs for the errors.
- ✅ C. Search CloudTrail logs with Amazon Athena queries to identify the errors.
- D. Search CloudTrail logs with Amazon QuickSight. Create a dashboard to identify the errors.

**Reason:** Amazon Athena is a serverless query service that can run SQL directly against CloudTrail logs stored in S3, letting you filter for specific error codes with minimal setup. AWS Glue and AWS Batch both require building custom ETL pipelines or scripts (much higher effort), and QuickSight requires setting up datasets and dashboards, which is more overhead than a simple ad hoc Athena query for troubleshooting.

---

## Question 211

**Full Question:** A company recently announced the deployment of its retail website to a global audience. The website runs on multiple Amazon EC2 instances behind an Elastic Load Balancer. The instances run in an Auto Scaling group across multiple Availability Zones. The company wants to provide its customers with different versions of content based on the devices that the customers use to access the website. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** How do you serve different content versions to global users based on their device type, with minimal overhead?

**Options:**
- ✅ A. Configure Amazon CloudFront to cache multiple versions of the content
- B. Configure a host header in a Network Load Balancer to forward traffic to different instances
- ✅ C. Configure a Lambda@Edge function to send specific objects to users based on the User-Agent header
- D. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up host-based routing to different EC2 instances
- E. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up path-based routing to different EC2 instances

**Reason:** CloudFront caches multiple content versions globally at edge locations, and Lambda@Edge can inspect the User-Agent header at the edge to serve the right version per device. NLBs operate at Layer 4 and cannot inspect HTTP headers, so host-based or path-based routing (Layer 7 features) is not possible with an NLB, ruling out B, D, and E.

---

## Question 212

**Full Question:** A company has deployed a database in Amazon RDS for MySQL. Due to increased transactions, the database support team is reporting slow reads against the DB instance and recommends adding a read replica. Which combination of actions should a solutions architect take before implementing this change? (Choose two.)

**Short Question:** What must be set up on an RDS for MySQL instance before you can create a read replica?

**Options:**
- A. Enable bin log replication on the RDS primary node
- B. Choose a failover priority for the source DB instance
- ✅ C. Allow long-running transactions to complete on the source DB instance
- D. Create a global table and specify the AWS Regions where the table will be available
- ✅ E. Enable automatic backups on the source instance by setting the backup retention period to a value other than zero

**Reason:** Automatic backups must be enabled (retention period > 0) because RDS uses a snapshot of the source instance to create the read replica, and binary logging is automatically enabled as part of that process rather than as a separate step. Long-running transactions should be allowed to finish first since they can lock the database and delay or fail the snapshot; failover priority applies to Aurora Global Database, and global tables are a DynamoDB feature, so neither applies here.

---

## Question 213

**Full Question:** A company has a three-tier web application that is deployed on AWS. The web servers are deployed in a public subnet in a VPC. The application servers and database servers are deployed in private subnets in the same VPC. The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC. The appliance is configured with an IP interface that can accept IP packets. A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead way to route all inbound web traffic through a third-party firewall appliance for inspection?

**Options:**
- A. Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection
- B. Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection
- C. Deploy a transit gateway in the inspection VPC. Configure route tables to route the incoming packets through the transit gateway
- ✅ D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance

**Reason:** Gateway Load Balancer plus a Gateway Load Balancer endpoint is the purpose-built AWS solution for transparently inserting third-party virtual appliances into a traffic path, managing scaling and availability of the appliance fleet with minimal architectural changes. NLB and ALB are meant for distributing traffic to application targets rather than transparent appliance insertion, and using a transit gateway for ingress inspection is far more complex than necessary.

---

## Question 214

**Full Question:** A company has an application that runs on several Amazon EC2 instances. Each EC2 instance has multiple Amazon Elastic Block Store (Amazon EBS) data volumes attached to it. The application's EC2 instance configuration and data need to be backed up nightly. The application also needs to be recoverable in a different AWS Region. Which solution will meet these requirements in the most operationally efficient way?

**Short Question:** What's the most efficient way to nightly-back-up both an EC2 instance's configuration and its attached EBS volumes, with cross-region recovery?

**Options:**
- A. Write an AWS Lambda function that schedules nightly snapshots of the application's EBS volumes and copies the snapshots to a different Region
- ✅ B. Create a backup plan by using AWS Backup to perform nightly backups. Copy the backups to another Region. Add the application's EC2 instances as resources
- C. Create a backup plan by using AWS Backup to perform nightly backups. Copy the backups to another Region. Add the application's EBS volumes as resources
- D. Write an AWS Lambda function that schedules nightly snapshots of the application's EBS volumes and copies the snapshots to a different Availability Zone

**Reason:** Adding the EC2 instances themselves (not just the EBS volumes) as resources in an AWS Backup plan captures both the instance configuration (as an AMI) and all attached EBS volume snapshots, and AWS Backup can automatically copy these to another Region. Options A and D only back up the EBS volumes (missing the instance configuration), option C also misses the instance configuration, and A/D use custom Lambda code which is less operationally efficient than the managed AWS Backup service; D also copies to another AZ instead of another Region.

---

## Question 215

**Full Question:** A company has two VPCs named Management and Production. The Management VPC uses VPNs through a customer gateway to connect to a single device in the data center. The Production VPC uses a virtual private gateway with two attached AWS Direct Connect connections. The Management and Production VPCs both use a single VPC peering connection to allow communication between the applications. What should a solutions architect do to mitigate any single point of failure in this architecture?

**Short Question:** How do you eliminate the single point of failure in this hybrid network setup (mixed VPN, Direct Connect, and VPC peering)?

**Options:**
- A. Add a set of VPNs between the Management and Production VPCs
- B. Add a second virtual private gateway and attach it to the Management VPC
- ✅ C. Add a second set of VPNs to the Management VPC from a second customer gateway device
- D. Add a second VPC peering connection between the Management VPC and the Production VPC

**Reason:** The actual single point of failure is the single on-premises customer gateway device that the Management VPC's VPN relies on, so adding a second customer gateway device and a second VPN connection creates the needed redundancy. A VPC can only have one virtual private gateway attached and only one active peering connection between the same two VPCs (ruling out B and D), and the VPC peering connection is already highly available so adding VPNs between the VPCs (A) doesn't address the real problem.

---

## Question 216

**Full Question:** A company needs to integrate with a third-party data feed. The data feed sends a webhook to notify an external service when new data is ready for consumption. A developer wrote an AWS Lambda function to retrieve data when the company receives a webhook call back. The developer must make the Lambda function available for the third party to call. Which solution will meet these requirements with the most operational efficiency?

**Short Question:** What is the simplest, lowest-overhead way to expose a Lambda function as a public endpoint a third party can call via webhook?

**Options:**
- ✅ A. Create a function URL for the Lambda function. Provide the Lambda function URL to the third party for the webhook.
- B. Deploy an Application Load Balancer (ALB) in front of the Lambda function. Provide the ALB URL to the third party for the webhook.
- C. Create an Amazon Simple Notification Service (Amazon SNS) topic. Attach the topic to the Lambda function. Provide the public host name of the SNS topic to the third party for the webhook.
- D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Attach the queue to the Lambda function. Provide the public host name of the SQS queue to the third party for the webhook.

**Reason:** Lambda function URLs give a dedicated public HTTPS endpoint built directly into Lambda, with no extra infrastructure to manage, unlike an ALB (requires VPC/subnets/target groups) or SNS/SQS (neither exposes a public HTTPS host name that a third party can simply POST to).

---

## Question 217

**Full Question:** A company has 700 terabytes of backup data stored in network attached storage (NAS) in its data center. This backup data needs to be accessible for infrequent regulatory requests and must be retained for 7 years. The company has decided to migrate this backup data from its data center to AWS. The migration must be complete within one month. The company has 500 megabits per second of dedicated bandwidth on its public internet connection available for data transfer. What should a solutions architect do to migrate and store the data at the lowest cost?

**Short Question:** What's the cheapest way to move 700 TB of rarely-needed, 7-year-retention backup data to AWS within one month given limited network bandwidth?

**Options:**
- ✅ A. Order AWS Snowball devices to transfer the data. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.
- B. Deploy a VPN connection between the data center and the Amazon VPC. Use the AWS CLI to copy the data from on-premises to Amazon S3 Glacier.
- C. Provision a 500 Mbps AWS Direct Connect connection and transfer the data to Amazon S3. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.
- D. Use AWS DataSync to transfer the data and deploy a DataSync agent on-premises. Use the DataSync task to copy files from the on-premises NAS storage to Amazon S3 Glacier.

**Reason:** At 500 Mbps, transferring 700 TB would take over 130 days, blowing past the 1-month deadline, so any network-based option (VPN, Direct Connect, DataSync) fails and none of them can write directly to Glacier anyway; AWS Snowball physically ships the data to bypass the network bottleneck, and a lifecycle policy into S3 Glacier Deep Archive gives the lowest-cost storage for long-term, rarely accessed data.

---

## Question 218

**Full Question:** A company must migrate 20 terabytes of data from a data center to the AWS Cloud within 30 days. The company's network bandwidth is limited to 15 megabits per second and cannot exceed 70% utilization. What should a solutions architect do to meet these requirements?

**Short Question:** What's the right way to migrate 20 TB of data within 30 days when available network bandwidth is far too slow?

**Options:**
- ✅ A. Use AWS Snowball.
- B. Use AWS DataSync.
- C. Use a secure VPN connection.
- D. Use Amazon S3 Transfer Acceleration.

**Reason:** At 15 Mbps capped to 70% utilization (10.5 Mbps effective), transferring 20 TB would take roughly 185 days, so any network-based approach (DataSync, VPN, S3 Transfer Acceleration) cannot meet the 30-day deadline; AWS Snowball physically transports the data and bypasses the bandwidth limitation entirely.

---

## Question 219

**Full Question:** A company is concerned that two NAT instances in use will no longer be able to support the traffic needed for the company's application. A solutions architect wants to implement a solution that is highly available, fault tolerant, and automatically scalable. What should the solutions architect recommend?

**Short Question:** What's the best highly-available, fault-tolerant, auto-scaling replacement for two NAT instances that are running out of capacity?

**Options:**
- A. Remove the two NAT instances and replace them with two NAT gateways in the same availability zone.
- B. Use Auto Scaling groups with Network Load Balancers for the NAT instances in different availability zones.
- ✅ C. Remove the two NAT instances and replace them with two NAT gateways in different availability zones.
- D. Replace the two NAT instances with Spot Instances in different availability zones and deploy a Network Load Balancer.

**Reason:** NAT Gateway is a fully managed service that is inherently fault tolerant and scales its bandwidth automatically, and deploying one per Availability Zone (with each subnet's route table pointing to its local gateway) avoids a single point of failure; option A puts both gateways in one AZ (single point of failure), B recreates NAT instances with a costly self-managed setup, and D uses Spot Instances which can be interrupted and are unsuitable for critical network infrastructure.

---

## Question 220

**Full Question:** A company has a web application hosted over 10 Amazon EC2 instances with traffic directed by Amazon Route 53. The company occasionally experiences a timeout error when attempting to browse the application. The networking team finds that some DNS queries return IP addresses of unhealthy instances, resulting in the timeout error. What should a solutions architect implement to overcome these timeout errors?

**Short Question:** How do you eliminate timeouts caused by DNS clients caching IP addresses of unhealthy EC2 instances behind Route 53?

**Options:**
- A. Create a Route 53 simple routing policy record for each EC2 instance. Associate a health check with each record.
- B. Create a Route 53 failover routing policy record for each EC2 instance. Associate a health check with each record.
- C. Create an Amazon CloudFront distribution with the EC2 instances as its origin. Associate a health check with the EC2 instances.
- ✅ D. Create an Application Load Balancer (ALB) with a health check in front of the EC2 instances. Route to the ALB from Route 53.

**Reason:** The root cause is DNS caching (clients keep using a cached IP for an unhealthy instance until the TTL expires); putting an ALB in front of the instances lets it perform fast, frequent health checks and stop routing to unhealthy targets almost instantly, while Route 53 just points a single record to the ALB — simple routing doesn't support health checks, failover routing is meant for active/passive pairs (not distributing across 10 active instances), and CloudFront isn't the purpose-built tool for EC2 fleet health/traffic management.

---

## Question 221

**Full Question:** A company hosts its multi-tier applications on AWS. For compliance, governance, auditing, and security, the company must track configuration changes on its AWS resources and record a history of API calls made to these resources. What should a solutions architect do to meet these requirements?

**Short Question:** Which AWS services should be paired to track resource configuration changes and log API call history?

**Options:**
- A. Use AWS CloudTrail to track configuration changes and AWS Config to record API calls
- ✅ B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls
- C. Use AWS Config to track configuration changes and Amazon CloudWatch to record API calls
- D. Use AWS CloudTrail to track configuration changes and Amazon CloudWatch to record API calls

**Reason:** AWS Config is purpose-built to monitor and record how resource configurations change over time, while AWS CloudTrail is purpose-built to log the history of API calls; the other options either swap these roles or substitute CloudWatch, which is for metrics/logs rather than API auditing.

---

## Question 222

**Full Question:** A company needs to create an Amazon Elastic Kubernetes Service (Amazon EKS) cluster to host a digital media streaming application. The EKS cluster will use a managed node group that is backed by Amazon Elastic Block Store (Amazon EBS) volumes for storage. The company must encrypt all data at rest by using a customer-managed key that is stored in AWS Key Management Service (AWS KMS). Which combination of actions will meet this requirement with the least operational overhead? (Choose two.)

**Short Question:** With minimal effort, how do you ensure EKS-managed-node-group EBS volumes are encrypted with a specific customer-managed KMS key?

**Options:**
- A. Use a Kubernetes plug-in that uses the customer managed key to perform data encryption
- B. After creation of the EKS cluster, locate the EBS volumes. Enable encryption by using the customer managed key
- ✅ C. Enable EBS encryption by default in the AWS Region where the EKS cluster will be created. Select the customer managed key as the default key
- ✅ D. Create the EKS cluster. Create an IAM role that has a policy that grants permission to the customer managed key. Associate the role with the EKS cluster
- E. Store the customer managed key as a Kubernetes secret in the EKS cluster. Use the customer managed key to encrypt the EBS volumes

**Reason:** Turning on account/region-level "EBS encryption by default" with the customer-managed key as default automatically encrypts every new EBS volume with no ongoing effort, but the EKS cluster's IAM role also needs an explicit policy granting it permission to use that KMS key; a Kubernetes plug-in or secret misunderstands that EBS encryption happens at the AWS infrastructure layer via IAM, not inside Kubernetes, and existing unencrypted volumes can't simply have encryption enabled after the fact.

---

## Question 223

**Full Question:** A company recently created a disaster recovery site in a different AWS Region. The company needs to transfer large amounts of data back and forth between NFS file systems in the two regions on a periodic basis. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the easiest way to regularly move large amounts of data between NFS file systems in two AWS Regions?

**Options:**
- ✅ A. Use AWS DataSync
- B. Use AWS Snowball devices
- C. Set up an SFTP server on Amazon EC2
- D. Use AWS Database Migration Service (AWS DMS)

**Reason:** AWS DataSync is a fully managed service built specifically to move large data sets between NFS locations, including across AWS Regions, and supports scheduled recurring transfers with minimal upkeep; Snowball suits one-time offline migrations, a self-managed SFTP server adds heavy operational burden, and DMS is meant for databases, not file systems.

---

## Question 224

**Full Question:** A hospital wants to create digital copies for its large collection of historical written records. The hospital will continue to add hundreds of new documents each day. The hospital's data team will scan the documents and will upload the documents to the AWS Cloud. A solutions architect must implement a solution to analyze the documents, extract the medical information, and store the documents so that an application can run SQL queries on the data. The solution must maximize scalability and operational efficiency. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

**Short Question:** What's the most scalable, low-overhead way to extract medical data from scanned documents and make it SQL-queryable?

**Options:**
- A. Write the document information to an Amazon EC2 instance that runs a MySQL database
- ✅ B. Write the document information to an Amazon S3 bucket. Use Amazon Athena to query the data
- C. Create an autoscaling group of Amazon EC2 instances to run a custom application that processes the scan files and extracts the medical information
- D. Create an AWS Lambda function that runs when new documents are uploaded. Use Amazon Rekognition to convert the documents to raw text. Use Amazon Transcribe Medical to detect and extract relevant medical information from the text
- ✅ E. Create an AWS Lambda function that runs when new documents are uploaded. Use Amazon Textract to convert the documents to raw text. Use Amazon Comprehend Medical to detect and extract relevant medical information from the text

**Reason:** Amazon Textract is the correct OCR service for pulling text out of scanned documents and Amazon Comprehend Medical is the correct NLP service for extracting medical entities from that text, all triggered serverlessly by Lambda; storing the results in S3 and querying with Athena gives scalable, serverless SQL access, whereas a self-managed EC2 database or EC2 fleet adds unnecessary overhead, and Rekognition/Transcribe Medical are the wrong tools (Rekognition isn't the primary OCR service and Transcribe Medical handles speech audio, not document text).

---

## Question 225

**Full Question:** An online learning company is migrating to the AWS Cloud. The company maintains its student records in a PostgreSQL database. The company needs a solution in which its data is available and online across multiple AWS Regions at all times. Which solution will meet these requirements with the least amount of operational overhead?

**Short Question:** What's the lowest-effort way to keep a PostgreSQL database continuously online and available across multiple AWS Regions?

**Options:**
- A. Migrate the PostgreSQL database to a PostgreSQL cluster on Amazon EC2 instances
- B. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance with the Multi-AZ feature turned on
- ✅ C. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Create a read replica in another Region
- D. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Set up DB snapshots to be copied to another Region

**Reason:** Amazon RDS cross-region read replicas give a continuously updated, online copy of the database in a second Region with AWS managing the replication, meeting the "always online" requirement with minimal effort; Multi-AZ only protects against failure within a single Region, snapshots require a time-consuming restore before becoming usable, and a self-managed EC2 cluster demands the most manual setup and maintenance.
