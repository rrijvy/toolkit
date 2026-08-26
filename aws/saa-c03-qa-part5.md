# AWS SAA-C03 Real Exam Questions & Answers — Part 5 (Q101–Q125)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q101
**Full Question:** A company is building an e-commerce web application on AWS. The application sends information about new orders to an Amazon API Gateway REST API to process. The company wants to ensure that orders are processed in the order that they are received. Which solution will meet these requirements?

**Short Question:** New-order events sent to API Gateway must be processed in the exact order they were received.
- A. API Gateway → SNS topic → Lambda subscriber
- B. API Gateway → SQS FIFO queue → Lambda for processing ✅
- C. API Gateway authorizer blocks requests while an order is processed
- D. API Gateway → SQS standard queue → Lambda for processing

**Reason:** Only an SQS FIFO queue guarantees strict message ordering — SNS and standard SQS don't guarantee order, and an authorizer isn't a processing/ordering mechanism.

---

## Q102
**Full Question:** A company is designing a shared storage solution for a gaming application that is hosted in the AWS cloud. The company needs the ability to use SMB clients to access data. The solution must be fully managed. Which AWS solution meets these requirements?

**Short Question:** Fully managed shared storage for a gaming app, accessible via SMB clients.
- A. AWS DataSync task shared as a mountable file system
- B. EC2 Windows instance with a Windows file share role installed
- C. Amazon FSx for Windows File Server ✅
- D. S3 bucket mounted to the application server

**Reason:** FSx for Windows File Server natively speaks SMB and is fully managed by AWS — DataSync is a transfer tool, EC2 requires self-management, and S3 isn't an SMB file system.

---

## Q103
**Full Question:** A solutions architect needs to design a system to store client case files. The files are core company assets and are important. The number of files will grow over time. The files must be simultaneously accessible from multiple application servers that run on Amazon EC2 instances. The solution must have built-in redundancy. Which solution meets these requirements?

**Short Question:** Growing repository of important files, needs simultaneous multi-instance access, built-in redundancy.
- A. Amazon EFS ✅
- B. Amazon EBS
- C. S3 Glacier Deep Archive
- D. AWS Backup

**Reason:** EFS is a shared, multi-AZ-redundant file system that many EC2 instances can mount at once and that scales automatically — EBS attaches to only one instance, Glacier is archival (slow), and Backup isn't a primary storage service.

---

## Q104
**Full Question:** A company has a Microsoft .NET application that runs on an on-premises Windows server. The application stores data by using an Oracle database standard edition server. The company is planning a migration to AWS and wants to minimize development changes while moving the application. The AWS application environment should be highly available. Which combination of actions should the company take to meet these requirements? (Choose 2)

**Short Question:** Lift-and-shift a .NET app + Oracle DB to AWS with minimal code changes, highly available.
- A. Refactor the app as serverless Lambda functions running .NET Core
- B. Rehost the app on Elastic Beanstalk (.NET platform), Multi-AZ ✅
- C. Replatform the app onto EC2 with an Amazon Linux AMI
- D. DMS: Oracle → DynamoDB, Multi-AZ
- E. DMS: Oracle → Oracle on Amazon RDS, Multi-AZ ✅

**Reason:** Elastic Beanstalk (.NET) rehosts the app almost unchanged with built-in HA, and a homogeneous Oracle-to-Oracle-on-RDS migration via DMS moves the database with minimal changes — refactoring, switching OS, or switching database engines all require significant rework.

---

## Q105
**Full Question:** A company has a service that produces event data. The company wants to use AWS to process the event data as it is received. The data is written in a specific order that must be maintained throughout processing. The company wants to implement a solution that minimizes operational overhead. How should a solutions architect accomplish this?

**Short Question:** Process incoming events in AWS while strictly preserving their write order, least operational overhead.
- A. SQS FIFO queue + Lambda to process messages ✅
- B. SNS topic + Lambda subscriber
- C. SQS standard queue + Lambda processing independently
- D. SNS topic + SQS queue subscriber

