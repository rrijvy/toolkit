# AWS SAA-C03 Real Exam Questions & Answers — Part 1 (Q1–Q25)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q1
**Full Question:** A bicycle sharing company is developing a multi-tier architecture to track the location of its bicycles during peak operating hours. The company wants to use these data points in its existing analytics platform. A solutions architect must determine the most viable multi-tier option to support this architecture. The data points must be accessible from the REST API. Which action meets these requirements for storing and retrieving location data?

**Short Question:** Store/retrieve real-time bike location data via REST API for an analytics platform.
- A. Athena + S3
- B. API Gateway + Lambda
- C. QuickSight + Redshift
- D. API Gateway + Kinesis Data Analytics ✅

**Reason:** Handles high-volume real-time streaming ingestion.

---

## Q2
**Full Question:** A solutions architect needs to implement a solution to reduce a company's storage costs. All the company's data is in the Amazon S3 Standard storage class. The company must keep all data for at least 25 years. Data from the most recent 2 years must be highly available and immediately retrievable. Which solution will meet these requirements?

**Short Question:** Keep S3 data 25 years; last 2 years must stay highly available and instant.
- A. Move to Glacier Deep Archive immediately
- B. Move to Glacier Deep Archive after 2 years ✅
- C. Intelligent-Tiering with archive option
- D. Move to One Zone-IA immediately, then Glacier Deep Archive after 2 years

**Reason:** Lifecycle policy — Standard for 2 years, then Deep Archive.

---

## Q3
**Full Question:** A company operates a food delivery service. Because of recent growth, the company's order processing system is experiencing scaling problems during peak traffic hours. The current architecture includes Amazon EC2 instances in an Auto Scaling group that collect orders from an application. A second group of EC2 instances in an Auto Scaling group fulfills the orders. The order collection process occurs quickly, but the order fulfillment process can take longer. Data must not be lost because of a scaling event. A solutions architect must ensure that the order collection process and the order fulfillment process can both scale adequately during peak traffic hours. Which solution will meet these requirements?

**Short Question:** Order collection (fast) and order fulfillment (slow) each need independent scaling, no data loss.
- A. Set ASG min capacity to peak value
- B. CloudWatch alarm → SNS → create new ASGs
- C. Two SQS queues, scale ASG on queue notifications
- D. Two SQS queues, scale ASG on number of messages in queue ✅

**Reason:** Decouple with SQS, scale based on queue message count.

---

## Q4
**Full Question:** A company produces batch data that comes from different databases. The company also produces live stream data from network sensors and application APIs. The company needs to consolidate all the data into one place for business analytics. The company needs to process the incoming data and then stage the data in different Amazon S3 buckets. Teams will later run one-time queries and import the data into a business intelligence tool to show key performance indicators (KPIs). Which combination of steps will meet these requirements with the least operational overhead? (Choose 2)

**Short Question:** Consolidate batch + streaming data into S3 for one-time queries and BI dashboards, least overhead.
- A. Athena + QuickSight ✅
- B. Kinesis Data Analytics + QuickSight
- C. Lambda functions to move records to Redshift
- D. Glue ETL → JSON → OpenSearch clusters
- E. Lake Formation blueprints + Glue crawler → S3 in Parquet ✅

**Reason:** Glue/Lake Formation builds the data lake; Athena/QuickSight query and visualize it.

---

## Q5
**Full Question:** A company performs monthly maintenance on its AWS infrastructure. During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Rotate RDS MySQL credentials across multiple regions, least overhead.
- A. Secrets Manager + multi-region replication + scheduled rotation ✅
- B. Systems Manager Parameter Store + replication + rotation
- C. S3 + EventBridge + Lambda to rotate
- D. KMS multi-region keys + DynamoDB global table + Lambda + RDS API

**Reason:** Secrets Manager natively supports rotation and multi-region replication.

---

