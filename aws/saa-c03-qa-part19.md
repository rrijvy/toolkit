# AWS SAA-C03 Real Exam Questions & Answers — Part 19 (Q441–Q460)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 19](https://www.youtube.com/watch/epTFIcCvLrY)

---

## Question 441

**Full Question:** A company is hosting a three-tier e-commerce application in the AWS Cloud. The company hosts the website on Amazon S3 and integrates the website with an API that handles sales requests. The company hosts the API on three Amazon EC2 instances behind an Application Load Balancer (ALB). The API consists of static and dynamic front-end content along with back-end workers that process sales requests asynchronously. The company is expecting a significant and sudden increase in the number of sales requests during events for the launch of new products. What should a solutions architect recommend to ensure that all the requests are processed successfully?

**Short Question:** How do you make sure a sudden traffic spike on a sales API doesn't cause lost requests?

**Options:**
- A. Add an Amazon CloudFront distribution for the dynamic content. Increase the number of EC2 instances to handle the increase in traffic.
- B. Add an Amazon CloudFront distribution for the static content. Place the EC2 instances in an Auto Scaling group to launch new instances based on network traffic.
- C. Add an Amazon CloudFront distribution for the dynamic content. Add an Amazon ElastiCache instance in front of the ALB to reduce traffic for the API to handle.
- ✅ D. Add an Amazon CloudFront distribution for the static content. Add an Amazon SQS queue to receive requests from the website for later processing by the EC2 instances.

**Reason:** An SQS queue decouples the front end from the back-end processing, durably buffering sudden spikes so no sales requests are lost while EC2 instances process them at a sustainable rate; CloudFront for static content further reduces load. Manual scaling, reactive Auto Scaling, and ElastiCache (an in-memory cache, not a request buffer) can all still drop requests during a sudden spike.

---

## Question 442

**Full Question:** A company designed a stateless two-tier application that uses Amazon EC2 in a single Availability Zone and an Amazon RDS Multi-AZ DB instance. New company management wants to ensure the application is highly available. What should a solutions architect do to meet this requirement?

**Short Question:** How do you fix a single-AZ EC2 tier to make the whole application highly available?

**Options:**
- ✅ A. Configure the application to use multi-AZ EC2 Auto Scaling and create an Application Load Balancer.
- B. Configure the application to take snapshots of the EC2 instances and send them to a different AWS Region.
- C. Configure the application to use Amazon Route 53 latency-based routing to feed requests to the application.
- D. Configure Amazon Route 53 rules to handle incoming requests and create a multi-AZ Application Load Balancer.

**Reason:** The compute tier is a single point of failure because it runs in only one Availability Zone, so it needs an Auto Scaling group spanning multiple AZs plus an ALB to distribute traffic across them. Snapshotting to another region is disaster recovery, not high availability, and both Route 53 options fail to fix the underlying problem since the EC2 instances are never spread across multiple AZs.

---

## Question 443

**Full Question:** A company plans to use Amazon ElastiCache for its multi-tier web application. A solutions architect creates a cache VPC for the ElastiCache cluster and an app VPC for the application's Amazon EC2 instances. Both VPCs are in the us-east-1 Region. The solutions architect must implement a solution to provide the application's EC2 instances with access to the ElastiCache cluster. Which solution will meet these requirements most cost-effectively?

**Short Question:** What's the cheapest way to connect two same-region VPCs so EC2 instances can reach an ElastiCache cluster in another VPC?

**Options:**
- ✅ A. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the ElastiCache cluster security group to allow inbound connection from the application security group.
- B. Create a transit VPC. Update the VPC route tables in the cache VPC and the app VPC to route traffic through the transit VPC. Configure an inbound rule for the ElastiCache cluster security group to allow inbound connection from the application security group.
- C. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the peering connection security group to allow inbound connection from the application security group.
- D. Create a transit VPC. Update the VPC route tables in the cache VPC and the app VPC to route traffic through the transit VPC. Configure an inbound rule for the transit VPC security group to allow inbound connection from the application security group.

