# AWS SAA-C03 Real Exam Questions & Answers — Part 6 (Q126–Q150)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q126
**Full Question:** A company needs to give a globally distributed development team secure access to the company's AWS resources in a way that complies with security policies. The company currently uses an on-premises Active Directory for internal authentication. The company uses AWS Organizations to manage multiple AWS accounts that support multiple projects. The company needs a solution to integrate with the existing infrastructure to provide centralized identity management and access control. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Give a global dev team secure access to many AWS accounts, centrally, using the existing on-prem AD.
- A. AWS Managed Microsoft AD + trust with on-prem AD + IAM roles per AD group
- B. IAM user per developer, manual permissions, MFA
- C. AD Connector + AWS IAM Identity Center + permission sets mapped to AD groups ✅
- D. Amazon Cognito identity federation with on-prem AD

**Reason:** AD Connector + IAM Identity Center is the purpose-built, low-overhead way to federate an existing AD into centralized multi-account access via permission sets — the other options are either high-maintenance or the wrong service for this use case.

---

## Q127
**Full Question:** A financial company hosts a web application on AWS. The application uses an Amazon API Gateway regional API endpoint to give users the ability to retrieve current stock prices. The company's security team has noticed an increase in the number of API requests. The security team is concerned that HTTP flood attacks might take the application offline. A solutions architect must design a solution to protect the application from this type of attack. Which solution meets these requirements with the least operational overhead?

**Short Question:** Protect a regional API Gateway endpoint from HTTP flood attacks, least operational overhead.
- A. CloudFront in front of API Gateway with a 24-hour max TTL
- B. Regional AWS WAF web ACL with a rate-based rule, associated with the API Gateway stage ✅
- C. CloudWatch metric monitoring + alert the security team
- D. CloudFront with Lambda@Edge + custom Lambda function to block over-rate IPs

**Reason:** A WAF rate-based rule automatically counts and blocks requests per IP that exceed a threshold — a fully managed feature purpose-built for this, unlike caching (A), passive alerting (C), or a custom Lambda solution (D).

---

## Q128
**Full Question:** A company is implementing new data retention policies for all databases that run on Amazon RDS DB instances. The company must retain daily backups for a minimum period of 2 years. The backups must be consistent and restorable. Which solution should a solutions architect recommend to meet these requirements?

**Short Question:** Retain daily RDS backups for at least 2 years, consistent and restorable.
- A. AWS Backup vault + backup plan (daily schedule, 2-year expiration), assign RDS instances ✅
- B. RDS backup window + 2-year snapshot retention policy + Amazon DLM to schedule deletions
- C. Auto-backup database transaction logs to CloudWatch Logs, 2-year expiration
- D. AWS DMS replication task with CDC streaming changes to S3, S3 lifecycle policy deletes after 2 years

**Reason:** AWS Backup centrally manages backup plans with custom retention — RDS's own automated backups cap out at 35 days, DLM is for EBS (not RDS) snapshots, and CloudWatch Logs/DMS aren't restorable full-database backup mechanisms.

---

## Q129
**Full Question:** An online gaming company is transitioning user data storage to Amazon DynamoDB to support the company's growing user base. The current architecture includes DynamoDB tables that contain user profiles, achievements, and in-game transactions. The company needs to design a robust, continuously available and resilient DynamoDB architecture to maintain a seamless gaming experience for users. Which solution will meet these requirements most cost effectively?

**Short Question:** Design a continuously available, resilient, cost-effective multi-region DynamoDB architecture for a growing gaming user base.
- A. Single-region tables, on-demand capacity, "use global tables to replicate across regions" (contradiction — global tables require multi-region tables)
- B. DAX caching + single-region tables with autoscaling + manual cross-region replication
- C. Multi-region tables, on-demand capacity, DynamoDB Streams for manual cross-region replication
- D. DynamoDB Global Tables (automatic multi-region replication), multi-region tables, provisioned capacity + autoscaling ✅