## Q6
**Full Question:** A company hosts an application in a private subnet. The company has already integrated the application with Amazon Cognito. The company uses an Amazon Cognito user pool to authenticate users. The company needs to modify the application so the application can securely store user documents in an Amazon S3 bucket. Which combination of steps will securely integrate Amazon S3 with the application? (Choose 2)

**Short Question:** App in a private subnet uses Cognito user pool for login; needs secure access to S3.
- A. Create a Cognito identity pool for temporary S3 credentials ✅
- B. Use the user pool itself to generate S3 tokens
- C. Create an S3 VPC endpoint in the same VPC ✅
- D. Create a NAT gateway + deny S3 requests not from Cognito
- E. S3 policy allowing only user IP addresses

**Reason:** Identity pool grants temp AWS credentials; VPC endpoint gives private S3 access.

---

## Q7
**Full Question:** A company has an automobile sales website that stores its listings in a database on Amazon RDS. When an automobile is sold, the listing needs to be removed from the website and the data must be sent to multiple target systems. Which design should a solutions architect recommend?

**Short Question:** Automobile listing removed from RDS on sale; data must go to multiple target systems.
- A. Lambda (RDS trigger) → SQS queue
- B. Lambda (RDS trigger) → SQS FIFO queue
- C. RDS event → SQS → fan out to multiple SNS topics
- D. RDS event → SNS topic → fan out to multiple SQS queues → Lambda updates targets ✅

**Reason:** Correct fan-out pattern — one SNS topic to many SQS queues.

---

## Q8
**Full Question:** A company has a small Python application that processes JSON documents and outputs the results to an on-premises SQL database. The application runs thousands of times each day. The company wants to move the application to the AWS Cloud. The company needs a highly available solution that maximizes scalability and minimizes operational overhead. Which solution will meet these requirements?

**Short Question:** Python app processes JSON → on-prem SQL DB, runs thousands of times/day. Needs HA, max scalability, min overhead.
- A. S3 + EC2 (multiple) + Aurora
- B. S3 + Lambda (triggered on upload) + Aurora ✅
- C. EBS multi-attach + EC2 + RDS
- D. SQS + ECS on EC2 launch type + RDS

**Reason:** S3 event triggers serverless Lambda; Aurora is fully managed.

---

## Q9
**Full Question:** A company has an AWS Glue extract, transform, and load (ETL) job that runs every day at the same time. The job processes XML data that is in an Amazon S3 bucket. New data is added to the S3 bucket every day. A solutions architect notices that AWS Glue is processing all the data during each run. What should the solutions architect do to prevent AWS Glue from reprocessing old data?

**Short Question:** AWS Glue ETL job reprocesses all data every run instead of just new data.
- A. Use job bookmarks ✅
- B. Delete data after processing
- C. Set number of workers to 1
- D. Use FindMatches ML transform

**Reason:** Job bookmarks track already-processed data.

---

## Q10
**Full Question:** A company has created an image analysis application in which users can upload photos and add photo frames to their images. The users upload images and metadata to indicate which photo frames they want to add to their images. The application uses a single Amazon EC2 instance and Amazon DynamoDB to store the metadata. The application is becoming more popular and the number of users is increasing. The company expects the number of concurrent users to vary significantly depending on the time of day and day of week. The company must ensure that the application can scale to meet the needs of the growing user base. Which solution meets these requirements?

**Short Question:** Image app on single EC2 + DynamoDB metadata; needs to scale with variable traffic.
- A. Lambda; store photos + metadata in DynamoDB
- B. Kinesis Data Firehose for photos + metadata
- C. Lambda; photos in S3, metadata in DynamoDB ✅
- D. Scale to 3 EC2 instances + io2 EBS volumes

**Reason:** S3 for large objects (photos), DynamoDB for small metadata, Lambda scales automatically.

---