**Reason:** VPC peering is the simplest and most cost-effective way to connect just two VPCs in the same region, with no hourly connection charges, and the security group rule correctly belongs on the ElastiCache cluster itself. A transit VPC is unnecessary overkill for two VPCs, and peering connections don't have their own security groups to configure.

---

## Question 444

**Full Question:** An image hosting company uploads its large assets to Amazon S3 Standard buckets. The company uses multipart upload in parallel by using S3 APIs and overwrites if the same object is uploaded again. For the first 30 days after upload, the objects will be accessed frequently. The objects will be used less frequently after 30 days, but the access patterns for each object will be inconsistent. The company must optimize its S3 storage costs while maintaining high availability and resiliency of stored assets. Which combination of actions should a solutions architect recommend to meet these requirements? (Choose two.)

**Short Question:** Which two S3 cost-optimization actions fit unpredictable access patterns after 30 days plus multipart uploads, while keeping data highly available?

**Options:**
- ✅ A. Move assets to S3 Intelligent-Tiering after 30 days.
- ✅ B. Configure an S3 lifecycle policy to clean up incomplete multipart uploads.
- C. Configure an S3 lifecycle policy to clean up expired object delete markers.
- D. Move assets to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.
- E. Move assets to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days.

**Reason:** S3 Intelligent-Tiering automatically handles data with unpredictable access patterns without retrieval fees, and cleaning up incomplete multipart uploads via a lifecycle policy removes hidden storage costs from failed uploads. Delete marker cleanup only applies to versioned buckets (not mentioned here), Standard-IA is meant for predictable infrequent access (risking retrieval fees on unpredictable spikes), and One Zone-IA fails the high-availability/resiliency requirement since it stores data in only one AZ.

---

## Question 445

**Full Question:** A company uses on-premises servers to host its applications. The company is running out of storage capacity. The applications use both block storage and NFS storage. The company needs a high-performing solution that supports local caching without rearchitecting its existing applications. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two hybrid storage services extend on-premises block and NFS storage to AWS with local caching, without changing the applications?

**Options:**
- A. Mount Amazon S3 as a file system to the on-premises servers.
- ✅ B. Deploy an AWS Storage Gateway file gateway to replace NFS storage.
- C. Deploy AWS Snowball Edge to provision NFS mounts to on-premises servers.
- ✅ D. Deploy an AWS Storage Gateway volume gateway to replace the block storage.
- E. Deploy Amazon Elastic File System (Amazon EFS) volumes, and mount them to on-premises servers.

**Reason:** An S3 File Gateway provides a standard NFS/SMB interface backed by S3 with local caching, and a Volume Gateway (in cached mode) provides standard iSCSI block storage backed by S3 with local caching — together covering both storage types without any application changes. S3 can't be natively mounted as a file system, Snowball Edge is for data migration/edge computing rather than permanent hybrid storage, and EFS alone only addresses the NFS need, leaving block storage unresolved.

---

## Question 446

**Full Question:** A company hosts a front-end application that uses an Amazon API Gateway API backend that is integrated with AWS Lambda. When the API receives requests, the Lambda function loads many libraries, then connects to an Amazon RDS database, processes the data, and returns the data to the front-end application. The company wants to ensure that response latency is as low as possible for all its users with the fewest number of changes to the company's operations. Which solution will meet these requirements?

**Short Question:** How do you eliminate Lambda cold-start latency with minimal operational changes?

**Options:**
- A. Establish a connection between the front-end application and the database to make queries faster, bypassing the API
- ✅ B. Configure provisioned concurrency for the Lambda function that handles the requests
- C. Cache the results of the queries in Amazon S3 for faster retrieval of similar data sets
- D. Increase the size of the database to increase the number of connections Lambda can establish at one time

