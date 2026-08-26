# AWS SAA-C03 Real Exam Questions & Answers — Part 2 (Q26–Q50)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q26
**Full Question:** A company is migrating its on-premises PostgreSQL database to Amazon Aurora PostgreSQL. The on-premises database must remain online and accessible during the migration. The Aurora database must remain synchronized with the on-premises database. Which combination of actions must a solutions architect take to meet these requirements? (Choose 2)

**Short Question:** Live-migrate on-prem PostgreSQL to Aurora with zero downtime, keep both in sync.
- A. Create an ongoing replication task ✅
- B. Create a database backup of the on-premises database
- C. Create an AWS DMS replication server ✅
- D. Convert the database schema using AWS SCT
- E. Create an EventBridge/CloudWatch Events rule to monitor synchronization

**Reason:** DMS replication server runs the migration; ongoing replication task (CDC) keeps target in sync.

---

## Q27
**Full Question:** A company runs a stateless web application in production on a group of Amazon EC2 on-demand instances behind an Application Load Balancer. The application experiences heavy usage during an 8-hour period each business day. Application usage is moderate and steady overnight. Application usage is low during weekends. The company wants to minimize its EC2 costs without affecting the availability of the application. Which solution will meet these requirements?

**Short Question:** Minimize EC2 cost for a workload with steady overnight baseline + unpredictable daytime peak, no availability impact.
- A. Spot instances for the entire workload
- B. Reserved instances for baseline + spot instances for extra capacity ✅
- C. On-demand instances for baseline + spot instances for extra capacity
- D. Dedicated instances for baseline + on-demand instances for extra capacity

**Reason:** Reserved covers the predictable steady baseline cheaply; spot covers flexible extra capacity cheaply.

---

## Q28
**Full Question:** A company is running an online transaction processing (OLTP) workload on AWS. This workload uses an unencrypted Amazon RDS DB instance in a Multi-AZ deployment. Daily database snapshots are taken from this instance. What should a solutions architect do to ensure the database and snapshots are always encrypted moving forward?

**Short Question:** Turn an existing unencrypted RDS instance (and its snapshots) into an encrypted one going forward.
- A. Encrypt a copy of the latest DB snapshot, replace the existing DB instance by restoring the encrypted snapshot ✅
- B. New encrypted EBS volume, copy snapshots to it, enable encryption on the DB instance
- C. Copy snapshot + enable KMS encryption, restore encrypted snapshot to the existing DB instance
- D. Copy snapshots to an SSE-KMS encrypted S3 bucket

**Reason:** You can't encrypt a live RDS instance — encrypt a snapshot copy, then restore it as a new instance.

---

## Q29
**Full Question:** A company sells ringtones created from clips of popular songs. The files containing the ringtones are stored in Amazon S3 Standard and are at least 128 KB in size. The company has millions of files, but downloads are infrequent for ringtones older than 90 days. The company needs to save money on storage while keeping the most accessed files readily available for its users. Which action should the company take to meet these requirements most cost-effectively?

**Short Question:** Millions of S3 files, frequently accessed for 90 days then rarely — cheapest storage setup?
- A. Start all objects directly in S3 Standard-IA
- B. S3 Intelligent-Tiering, move to cheaper tier after 90 days
- C. S3 Inventory to manage and move objects to S3 Standard-IA after 90 days
- D. S3 Lifecycle policy: Standard → Standard-IA after 90 days ✅

**Reason:** Access pattern is known/predictable, so a plain lifecycle policy is simplest and cheapest — no need for Intelligent-Tiering's monitoring fee.

---

## Q30
**Full Question:** A company has a highly dynamic batch processing job that uses many Amazon EC2 instances to complete it. The job is stateless in nature, can be started and stopped at any given time with no negative impact, and typically takes upwards of 60 minutes total to complete. The company has asked a solutions architect to design a scalable and cost-effective solution that meets the requirements of the job. What should the solutions architect recommend?

**Short Question:** Stateless, interruptible batch job, ~60+ min runtime — scalable and cost-effective compute choice?
- A. EC2 Spot Instances ✅
- B. EC2 Reserved Instances
- C. EC2 On-Demand Instances
- D. AWS Lambda

**Reason:** Spot fits stateless/interruptible workloads at up to 90% discount; Lambda's 15-min timeout rules it out.

---

