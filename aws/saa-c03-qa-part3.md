# AWS SAA-C03 Real Exam Questions & Answers — Part 3 (Q51–Q75)

Source: YouTube transcript (tactiq.io)

Each question has:
- **Full Question** — original question text
- **Short Question** — quick summary
- **Options** — ✅ marks correct answer
- **Reason** — why

---

## Q51
**Full Question:** A company wants to migrate its existing on-premises monolithic application to AWS. The company wants to keep as much of the front-end code and the backend code as possible. However, the company wants to break the application into smaller applications. A different team will manage each application. The company needs a highly scalable solution that minimizes operational overhead. Which solution will meet these requirements?

**Short Question:** Break a monolithic app into smaller team-owned apps, keep the existing code, highly scalable, low overhead.
- A. Host on Lambda + API Gateway
- B. Host with AWS Amplify + API Gateway + Lambda
- C. Host on EC2 + ALB + Auto Scaling group
- D. Host on ECS with an ALB targeting ECS ✅

**Reason:** Containerizing the existing code onto ECS lets separate teams own separate services without a full rewrite (unlike Lambda, which needs re-architecting).

---

## Q52
**Full Question:** A company is preparing to deploy a new serverless workload. A solutions architect must use the principle of least privilege to configure permissions that will be used to run an AWS Lambda function. An Amazon EventBridge (Amazon CloudWatch Events) rule will invoke the function. Which solution meets these requirements?

**Short Question:** Least-privilege permissions so an EventBridge rule can invoke a Lambda function.
- A. Execution role with lambda:InvokeFunction, principal `*`
- B. Execution role with lambda:InvokeFunction, principal `lambda.amazonaws.com`
- C. Resource-based policy with `lambda:*`, principal `events.amazonaws.com`
- D. Resource-based policy with lambda:InvokeFunction, principal `events.amazonaws.com` ✅

**Reason:** A resource-based policy (not an execution role) grants another service permission to invoke the function; scoping the action to InvokeFunction and the principal to EventBridge's service principal follows least privilege.

---

## Q53
**Full Question:** A company uses AWS Systems Manager for routine management and patching of Amazon EC2 instances. The EC2 instances are in an IP address type target group behind an Application Load Balancer (ALB). New security protocols require the company to remove EC2 instances from service during a patch. When the company attempts to follow the security protocol during the next patch, the company receives errors during the patching window. Which combination of solutions will resolve the errors? (Choose 2)

**Short Question:** Patching EC2 instances behind an ALB throws errors when trying to remove them from service first — fix it.
- A. Change target group type from IP address to instance
- B. Keep the existing Systems Manager document unchanged
- C. Use the AWS-provided `AWSEC2-PatchLoadBalancerInstance` Systems Manager automation document ✅
- D. Use Systems Manager Maintenance Windows to schedule the patch removal-from-service ✅
- E. Use State Manager to remove instances from service + rely on ALB health checks

**Reason:** Maintenance Windows schedules the patch; the purpose-built automation document correctly drains, patches, and re-registers the instance with the load balancer.

---

## Q54
**Full Question:** A company has implemented a self-managed DNS solution on three Amazon EC2 instances behind a Network Load Balancer (NLB) in the US West 2 region. Most of the company's users are located in the United States and Europe. The company wants to improve the performance and availability of the solution. The company launches and configures three EC2 instances in the EU West 1 region and adds the EC2 instances as targets for a new NLB. Which solution can the company use to route traffic to all the EC2 instances?

**Short Question:** Self-managed DNS (UDP) service now runs behind two regional NLBs (US + EU) — route users to the nearest one.
- A. Route 53 geolocation routing to one of the two NLBs + CloudFront in front
- B. AWS Global Accelerator standard accelerator with endpoint groups in both regions, NLBs as endpoints ✅
- C. Elastic IPs on all 6 instances + Route 53 geolocation + CloudFront
- D. Replace NLBs with ALBs + Route 53 latency routing + CloudFront

**Reason:** DNS traffic is UDP — CloudFront/ALB (HTTP-only) don't apply. Global Accelerator supports non-HTTP (UDP/TCP) traffic and routes to the nearest healthy regional NLB over the AWS network.

---

## Q55
**Full Question:** A company runs a production application on a fleet of Amazon EC2 instances. The application reads the data from an Amazon SQS queue and processes the messages in parallel. The message volume is unpredictable and often has intermittent traffic. This application should continually process messages without any downtime. Which solution meets these requirements most cost-effectively?