**Reason:** The described symptoms (loading libraries and connecting to the database on each invocation) are classic Lambda cold-start behavior, and provisioned concurrency keeps execution environments pre-initialized and warm so requests skip that startup latency entirely. Bypassing the API exposes the database insecurely, S3 caching requires significant code changes, and a bigger database doesn't address initialization latency.

---

## Question 447

**Full Question:** A company uses AWS Organizations to run workloads within multiple AWS accounts. A member account has purchased a Compute Savings Plan. Because of changes in the workloads inside the member account, the account no longer receives the full benefit of the Compute Savings Plan commitment. The company uses less than 50% of its purchased compute power. Which solution will resolve this issue?

**Short Question:** How do you let an underused Savings Plan in one member account benefit other accounts in the organization?

**Options:**
- A. Turn on discount sharing from the billing preferences section of the account console in the member account that purchased the compute savings plan
- ✅ B. Turn on discount sharing from the billing preferences section of the account console in the company's organization's management account
- C. Migrate additional compute workloads from another AWS account to the account that has the compute savings plan
- D. Sell the excess savings plan commitment in the reserved instance marketplace

**Reason:** Discount sharing across an AWS Organization is a centralized setting controlled only from the management (payer) account, and turning it on lets the unused Savings Plan discount automatically apply to eligible usage elsewhere in the organization. Migrating workloads is unnecessarily complex, and Savings Plans (unlike certain Reserved Instances) cannot be sold on the Reserved Instance Marketplace.

---

## Question 448

**Full Question:** A company has multiple Windows file servers on premises. The company wants to migrate and consolidate its files into an Amazon FSx for Windows File Server file system. File permissions must be preserved to ensure that access rights do not change. Which solutions will meet these requirements? (Choose two.)

**Short Question:** Which two migration methods to FSx for Windows File Server preserve Windows ACL permissions?

**Options:**
- ✅ A. Deploy AWS DataSync agents on premises. Schedule DataSync tasks to transfer the data to the FSx for Windows File Server file system
- B. Copy the shares on each file server into Amazon S3 buckets by using the AWS CLI. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system
- C. Remove the drives from each file server. Ship the drives to AWS for import into Amazon S3. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system
- ✅ D. Order an AWS Snowcone device. Connect the device to the on-premises network. Launch AWS DataSync agents on the device. Schedule DataSync tasks to transfer the data to the FSx for Windows File Server file system
- E. Order an AWS Snowball Edge Storage Optimized device. Connect the device to the on-premises network. Copy data to the device by using the AWS CLI. Ship the device back to AWS for import into Amazon S3. Schedule AWS DataSync tasks to transfer the data to the FSx for Windows File Server file system

**Reason:** AWS DataSync is purpose-built to preserve metadata, including Windows ACLs, during transfer, whether run online via an on-premises agent (A) or offline via an agent on a Snowcone device (D). Options B, C, and E all route the data through Amazon S3 using the CLI or standard file import, which does not support Windows file system permissions and strips the ACLs.

---

## Question 449

**Full Question:** A solutions architect needs to optimize storage costs. The solutions architect must identify any Amazon S3 buckets that are no longer being accessed or are rarely accessed. Which solution will accomplish this goal with the least operational overhead?

**Short Question:** What's the lowest-effort way to find unused or rarely accessed S3 buckets?

**Options:**
- ✅ A. Analyze bucket access patterns by using the S3 Storage Lens dashboard for advanced activity metrics
- B. Analyze bucket access patterns by using the S3 dashboard in the AWS Management Console
- C. Turn on the Amazon CloudWatch BucketSizeBytes metric for buckets. Analyze bucket access patterns by using the metrics data with Amazon Athena
- D. Turn on AWS CloudTrail for S3 object monitoring. Analyze bucket access patterns by using CloudTrail logs that are integrated with Amazon CloudWatch Logs