## Q31
**Full Question:** An application runs on Amazon EC2 instances across multiple Availability Zones. The instances run in an Amazon EC2 Auto Scaling group behind an Application Load Balancer. The application performs best when the CPU utilization of the EC2 instances is at or near 40%. What should a solutions architect do to maintain the desired performance across all instances in the group?

**Short Question:** Keep EC2 Auto Scaling group's average CPU utilization near a fixed target (40%).
- A. Simple scaling policy
- B. Target tracking policy ✅
- C. Lambda function to update desired capacity
- D. Scheduled scaling actions

**Reason:** Target tracking is purpose-built to hold a metric at a set target automatically.

---

## Q32
**Full Question:** A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB). The web application has static data and dynamic data. The company stores its static data in an Amazon S3 bucket. The company wants to improve performance and reduce latency for the static data and dynamic data. The company is using its own domain name registered with Amazon Route 53. What should a solutions architect do to meet these requirements?

**Short Question:** Reduce latency for both static (S3) and dynamic (ALB) content on one custom domain.
- A. One CloudFront distribution with S3 bucket + ALB as origins, Route 53 → CloudFront ✅
- B. CloudFront (ALB origin) + separate Global Accelerator (S3 endpoint), Route 53 → CloudFront
- C. CloudFront (S3 origin) + Global Accelerator fronting ALB and CloudFront, custom domain → accelerator
- D. CloudFront (ALB origin) + Global Accelerator (S3 endpoint), two separate domain names

**Reason:** One CloudFront distribution can use multiple origins with cache behaviors routing by path — simplest, single-domain solution.

---

## Q33
**Full Question:** A company's containerized application runs on an Amazon EC2 instance. The application needs to download security certificates before it can communicate with other business applications. The company wants a highly secure solution to encrypt and decrypt the certificates in near real time. The solution also needs to store data in highly available storage after the data is encrypted. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Near-real-time encrypt/decrypt certificates on EC2, then store encrypted data in highly available storage, least overhead.
- A. Secrets Manager secrets + manual certificate updates + fine-grained IAM
- B. Custom Lambda function using a Python crypto library, store function in S3
- C. KMS customer managed key (via EC2 role) + store encrypted data in S3 ✅
- D. KMS customer managed key (via EC2 role) + store encrypted data on EBS

**Reason:** KMS handles real-time encryption; S3 (not EBS) provides the required high availability.

---

## Q34
**Full Question:** A company recently migrated a message processing system to AWS. The system receives messages into an ActiveMQ running on an Amazon EC2 instance. Messages are processed by a consumer application running on Amazon EC2. The consumer application processes the messages and writes results to a MySQL database running on Amazon EC2. The company wants this application to be highly available with low operational complexity. Which architecture offers the highest availability?

**Short Question:** ActiveMQ → consumer app → MySQL, all self-managed on EC2 in one AZ. Redesign for highest availability, low complexity.
- A. Second self-managed ActiveMQ + extra consumer EC2 + replicated MySQL on EC2 (all self-managed, multi-AZ)
- B. Amazon MQ active/standby + extra consumer EC2 instance + replicated MySQL on EC2
- C. Amazon MQ active/standby + one extra consumer EC2 instance + RDS MySQL Multi-AZ
- D. Amazon MQ active/standby + Auto Scaling group for consumers + RDS MySQL Multi-AZ ✅

**Reason:** Fully managed services (Amazon MQ, RDS Multi-AZ) plus an Auto Scaling group for the app tier gives HA at all three layers with least overhead.

---

## Q35
**Full Question:** A company hosts a website analytics application on a single Amazon EC2 on-demand instance. The analytics software is written in PHP and uses a MySQL database. The analytics software, the web server that provides PHP, and the database server are all hosted on the EC2 instance. The application is showing signs of performance degradation during busy times and is presenting 5xx errors. The company needs to make the application scale seamlessly. Which solution will meet these requirements most cost-effectively?

**Short Question:** Monolithic PHP+MySQL app on one EC2 instance, throwing 5xx errors under load — make it scale seamlessly, cheaply.
- A. RDS MySQL + AMI + second on-demand EC2 instance + ALB
- B. RDS MySQL + AMI + second on-demand EC2 instance + Route 53 weighted routing
- C. Aurora MySQL + Lambda that stops EC2 and changes instance type on a CloudWatch alarm
- D. Aurora MySQL + AMI/launch template + Auto Scaling group (spot fleet) + ALB ✅

**Reason:** Decouple the DB into managed Aurora, then use an ASG with spot fleet + ALB for real automatic, cost-effective scaling.