**Short Question:** EC2 fleet processes an SQS queue with unpredictable, bursty traffic; must never go down; cheapest setup.
- A. Spot instances exclusively for max capacity
- B. Reserved instances exclusively for max capacity
- C. Reserved instances for baseline + spot instances for extra capacity ✅
- D. Reserved instances for baseline + on-demand instances for extra capacity

**Reason:** Reserved covers the always-on baseline cheaply; spot is safe here because SQS buffers work, so interruptions don't cause downtime — and spot is cheaper than on-demand for the bursty part.

---

## Q56
**Full Question:** A company is migrating an application from on-premises servers to Amazon EC2 instances. As part of the migration design requirements, a solutions architect must implement infrastructure metric alarms. The company does not need to take action if CPU utilization increases to more than 50% for a short burst of time. However, if the CPU utilization increases to more than 50% and read/write ops on the disk are high at the same time, the company needs to act as soon as possible. The solutions architect also must reduce false alarms. What should the solutions architect do to meet these requirements?

**Short Question:** Alarm only when CPU AND disk I/O are both high at once — reduce false alarms.
- A. CloudWatch composite alarms ✅
- B. CloudWatch dashboards
- C. CloudWatch Synthetics canaries
- D. Single CloudWatch metric alarm with multiple thresholds

**Reason:** Composite alarms combine two separate alarms (CPU, disk I/O) with a logical AND — the only native way to alert on two metrics together.

---

## Q57
**Full Question:** A solutions architect must design a highly available infrastructure for a website. The website is powered by Windows web servers that run on Amazon EC2 instances. The solutions architect must implement a solution that can mitigate a large-scale DDoS attack that originates from thousands of IP addresses. Downtime is not acceptable for the website. Which actions should the solutions architect take to protect the website from such an attack? (Choose 2)

**Short Question:** Protect a no-downtime EC2 website from a large-scale DDoS attack from thousands of IPs.
- A. AWS Shield Advanced ✅
- B. GuardDuty to auto-block attackers
- C. CloudFront in front of both static and dynamic content ✅
- D. Lambda function to add attacker IPs to a network ACL
- E. Spot instances in an ASG with 80% CPU target tracking

<br>

**Reason:** Shield Advanced gives dedicated DDoS mitigation; CloudFront absorbs/distributes attack traffic at the edge before it reaches the origin — NACLs can't scale to thousands of IPs, and spot instances risk downtime.

---

## Q58
**Full Question:** A media company is evaluating the possibility of moving its systems to the AWS cloud. The company needs at least 10 TB of storage with the maximum possible IO performance for video processing, 300 TB of very durable storage for storing media content, and 900 TB of storage to meet requirements for archival media that is not in use anymore. Which set of services should a solutions architect recommend to meet these requirements?

**Short Question:** Need max-IOPS scratch storage, 300TB durable "hot" storage, and 900TB cold archive storage.
- A. EBS (max performance) + S3 (durable) + S3 Glacier (archival)
- B. EBS (max performance) + EFS (durable) + S3 Glacier (archival)
- C. EC2 instance store (max performance) + EFS (durable) + S3 (archival)
- D. EC2 instance store (max performance) + S3 (durable) + S3 Glacier (archival) ✅

**Reason:** Instance store gives the lowest-latency/highest-IOPS local disk for processing; S3 is the durable object store; Glacier is the purpose-built cheap archive tier.

---

## Q59
**Full Question:** A company has two applications: a sender application that sends messages with payloads to be processed and a processing application intended to receive the messages with payloads. The company wants to implement an AWS service to handle messages between the two applications. The sender application can send about 1,000 messages each hour. The messages may take up to 2 days to be processed. If the messages fail to process, they must be retained so that they do not impact the processing of any remaining messages. Which solution meets these requirements and is the most operationally efficient?

**Short Question:** Decouple two apps; messages can take up to 2 days to process; failed messages must not block the rest.
- A. Self-managed Redis on EC2
- B. Kinesis Data Stream + KCL
- C. SQS with a dead-letter queue ✅
- D. SNS topic subscription

**Reason:** SQS holds messages up to 14 days (covers the 2-day lag) and its dead-letter queue isolates failed messages automatically — fully managed, low volume fits SQS better than Kinesis.

---

