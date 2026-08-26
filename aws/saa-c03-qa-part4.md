# AWS SAA-C03 Real Exam Questions & Answers — Part 4 (Q76–Q100)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q76
**Full Question:** As part of budget planning, management wants a report of AWS billed items listed by user. The data will be used to create department budgets. A solutions architect needs to determine the most efficient way to obtain this report information. Which solution meets these requirements?

**Short Question:** Get a per-user breakdown of AWS costs for department budgeting, most efficiently.
- A. Query with Amazon Athena
- B. Create a report in Cost Explorer and download it ✅
- C. Download the bill from the billing dashboard
- D. Modify a cost budget in AWS Budgets to alert via SES

**Reason:** Cost Explorer natively filters/groups costs by dimensions like linked account or cost allocation tags and exports to CSV — no CUR/Glue/Athena setup needed.

---

## Q77
**Full Question:** A company hosts its web application on AWS using seven Amazon EC2 instances. The company requires that the IP addresses of all healthy EC2 instances be returned in response to DNS queries. Which policy should be used to meet this requirement?

**Short Question:** DNS must return the IPs of ALL currently healthy EC2 instances in one response.
- A. Simple routing policy
- B. Latency routing policy
- C. Multivalue answer routing policy ✅
- D. Geolocation routing policy

**Reason:** Multivalue answer routing returns multiple IPs and can attach a health check to each record, excluding unhealthy ones — simple routing doesn't health-check.

---

## Q78
**Full Question:** A company runs analytics software on Amazon EC2 instances. The software accepts job requests from users to process data that has been uploaded to Amazon S3. Users report that some submitted data is not being processed. Amazon CloudWatch reveals that the EC2 instances have a consistent CPU utilization at or near 100%. The company wants to improve system performance and scale the system based on user load. What should a solutions architect do to meet these requirements?

**Short Question:** Single-instance analytics processor pegged at 100% CPU, dropping jobs — make it scale with load.
- A. Duplicate the instance, put both behind an ALB
- B. S3 VPC endpoint, update software to use it
- C. Stop instances, resize to bigger type, restart
- D. Route requests through SQS, scale an EC2 Auto Scaling group on queue size ✅

**Reason:** SQS decouples ingestion from processing and buffers load; scaling the ASG off queue size matches capacity to actual demand without losing jobs.

---

## Q79
**Full Question:** A solutions architect needs to design a highly available application consisting of web application and database tiers. HTTPS content delivery should be as close to the edge as possible with the least delivery time. Which solution meets these requirements and is most secure?

**Short Question:** HA web+DB app; HTTPS content served from as close to the edge as possible; most secure design.
- A. Public ALB + EC2 in public subnets; CloudFront → public ALB origin
- B. Public ALB + EC2 in private subnets; CloudFront → EC2 instances as origin
- C. Public ALB + EC2 in private subnets; CloudFront → public ALB origin ✅
- D. Public ALB + EC2 in public subnets; CloudFront → EC2 instances as origin

**Reason:** EC2 must sit in private subnets (not directly internet-facing), and CloudFront's origin should be the ALB (stable), not individual instances (IPs change).

---

## Q80
**Full Question:** A company has a multi-tier application that runs six front-end web servers in an Amazon EC2 Auto Scaling group in a single Availability Zone behind an Application Load Balancer (ALB). A solutions architect needs to modify the infrastructure to be highly available without modifying the application. Which architecture should the solutions architect choose that provides high availability?

**Short Question:** Front-end fleet lives in one AZ only — make it highly available without touching the app.
- A. Auto Scaling group spanning two regions, 3 instances each
- B. Modify the Auto Scaling group to run 3 instances across each of two AZs ✅
- C. A launch template ready to spin up instances in another region
- D. "Round robin" config on the existing ALB

**Reason:** Standard HA practice is spreading across multiple AZs within one region — cheaper and more standard than multi-region, and a launch template alone doesn't run anything.

---

## Q81
**Full Question:** A company's website provides users with downloadable historical performance reports. The website needs a solution that will scale to meet the company's website demands globally. The solution should be cost-effective, limit the provisioning of infrastructure resources, and provide the fastest possible response time. Which combination should a solutions architect recommend to meet these requirements?

**Short Question:** Serve static downloadable report files globally — cheap, minimal infrastructure, fastest response.
- A. CloudFront + S3 ✅
- B. Lambda + DynamoDB
- C. ALB + EC2 Auto Scaling
- D. Route 53 + internal ALBs