## Q11
**Full Question:** A solutions architect must design a solution that uses Amazon CloudFront with an Amazon S3 origin to store a static website. The company's security policy requires that all website traffic be inspected by AWS WAF. How should the solutions architect comply with these requirements?

**Short Question:** CloudFront + S3 static site; all traffic must be inspected by AWS WAF.
- A. S3 bucket policy accepts only WAF ARN
- B. CloudFront forwards requests to WAF before S3
- C. Security group for CloudFront IPs on S3 + WAF on CloudFront
- D. Origin Access Identity (OAI) on S3 + WAF enabled on CloudFront distribution ✅

**Reason:** OAI restricts S3 to CloudFront only; WAF inspects traffic at the CloudFront edge.

---

## Q12
**Full Question:** A company's image hosting website gives users around the world the ability to upload, view, and download images from their mobile devices. The company currently hosts the static website in an Amazon S3 bucket. Because of the website's growing popularity, the website's performance has decreased. Users have reported latency issues when they upload and download images. The company must improve the performance of the website. Which solution will meet these requirements with the least implementation effort?

**Short Question:** Global image hosting site on S3 has upload/download latency; fix with least implementation effort.
- A. CloudFront (downloads) + S3 Transfer Acceleration (uploads) ✅
- B. Multi-region EC2 + ALB + Global Accelerator
- C. CloudFront + multi-region S3 buckets + replication + redirect logic
- D. Global Accelerator for S3

**Reason:** Both are simple built-in features requiring minimal setup.

---

## Q13
**Full Question:** A global company is using Amazon API Gateway to design REST APIs for its loyalty club users in the us-east-1 Region and the ap-southeast-2 Region. A solutions architect must design a solution to protect these API Gateway-managed REST APIs across multiple accounts from SQL injection and cross-site scripting attacks. Which solution will meet these requirements with the least amount of administrative effort?

**Short Question:** Protect API Gateway REST APIs (multi-region, multi-account) from SQL injection/XSS, least admin effort.
- A. WAF set up separately in both regions
- B. Firewall Manager in both regions, centrally configure WAF rules ✅
- C. AWS Shield in both regions + web ACL
- D. AWS Shield in one region + web ACL

**Reason:** Firewall Manager centrally manages WAF across accounts/regions.

---

## Q14
**Full Question:** A solutions architect is optimizing a website for an upcoming musical event. Videos of the performances will be streamed in real time and then will be available on demand. The event is expected to attract a global online audience. Which service will improve the performance of both the real-time and on-demand streaming?

**Short Question:** Improve performance of live + on-demand video streaming for a global audience.
- A. Amazon CloudFront ✅
- B. AWS Global Accelerator
- C. Amazon Route 53
- D. S3 Transfer Acceleration

**Reason:** CDN caches and accelerates both live and on-demand video delivery.

---

## Q15
**Full Question:** A solutions architect needs to help a company optimize the cost of running an application on AWS. The application will use Amazon EC2 instances, AWS Fargate, and AWS Lambda for compute within the architecture. The EC2 instances will run the data ingestion layer of the application. EC2 usage will be sporadic and unpredictable. Workloads that run on EC2 instances can be interrupted at any time. The application front end will run on Fargate and Lambda will serve the API layer. The front-end utilization and API layer utilization will be predictable over the course of the next year. Which combination of purchasing options will provide the most cost-effective solution for hosting this application? (Choose 2)

**Short Question:** Cost optimization: EC2 (sporadic, interruptible), Fargate + Lambda (predictable for 1 year).
- A. Spot instances for data ingestion ✅
- B. On-demand instances for data ingestion
- C. 1-year Compute Savings Plan for front end + API ✅
- D. 1-year all-upfront Reserved Instances for data ingestion
- E. 1-year EC2 Instance Savings Plan for front end + API

**Reason:** Spot fits the interruptible workload; Compute Savings Plan covers EC2/Fargate/Lambda together.

---