---

## Q36
**Full Question:** A company wants to build a scalable key management infrastructure to support developers who need to encrypt data in their applications. What should a solutions architect do to reduce the operational burden?

**Short Question:** Scalable key management for developers to encrypt app data, least operational burden.
- A. MFA to protect encryption keys
- B. AWS KMS ✅
- C. AWS Certificate Manager (ACM)
- D. IAM policy limiting who can access the keys

**Reason:** KMS is the fully managed service for creating, storing, rotating, and auditing encryption keys.

---

## Q37
**Full Question:** A company hosts a data lake on AWS. The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL. The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake. Only the company's management team should have full access to all the visualizations. The rest of the company should have only limited access. Which solution will meet these requirements?

**Short Question:** Build dashboards over S3 + RDS PostgreSQL data lake; management gets full access, everyone else limited access.
- A. QuickSight analysis over all sources, share dashboards with IAM roles
- B. QuickSight analysis over all sources, share dashboards with QuickSight users/groups ✅
- C. Glue table/crawler + Glue ETL job → static reports in S3, access via S3 bucket policies
- D. Glue table/crawler + Athena federated query (incl. RDS) → reports in S3, access via S3 bucket policies

**Reason:** QuickSight natively visualizes multiple data sources and shares dashboards via its own users/groups with tiered permissions (viewer/co-owner) — not via IAM roles directly.

---

## Q38
**Full Question:** A company runs its media rendering application on premises. The company wants to reduce storage costs and has moved all data to Amazon S3. The on-premises rendering application needs low latency access to storage. The company needs to design a storage solution for the application. The storage solution must maintain the desired application performance. Which storage solution will meet these requirements in the most cost-effective way?

**Short Question:** On-prem rendering app needs low-latency access to data that now lives in S3 — cheapest way to keep performance.
- A. Mountpoint for Amazon S3
- B. Amazon S3 File Gateway ✅
- C. Copy data to Amazon FSx for Windows File Server + FSx File Gateway
- D. On-prem file server using the S3 API directly

**Reason:** S3 File Gateway caches hot data on-prem for low latency while keeping the bulk of data in S3, fully managed.

---

## Q39
**Full Question:** A digital image processing company wants to migrate its on-premises monolithic application to the AWS cloud. The company processes thousands of images and generates large files as part of the processing workflow. The company needs a solution to manage the growing number of image processing jobs. The solution must also reduce the manual tasks in the image processing workflow. The company does not want to manage the underlying infrastructure of the solution. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Migrate a monolithic image-processing app to AWS: manage growing job volume, cut manual steps, no infra to manage.
- A. ECS on EC2 Spot + SQS orchestration + store output in EFS
- B. AWS Batch + Step Functions orchestration + store output in S3 ✅
- C. Lambda + EC2 Spot + store output in FSx
- D. Group of EC2 instances + Step Functions + store output on EBS

**Reason:** AWS Batch is fully managed batch compute; Step Functions removes custom workflow code; S3 is the cost-effective, durable store — all serverless/managed.

---

## Q40
**Full Question:** A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC. The company had an inspection server in its on-premises data center. The inspection server performed specific operations such as traffic flow inspection and traffic filtering. The company wants to have the same functionalities in the AWS cloud. Which solution will meet these requirements?

**Short Question:** Replace an on-prem traffic inspection/filtering appliance with something equivalent, inside a VPC.
- A. Amazon GuardDuty
- B. Traffic Mirroring
- C. AWS Network Firewall ✅
- D. AWS Firewall Manager

**Reason:** Network Firewall does actual inline stateful/stateless inspection and filtering inside a VPC — the direct cloud equivalent of the on-prem appliance.

---

## Q41
**Full Question:** A company is building a containerized application on premises and decides to move the application to AWS. The application will have thousands of users soon after it is deployed. The company is unsure how to manage the deployment of containers at scale. The company needs to deploy the containerized application in a highly available architecture that minimizes operational overhead. Which solution will meet these requirements?

**Short Question:** Deploy a containerized app at scale, highly available, least operational overhead.
- A. ECR + ECS on Fargate launch type + target tracking scaling ✅
- B. ECR + ECS on EC2 launch type + target tracking scaling
- C. Self-hosted image repo on EC2 + containers on EC2 across AZs + manual CloudWatch-based scaling
- D. Bake container into an EC2 AMI + Auto Scaling group + CloudWatch alarm scaling