**Reason:** SQS FIFO is the only option here that guarantees strict message order — SNS and standard SQS don't guarantee ordering.

---

## Q106
**Full Question:** A company has an e-commerce company hosts its analytics application in the AWS cloud. The application generates about 300 megabytes of data each month. The data is stored in JSON format. The company is evaluating a disaster recovery solution to back up the data. The data must be accessible in milliseconds if it is needed and the data must be kept for 30 days. Which solution meets these requirements most cost effectively?

**Short Question:** Back up ~300MB/month of JSON data for exactly 30 days, must be retrievable in milliseconds, cheapest option.
- A. Amazon OpenSearch Service
- B. Amazon S3 Glacier
- C. Amazon S3 Standard ✅
- D. Amazon RDS for PostgreSQL

**Reason:** S3 Standard gives millisecond access with no retrieval fees, which fits a small volume and a short 30-day retention window — Glacier has a 90-day minimum storage charge, and OpenSearch/RDS are the wrong tool entirely for simple backup storage.

---

## Q107
**Full Question:** A company tracks customer satisfaction by using surveys that the company hosts on its website. The surveys sometimes reach thousands of customers every hour. Survey results are currently sent in email messages to the company so company employees can manually review results and assess customer sentiment. The company wants to automate the customer survey process. Survey results must be available for the previous 12 months. Which solution will meet these requirements in the most scalable way?

**Short Question:** Automate a survey pipeline: run sentiment analysis at thousands/hour, keep results for 12 months, most scalable.
- A. API Gateway → SQS → Lambda (pulls queue) → Amazon Comprehend (sentiment) → DynamoDB, TTL = 365 days ✅
- B. API on EC2 → DynamoDB → Comprehend → second DynamoDB table, TTL = 365 days
- C. S3 → S3 event → Lambda → Amazon Rekognition (sentiment) → second S3 bucket, lifecycle expiry 365 days
- D. API Gateway → SQS → Lambda → Amazon Lex (sentiment) → DynamoDB, TTL = 365 days

**Reason:** API Gateway + SQS + Lambda is a fully scalable serverless pipeline, Comprehend is the correct text-sentiment-analysis service (Rekognition is for images/video, Lex is for chatbots), and DynamoDB TTL auto-expires records after 365 days with zero overhead.

---

## Q108
**Full Question:** A company has a dynamic web application hosted on two Amazon EC2 instances. The company has its own SSL certificate which is on each instance to perform SSL termination. There has been an increase in traffic recently and the operations team determined that SSL encryption and decryption is causing the compute capacity of the web servers to reach their maximum limit. What should a solutions architect do to increase the application's performance?

**Short Question:** SSL termination on the EC2 instances themselves is maxing out their CPU — offload it to boost performance.
- A. New ACM certificate installed on each instance (still terminates on the instances)
- B. Store the SSL certificate in an S3 bucket, reference it from EC2
- C. New proxy EC2 instance to handle SSL termination
- D. Import the certificate into ACM + ALB with an HTTPS listener using it ✅

**Reason:** Terminating SSL/TLS at the ALB (backed by ACM) offloads the CPU-intensive encryption work from the EC2 instances entirely — the other options either keep termination on the instances or add unmanaged infrastructure.

---

## Q109
**Full Question:** A company stores data in an Amazon Aurora PostgreSQL DB cluster. The company must store all the data for 5 years and must delete all the data after 5 years. The company also must indefinitely keep audit logs of actions that are performed within the database. Currently, the company has automated backups configured for Aurora. Which combination of steps should a solutions architect take to meet these requirements? (Choose 2)