**Reason:** Amazon S3 Storage Lens is purpose-built for exactly this use case, providing an organization-wide dashboard with advanced activity metrics (like request counts and retrieval rates) to spot cold buckets with minimal setup. The standard console dashboard lacks detailed access metrics, Athena can't query CloudWatch metrics, and enabling CloudTrail data events plus manual log analysis is far higher overhead.

---

## Question 450

**Full Question:** A company wants to implement a disaster recovery plan for its primary on-premises file storage volume. The file storage volume is mounted from an internet Small Computer Systems Interface (iSCSI) device on a local storage server. The file storage volume holds hundreds of terabytes (TB) of data. The company wants to ensure that end users retain immediate access to all file types from the on-premises systems without experiencing latency. Which solution will meet these requirements with the least amount of change to the company's existing infrastructure?

**Short Question:** What DR setup backs up an on-premises iSCSI volume to AWS while keeping all data locally available with zero latency?

**Options:**
- A. Provision an Amazon S3 File Gateway as a virtual machine (VM) that is hosted on premises. Set the local cache to 10 TB. Modify existing applications to access the files through the NFS protocol. To recover from a disaster, provision an Amazon EC2 instance and mount the S3 bucket that contains the files
- B. Provision an AWS Storage Gateway Tape Gateway. Use a data backup solution to back up all existing data to a virtual tape library. Configure the data backup solution to run nightly after the initial backup is complete. To recover from a disaster, provision an Amazon EC2 instance and restore the data to an Amazon Elastic Block Store (Amazon EBS) volume from the volumes in the virtual tape library
- C. Provision an AWS Storage Gateway Volume Gateway cached volume. Set the local cache to 10 TB. Mount the Volume Gateway cached volume to the existing file server by using iSCSI and copy all files to the storage volume. Configure scheduled snapshots of the storage volume. To recover from a disaster, restore a snapshot to an Amazon EBS volume, and attach the EBS volume to an Amazon EC2 instance
- ✅ D. Provision an AWS Storage Gateway Volume Gateway stored volume with the same amount of disk space as the existing file storage volume. Mount the Volume Gateway stored volume to the existing file server by using iSCSI and copy all files to the storage volume. Configure scheduled snapshots of the storage volume. To recover from a disaster, restore a snapshot to an Amazon EBS volume, and attach the EBS volume to an Amazon EC2 instance

**Reason:** A Volume Gateway in stored volume mode keeps the entire data set on premises for immediate low-latency access while asynchronously backing up point-in-time snapshots to Amazon S3 for disaster recovery, matching both requirements with minimal architecture change. The File Gateway (A) requires switching to NFS and rearchitecting applications, the Tape Gateway (B) is for archival backup rather than live iSCSI access, and the cached volume mode (C) stores the primary data in S3 with only a partial local cache, which breaks the "immediate access to all data" requirement.

---

## Question 451

**Full Question:** A company is building an application that consists of several microservices. The company has decided to use container technologies to deploy its software on AWS. The company needs a solution that minimizes the amount of ongoing effort for maintenance and scaling. The company cannot manage additional infrastructure. Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two actions deploy microservices in containers on AWS with the least infrastructure management?

**Options:**
- ✅ A. Deploy an Amazon Elastic Container Service (Amazon ECS) cluster
- B. Deploy the Kubernetes control plane on Amazon EC2 instances that span multiple availability zones
- C. Deploy an Amazon ECS service with an Amazon EC2 launch type. Specify a desired task number level of greater than or equal to two
- ✅ D. Deploy an Amazon ECS service with a Fargate launch type. Specify a desired task number level of greater than or equal to two
- E. Deploy Kubernetes worker nodes on Amazon EC2 instances that span multiple availability zones. Create a deployment that specifies two or more replicas for each microservice

**Reason:** Creating an ECS cluster (A) plus running the service on the serverless Fargate launch type (D) with at least two tasks gives a fully managed, highly available container platform with no servers to patch or scale. The EC2 launch type and any self-managed Kubernetes option (B, C, E) all require the company to manage underlying EC2 worker nodes, which violates the "no additional infrastructure" requirement.