**Reason:** S3 is the cheap scalable store for static files, and CloudFront caches them at edge locations worldwide for the fastest response — no servers to provision.

---

## Q82
**Full Question:** A company uses 50 TB of data for reporting. The company wants to move this data from on-premises to AWS. A custom application in the company's data center runs a weekly data transformation job. The company plans to pause the application until the data transfer is complete and needs to begin the transfer process as soon as possible. The data center does not have any available network bandwidth for additional workloads. A solutions architect must transfer the data and must configure the transformation job to continue to run in the AWS cloud. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Move 50TB with zero spare network bandwidth, then keep the weekly transform job running in AWS.
- A. DataSync (network-based) + AWS Glue
- B. Snowcone device + deploy the transform app onto the device
- C. Snowball Edge Storage Optimized device → S3 + AWS Glue for the transform job ✅
- D. Snowball Edge Storage Optimized (with EC2 compute) + run transform on a new EC2 instance

**Reason:** No bandwidth rules out DataSync; Snowcone is undersized for 50TB. Snowball Edge (storage-only) moves the data offline, and a fully managed Glue job (not custom EC2 compute) keeps ongoing operational overhead lowest.

---

## Q83
**Full Question:** A company is preparing a new data platform that will ingest real-time streaming data from multiple sources. The company needs to transform the data before writing the data to Amazon S3. The company needs the ability to use SQL to query the transformed data. Which solutions will meet these requirements? (Choose 2)

**Short Question:** Ingest real-time streams, transform them, write to S3, then query the result with SQL.
- A. Kinesis Data Streams → Kinesis Data Analytics (transform) → Kinesis Data Firehose (write to S3) → Athena (query) ✅
- B. Amazon MSK (stream) → AWS Glue (transform + write to S3) → Athena (query) ✅
- C. AWS DMS (ingest) → Amazon EMR (transform + write to S3) → Athena (query)
- D. Amazon MSK (stream) → Kinesis Data Analytics (transform + write to S3) → RDS query editor (query)
- E. Kinesis Data Streams (stream) → AWS Glue (transform) → Kinesis Data Firehose (write to S3) → RDS query editor (query)

**Reason:** A and B are both complete, correctly-matched pipelines; C uses DMS (database migration, not generic stream ingestion) as the wrong tool, and D/E wrongly use the RDS query editor to query data that lives in S3.

---

## Q84
**Full Question:** A company needs to review its AWS cloud deployment to ensure that its Amazon S3 buckets do not have unauthorized configuration changes. What should a solutions architect do to accomplish this goal?

**Short Question:** Continuously detect unauthorized configuration changes on S3 buckets.
- A. Turn on AWS Config with the appropriate rules ✅
- B. Turn on Trusted Advisor with the appropriate checks
- C. Turn on Amazon Inspector with an assessment template
- D. Turn on S3 server access logging + EventBridge

**Reason:** AWS Config continuously records resource configurations and evaluates them against rules, flagging non-compliant changes — the other tools don't do continuous configuration compliance tracking for S3.

---

## Q85
**Full Question:** A company is designing a cloud communications platform that is driven by APIs. The application is hosted on Amazon EC2 instances behind a Network Load Balancer (NLB). The company uses Amazon API Gateway to provide external users with access to the application through APIs. The company wants to protect the platform against web exploits like SQL injection and also wants to detect and mitigate large sophisticated DDoS attacks. Which combination of solutions provides the most protection? (Choose 2)

**Short Question:** Protect an NLB+API Gateway platform from both SQL injection AND large sophisticated DDoS attacks.
- A. WAF on the NLB
- B. AWS Shield Advanced on the NLB ✅
- C. AWS WAF on API Gateway ✅
- D. GuardDuty + Shield Standard
- E. Shield Standard on API Gateway

**Reason:** WAF (layer 7) can't attach to an NLB (layer 4) — Shield Advanced covers the NLB against DDoS, and WAF covers API Gateway against web exploits like SQL injection.

---

## Q86
**Full Question:** A company has a serverless website with millions of objects in an Amazon S3 bucket. The company uses the S3 bucket as the origin for an Amazon CloudFront distribution. The company did not set encryption on the S3 bucket before the objects were loaded. A solutions architect needs to enable encryption for all existing objects and for all objects that are added to the S3 bucket in the future. Which solution will meet these requirements with the least amount of effort?