## Q60
**Full Question:** A company runs an environment where data is stored in an Amazon S3 bucket. The objects are accessed frequently throughout the day. The company has strict data encryption requirements for data that is stored in the S3 bucket. The company currently uses AWS Key Management Service (AWS KMS) for encryption. The company wants to optimize costs associated with encrypting S3 objects without making additional calls to AWS KMS. Which solution will meet these requirements?

**Short Question:** Cut S3-KMS encryption costs (too many KMS API calls) without dropping KMS entirely.
- A. Switch to SSE-S3
- B. S3 Bucket Key with SSE-KMS on new objects ✅
- C. Client-side encryption with KMS customer managed keys
- D. SSE-C with the key stored in KMS

**Reason:** S3 Bucket Keys generate a short-lived bucket-level key from KMS and reuse it for many objects, cutting KMS API calls (and cost) by up to 99% while staying on SSE-KMS.

---

## Q61
**Full Question:** A company's web application is running on Amazon EC2 instances behind an Application Load Balancer. The company recently changed its policy which now requires the application to be accessed from one specific country only. Which configuration will meet this requirement?

**Short Question:** Restrict a web app (EC2 + ALB) to users from one specific country only.
- A. EC2 security group
- B. ALB security group
- C. AWS WAF on the ALB (geo match rule) ✅
- D. Network ACL on the EC2 subnet

**Reason:** Only WAF has a geo-match rule that filters requests by the country they originate from — security groups/NACLs only understand IPs, not geography.

---

## Q62
**Full Question:** A company needs to store data in Amazon S3 and must prevent the data from being changed. The company wants new objects that are uploaded to Amazon S3 to remain unchangeable for a non-specific amount of time until the company decides to modify the objects. Only specific users in the company's AWS account can have the ability to delete the objects. What should a solutions architect do to meet these requirements?

**Short Question:** New S3 objects must stay unmodifiable indefinitely until the company decides otherwise; only specific users may ever delete them.
- A. S3 Glacier vault + WORM vault lock policy
- B. S3 Object Lock + versioning + 100-year retention in governance mode
- C. S3 bucket + CloudTrail tracking + manual restore from backups on change
- D. S3 Object Lock + versioning + legal hold + grant `s3:PutObjectLegalHold` only to specific users ✅

**Reason:** A legal hold (not a fixed retention period) matches "non-specific amount of time," and giving only certain users the permission to remove the hold controls who can ever unlock/delete the object.

---

## Q63
**Full Question:** A company is running a publicly accessible serverless application that uses Amazon API Gateway and AWS Lambda. The application's traffic recently spiked due to fraudulent requests from botnets. Which steps should a solutions architect take to block requests from unauthorized users? (Choose 2)

**Short Question:** Public serverless API (API Gateway + Lambda) is getting hit by botnet traffic — block unauthorized requests.
- A. Usage plan with an API key shared only with genuine users ✅
- B. Add IP-filtering logic inside the Lambda function
- C. AWS WAF rule targeting malicious requests ✅
- D. Convert the public API to a private API
- E. Create an IAM role per end user

**Reason:** API keys/usage plans gate access for known legitimate clients; WAF (with bot control/managed rules) blocks malicious traffic at the edge before it ever reaches Lambda — cheaper and more effective than filtering inside the function.

---

## Q64
**Full Question:** A company uses a three-tier web application to provide training to new employees. The application is accessed for only 12 hours every day. The company is using an Amazon RDS for MySQL DB instance to store information and wants to minimize costs. What should a solutions architect do to meet these requirements?

**Short Question:** RDS MySQL is only needed 12 hours/day — automate stop/start to save money.
- A. Systems Manager Session Manager IAM setup for auto start/stop
- B. ElastiCache for Redis as a stand-in while the DB is stopped
- C. EC2 instance + IAM role + cron job to start/stop the EC2 instance
- D. Lambda functions to start/stop the DB, invoked by EventBridge scheduled rules ✅

**Reason:** EventBridge-scheduled Lambda functions are fully serverless and directly start/stop the RDS instance on a schedule — least overhead, no extra infrastructure to manage.

---

## Q65
**Full Question:** A company runs an application in a private subnet behind an Application Load Balancer (ALB) in a VPC. The VPC has a NAT gateway and an internet gateway. The application calls the Amazon S3 API to store objects. According to the company's security policy, traffic from the application must not travel across the internet. Which solution will meet these requirements most cost-effectively?

**Short Question:** App in a private subnet calls S3 — keep that traffic off the public internet, cheapest way.
- A. S3 interface endpoint + security group
- B. S3 gateway endpoint + update route table ✅
- C. S3 bucket policy allowing the NAT gateway's Elastic IP
- D. Second NAT gateway in the same subnet