---

## Question 452

**Full Question:** A company stores raw collected data in an Amazon S3 bucket. The data is used for several types of analytics on behalf of the company's customers. The type of analytics requested determines the access pattern on the S3 objects. The company cannot predict or control the access pattern. The company wants to reduce its S3 costs. Which solution will meet these requirements?

**Short Question:** How do you cut S3 storage costs when object access patterns are unknown and unpredictable?

**Options:**
- A. Use S3 replication to transition infrequently accessed objects to S3 Standard-Infrequent Access (S3 Standard-IA)
- B. Use S3 lifecycle rules to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA)
- ✅ C. Use S3 lifecycle rules to transition objects from S3 Standard to S3 Intelligent-Tiering
- D. Use S3 inventory to identify and transition objects that have not been accessed from S3 Standard to S3 Intelligent-Tiering

**Reason:** S3 Intelligent-Tiering automatically monitors each object's access pattern and moves it between frequent and infrequent access tiers, which is exactly suited for unpredictable access — enrolling new objects via a lifecycle rule is the standard, automated way to do this. S3 replication only copies data to another bucket (it doesn't change storage class), age-based lifecycle rules to Standard-IA ignore actual access patterns and risk retrieval fees, and S3 inventory only reports on objects without performing any transition itself.

---

## Question 453

**Full Question:** A company is hosting a web application from an Amazon S3 bucket. The application uses Amazon Cognito as an identity provider to authenticate users and return a JSON Web Token (JWT) that provides access to protected resources that are stored in another S3 bucket. Upon deployment of the application, users report errors and are unable to access the protected content. A solutions architect must resolve this issue by providing proper permissions so that users can access the protected content. Which solution meets these requirements?

**Short Question:** How do you fix an Amazon Cognito-authenticated app so users get proper permissions to protected S3 content?

**Options:**
- ✅ A. Update the Amazon Cognito identity pool to assume the proper IAM role for access to the protected content
- B. Update the S3 ACL to allow the application to access the protected content
- C. Redeploy the application to Amazon S3 to prevent eventually consistent reads in the S3 bucket from affecting the ability of users to access the protected content
- D. Update the Amazon Cognito pool to use custom attribute mappings within the identity pool and grant users the proper permissions to access the protected content

**Reason:** A Cognito identity pool grants authenticated users temporary AWS credentials whose permissions come from an attached IAM role, so configuring that role with the correct S3 permissions (e.g., s3:GetObject) is the correct fix. S3 ACLs are a legacy mechanism not meant for Cognito integration, the issue isn't S3's eventual consistency, and attribute mappings only map user attributes rather than granting IAM permissions.

---

## Question 454

**Full Question:** A company is hosting a static website on Amazon S3 and is using Amazon Route 53 for DNS. The website is experiencing increased demand from around the world. The company must decrease latency for users who access the website. Which solution meets these requirements most cost-effectively?

**Short Question:** What's the cheapest way to reduce latency for a globally accessed static website hosted on S3?

**Options:**
- A. Replicate the S3 bucket that contains the website to all AWS regions. Add Route 53 geolocation routing entries
- B. Provision accelerators in AWS Global Accelerator. Associate the supplied IP addresses with the S3 bucket. Edit the Route 53 entries to point to the IP addresses of the accelerators
- ✅ C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution
- D. Enable S3 Transfer Acceleration on the bucket. Edit the Route 53 entries to point to the new endpoint

**Reason:** Amazon CloudFront is a global CDN that caches static content at edge locations close to users, which is the standard, most cost-effective way to reduce latency for a static S3-hosted site. Replicating the bucket to every region is expensive and complex, AWS Global Accelerator targets dynamic TCP/UDP applications rather than static content caching, and S3 Transfer Acceleration only speeds up uploads to the bucket, not content delivery to viewers.

---

## Question 455