**Reason:** Fargate removes all EC2/cluster management — the only truly serverless, low-overhead option among these.

---

## Q42
**Full Question:** A company has a three-tier web application that processes orders from customers. The web tier consists of Amazon EC2 instances behind an Application Load Balancer. The processing tier consists of EC2 instances. The company decoupled the web tier and processing tier by using Amazon SQS. The storage layer uses Amazon DynamoDB. At peak times, some users report processing delays and stalls. The company has noticed that during these delays, the EC2 instances are running at 100% CPU usage and the SQS queue fills up. The peak times are variable and unpredictable. The company needs to improve the performance of the application. Which solution will meet these requirements?

**Short Question:** Processing-tier EC2 instances hit 100% CPU and SQS queue backs up during unpredictable peaks — fix the bottleneck.
- A. Scheduled scaling for the processing tier, based on CPU utilization
- B. ElastiCache for Redis in front of DynamoDB, scale on target utilization
- C. CloudFront caching for the web tier, scale on HTTP latency
- D. Target tracking Auto Scaling for the processing tier, scale on SQS "approximate number of messages" ✅

**Reason:** Scaling directly off queue depth matches capacity to the real backlog, and handles unpredictable spikes (unlike a fixed schedule).

---

## Q43
**Full Question:** A company wants to migrate its on-premises data center to AWS. According to the company's compliance requirements, the company can use only the AP Northeast 3 region. Company administrators are not permitted to connect VPCs to the internet. Which solutions will meet these requirements? (Choose 2)

**Short Question:** Enforce: only AP-Northeast-3 region allowed, and no VPC may ever get internet access, org-wide.
- A. AWS Control Tower data residency guardrails: deny internet access + deny all regions except AP Northeast 3 ✅
- B. AWS WAF rules to block internet access + region restriction in account settings
- C. AWS Organizations SCPs: deny VPC internet access + deny all regions except AP Northeast 3 ✅
- D. Network ACL outbound deny-all rule in each VPC + per-user IAM policy restricting region
- E. AWS Config managed rules to detect/alert on internet gateways and out-of-region resources

**Reason:** Control Tower guardrails and Organization SCPs are both centralized, preventative controls; WAF/NACL-per-VPC/Config are either wrong tool or not preventative/scalable.

---

## Q44
**Full Question:** A gaming company is designing a highly available architecture. The application runs on a modified Linux kernel and supports only UDP-based traffic. The company needs the front-end tier to provide the best possible user experience. That tier must have low latency, route traffic to the nearest edge location, and provide static IP addresses for entry into the application endpoints. What should a solutions architect do to meet these requirements?

**Short Question:** UDP-only gaming app: needs lowest latency, nearest-edge routing, and static entry IPs.
- A. Route 53 → ALB, Lambda app in Application Auto Scaling
- B. CloudFront → NLB, Lambda app in Application Auto Scaling
- C. AWS Global Accelerator → NLB, EC2 app in an EC2 Auto Scaling group ✅
- D. API Gateway → ALB, EC2 app in an EC2 Auto Scaling group

**Reason:** Only Global Accelerator (layer 4, static IPs, edge routing) + NLB (only ELB that supports UDP) fit a UDP-only game server.

---

## Q45
**Full Question:** A company runs its two-tier e-commerce website on AWS. The web tier consists of a load balancer that sends traffic to Amazon EC2 instances. The database tier uses an Amazon RDS DB instance. The EC2 instances and the RDS DB instance should not be exposed to the public internet. The EC2 instances require internet access to complete payment processing of orders through a third-party web service. The application must be highly available. Which combination of configuration options will meet these requirements? (Choose 2)

**Short Question:** Keep EC2 + RDS private, but EC2 still needs outbound internet for payments; app must be highly available.
- A. Auto Scaling group launches EC2 in private subnets; RDS Multi-AZ instance in private subnets ✅
- B. VPC with 2 private subnets + 2 NAT gateways across 2 AZs; ALB in the private subnets
- C. Auto Scaling group launches EC2 in public subnets across 2 AZs; RDS Multi-AZ in private subnets
- D. VPC with 1 public + 1 private subnet + 2 NAT gateways across 2 AZs; ALB in the public subnet
- E. VPC with 2 public + 2 private subnets + 2 NAT gateways across 2 AZs; ALB in the public subnets ✅

**Reason:** EC2/RDS must sit in private subnets (A) inside a VPC with public subnets (for the ALB) and private subnets (for EC2/RDS) across 2 AZs, with NAT gateways in the public subnets for outbound internet (E).