**Short Question:** Aurora data must be kept exactly 5 years then deleted; database audit logs must be kept forever.
- A. Take a manual snapshot of the DB cluster
- B. Create a "lifecycle policy" for the automated backups (not how Aurora backup retention works)
- C. Configure automated backup retention for 5 years (partial — doesn't cover audit logs)
- D. Configure a CloudWatch Logs export for the DB cluster (route audit logs to long-term storage) ✅
- E. Use AWS Backup to take backups and retain them for exactly 5 years ✅

**Reason:** AWS Backup manages the 5-year data retention/deletion cycle, and exporting logs to CloudWatch Logs (and from there to indefinite storage like S3) handles the separate indefinite audit-log requirement — one option alone can't satisfy both requirements.

---

## Q110
**Full Question:** A company is reviewing a recent migration of a three-tier application to a VPC. The security team discovers that the principle of least privilege is not being applied to Amazon EC2 security group ingress and egress rules between the application tiers. What should a solutions architect do to correct this issue?

**Short Question:** Security group rules between app tiers are too broad — tighten them to least privilege.
- A. Rules using instance IDs as source/destination
- B. Rules using the security group ID as source/destination ✅
- C. Rules using the VPC CIDR block as source/destination
- D. Rules using subnet CIDR blocks as source/destination

**Reason:** Referencing another security group ID scopes access to exactly the instances in that tier — instance IDs aren't valid rule sources, and VPC/subnet CIDRs are far too broad.

---

## Q111
**Full Question:** A development team has launched a new application that is hosted on Amazon EC2 instances inside a development VPC. A solutions architect needs to create a new VPC in the same account. The new VPC will be peered with the development VPC. The VPC CIDR block for the development VPC is 192.168.0.0/24. The solutions architect needs to create a CIDR block for the new VPC. The CIDR block must be valid for a VPC peering connection to the development VPC. What is the smallest CIDR block that meets these requirements?

**Short Question:** Pick the smallest valid, non-overlapping CIDR block for a new VPC to peer with an existing 192.168.0.0/24 VPC.
- A. 10.0.1.0/32
- B. 192.168.0.0/24
- C. 192.168.1.0/32
- D. 10.0.1.0/24 ✅

**Reason:** /32 blocks are invalid for a VPC (valid range is /16–/28); 192.168.0.0/24 directly overlaps the existing VPC (peering forbids overlap); 10.0.1.0/24 doesn't overlap and is a valid VPC CIDR size.

---

## Q112
**Full Question:** An application running on an Amazon EC2 instance in VPC A needs to access files in another EC2 instance in VPC B. Both VPCs are in separate AWS accounts. The network administrator needs to design a solution to configure secure access to the EC2 instance in VPC B from VPC A. The connectivity should not have a single point of failure or bandwidth concerns. Which solution will meet these requirements?

**Short Question:** Securely connect EC2-to-EC2 across two VPCs in different accounts, no single point of failure, no bandwidth limits.
- A. VPC peering connection between VPC A and VPC B ✅
- B. VPC gateway endpoints for the EC2 instance in VPC B
- C. Virtual private gateway attached to VPC B
- D. Private VIF for the EC2 instance in VPC B

**Reason:** VPC peering is a direct private network link with no single point of failure or bandwidth cap — gateway endpoints are for AWS services (not EC2-to-EC2), and virtual private gateways/VIFs are for on-prem-to-AWS connections (Direct Connect/VPN), not VPC-to-VPC.

---

## Q113
**Full Question:** A medical company wants to perform transformations on a large amount of clinical trial data that comes from several customers. The company must extract the data from a relational database that contains the customer data. Then the company will transform the data by using a series of complex rules. The company will load the data to Amazon S3 when the transformations are complete. All data must be encrypted where it is processed before the company stores the data in Amazon S3. All data must be encrypted by using customer-specific keys. Which solution will meet these requirements with the least amount of operational effort?

**Short Question:** ETL clinical data per customer, encrypt during processing using each customer's own key, before it lands in S3.
- A. One Glue job per customer, security config using SSE-S3
- B. One EMR cluster per customer, client-side encryption with a custom root key
- C. One Glue job per customer, security config using client-side encryption with KMS-managed keys ✅
- D. One EMR cluster per customer, server-side encryption with KMS keys

**Reason:** Glue is fully managed/serverless (least operational effort), and its security configuration supports client-side encryption with customer-specific KMS keys — SSE-S3 uses AWS-managed keys (not customer-specific), and EMR requires managing clusters.

---

## Q114
**Full Question:** A company's compliance team needs to move its file shares to AWS. The shares run on a Windows Server SMB file share. A self-managed on-premises Active Directory controls access to the files and folders. The company wants to use Amazon FSx for Windows File Server as part of the solution. The company must ensure that the on-premises Active Directory groups restrict access to the FSx for Windows File Server SMB compliance shares, folders, and files. After the move to AWS, the company has created an FSx for Windows File Server file system. Which solution will meet these requirements?

**Short Question:** New FSx for Windows File Server must enforce access using the existing on-prem Active Directory groups.
- A. AD Connector + map AD groups to IAM groups
- B. Tag the file system + map AD groups to IAM groups
- C. IAM service-linked role linked to FSx
- D. Join the file system to the Active Directory ✅

**Reason:** Joining FSx to the Active Directory lets it natively use existing AD group permissions to control share/folder/file access — IAM groups and tags don't control SMB file-level permissions, and a service-linked role isn't for end-user access control.

---

## Q115
**Full Question:** A company needs to run a critical application on AWS. The company needs to use Amazon EC2 for the application's database. The database must be highly available and must fail over automatically if a disruptive event occurs. Which solution will meet these requirements?

**Short Question:** Self-managed database on EC2 needs high availability and automatic failover.
- A. Two EC2 instances in different AZs, database installed on both, clustered with replication ✅
- B. Single EC2 instance + AMI backup + CloudFormation to reprovision on failure
- C. Two EC2 instances in different regions, replication, manual failover to the second region
- D. Single EC2 instance + AMI backup + EC2 automatic recovery

**Reason:** A clustered, replicated database across two AZs is the standard way to get automatic failover for a self-managed database — a single instance (B/D) is a single point of failure, and cross-region (C) is slower/more complex than needed for AZ-level HA.

---

## Q116
**Full Question:** A company runs an e-commerce application on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. The Auto Scaling group scales based on CPU utilization metrics. The e-commerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance. The database's performance degrades quickly as application load increases. The application handles more read requests than write transactions. The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability. Which solution will meet these requirements?

**Short Question:** Self-managed MySQL on one EC2 instance degrades under load; app is read-heavy — auto-scale reads, stay highly available.
- A. Amazon Redshift, single node
- B. RDS, single-AZ, add reader instances
- C. Aurora with Multi-AZ deployment + Aurora Auto Scaling with Aurora Replicas ✅
- D. ElastiCache with EC2 spot instances

**Reason:** Aurora Replicas offload read traffic and Aurora Auto Scaling adds/removes them automatically based on demand, while Multi-AZ keeps it highly available — Redshift is for analytics (not OLTP), single-AZ RDS isn't HA, and ElastiCache is a cache, not a primary database.

---

## Q117
**Full Question:** A company has an application that runs on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster on Amazon EC2 instances. The application has a UI that uses Amazon DynamoDB and data services that use Amazon S3 as part of the application deployment. The company must ensure that the EKS pods for the UI can access only Amazon DynamoDB and that the EKS pods for the data services can access only Amazon S3. The company uses AWS Identity and Access Management (IAM). Which solution meets these requirements?

**Short Question:** UI pods must access only DynamoDB; data-service pods must access only S3 — enforce this per-pod in EKS.
- A. Both S3 and DynamoDB IAM policies attached to the shared EC2 instance profile, controlled via Kubernetes RBAC
- B. IAM policies attached directly to the EKS pods
- C. Separate Kubernetes service accounts per component, each assuming its own IAM role (S3 role → data services, DynamoDB role → UI) ✅
- D. Separate Kubernetes service accounts, IRSA — but with the S3/DynamoDB access reversed (UI→S3, data services→DynamoDB)

**Reason:** IAM Roles for Service Accounts (IRSA) gives each pod's service account its own scoped IAM role — an instance profile (A) gives every pod on the node the same permissions, IAM policies can't attach directly to pods (B), and D assigns the wrong service to each tier.

---

## Q118
**Full Question:** A company uses Amazon S3 as its data lake. The company has a new partner that must use SFTP to upload data files. A solutions architect needs to implement a highly available SFTP solution that minimizes operational overhead. Which solution will meet these requirements?

**Short Question:** Give a partner a highly available SFTP endpoint that writes straight into the S3 data lake, least overhead.
- A. AWS Transfer Family with an SFTP-enabled server, publicly accessible, S3 as the destination ✅
- B. S3 File Gateway exposed as an "SFTP server" (it's actually SMB/NFS)
- C. EC2 instance in a private subnet + VPN + cron job to push files to S3
- D. EC2 instances behind an NLB with an SFTP listener + cron job to push files to S3

**Reason:** AWS Transfer Family is the purpose-built, fully managed SFTP-to-S3 service — S3 File Gateway doesn't speak SFTP, and the EC2-based options (C/D) require self-managing servers, which isn't low-overhead or inherently highly available.

---

## Q119
**Full Question:** A company wants to experiment with individual AWS accounts for its engineer team. The company wants to be notified as soon as the Amazon EC2 instance usage for a given month exceeds a specific threshold for each account. What should a solutions architect do to meet this requirement most cost-effectively?

**Short Question:** Get notified per-account as soon as monthly EC2 spend crosses a threshold, cheapest way.
- A. Cost Explorer daily report + SES notification
- B. Cost Explorer monthly report + SES notification
- C. AWS Budgets: monthly cost budget scoped to EC2 per account, alert threshold → SNS topic ✅
- D. Cost and Usage Reports (hourly) + Athena + EventBridge schedule + SNS

**Reason:** AWS Budgets is purpose-built for threshold alerting (via SNS) and is free — Cost Explorer has no native alerting feature, and the CUR/Athena/EventBridge pipeline (D) is far more complex and costly than needed.

---

## Q120
**Full Question:** A company stores its application logs in an Amazon CloudWatch Logs log group. A new policy requires the company to store all application logs in Amazon OpenSearch Service (Amazon Elasticsearch Service) in near real time. Which solution will meet this requirement with the least operational overhead?

**Short Question:** Stream CloudWatch Logs into OpenSearch Service in near real time, least operational overhead.
- A. CloudWatch Logs subscription filter streaming directly to OpenSearch Service ✅
- B. Lambda function invoked by the log group, writes to OpenSearch Service
- C. Kinesis Data Firehose delivery stream (source: log group, destination: OpenSearch)
- D. Kinesis agent on every app server → Kinesis Data Streams → OpenSearch Service

**Reason:** CloudWatch Logs has a native, no-code subscription filter that streams straight to OpenSearch — the other options add a Lambda function, a Firehose stream, or agents on every server, all unnecessary extra pieces to manage.

---

## Q121
**Full Question:** An application runs on an Amazon EC2 instance that has an Elastic IP address in VPC A. The application requires access to a database in VPC B. Both VPCs are in the same AWS account. Which solution will provide the required access most securely?

**Short Question:** EC2 app in VPC A needs access to a database in VPC B (same account), most securely.
- A. DB security group allowing traffic from the app server's public IP
- B. VPC peering connection between VPC A and VPC B ✅
- C. Make the DB instance publicly accessible with a public IP
- D. Proxy EC2 instance with an Elastic IP in VPC B

**Reason:** VPC peering creates a private, direct link so traffic never touches the public internet — the other options route traffic publicly or expose the database directly, both security risks.

---

## Q122
**Full Question:** A solutions architect is implementing a document review application using an Amazon S3 bucket for storage. The solution must prevent accidental deletion of the documents and ensure that all versions of the documents are available. Users must be able to download, modify, and upload documents. Which combination of actions should be taken to meet these requirements? (Choose 2)

**Short Question:** S3-backed document app: keep every version, prevent accidental deletes, but users must still be able to upload/modify freely.
- A. Enable a read-only bucket ACL (blocks the required upload/modify)
- B. Enable versioning on the bucket ✅
- C. Attach an IAM policy to the bucket (controls access, not deletion/versioning)
- D. Enable MFA Delete on the bucket ✅
- E. Encrypt the bucket with AWS KMS

**Reason:** Versioning preserves every prior version of a document, and MFA Delete requires a second factor to permanently delete a version — together they meet both requirements without blocking normal uploads/edits (unlike a read-only ACL).

---

## Q123
**Full Question:** A development team runs monthly resource-intensive tests on its general purpose Amazon RDS for MySQL DB instance with Performance Insights enabled. The testing lasts for 48 hours once a month and is the only process that uses the database. The team wants to reduce the cost of running the tests without reducing the compute and memory attributes of the DB instance. Which solution meets these requirements most cost effectively?

**Short Question:** RDS instance used only 48 hours/month for testing — cut cost for the other ~29 idle days, without downgrading its specs.
- A. Stop the DB instance when not in use, restart before tests
- B. Auto Scaling policy on the DB instance
- C. Snapshot after tests, terminate the instance, restore from snapshot when needed again ✅
- D. Downsize the instance class when idle, resize back up before tests

**Reason:** A snapshot only incurs (cheap) storage cost while the instance doesn't exist at all — stopping (A) still bills for provisioned storage, RDS has no such auto-scaling feature (B), and resizing (D) still keeps a running instance billed the whole time.

---

## Q124
**Full Question:** A company provides an API to its users that automates inquiries for tax computations based on item prices. The company experiences a larger number of inquiries during the holiday season only that cause slower response times. A solutions architect needs to design a solution that is scalable and elastic. What should the solutions architect do to accomplish this?

**Short Question:** Tax-computation API only spikes during the holiday season — needs to scale elastically without over-provisioning year-round.
- A. API hosted directly on one EC2 instance
- B. API Gateway (REST API) → Lambda for the tax computation ✅
- C. ALB with two fixed EC2 instances behind it
- D. API Gateway → single EC2 instance backend

**Reason:** API Gateway + Lambda scales automatically from zero to thousands of concurrent executions with no server management — a single instance (A/D) or a fixed pair (C) becomes the bottleneck during the spike.

---

## Q125
**Full Question:** A rapidly growing e-commerce company is running its workloads in a single AWS region. A solutions architect must create a disaster recovery (DR) strategy that includes a different AWS region. The company wants its database to be up-to-date in the DR region with the least possible latency. The remaining infrastructure in the DR region needs to run at reduced capacity and must be able to scale up if necessary. Which solution will meet these requirements with the lowest recovery time objective (RTO)?

**Short Question:** Cross-region DR: database must stay low-latency up-to-date; the rest of the stack runs at reduced capacity in DR, ready to scale up — lowest RTO.
- A. Aurora Global Database + pilot light deployment
- B. Aurora Global Database + warm standby deployment ✅
- C. RDS Multi-AZ + pilot light deployment
- D. RDS Multi-AZ + warm standby deployment

**Reason:** Aurora Global Database is the low-latency cross-region replication solution (RDS Multi-AZ is single-region only, ruling out C/D), and warm standby (already running at reduced capacity, just needs scaling up) gives a faster RTO than pilot light (core resources only, not a running app).