**Full Question:** A company has applications hosted on Amazon EC2 instances with IPv6 addresses. The applications must initiate communications with other external applications using the internet. However, the company's security policy states that any external service cannot initiate a connection to the EC2 instances. What should a solutions architect recommend to resolve this issue?

**Short Question:** How do you give IPv6 EC2 instances outbound-only internet access while blocking inbound connections?

**Options:**
- A. Create a NAT gateway and make it the destination of the subnet's route table
- B. Create an internet gateway and make it the destination of the subnet's route table
- C. Create a virtual private gateway and make it the destination of the subnet's route table
- ✅ D. Create an egress-only internet gateway and make it the destination of the subnet's route table

**Reason:** An egress-only internet gateway is purpose-built for IPv6 to allow outbound-only internet access while blocking any inbound connection attempts from the internet. A NAT gateway only works for IPv4, a regular internet gateway allows both inbound and outbound traffic (violating the security policy), and a virtual private gateway is for VPN/Direct Connect links to on-premises networks, not general internet access.

---

## Question 456

**Full Question:** A company runs an e-commerce application in the AWS Cloud that is integrated with an on-premises warehouse solution. The company uses Amazon Simple Notification Service (Amazon SNS) to send order messages to an on-premises HTTPS endpoint so the warehouse application can process the orders. The local data center team has detected that some of the order messages were not received. A solutions architect needs to retain messages that are not delivered and analyze the messages for up to 14 days. Which solution will meet these requirements with the least development effort?

**Short Question:** What's the lowest-effort way to capture and retain SNS messages that fail to deliver to an on-premises HTTPS endpoint for up to 14 days?

**Options:**
- A. Configure an Amazon SNS dead-letter queue that has an Amazon Kinesis Data Streams target with a retention period of 14 days
- B. Add an Amazon Simple Queue Service (Amazon SQS) queue with a retention period of 14 days between the application and Amazon SNS
- ✅ C. Configure an Amazon SNS dead-letter queue that has an Amazon SQS target with a retention period of 14 days
- D. Configure an Amazon SNS dead-letter queue that has an Amazon DynamoDB target with a TTL attribute set for a retention period of 14 days

**Reason:** SNS dead-letter queues only support Amazon SQS as a target, so a DLQ pointing to an SQS queue with 14-day retention is the purpose-built, low-effort solution; Kinesis Data Streams and DynamoDB are not valid DLQ targets, and adding a queue before SNS does nothing to capture failures that occur during delivery to the on-premises endpoint.

---

## Question 457

**Full Question:** A company needs to minimize the cost of its 1 Gbps AWS Direct Connect connection. The company's average connection utilization is less than 10%. A solutions architect must recommend a solution that will reduce the cost without compromising security. Which solution will meet these requirements?

**Short Question:** How can a company cut costs on a 1 Gbps Direct Connect connection that's barely used, without sacrificing security?

**Options:**
- A. Set up a new 1 Gbps Direct Connect connection. Share the connection with another AWS account
- B. Set up a new 200 Mbps Direct Connect connection in the AWS Management Console
- C. Contact an AWS Direct Connect Partner to order a 1 Gbps connection. Share the connection with another AWS account
- ✅ D. Contact an AWS Direct Connect Partner to order a 200 Mbps hosted connection for an existing AWS account

**Reason:** Dedicated Direct Connect connections from AWS only come in 1 Gbps or larger, so smaller, more granular bandwidth (like 200 Mbps) must be ordered as a hosted connection through an AWS Direct Connect Partner, which rightsizes the connection to actual usage and lowers cost while keeping the same security model.

---

## Question 458

**Full Question:** A company is developing an application that provides marketing services to stores. The services are based on previous purchases by store customers. The stores upload transaction data to the company through SFTP, and the data is processed and analyzed to generate new marketing offers. Some of the files can exceed 200 GB in size. Recently, the company discovered that some of the stores have uploaded files that contain personally identifiable information (PII) that should not have been included. The company wants administrators to be alerted if PII is shared again. The company also wants to automate remediation. What should a solutions architect do to meet these requirements with the least development effort?