**Short Question:** Retroactively encrypt millions of existing S3 objects, and make sure all future objects are encrypted too.
- A. New bucket + default encryption + manually download/re-upload everything
- B. Default encryption for future objects + S3 Inventory to list unencrypted objects + S3 Batch Operations copy job to encrypt them ✅
- C. New KMS key + SSE-KMS default encryption + versioning (nothing for existing objects)
- D. Manually select/modify each unencrypted object in the console

**Reason:** Default encryption only covers new uploads; S3 Inventory + Batch Operations is the scalable, automated way to re-encrypt millions of existing objects without downloading/re-uploading or doing it by hand.

---

## Q87
**Full Question:** A company runs a web-based portal that provides users with global breaking news, local alerts, and weather updates. The portal delivers each user a personalized view by using a mixture of static and dynamic content. Content is served over HTTPS through an API server running on an Amazon EC2 instance behind an Application Load Balancer (ALB). The company wants the portal to provide this content to its users across the world as quickly as possible. How should a solutions architect design the application to ensure the least amount of latency for all users?

**Short Question:** Deliver a mix of static + dynamic content to a global audience with the least possible latency.
- A. Single region + CloudFront in front of everything, ALB as origin, using cache behaviors ✅
- B. Two regions + Route 53 latency routing to the closer ALB
- C. Single region + CloudFront for static only, dynamic served directly from the ALB
- D. Two regions + Route 53 geolocation routing to the closer ALB

**Reason:** One CloudFront distribution with cache behaviors handles both content types at the edge — DNS-level routing (B/D) still serves from a full regional stack, and serving dynamic content directly (C) skips the CDN's latency benefit for half the traffic.

---

## Q88
**Full Question:** A solutions architect is designing a VPC with public and private subnets. The VPC and subnets use IPv4 CIDR blocks. There is one public subnet and one private subnet in each of three Availability Zones for high availability. An internet gateway is used to provide internet access for the public subnets. The private subnets require access to the internet to allow Amazon EC2 instances to download software updates. What should the solutions architect do to enable internet access for the private subnets?

**Short Question:** Give private subnets in 3 AZs outbound internet access, highly available.
- A. One NAT gateway per AZ (in each public subnet), private route table per AZ pointing to its own NAT gateway ✅
- B. One NAT instance per AZ, placed in the private subnets
- C. A second internet gateway placed in a private subnet
- D. An egress-only internet gateway (IPv6-only feature)

**Reason:** A NAT gateway per AZ (in the public subnet) keeps each AZ self-sufficient if another AZ fails; NAT instances are legacy/higher-overhead and belong in public subnets, a VPC can only have one internet gateway, and egress-only gateways are IPv6-only (this VPC is IPv4).

---

## Q89
**Full Question:** A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database. The EC2 instances connect to the database by using usernames and passwords that are stored locally in a file. The company wants to minimize the operational overhead of credential management. What should a solutions architect do to accomplish this goal?

**Short Question:** Stop storing DB credentials in a local file on EC2 — minimize ongoing credential-management effort.
- A. AWS Secrets Manager with automatic rotation turned on ✅
- B. Systems Manager Parameter Store with automatic rotation
- C. S3 bucket (KMS-encrypted) storing the credential file
- D. Encrypted EBS volume storing the credential file

**Reason:** Secrets Manager natively auto-rotates credentials and integrates directly with Aurora — Parameter Store has no built-in rotation, and S3/EBS just relocate the static-file problem.

---

## Q90
**Full Question:** A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application. The media files must be resilient to the loss of an Availability Zone. Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern. The solutions architect must minimize the costs of storing and retrieving the media files. Which storage option meets these requirements?

**Short Question:** Media files must survive an AZ loss; access pattern flips unpredictably between frequent and rare; minimize storage+retrieval cost.
- A. S3 Standard
- B. S3 Intelligent-Tiering ✅
- C. S3 Standard-IA
- D. S3 One Zone-IA

**Reason:** Intelligent-Tiering automatically moves objects between access tiers with unpredictable patterns (no retrieval fees like Standard-IA) and still replicates across multiple AZs (unlike One Zone-IA).

---

## Q91
**Full Question:** A company has an e-commerce checkout workflow that writes an order to a database and calls a service to process the payment. Users are experiencing timeouts during the checkout process. When users resubmit the checkout form, multiple unique orders are created for the same desired transaction. How should a solutions architect refactor this workflow to prevent the creation of multiple orders?

**Short Question:** Checkout timeouts + resubmits are creating duplicate orders — fix the workflow so each order is processed exactly once.
- A. Order message → Kinesis Data Firehose, payment service pulls from it
- B. CloudTrail rule → Lambda queries DB → calls payment service
- C. Store order, message to SNS topic, payment service polls SNS
- D. Store order, message to an SQS FIFO queue; payment service processes then deletes the message ✅