**Reason:** Global Tables is the fully managed multi-region replication feature (ruling out A's self-contradiction and the manual/custom replication in B/C), and provisioned capacity with autoscaling is cheaper than on-demand for a predictable, steadily growing workload.

---

## Q130
**Full Question:** A company runs an Oracle database on premises. As part of the company's migration to AWS, the company wants to upgrade the database to the most recent available version. The company also wants to set up disaster recovery (DR) for the database. The company needs to minimize the operational overhead for normal operations and DR setup. The company also needs to maintain access to the database's underlying operating system. Which solution will meet these requirements?

**Short Question:** Migrate + upgrade Oracle DB, set up DR, minimize operational overhead, but must keep OS-level access to the DB server.
- A. Migrate to EC2, set up database replication to another region
- B. Migrate to standard RDS for Oracle, cross-region automated backup replication
- C. Migrate to RDS Custom for Oracle, cross-region read replica ✅
- D. Migrate to standard RDS for Oracle, standby in another AZ

**Reason:** RDS Custom is the only managed RDS option that still exposes OS-level access — standard RDS (B/D) hides the OS, and plain EC2 (A) has the highest operational overhead of all.

---

## Q131
**Full Question:** An e-commerce company is building a distributed application that involves several serverless functions and AWS services to complete order processing tasks. These tasks require manual approvals as part of the workflow. A solutions architect needs to design an architecture for the order processing application. The solution must be able to combine multiple AWS Lambda functions into responsive serverless applications. The solution also must orchestrate data and services that run on Amazon EC2 instances, containers, or on-premises servers. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Orchestrate a multi-step order-processing workflow (with manual approval steps) across Lambda, EC2, containers, and on-prem servers.
- A. AWS Step Functions ✅
- B. Integrate everything inside one AWS Glue job
- C. Amazon SQS
- D. Lambda functions + EventBridge events

**Reason:** Step Functions is the purpose-built serverless workflow orchestrator that natively supports human approval steps and can coordinate tasks across Lambda, EC2, ECS, and on-prem — Glue is an ETL tool, SQS is just a queue, and Lambda+EventBridge alone would require custom state management.

---

## Q132
**Full Question:** A company is planning to store data on Amazon RDS DB instances. The company must encrypt the data at rest. What should a solutions architect do to meet this requirement?

**Short Question:** Encrypt RDS data at rest, the standard way.
- A. Create a key in AWS KMS, enable encryption for the DB instances ✅
- B. Create an encryption key, store it in Secrets Manager, use it to encrypt the DB instances
- C. Generate a certificate in ACM, enable SSL/TLS on the DB instances
- D. Generate a certificate in IAM, enable SSL/TLS on the DB instances

**Reason:** RDS encryption-at-rest natively integrates with KMS keys — Secrets Manager stores secrets (not encryption keys for RDS), and ACM/IAM certificates are for encryption in transit (SSL/TLS), a different requirement entirely.

---

## Q133
**Full Question:** A company is building a solution that will report Amazon EC2 Auto Scaling events across all the applications in an AWS account. The company needs to use a serverless solution to store the EC2 Auto Scaling status data in Amazon S3. The company then will use the data in Amazon S3 to provide near real-time updates in a dashboard. The solution must not affect the speed of EC2 instance launches. How should the company move the data to Amazon S3 to meet these requirements?

**Short Question:** Stream EC2 Auto Scaling events to S3 for a near-real-time dashboard, serverless, with zero impact on instance launch speed.
- A. CloudWatch metric stream → Kinesis Data Firehose → S3 ✅
- B. EMR cluster collects the data → Firehose → S3
- C. EventBridge rule invokes a scheduled Lambda that writes directly to S3
- D. Kinesis agent installed via bootstrap script on each instance → Kinesis Data Streams → S3

**Reason:** A CloudWatch metric stream + Firehose is a fully serverless, decoupled, near-real-time pipeline — EMR is overkill, a scheduled Lambda isn't near-real-time, and a launch-time bootstrap script directly slows down instance launches.

---

## Q134
**Full Question:** A company is launching a new application deployed on an Amazon Elastic Container Service (Amazon ECS) cluster and is using the Fargate launch type for ECS tasks. The company is monitoring CPU and memory usage because it is expecting high traffic to the application upon its launch. However, the company wants to reduce costs when utilization decreases. What should a solutions architect recommend?

**Short Question:** Auto-scale an ECS-on-Fargate service up for launch-day traffic and back down to save cost afterward.
- A. EC2 Auto Scaling based on past traffic patterns
- B. Custom Lambda function scaling ECS, triggered by a CloudWatch alarm
- C. EC2 Auto Scaling with simple scaling policies triggered by ECS metric alarms
- D. AWS Application Auto Scaling with target tracking policies triggered by ECS metric alarms ✅

**Reason:** Fargate has no EC2 instances to scale (ruling out A/C), and Application Auto Scaling with target tracking is the native, managed way to scale ECS task count to a metric target — no custom Lambda needed (B).

---

## Q135
**Full Question:** A company's application runs on Amazon EC2 instances behind an Application Load Balancer (ALB). The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. On the first day of every month at midnight, the application becomes much slower when the month-end financial calculation batch runs. This causes the CPU utilization of the EC2 instances to immediately peak to 100% which disrupts the application. What should a solutions architect recommend to ensure the application is able to handle the workload and avoid downtime?

**Short Question:** A predictable monthly batch job spikes CPU to 100% instantly and disrupts the app — scale ahead of it.
- A. CloudFront in front of the ALB
- B. EC2 Auto Scaling simple scaling policy based on CPU utilization
- C. EC2 Auto Scaling scheduled scaling policy on the monthly schedule ✅
- D. ElastiCache to offload workload from the EC2 instances

**Reason:** Scheduled scaling proactively adds capacity right before the known monthly spike — a reactive policy (B) only scales after the CPU has already spiked, and CloudFront/ElastiCache don't address a compute-bound batch calculation.

---

## Q136
**Full Question:** A company's order system sends requests from clients to Amazon EC2 instances. The EC2 instances process the orders and then store the orders in a database on Amazon RDS. Users report that they must reprocess orders when the system fails. The company wants a resilient solution that can process orders automatically if a system outage occurs. What should a solutions architect do to meet these requirements?

**Short Question:** Orders get lost/must be manually reprocessed when the EC2 order processor fails — make failure recovery automatic.
- A. Auto Scaling group + EventBridge rule targeting an ECS task
- B. Auto Scaling group behind an ALB, order system sends directly to the ALB
- C. Auto Scaling group + order system sends to an SQS queue, EC2 instances consume from it ✅
- D. SNS topic + Lambda subscriber + Systems Manager Run Command on EC2 to process messages

**Reason:** SQS provides a durable buffer so in-flight orders survive an instance failure — a direct ALB call (B) still loses in-flight requests on failure, and SNS (D) doesn't durably buffer messages the way a queue does.

---

## Q137
**Full Question:** An application runs on Amazon EC2 instances in private subnets. The application needs to access an Amazon DynamoDB table. What is the most secure way to access the table while ensuring that the traffic does not leave the AWS network?

**Short Question:** Private-subnet EC2 app needs DynamoDB access that never leaves the AWS network, most securely.
- A. VPC endpoint (gateway) for DynamoDB ✅
- B. NAT gateway in a public subnet
- C. NAT instance in a private subnet
- D. The VPC's internet gateway

**Reason:** A DynamoDB gateway VPC endpoint keeps traffic entirely inside the AWS network with no public IPs involved — NAT gateways/instances and internet gateways all route (or risk routing) traffic over the public internet.

---

## Q138
**Full Question:** A company uses AWS Organizations to create dedicated AWS accounts for each business unit to manage each business unit's account independently upon request. The root email recipient missed a notification that was sent to the root user email address of one account. The company wants to ensure that all future notifications are not missed. Future notifications must be limited to account administrators. Which solution will meet these requirements?

**Short Question:** A root-user notification was missed on one account — make sure future ones always reach the right admins, without going company-wide.
- A. Forward all root-user email to every user in the organization
- B. Configure root user email addresses as distribution lists to a few admins + configure AWS account alternate contacts ✅
- C. Route all root-user email to one single administrator who manually forwards it
- D. Use the same root user email address across all accounts + configure alternate contacts

**Reason:** A distribution list (not a single person, not the whole org) plus alternate contacts ensures multiple admins reliably see notifications without a single point of failure — options A/C/D either over-share or under-cover the requirement, and D also violates the best practice of unique root emails per account.

---

## Q139
**Full Question:** An application development team is designing a microservice that will convert large images to smaller compressed images. When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket. A solutions architect needs to design a solution that uses durable stateless components to process the images automatically. Which combination of actions will meet these requirements? (Choose 2)

**Short Question:** Auto-process uploaded images (S3 → compress → S3) using durable, stateless components only.
- A. SQS queue + S3 sends a notification to it on upload ✅
- B. Lambda invoked directly by the SQS queue as its source, deletes the message on success ✅
- C. Lambda polls S3 for new uploads, tracks processed files in an in-memory text file
- D. EC2 instance monitors the SQS queue, logs filenames to a local text file, invokes Lambda
- E. EventBridge monitors S3 uploads, sends an email alert via SNS for manual processing

**Reason:** S3 → SQS → Lambda (with delete-on-success) is a fully durable, stateless, event-driven pattern — C keeps state in ephemeral Lambda storage, D introduces a stateful EC2 component, and E is a manual-alert workflow, not automated processing.

---

## Q140
**Full Question:** A company is designing an application. The application uses an AWS Lambda function to receive information through Amazon API Gateway and to store the information in an Amazon Aurora PostgreSQL database. During the proof of concept stage, the company has to increase the Lambda quotas significantly to handle the high volumes of data that the company needs to load into the database. A solutions architect must recommend a new design to improve scalability and minimize the configuration effort. Which solution will meet these requirements?

**Short Question:** One Lambda doing both "receive" and "write to Aurora" is hitting quota limits under high volume — redesign for scalability, minimal config effort.
- A. Refactor into Apache Tomcat on EC2, connect via JDBC
- B. Switch database from Aurora to DynamoDB + DAX cluster
- C. Two Lambda functions (receive / load), integrated via an SNS topic
- D. Two Lambda functions (receive / load), integrated via an SQS queue ✅

**Reason:** Splitting into two Lambdas connected by an SQS queue decouples ingestion from database writes and buffers load without losing messages — SNS (C) doesn't buffer reliably, and A/B both require major rearchitecting.

---

## Q141
**Full Question:** A company wants to run an in-memory database for a latency-sensitive application that runs on Amazon EC2 instances. The application processes more than 100,000 transactions each minute and requires high network throughput. A solutions architect needs to provide a cost-effective network design that minimizes data transfer charges. Which solution meets these requirements?

**Short Question:** In-memory DB app needs ultra-low latency + very high network throughput between EC2 instances, cheapest network design.
- A. All EC2 instances in one AZ, cluster placement group ✅
- B. EC2 instances across multiple AZs, partition placement group
- C. Auto Scaling group across multiple AZs, scaling on network utilization
- D. Auto Scaling group across multiple AZs, step scaling policy

**Reason:** A cluster placement group within a single AZ packs instances physically close together for the lowest latency/highest throughput, and same-AZ traffic is free — multi-AZ options (B/C/D) add latency and don't optimize physical placement.

---

## Q142
**Full Question:** A company is developing a real-time multiplayer game that uses UDP for communications between the client and servers in an Auto Scaling group. Spikes in demand are anticipated during the day, so the game server platform must adapt accordingly. Developers want to store gamer scores and other non-relational data in a database solution that will scale without intervention. Which solution should a solutions architect recommend?

**Short Question:** Real-time UDP multiplayer game with unpredictable spikes, needs a non-relational DB that auto-scales without manual work.
- A. Route 53 for traffic distribution + Aurora Serverless
- B. Network Load Balancer + DynamoDB on-demand ✅
- C. Network Load Balancer + Aurora Global Database
- D. Application Load Balancer + DynamoDB Global Tables

**Reason:** Only an NLB supports UDP (ruling out A's Route 53 and D's ALB), and DynamoDB (not the relational Aurora in A/C) fits "non-relational data that scales without intervention" via on-demand capacity.

---

## Q143
**Full Question:** A company runs an application on a group of Amazon Linux EC2 instances. For compliance reasons, the company must retain all application log files for 7 years. The log files will be analyzed by a reporting tool that must be able to access all the files concurrently. Which storage solution meets these requirements most cost effectively?

**Short Question:** Retain application logs for 7 years, with concurrent multi-file access for a reporting tool, cheapest option.
- A. Amazon EBS
- B. Amazon EFS
- C. Amazon EC2 instance store
- D. Amazon S3 ✅

**Reason:** S3 is durable, scales without limit, supports concurrent API access from any tool, and (especially with Glacier tiers) is by far the cheapest for 7-year retention — EBS/instance store aren't shareable/durable enough, and EFS is priced much higher for this use case.

---

## Q144
**Full Question:** A company hosts its static website by using Amazon S3. The company wants to add a contact form to its web page. The contact form will have dynamic server-side components for users to input their name, email address, phone number, and user message. The company anticipates that there will be fewer than 100 site visits each month. Which solution will meet these requirements most cost effectively?

**Short Question:** Add a dynamic server-side contact form to a static S3 site, with under 100 visits/month — cheapest option.
- A. ECS-hosted form page + SES
- B. API Gateway + Lambda backend calling SES ✅
- C. Amazon Lightsail + client-side scripting + Amazon WorkMail
- D. EC2 t2.micro running a LAMP stack + client-side scripting + Amazon WorkMail

**Reason:** With traffic this low, a serverless API Gateway + Lambda + SES stack likely stays entirely within the free tier and only charges per actual request — ECS/Lightsail/EC2 all incur a fixed cost for a server running 24/7 regardless of near-zero traffic.

---

## Q145
**Full Question:** A company wants to move its application to a serverless solution. The serverless solution needs to analyze existing and new data by using SQL. The company stores the data in an Amazon S3 bucket. The data requires encryption and must be replicated to a different AWS region. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Serverless SQL analysis of S3 data, must be encrypted and cross-region replicated, least operational overhead.
- A. New bucket + migrate data + SSE-KMS multi-region keys + cross-region replication + Athena
- B. New bucket + migrate data + SSE-KMS multi-region keys + cross-region replication + Amazon RDS
- C. Existing bucket + SSE-S3 + cross-region replication + Athena ✅
- D. Existing bucket + SSE-S3 + cross-region replication + Amazon RDS

**Reason:** Using the existing bucket avoids unnecessary data migration, SSE-S3 needs zero key management (vs. self-managed KMS keys in A/B), and Athena is the serverless SQL-on-S3 query engine — RDS (B/D) would require standing up and loading a database, which isn't serverless or low-overhead.

---

## Q146
**Full Question:** A company currently stores 5 terabytes of data in on-premises block storage systems. The company's current storage solution provides limited space for additional data. The company runs applications on premises that must be able to retrieve frequently accessed data with low latency. The company requires a cloud-based storage solution. Which solution will meet these requirements with the most operational efficiency?

**Short Question:** Extend limited on-prem block storage to the cloud, keep low-latency access to hot data, without changing the block-storage interface.
- A. S3 File Gateway with SMB (wrong protocol — apps use block storage)
- B. Storage Gateway Volume Gateway, cached volumes, iSCSI targets ✅
- C. Storage Gateway Volume Gateway, stored volumes, iSCSI targets (keeps full dataset on-prem — doesn't free up space)
- D. Storage Gateway Tape Gateway (archival/backup use case, not active data)

**Reason:** Cached volumes keep the full dataset in S3 (solving the space problem) while caching hot data locally for low latency over iSCSI (matching the existing block-storage apps) — stored volumes (C) keeps everything on-prem, File Gateway (A) is file-level not block-level, and Tape Gateway (D) is for backups.

---

## Q147
**Full Question:** A solutions architect is creating a new Amazon CloudFront distribution for an application. Some of the information submitted by users is sensitive. The application uses HTTPS but needs another layer of security. The sensitive information should be protected throughout the entire application stack and access to the information should be restricted to certain applications. Which action should the solutions architect take?

**Short Question:** Add a layer of security beyond HTTPS: protect specific sensitive form fields end-to-end, decryptable only by authorized backend components.
- A. CloudFront signed URL
- B. CloudFront signed cookie
- C. CloudFront field-level encryption profile ✅
- D. Set the CloudFront viewer/origin protocol policy to HTTPS-only (already in place)

**Reason:** Field-level encryption encrypts specific form fields at the edge with a public key so only backends holding the matching private key can decrypt them, protecting the data end-to-end even past TLS termination — signed URLs/cookies control content access (not data encryption), and D is already satisfied by "the application uses HTTPS."

---

## Q148
**Full Question:** A medical research lab produces data that is related to a new study. The lab wants to make the data available with minimum latency to clinics across the country for their on-premises file-based applications. The data files are stored in an Amazon S3 bucket that has read-only permissions for each clinic. What should a solutions architect recommend to meet these requirements?

**Short Question:** Give many on-prem clinics low-latency, file-based, read-only access to data centrally stored in S3.
- A. AWS Storage Gateway File Gateway VM at each clinic ✅
- B. Migrate/copy the files to each clinic's on-prem apps via DataSync
- C. AWS Storage Gateway Volume Gateway VM at each clinic (block-level, not file-level)
- D. Attach an EFS file system to each clinic's on-prem servers directly

**Reason:** File Gateway gives a standard NFS/SMB interface backed by S3 with local caching of hot files, directly delivering low-latency file access at each clinic — DataSync (B) just makes separate copies to manage, Volume Gateway (C) is block-level (wrong interface), and EFS accessed over the internet/VPN (D) doesn't provide local low-latency caching.

---

## Q149
**Full Question:** A company runs an application on a large fleet of Amazon EC2 instances. The application reads and writes entries into an Amazon DynamoDB table. The size of the DynamoDB table continuously grows, but the application needs only data from the last 30 days. The company needs a solution that minimizes cost and development effort. Which solution meets these requirements?

**Short Question:** DynamoDB table keeps growing but only the last 30 days of data is ever needed — clean up old items, cheapest, least dev effort.
- A. Redeploy the entire CloudFormation stack every 30 days, delete the old stack
- B. EC2 instance running a monitoring app + custom script that scans and deletes items older than 30 days
- C. DynamoDB Streams invokes a Lambda on item creation, Lambda deletes items older than 30 days
- D. Add an expiration-timestamp attribute to each item + configure DynamoDB TTL on that attribute ✅

**Reason:** DynamoDB TTL auto-deletes expired items at zero write-capacity cost with a one-line app change — B/C both require costly table scans to find old items, and A causes downtime and data loss.

---

## Q150
**Full Question:** A company is building an application in the AWS cloud. The application will store data in Amazon S3 buckets in two AWS regions. The company must use an AWS Key Management Service (AWS KMS) customer managed key to encrypt all data that is stored in the S3 buckets. The data in both S3 buckets must be encrypted and decrypted with the same KMS key. The data and the key must be stored in each of the two regions. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Encrypt S3 data in two regions with the exact same customer-managed KMS key (same key ID/material in both regions).
- A. S3 buckets in both regions + SSE-S3 + replication (SSE-S3 isn't customer-managed)
- B. Customer-managed multi-region KMS key + S3 buckets in both regions + replication + client-side encryption using the key ✅
- C. Customer-managed KMS key + S3 buckets + SSE-S3 + replication (still SSE-S3, not customer-managed)
- D. A separate customer-managed KMS key created "in each region" + SSE-KMS + replication (two distinct keys — violates "same key")

**Reason:** A KMS multi-region key is the specific feature that gives an identical key ID/material usable across regions — SSE-S3 (A/C) never uses customer-managed keys, and creating a key "in each region" (D) produces two different keys, not one shared key.