**Short Question:** What's the lowest-effort way to automatically detect PII in files uploaded to S3 and alert administrators?

**Options:**
- A. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Inspector to scan the objects in the bucket. If objects contain PII, trigger an S3 lifecycle policy to remove the objects that contain PII
- ✅ B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon SNS to trigger a notification to the administrators to remove the objects that contain PII
- C. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon SNS to trigger a notification to the administrators to remove the objects that contain PII
- D. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Email Service (Amazon SES) to trigger a notification to the administrators and trigger an S3 lifecycle policy to remove the objects that contain PII

**Reason:** Amazon Macie is a fully managed service purpose-built to use machine learning to discover sensitive data like PII in S3 buckets, and it can feed findings through EventBridge to SNS for administrator notification — this requires far less development effort than custom Lambda scanning logic, and Amazon Inspector is for scanning compute workloads (like EC2) for vulnerabilities, not S3 objects for PII.

---

## Question 459

**Full Question:** A company uses a fleet of Amazon EC2 instances to ingest data from on-premises data sources. The data is in JSON format, and ingestion rates can be as high as 1 megabyte per second. When an EC2 instance is rebooted, the data in-flight is lost. The company's data science team wants to query ingested data in near real time. Which solution provides near-real-time data querying that is scalable with minimal data loss?

**Short Question:** What's a scalable, durable way to ingest streaming JSON data and query it in near real time?

**Options:**
- ✅ A. Publish data to Amazon Kinesis Data Streams. Use Kinesis Data Analytics to query the data
- B. Publish data to Amazon Kinesis Data Firehose with Amazon Redshift as the destination. Use Amazon Redshift to query the data
- C. Store ingested data in an EC2 instance store. Publish data to Amazon Kinesis Data Firehose with Amazon S3 as the destination. Use Amazon Athena to query the data
- D. Store ingested data in an Amazon Elastic Block Store (Amazon EBS) volume. Publish data to Amazon ElastiCache for Redis. Subscribe to the Redis channel to query the data

**Reason:** Kinesis Data Streams is a durable, scalable ingestion service (replicated across Availability Zones) and Kinesis Data Analytics can run SQL queries directly against the stream in near real time; Firehose introduces buffering latency before data lands in Redshift or S3, an EC2 instance store is ephemeral and worsens data loss, and Redis pub/sub is a messaging mechanism, not a query engine.

---

## Question 460

**Full Question:** A company's facility has badge readers at every entrance throughout the building. When badges are scanned, the readers send a message over HTTPS to indicate who attempted to access that particular entrance. A solutions architect must design a system to process these messages from the sensors. The solution must be highly available, and the results must be made available for the company's security team to analyze. Which system architecture should the solutions architect recommend?

**Short Question:** What's a highly available architecture for ingesting and processing HTTPS messages from many badge-reader sensors?

**Options:**
- A. Launch an Amazon EC2 instance to serve as the HTTPS endpoint and to process the messages. Configure the EC2 instance to save the results to an Amazon S3 bucket
- ✅ B. Create an HTTPS endpoint in Amazon API Gateway. Configure the API Gateway endpoint to invoke an AWS Lambda function to process the messages and save the results to an Amazon DynamoDB table
- C. Use Amazon Route 53 to direct incoming sensor messages to an AWS Lambda function
- D. Create a gateway VPC endpoint for Amazon S3. Configure a site-to-site VPN connection from the facility network to the VPC so that sensor data can be written directly to an S3 bucket by way of the VPC endpoint

**Reason:** API Gateway plus Lambda plus DynamoDB is a fully serverless, inherently highly available architecture with no single point of failure, unlike a lone EC2 instance; Route 53 is only a DNS service and cannot itself receive HTTPS messages or invoke Lambda, and the VPC endpoint/VPN option is designed for private on-premises connectivity, not the public HTTPS traffic described, and includes no processing step.