**Reason:** An SQS FIFO queue guarantees exactly-once processing — Firehose isn't pollable, CloudTrail isn't a workflow tool, and SNS doesn't support polling or dedup.

---

## Q92
**Full Question:** A research laboratory needs to process approximately 8 TB of data. The laboratory requires sub-millisecond latencies and a minimum throughput of 6 GBps for the storage subsystem. Hundreds of Amazon EC2 instances that run Amazon Linux will distribute and process the data. Which solution will meet the performance requirements?

**Short Question:** Hundreds of Linux EC2 instances need a shared file system with sub-ms latency and 6GBps+ throughput over 8TB of data.
- A. FSx for NetApp ONTAP, tiering policy "all"
- B. S3 (raw data) + FSx for Lustre with persistent SSD storage, import/export from S3 ✅
- C. S3 (raw data) + FSx for Lustre with persistent HDD storage, import/export from S3
- D. FSx for NetApp ONTAP, tiering policy "none"

**Reason:** FSx for Lustre is the Linux-optimized HPC file system; persistent SSD (not HDD) is required to hit sub-millisecond latency and the 6GBps throughput target.

---

## Q93
**Full Question:** A company is building a web-based application running on Amazon EC2 instances in multiple Availability Zones. The web application will provide access to a repository of text documents totaling about 900 terabytes in size. The company anticipates that the web application will experience periods of high demand. A solutions architect must ensure that the storage component for the text documents can scale to meet the demand of the application at all times. The company is concerned about the overall cost of the solution. Which storage solution meets these requirements most cost-effectively?

**Short Question:** Store and serve a 900TB text document repository to a multi-AZ web app, cost-effectively, scaling automatically with demand.
- A. EBS
- B. EFS
- C. OpenSearch Service (Elasticsearch)
- D. Amazon S3 ✅

**Reason:** S3 is the cheapest-per-GB, effectively unlimited, auto-scaling object store — EBS is single-instance block storage, EFS costs much more per GB, and OpenSearch is a search engine, not bulk storage.

---

## Q94
**Full Question:** A solutions architect has created a new AWS account and must secure AWS account root user access. Which combination of actions will accomplish this? (Choose 2)

**Short Question:** Lock down a brand-new AWS account's root user, properly.
- A. Strong password on the root user ✅
- B. Enable MFA on the root user ✅
- C. Store root user access keys in an encrypted S3 bucket
- D. Add the root user to an IAM group with admin permissions
- E. Attach an inline IAM policy to the root user

**Reason:** A strong password + MFA are the two foundational root-user protections — root shouldn't have access keys at all, can't be added to an IAM group, and its permissions can't be restricted by a policy.

---

## Q95
**Full Question:** A company is building a new dynamic ordering website. The company wants to minimize server maintenance and patching. The website must be highly available and must scale read and write capacity as quickly as possible to meet changes in user demand. Which solution will meet these requirements?

**Short Question:** New dynamic ordering site: no servers to patch, highly available, near-instant read/write capacity scaling.
- A. S3 (static) + API Gateway/Lambda (dynamic) + DynamoDB on-demand + CloudFront ✅
- B. S3 (static) + API Gateway/Lambda (dynamic) + Aurora with Aurora Autoscaling + CloudFront
- C. EC2 Auto Scaling group + ALB + DynamoDB with provisioned write capacity
- D. EC2 Auto Scaling group + ALB + Aurora with Aurora Autoscaling

**Reason:** A fully serverless stack removes all server management, and DynamoDB on-demand scales read/write capacity instantly — Aurora Autoscaling reacts more slowly, and options C/D still involve managing EC2 servers.

---

## Q96
**Full Question:** A reporting team receives files each day in an Amazon S3 bucket. The reporting team manually reviews and copies the files from this initial S3 bucket to an analysis S3 bucket each day at the same time to use with Amazon QuickSight. Additional teams are starting to send more files in larger sizes to the initial S3 bucket. The reporting team wants to move the files automatically to the analysis S3 bucket as the files enter the initial S3 bucket. The reporting team also wants to use AWS Lambda functions to run pattern-matching code on the copied data. In addition, the reporting team wants to send the data files to a pipeline in Amazon SageMaker Pipelines. What should a solutions architect do to meet these requirements with the least operational overhead?