**Reason:** A gateway endpoint for S3 is free and routes S3 traffic privately via the route table — an interface endpoint works too but costs money, so it's not the most cost-effective choice.

---

## Q66
**Full Question:** A company uses a popular content management system (CMS) for its corporate website. However, the required patching and maintenance are burdensome. The company is redesigning its website and wants a new solution. The website will be updated four times a year and does not need to have any dynamic content available. The solution must provide high scalability and enhanced security. Which combination of changes will meet these requirements with the least operational overhead? (Choose 2)

**Short Question:** Replace a high-maintenance CMS with a rarely-updated, fully static site — scalable, secure, low overhead.
- A. CloudFront in front of the site for HTTPS ✅
- B. WAF web ACL in front of the site to "provide HTTPS"
- C. Lambda function to serve website content
- D. New S3 bucket with static website hosting enabled ✅
- E. New site on an EC2 Auto Scaling group behind an ALB

**Reason:** S3 static website hosting needs no servers at all; CloudFront in front adds HTTPS, global caching, and extra security — matching a rarely-updated, no-dynamic-content site with minimal ops.

---

## Q67
**Full Question:** A company wants to run applications in containers in the AWS cloud. These applications are stateless and can tolerate disruptions within the underlying infrastructure. The company needs a solution that minimizes cost and operational overhead. What should a solutions architect do to meet these requirements?

**Short Question:** Stateless, interruption-tolerant containerized apps — cheapest option with least ops overhead.
- A. Spot instances in an EC2 Auto Scaling group (self-managed containers)
- B. Spot instances in an EKS managed node group ✅
- C. On-demand instances in an EC2 Auto Scaling group
- D. On-demand instances in an EKS managed node group

**Reason:** EKS managed node group removes cluster/node management overhead, and spot instances are safe (and much cheaper) since the workload is stateless and interruption-tolerant.

---

## Q68
**Full Question:** A company is implementing a new business application. The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage. A solutions architect needs to ensure that the EC2 instances can access the S3 bucket. What should the solutions architect do to meet this requirement?

**Short Question:** Let two EC2 instances access an S3 bucket, the secure/standard way.
- A. IAM role granting S3 access, attached to the EC2 instances ✅
- B. IAM policy attached directly to the EC2 instances
- C. IAM group attached to the EC2 instances
- D. IAM user attached to the EC2 instances

**Reason:** Only an IAM role can be attached to an EC2 instance, giving it temporary credentials — policies/groups/users can't be attached directly, and a user would mean storing long-term credentials on the instance (bad practice).

---

## Q69
**Full Question:** A company's production environment consists of Amazon EC2 on-demand instances that run constantly between Monday and Saturday. The instances must run for only 12 hours on Sunday and cannot tolerate interruptions. The company wants to cost optimize the production environment. Which solution will meet these requirements most cost-effectively?

**Short Question:** EC2 fleet runs continuously Mon–Sat, plus a fixed 12-hour Sunday run with zero tolerance for interruption — cheapest setup.
- A. Scheduled Reserved Instances for the Sunday 12-hour run + Standard Reserved Instances for Mon–Sat ✅
- B. Convertible Reserved Instances for Sunday + Standard Reserved Instances for Mon–Sat
- C. Spot instances for Sunday + Standard Reserved Instances for Mon–Sat
- D. Spot instances for Sunday + Convertible Reserved Instances for Mon–Sat

**Reason:** Standard RIs are the cheapest option for a continuous predictable workload; Scheduled RIs are purpose-built for a fixed recurring time window like the 12-hour Sunday run — and spot is ruled out since Sunday can't tolerate interruptions.

---

## Q70
**Full Question:** A company needs to save the results from a medical trial to an Amazon S3 repository. The repository must allow a few scientists to add new files and must restrict all other users to read-only access. No users can have the ability to modify or delete any files in the repository. The company must keep every file in the repository for a minimum of one year after its creation date. Which solution will meet these requirements?

**Short Question:** S3 repository: nobody can modify/delete files, everything kept at least 1 year, only a few users can add new files.
- A. S3 Object Lock, governance mode, legal hold of 1 year
- B. S3 Object Lock, compliance mode, 365-day retention period ✅
- C. IAM role restricting delete/modify + bucket policy allowing only that role
- D. Lambda function tracking object hashes to flag modified objects