## Q16
**Full Question:** A company has a data ingestion workflow that includes the following components: an Amazon SNS topic that receives notifications about new data deliveries, and an AWS Lambda function that processes and stores the data. The ingestion workflow occasionally fails because of network connectivity issues. When failure occurs, the corresponding data is not ingested unless the company manually reruns the job. What should a solutions architect do to ensure that all notifications are eventually processed?

**Short Question:** SNS → Lambda ingestion workflow fails sometimes (network issues) and data is lost.
- A. Deploy Lambda across multiple AZs
- B. Increase Lambda CPU/memory
- C. Increase SNS retry count/wait time
- D. Configure SQS as Lambda's on-failure destination ✅

**Reason:** Failed events go to SQS automatically for later reprocessing.

---

## Q17
**Full Question:** A company runs multiple workloads on virtual machines (VMs) in an on-premises data center. The company is expanding rapidly. The on-premises data center is not able to scale fast enough to meet business needs. The company wants to migrate the workloads to AWS. The migration is time-sensitive. The company wants to use a lift-and-shift strategy for non-critical workloads. Which combination of steps will meet these requirements? (Choose 3)

**Short Question:** Lift-and-shift on-prem VMs to AWS, time-sensitive, non-critical workloads.
- A. AWS SCT to collect VM data
- B. AWS Application Migration Service (MGN) — install replication agent ✅
- C. Complete replication → launch test instances → acceptance testing ✅
- D. Stop on-prem VM → launch cutover instance ✅
- E. AWS App2Container to collect VM data
- F. AWS DMS to migrate VMs

**Reason:** MGN replicates, test before cutover, then cut over.

---

## Q18
**Full Question:** A security team wants to limit access to specific services or actions in all of the team's AWS accounts. All accounts belong to a large organization in AWS Organizations. The solution must be scalable and there must be a single point where permissions can be maintained. What should a solutions architect do to accomplish this?

**Short Question:** Limit access to specific services/actions across all accounts in an AWS Organization, scalable, single point of control.
- A. ACL
- B. Security group attached to user groups
- C. Cross-account roles in each account
- D. Service Control Policy (SCP) on root OU ✅

**Reason:** SCP centrally enforces permissions org-wide.

---

## Q19
**Full Question:** A company needs to retain application log files for a critical application for 10 years. The application team regularly accesses logs from the past month for troubleshooting, but logs older than 1 month are rarely accessed. The application generates more than 10 terabytes of logs per month. Which storage option meets these requirements most cost-effectively?

**Short Question:** Retain 10TB/month of logs for 10 years; last month accessed often, older logs rarely accessed. Most cost-effective?
- A. S3 + AWS Backup → Glacier Deep Archive
- B. S3 + S3 Lifecycle policy → Glacier Deep Archive after 1 month ✅
- C. CloudWatch Logs + AWS Backup → Glacier Deep Archive
- D. CloudWatch Logs + S3 Lifecycle policy → Glacier Deep Archive

**Reason:** S3 storage + native lifecycle policy is cheapest and simplest.

---

## Q20
**Full Question:** A medical records company is hosting an application on Amazon EC2 instances. The application processes customer data files that are stored on Amazon S3. The EC2 instances are hosted in public subnets. The EC2 instances access Amazon S3 over the internet, but they do not require any other network access. A new requirement mandates that the network traffic for file transfers take a private route and not be sent over the internet. Which change to the network architecture should a solutions architect recommend to meet this requirement?

**Short Question:** EC2 (public subnet) accesses S3 over internet; new rule requires private routing only.
- A. NAT gateway + route table to S3 via NAT
- B. Security group restricting outbound to S3 prefix list
- C. Move EC2 to private subnet + create S3 VPC endpoint ✅
- D. Remove internet gateway + use Direct Connect to S3

**Reason:** VPC endpoint keeps S3 traffic inside the AWS network.

---