**Short Question:** Auto-copy new files between two S3 buckets, then fan the copied files out to both a Lambda pattern-matcher AND a SageMaker pipeline.
- A. Lambda copies files + S3 event notification on the analysis bucket → Lambda + SageMaker as destinations
- B. Lambda copies files + analysis bucket → EventBridge → rule targets Lambda + SageMaker
- C. S3 replication between buckets + S3 event notification → Lambda + SageMaker as destinations
- D. S3 replication between buckets + analysis bucket → EventBridge → rule targets Lambda + SageMaker ✅

**Reason:** S3 replication (not a custom Lambda copier) is the managed way to move files automatically; S3 event notifications can't target SageMaker directly, but EventBridge can fan a single S3 event out to both Lambda and a SageMaker pipeline.

---

## Q97
**Full Question:** A social media company allows users to upload images to its website. The website runs on Amazon EC2 instances. During upload requests, the website resizes the images to a standard size and stores the resized images in Amazon S3. Users are experiencing slow upload requests to the website. The company needs to reduce coupling within the application and improve website performance. A solutions architect must design the most operationally efficient process for image uploads. Which combination of actions should the solutions architect take to meet these requirements? (Choose 2)

**Short Question:** EC2 web servers doing both upload-handling AND image resizing are slow — decouple and speed up uploads.
- A. Upload images to S3 Glacier
- B. Web server uploads originals to S3 (still via the server)
- C. Browser uploads directly to S3 via a pre-signed URL ✅
- D. S3 event notification → Lambda resizes the image ✅
- E. Scheduled EventBridge rule invokes Lambda to resize images

**Reason:** A pre-signed URL takes the EC2 server out of the upload path entirely, and an S3-triggered Lambda decouples resizing from the web server and runs it serverlessly in real time (unlike a batch schedule).

---

## Q98
**Full Question:** A company is developing an application in the AWS cloud. The application's HTTP API contains critical information that is published in Amazon API Gateway. The critical information must be accessible from only a limited set of trusted IP addresses that belong to the company's internal network. Which solution will meet these requirements?

**Short Question:** Restrict a published HTTP API in API Gateway to only a specific allow-list of trusted IP addresses.
- A. API Gateway private integration
- B. Resource policy on the API denying any IP not explicitly allowed ✅
- C. Deploy directly into a private subnet + network ACL rules
- D. Security group on API Gateway allowing only trusted IPs

**Reason:** A resource policy on the API can use the `aws:SourceIp` condition to allow/deny by IP — API Gateway is a managed service that doesn't live in a subnet and can't have a security group attached.

---

## Q99
**Full Question:** A company needs to migrate a legacy application from an on-premises data center to the AWS cloud because of hardware capacity constraints. The application runs 24 hours a day, 7 days a week. The application's database storage continues to grow over time. What should a solutions architect do to meet these requirements most cost effectively?

**Short Question:** Migrate a 24/7 legacy app whose database keeps growing — cheapest cloud setup.
- A. EC2 Spot (app) + S3 (data storage)
- B. EC2 Reserved Instances (app) + RDS on-demand (database)
- C. EC2 Reserved Instances (app) + Aurora Reserved Instances (database) ✅
- D. EC2 on-demand (app) + RDS Reserved Instances (database)

**Reason:** A constant 24/7 workload is the textbook case for Reserved Instances on both compute and database — Spot risks interruption for a non-interruptible app, S3 isn't a relational DB replacement, and on-demand EC2 (D) is needlessly expensive for steady-state usage.

---

## Q100
**Full Question:** A company has an application that places hundreds of CSV files into an Amazon S3 bucket every hour. The files are 1 gigabyte in size. Each time a file is uploaded, the company needs to convert the file to Apache Parquet format and place the output file into an S3 bucket. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Auto-convert each new 1GB CSV file dropped into S3 into Parquet format, least operational overhead.
- A. Lambda function downloads/converts/uploads the file, invoked per S3 put event
- B. Apache Spark job does the conversion, a Lambda function invokes the Spark job per S3 put event
- C. Glue table/crawler + scheduled Lambda periodically queries via Athena, converts results to Parquet
- D. AWS Glue ETL job converts CSV→Parquet, a Lambda function invokes the Glue job per S3 put event ✅

**Reason:** A 1GB file conversion risks exceeding Lambda's 15-minute limit on its own (A); Glue is the fully managed, serverless ETL tool for this exact CSV→Parquet task, with a lightweight Lambda just triggering it per upload — no cluster (B) or scheduled/Athena hack (C) needed.