---

## Q46
**Full Question:** A company hosts its enterprise resource planning (ERP) system in the US East 1 region. The system runs on Amazon EC2 instances. Customers use a public API that is hosted on the EC2 instances to exchange information with the ERP system. International customers report slow API response times from their data centers. Which solution will improve response times for the international customers most cost-effectively?

**Short Question:** Public API in a single region, international customers see slow response times — fix cheaply.
- A. Direct Connect (public VIF) per customer data center + Direct Connect Gateway to the API
- B. CloudFront in front of the API with the caching-optimized managed cache policy
- C. AWS Global Accelerator with listeners/endpoint groups routing to the API ✅
- D. Site-to-Site VPN tunnels between regions and each customer network

**Reason:** Global Accelerator routes users onto the AWS backbone from the nearest edge — cheap, scalable, and works for non-cacheable API traffic (unlike CloudFront).

---

## Q47
**Full Question:** A company runs workloads on AWS. The company needs to connect to a service from an external provider. The service is hosted in the provider's VPC. According to the company's security team, the connectivity must be private and must be restricted to the target service. The connection must be initiated only from the company's VPC. Which solution will meet these requirements?

**Short Question:** Private, restricted-to-one-service connection from your VPC to a service in a third party's VPC.
- A. VPC peering + route table update
- B. Provider creates a virtual private gateway + AWS PrivateLink
- C. NAT gateway in a public subnet + route table update
- D. Provider creates a VPC endpoint service; you connect via AWS PrivateLink ✅

**Reason:** PrivateLink + a provider-side VPC endpoint service restricts access to exactly that one service over a private connection — VPC peering would expose the whole network.

---

## Q48
**Full Question:** A company wants to use high performance computing (HPC) infrastructure on AWS for financial risk modeling. The company's HPC workloads run on Linux. Each HPC workflow runs on hundreds of Amazon EC2 spot instances, is short-lived, and generates thousands of output files that are ultimately stored in persistent storage for analytics and long-term future use. The company seeks a cloud storage solution that permits the copying of on-premises data to long-term persistent storage to make data available for processing by all EC2 instances. The solution should also be a high performance file system that is integrated with persistent storage to read and write data sets and output files. Which combination of AWS services meets these requirements?

**Short Question:** Linux HPC on hundreds of spot instances needs a shared high-performance file system tied to long-term persistent storage.
- A. Amazon FSx for Lustre integrated with Amazon S3 ✅
- B. Amazon FSx for Windows File Server integrated with Amazon S3
- C. Amazon S3 Glacier integrated with Amazon EBS
- D. Amazon S3 bucket with a VPC endpoint integrated with an EBS GP2 volume

**Reason:** FSx for Lustre is a Linux-optimized high-performance shared file system with native S3 integration for long-term storage — exactly this use case.

---

## Q49
**Full Question:** A company recently started using Amazon Aurora as the data store for its global e-commerce application. When large reports are run, developers report that the e-commerce application is performing poorly. After reviewing metrics in Amazon CloudWatch, a solutions architect finds that the reads and CPU utilization metrics are spiking when monthly reports run. What is the most cost-effective solution?

**Short Question:** Monthly reports spike reads/CPU on the primary Aurora instance and slow down the live app — cheapest fix.
- A. Migrate monthly reporting to Amazon Redshift
- B. Migrate monthly reporting to an Aurora replica ✅
- C. Migrate the Aurora database to a larger instance class
- D. Increase provisioned IOPS on the Aurora instance

**Reason:** An Aurora read replica offloads the read-heavy reporting workload and only costs extra while it's running — cheaper than resizing or standing up Redshift.

---

## Q50
**Full Question:** A large media company hosts a web application on AWS. The company wants to start caching confidential media files so that users around the world will have reliable access to the files. The content is stored in Amazon S3 buckets. The company must deliver the content quickly regardless of where the requests originate geographically. Which solution will meet these requirements?

**Short Question:** Cache and reliably deliver confidential media files from S3 to a global audience, quickly.
- A. AWS DataSync to connect S3 to the web application
- B. AWS Global Accelerator to connect S3 to the web application
- C. Amazon CloudFront in front of the S3 buckets ✅
- D. Amazon SQS to connect S3 to the web application

**Reason:** CloudFront is the CDN that actually caches content at edge locations worldwide — the others don't cache at all.