**Reason:** A fixed 365-day retention period (not a legal hold) matches "minimum of one year," and compliance mode is the only mode nobody — not even the root user — can override before it expires.

---

## Q71
**Full Question:** A company hosts a website analytics application on a single Amazon EC2 on-demand instance. The analytics application is highly resilient and is designed to run in stateless mode. The company notices that the application is showing signs of performance degradation during busy times and is presenting 5xx errors. The company needs to make the application scale seamlessly. Which solution will meet these requirements most cost-effectively?

**Short Question:** Stateless analytics app on one EC2 instance throws 5xx errors under load — scale it seamlessly, cheaply.
- A. AMI + second on-demand EC2 instance + ALB
- B. AMI + second on-demand EC2 instance + Route 53 weighted routing
- C. Lambda function that stops the instance and changes its type on a CloudWatch alarm
- D. AMI + launch template + Auto Scaling group (spot fleet) + ALB ✅

**Reason:** An Auto Scaling group gives real seamless scaling, and since the app is stateless/resilient, a spot fleet is the cheapest compute option — a fixed second instance (A/B) can't scale, and C causes downtime.

---

## Q72
**Full Question:** A company is developing a file sharing application that will use an Amazon S3 bucket for storage. The company wants to serve all the files through an Amazon CloudFront distribution. The company does not want the files to be accessible through direct navigation to the S3 URL. What should a solutions architect do to meet these requirements?

**Short Question:** Serve S3 files only via CloudFront; block direct access to the S3 URL.
- A. Write individual per-bucket policies granting only "CloudFront" access
- B. Create an IAM user, grant it S3 read access, "assign" it to CloudFront
- C. Bucket policy with the CloudFront distribution ID as the principal
- D. Origin Access Identity (OAI) assigned to CloudFront; bucket permissions allow only the OAI ✅

**Reason:** OAI is CloudFront's special identity for this exact purpose — you can't grant an S3 bucket policy access directly to a distribution ID or an IAM user for this use case.

---

## Q73
**Full Question:** Organizers for a global event want to put daily reports online as static HTML pages. The pages are expected to generate millions of views from users around the world. The files are stored in an Amazon S3 bucket. A solutions architect has been asked to design an efficient and effective solution. Which action should the solutions architect take to accomplish this?

**Short Question:** Serve static HTML report pages from S3 to millions of global viewers — efficiently.
- A. Generate pre-signed URLs for the files
- B. Cross-region replication to all regions
- C. Route 53 geoproximity routing
- D. CloudFront with the S3 bucket as its origin ✅

**Reason:** CloudFront is the CDN that actually caches and serves content from edge locations worldwide at scale — the other options don't cache or aren't built for public mass distribution.

---

## Q74
**Full Question:** A company wants to migrate an on-premises data center to AWS. The data center hosts an SFTP server that stores its data on an NFS-based file system. The server holds 200 GB of data that needs to be transferred. The server must be hosted on an Amazon EC2 instance that uses an Amazon Elastic File System (Amazon EFS) file system. Which combination of steps should a solutions architect take to automate this task? (Choose 2)

**Short Question:** Automate moving 200GB from an on-prem NFS-based SFTP server to an EC2 instance backed by EFS.
- A. Launch the EC2 instance in the same AZ as the EFS file system
- B. Install an AWS DataSync agent in the on-premises data center ✅
- C. Create a secondary EBS volume on the EC2 instance for the data
- D. Manually use an OS copy command to push the data
- E. Use AWS DataSync to create a location configuration for the on-premises SFTP/NFS server ✅

**Reason:** DataSync is the managed automated transfer tool: an on-prem agent reads the NFS source, and a DataSync location configuration defines where to read from/write to (EFS) — no manual copying or EBS involved.

---

## Q75
**Full Question:** A company hosts a containerized web application on a fleet of on-premises servers that process incoming requests. The number of requests is growing quickly. The on-premises servers cannot handle the increased number of requests. The company wants to move the application to AWS with minimum code changes and minimum development effort. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Move a growing containerized web app off overloaded on-prem servers to AWS — minimal code changes, least overhead.
- A. ECS on Fargate with service Auto Scaling + ALB ✅
- B. Two EC2 instances + ALB
- C. Rewrite as Lambda functions behind API Gateway
- D. AWS ParallelCluster (HPC) to process requests

**Reason:** The app is already containerized, so Fargate on ECS requires no rewrite and removes all server management — Lambda (C) means a rewrite, and B doesn't scale automatically.