## Q21
**Full Question:** A company is concerned about the security of its public web application due to recent web attacks. The application uses an Application Load Balancer (ALB). A solutions architect must reduce the risk of DDoS attacks against the application. What should the solutions architect do to meet this requirement?

**Short Question:** Public web app behind ALB needs DDoS attack risk reduction.
- A. Amazon Inspector agent on ALB
- B. Amazon Macie
- C. AWS Shield Advanced ✅
- D. Amazon GuardDuty monitoring ALB

**Reason:** Shield Advanced is built for active DDoS mitigation.

---

## Q22
**Full Question:** A company runs its e-commerce application on AWS. Every new order is published as a message in a RabbitMQ queue that runs on an Amazon EC2 instance in a single Availability Zone. These messages are processed by a different application that runs on a separate EC2 instance. This application stores the details in a PostgreSQL database on another EC2 instance. All the EC2 instances are in the same Availability Zone. The company needs to redesign its architecture to provide the highest availability with the least operational overhead. What should a solutions architect do to meet these requirements?

**Short Question:** RabbitMQ on EC2 → app on EC2 → PostgreSQL on EC2, all in one AZ. Redesign for highest availability, least overhead.
- A. Amazon MQ (active/standby) + multi-AZ ASG app + multi-AZ ASG for Postgres on EC2
- B. Amazon MQ (active/standby) + multi-AZ ASG app + multi-AZ RDS for PostgreSQL ✅
- C. Multi-AZ ASG for RabbitMQ + multi-AZ ASG app + multi-AZ RDS for PostgreSQL
- D. Multi-AZ ASG for all three (RabbitMQ, app, Postgres) on EC2

**Reason:** Use managed services (Amazon MQ, RDS) wherever possible to cut operational overhead.

---

## Q23
**Full Question:** A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents. The average volume of data that the company collects from each site daily is 500 GB. Each site has a high-speed internet connection. The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket. The solution must minimize operational complexity. Which solution meets these requirements?

**Short Question:** Aggregate 500GB/day per site (multiple continents, high-speed internet) into one S3 bucket, fastest + simplest.
- A. S3 Transfer Acceleration + multipart upload ✅
- B. Upload to nearest regional S3 bucket + cross-region replication + delete origin
- C. Snowball Edge daily + cross-region replication
- D. EC2 + EBS + snapshot copy + restore

**Reason:** Built-in acceleration + multipart upload is fastest with least complexity.

---

## Q24
**Full Question:** A company is running a multi-tier web application on premises. The web application is containerized and runs on a number of Linux hosts connected to a PostgreSQL database that contains user records. The operational overhead of maintaining the infrastructure and capacity planning is limiting the company's growth. A solutions architect must improve the application's infrastructure. Which combination of actions should the solutions architect take to accomplish this? (Choose 2)

**Short Question:** Containerized on-prem app (Linux hosts) + PostgreSQL; reduce infrastructure/capacity-planning overhead.
- A. Migrate PostgreSQL to Amazon Aurora ✅
- B. Migrate app to EC2 instances
- C. Add CloudFront distribution
- D. Add ElastiCache between app and database
- E. Migrate app to AWS Fargate with ECS ✅

**Reason:** Managed database (Aurora) + serverless containers (Fargate) remove infra management.

---

## Q25
**Full Question:** A company is preparing to store confidential data in Amazon S3. For compliance reasons, the data must be encrypted at rest. Encryption key usage must be logged for auditing purposes. Keys must be rotated every year. Which solution meets these requirements and is the most operationally efficient?

**Short Question:** Store confidential data in S3: must be encrypted at rest, key usage logged for audits, keys rotated yearly.
- A. SSE-C (customer-provided keys)
- B. SSE-S3 (S3-managed keys)
- C. SSE-KMS with manual rotation
- D. SSE-KMS with automatic rotation ✅

**Reason:** KMS logs key usage to CloudTrail and can auto-rotate yearly.
