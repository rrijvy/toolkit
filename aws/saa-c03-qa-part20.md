# AWS SAA-C03 Real Exam Questions & Answers — Part 20 (Q461–Q480)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 20](https://www.youtube.com/watch/iFm7lL64AxQ)

---

## Question 461

**Full Question:** A company is building an application to transfer data to a product manufacturer. The company has its own identity provider (IdP). The company wants the IdP to authenticate application users while the users use the application to transfer data. The company must use the Applicability Statement 2 (AS2) protocol. Which solution will meet these requirements?

**Short Question:** Which AWS service supports AS2 file transfer while also letting you authenticate users against your own custom identity provider?

**Options:**
- A. Use AWS DataSync to transfer the data. Create an AWS Lambda function for IdP authentication.
- B. Use Amazon AppFlow flows to transfer the data. Create an Amazon Elastic Container Service (Amazon ECS) task for IdP authentication.
- ✅ C. Use AWS Transfer Family to transfer the data. Create an AWS Lambda function for IdP authentication.
- D. Use AWS Storage Gateway to transfer the data. Create an Amazon Cognito identity pool for IdP authentication.

**Reason:** AWS Transfer Family is the only option here that natively supports the AS2 protocol for managed file transfers, and it supports custom identity provider integration via an API Gateway endpoint backed by a Lambda function. DataSync, AppFlow, and Storage Gateway do not support AS2.

---

## Question 462

**Full Question:** A solutions architect is designing a multi-tier application for a company. The application's users upload images from a mobile device. The application generates a thumbnail of each image and returns a message to the user to confirm that the image was uploaded successfully. The thumbnail generation can take up to 60 seconds, but the company wants to provide a faster response time to its users to notify them that the original image was received. The solutions architect must design the application to asynchronously dispatch requests to the different application tiers. What should the solutions architect do to meet these requirements?

**Short Question:** How do you decouple a slow (60-second) thumbnail-generation step so users get an immediate upload confirmation instead of waiting?

**Options:**
- A. Write a custom AWS Lambda function to generate the thumbnail and alert the user. Use the image upload process as an event source to invoke the Lambda function.
- B. Create an AWS Step Functions workflow. Configure Step Functions to handle the orchestration between the application tiers and alert the user when thumbnail generation is complete.
- ✅ C. Create an Amazon Simple Queue Service (Amazon SQS) message queue. As images are uploaded, place a message on the SQS queue for thumbnail generation. Alert the user through an application message that the image was received.
- D. Create Amazon Simple Notification Service (Amazon SNS) notification topics and subscriptions. Use one subscription with the application to generate the thumbnail after the image upload is complete. Use a second subscription to message the user's mobile app by way of a push notification after thumbnail generation is complete.

**Reason:** Placing a message on an SQS queue lets the front end immediately confirm the upload while a separate back-end process pulls from the queue to do the slow thumbnail generation asynchronously. The other options (A, B, D) all notify the user only after the full thumbnail generation finishes, which fails the fast-response requirement.

---

## Question 463

**Full Question:** A company's website uses an Amazon EC2 instance store for its catalog of items. The company wants to make sure that the catalog is highly available and that the catalog is stored in a durable location. What should a solutions architect do to meet these requirements?

**Short Question:** What's the best durable, highly available storage replacement for data currently sitting on an ephemeral EC2 instance store?

**Options:**
- A. Move the catalog to Amazon ElastiCache for Redis.
- B. Deploy a larger EC2 instance with a larger instance store.
- C. Move the catalog from the instance store to Amazon S3 Glacier Deep Archive.
- ✅ D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.

**Reason:** Amazon EFS is a fully managed, durable file system that redundantly stores data across multiple Availability Zones and can be mounted like a local file system, replacing the ephemeral instance store. ElastiCache is just a cache (data can be lost), a bigger instance store is still ephemeral, and S3 Glacier Deep Archive is archival storage with retrieval times too slow for a live website.

---

## Question 464

**Full Question:** A company sells data sets to customers who do research in artificial intelligence and machine learning (AI or ML). The data sets are large formatted files that are stored in an Amazon S3 bucket in the US East 1 Region. The company hosts a web application that the customers use to purchase access to a given data set. The web application is deployed on multiple Amazon EC2 instances behind an Application Load Balancer. After a purchase is made, customers receive an S3 signed URL that allows access to the files. The customers are distributed across North America and Europe. The company wants to reduce the cost that is associated with data transfers and wants to maintain or improve performance. What should a solutions architect do to meet these requirements?

**Short Question:** How do you cut data-transfer costs and improve download performance for global customers downloading large files from a single S3 bucket?

**Options:**
- A. Configure S3 Transfer Acceleration on the existing S3 bucket. Direct customer requests to the S3 Transfer Acceleration endpoint. Continue to use S3 signed URLs for access control.
- ✅ B. Deploy an Amazon CloudFront distribution with the existing S3 bucket as the origin. Direct customer requests to the CloudFront URL. Switch to CloudFront signed URLs for access control.
- C. Set up a second S3 bucket in the EU Central 1 Region with S3 Cross-Region Replication between the buckets. Direct customer requests to the closest region. Continue to use S3 signed URLs for access control.
- D. Modify the web application to enable streaming of the data sets to end users. Configure the web application to read the data from the existing S3 bucket. Implement access control directly in the application.

**Reason:** CloudFront caches the files at edge locations near customers in North America and Europe, which lowers latency and is generally cheaper for data transfer out than S3 directly, and CloudFront signed URLs preserve secure temporary access. Transfer Acceleration only speeds up uploads (not downloads), cross-region replication is more complex/costly than using a CDN, and streaming through the EC2 app tier would create a bottleneck and raise costs.

---

## Question 465

**Full Question:** A company is migrating its applications and databases to the AWS Cloud. The company will use Amazon Elastic Container Service (Amazon ECS), AWS Direct Connect, and Amazon RDS. Which activities will be managed by the company's operational team? (Choose three.)

**Short Question:** Under the AWS shared responsibility model, which three tasks fall on the customer when using RDS, ECS, and Direct Connect?

**Options:**
- A. Management of the Amazon RDS infrastructure layer, operating system, and platforms
- ✅ B. Creation of an Amazon RDS DB instance and configuring the scheduled maintenance window
- ✅ C. Configuration of additional software components on Amazon ECS for monitoring, patch management, log management, and host intrusion detection
- D. Installation of patches for all minor and major database versions for Amazon RDS
- E. Ensure the physical security of the Amazon RDS infrastructure in the data center
- ✅ F. Encryption of the data that moves in transit through AWS Direct Connect

**Reason:** As the customer, the operational team creates and configures its RDS instances (including maintenance windows), manages any extra software/agents on the EC2 instances behind ECS, and must add its own encryption (e.g., VPN or MACsec) over Direct Connect since that link isn't encrypted by default. AWS, not the customer, manages the RDS underlying infrastructure/OS/platform, database engine patching, and physical data center security.

---

## Question 466

**Full Question:** A company runs an application on Amazon EC2 Linux instances across multiple Availability Zones. The application needs a storage layer that is highly available and portable operating system interface (POSIX) compliant. The storage layer must provide maximum data durability and must be sharable across the EC2 instances. The data in the storage layer will be accessed frequently for the first 30 days and will be accessed infrequently after that time. Which solution will meet these requirements most cost effectively?

**Short Question:** Which storage option gives a highly available, durable, shared, POSIX-compliant file system for EC2 Linux instances, with automatic cost savings as data ages past 30 days?

**Options:**
- A. Use the Amazon S3 Standard storage class. Create an S3 lifecycle policy to move infrequently accessed data to S3 Glacier
- B. Use the Amazon S3 Standard storage class. Create an S3 lifecycle policy to move infrequently accessed data to S3 Standard-Infrequent Access (S3 Standard-IA)
- ✅ C. Use the Amazon Elastic File System (Amazon EFS) Standard storage class. Create a lifecycle management policy to move infrequently accessed data to EFS Standard-Infrequent Access (EFS Standard-IA)
- D. Use the Amazon Elastic File System (Amazon EFS) One Zone storage class. Create a lifecycle management policy to move infrequently accessed data to EFS One Zone-Infrequent Access (EFS One Zone-IA)

**Reason:** Amazon EFS Standard is a fully managed, POSIX-compliant shared file system that is highly available and durable across multiple Availability Zones, and its built-in lifecycle management can automatically move data untouched for 30 days to the cheaper EFS Standard-IA tier. S3 is an object store, not a POSIX file system (A, B), and EFS One Zone (D) does not meet the multi-AZ high-availability/durability requirement.

---

## Question 467

**Full Question:** What should a solutions architect do to ensure that all objects uploaded to an Amazon S3 bucket are encrypted?

**Short Question:** What bucket policy condition forces every object uploaded to S3 to be encrypted?

**Options:**
- A. Update the bucket policy to deny if the PutObject request does not have an x-amz-acl header set
- B. Update the bucket policy to deny if the PutObject request does not have an x-amz-acl header set to private
- C. Update the bucket policy to deny if the PutObject request does not have an x-amz-server-side-encryption header set to true (via aws:SecureTransport)
- D. Update the bucket policy to deny if the PutObject request does not have an x-amz-server-side-encryption header set

**Reason:** The x-amz-server-side-encryption header is the specific field clients must include on a PutObject request to enable server-side encryption, so a bucket policy that denies uploads lacking this header forces all objects to be encrypted. The x-amz-acl header (A, B) controls access permissions, not encryption, and aws:SecureTransport (C) only enforces HTTPS in transit, not encryption at rest.

---

## Question 468

**Full Question:** A company uses AWS Organizations. The company wants to operate some of its AWS accounts with different budgets. The company wants to receive alerts and automatically prevent provisioning of additional resources on AWS accounts when the allocated budget threshold is met during a specific period. Which combination of solutions will meet these requirements? (Choose three.)

**Short Question:** Which three steps set up automatic alerts and blocking of new resource creation when an AWS account hits its budget limit?

**Options:**
- A. Use AWS Budgets to create a budget. Set the budget amount under the Cost and Usage Report section of the required AWS accounts
- ✅ B. Use AWS Budgets to create a budget. Set the budget amount under the Billing dashboards of the required AWS accounts
- C. Create an IAM user for AWS Budgets to run budget actions with the required permissions
- ✅ D. Create an IAM role for AWS Budgets to run budget actions with the required permissions
- E. Add an alert to notify the company when each account meets its budget threshold. Add a budget action that selects the IAM identity created with the appropriate AWS Config rule to prevent provisioning of additional resources
- ✅ F. Add an alert to notify the company when each account meets its budget threshold. Add a budget action that selects the IAM identity created with the appropriate service control policy (SCP) to prevent provisioning of additional resources

**Reason:** Budgets are created and scoped to accounts through the AWS Billing console (B); AWS Budgets needs an IAM role (not a long-lived IAM user) to assume permissions and take action (D); and a budget action tied to a service control policy (SCP) can automatically deny further resource creation account-wide once the threshold is hit (F). AWS Config rules (E) are detective, not preventive, controls, so they can't block provisioning.

---

## Question 469

**Full Question:** A company wants to host a scalable web application on AWS. The application will be accessed by users from different geographic regions of the world. Application users will be able to download and upload unique data up to gigabytes in size. The development team wants a cost-effective solution to minimize upload and download latency and maximize performance. What should a solutions architect do to accomplish this?

**Short Question:** What's the cost-effective way to minimize upload/download latency for large, unique files accessed by a global user base?

**Options:**
- ✅ A. Use Amazon S3 with Transfer Acceleration to host the application
- B. Use Amazon S3 with cache control headers to host the application
- C. Use Amazon EC2 with Auto Scaling and Amazon CloudFront to host the application
- D. Use Amazon EC2 with Auto Scaling and Amazon ElastiCache to host the application

**Reason:** Amazon S3 Transfer Acceleration routes uploads and downloads through AWS edge locations over the optimized AWS global network, cutting latency for large files in both directions at low cost. Cache control headers (B) and CloudFront (C) only help with caching reusable content, which doesn't help since each file here is unique, and ElastiCache (D) is an in-memory database cache with no role in accelerating file transfers.

---

## Question 470

**Full Question:** A company stores several petabytes of data across multiple AWS accounts. The company uses AWS Lake Formation to manage its data lake. The company's data science team wants to securely share selective data from its accounts with the company's engineering team for analytical purposes. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead way to securely share specific data lake data across AWS accounts between teams?

**Options:**
- A. Copy the required data to a common account. Create an IAM role in that account. Grant access by specifying a permission policy that includes users from the engineering team accounts as trusted entities
- B. Use the Lake Formation PermissionsGrant command in each account where the data is stored to allow the required engineering team users to access the data
- C. Use AWS Data Exchange to privately publish the required data to the required engineering team accounts
- ✅ D. Use Lake Formation tag-based access control (LF-TBAC) to authorize and grant cross-account permissions for the required data to the engineering team accounts

**Reason:** Lake Formation tag-based access control lets you apply LF-tags to databases, tables, and columns and grant access based on those tags, so permissions are managed on a handful of tags instead of thousands of individual resources across accounts. Copying petabytes of data (A) is costly and creates duplication/sync overhead, granting permissions resource-by-resource (B) doesn't scale, and AWS Data Exchange (C) is meant for third-party data products, not internal data lake sharing.

---

## Question 471

**Full Question:** A company is developing a new machine learning (ML) model solution on AWS. The models are developed as independent microservices that fetch approximately 1 GB of model data from Amazon S3 at startup and load the data into memory. Users access the models through an asynchronous API: users can send a request or a batch of requests and specify where the results should be sent. The company provides models to hundreds of users. The usage patterns for the models are irregular — some models could be unused for days or weeks, while other models could receive batches of thousands of requests at a time. Which design should a solutions architect recommend to meet these requirements?

**Short Question:** What's the best scalable architecture for an asynchronous ML API with spiky traffic and slow-loading (1 GB) models?

**Options:**
- A. Direct the requests from the API to a Network Load Balancer (NLB). Deploy the models as AWS Lambda functions that are invoked by the NLB.
- B. Direct the requests from the API to an Application Load Balancer (ALB). Deploy the models as Amazon ECS services that read from an Amazon SQS queue. Use AWS App Mesh to scale the instances of the ECS cluster based on the SQS queue size.
- C. Direct the requests from the API into an Amazon SQS queue. Deploy the models as AWS Lambda functions that are invoked by SQS events. Use AWS Auto Scaling to increase the number of vCPUs for the Lambda functions based on the SQS queue size.
- ✅ D. Direct the requests from the API into an Amazon SQS queue. Deploy the models as Amazon ECS services that read from the queue. Enable AWS Auto Scaling on Amazon ECS for both the cluster and copies of the service based on the queue size.

**Reason:** SQS decouples the bursty asynchronous requests from processing, and ECS containers can load the 1 GB model once and then efficiently process many queued messages, scaling via ECS Auto Scaling on queue depth. Lambda is a poor fit due to cold-start delays from the large model load, and you can't autoscale Lambda's vCPUs directly (option C), while mixing a synchronous ALB entry point with async SQS-based processing (option B) is architecturally inconsistent.

---

## Question 472

**Full Question:** A company used an Amazon RDS for MySQL DB instance during application testing. Before terminating the DB instance at the end of the test cycle, a solutions architect created two backups: the first backup was created by using the mysqldump utility to create a database dump, and the second backup was created by enabling the final DB snapshot option on RDS termination. The company is now planning for a new test cycle and wants to create a new DB instance from the most recent backup. The company has chosen a MySQL-compatible edition of Amazon Aurora to host the DB instance. Which solutions will create the new DB instance? (Choose two.)

**Short Question:** Which two methods correctly restore an RDS MySQL backup (snapshot or mysqldump file) into a new Aurora MySQL cluster?

**Options:**
- ✅ A. Import the RDS snapshot directly into Aurora.
- B. Upload the RDS snapshot to Amazon S3. Then import the RDS snapshot into Aurora.
- ✅ C. Upload the database dump to Amazon S3. Then import the database dump into Aurora.
- D. Use AWS Database Migration Service (AWS DMS) to import the RDS snapshot into Aurora.
- E. Upload the database dump to Amazon S3. Then use AWS Database Migration Service (AWS DMS) to import the database dump into Aurora.

**Reason:** RDS provides a built-in option to migrate a native snapshot directly into a new Aurora cluster (no manual S3 step needed), and a logical mysqldump file can be uploaded to S3 and then imported using a MySQL client against a new Aurora cluster. AWS DMS requires a live source database, not a snapshot or dump file, so options D and E are invalid, and native RDS snapshots cannot be manually uploaded to S3 (option B).

---

## Question 473

**Full Question:** A solutions architect is implementing a complex Java application with a MySQL database. The Java application must be deployed on Apache Tomcat and must be highly available. What should the solutions architect do to meet these requirements?

**Short Question:** What's the best way to deploy a highly available Java/Tomcat application with a MySQL backend?

**Options:**
- A. Deploy the application in AWS Lambda. Configure an Amazon API Gateway API to connect with the Lambda functions.
- ✅ B. Deploy the application by using AWS Elastic Beanstalk. Configure a load-balanced environment and a rolling deployment policy.
- C. Migrate the database to Amazon ElastiCache. Configure the ElastiCache security group to allow access from the application.
- D. Launch an Amazon EC2 instance. Install a MySQL server on the EC2 instance. Configure the application on the server. Create an AMI. Use the AMI to create a launch template with an Auto Scaling group.

**Reason:** AWS Elastic Beanstalk has a preconfigured Java/Tomcat platform and, with a load-balanced environment, automatically sets up an Application Load Balancer and an Auto Scaling group across multiple Availability Zones for high availability with minimal operational overhead. Lambda requires a full rearchitecture of a long-running Tomcat app, ElastiCache is a cache (not a MySQL replacement), and running the app and database together on a single EC2 instance creates a single point of failure.

---

## Question 474

**Full Question:** The following IAM policy is attached to an IAM group, and it is the only policy applied to the group. The policy's first statement allows all Amazon EC2 actions (ec2:*) restricted to the us-east-1 region, and a second statement denies the ec2:StopInstances and ec2:TerminateInstances actions unless the request is authenticated with multi-factor authentication (MFA). What are the effective IAM permissions of this policy for group members?

**Short Question:** Given an IAM policy that allows all EC2 actions in us-east-1 but denies stop/terminate instances without MFA, what permissions do group members actually end up with?

**Options:**
- A. Group members are permitted any Amazon EC2 action within the us-east-1 region; statements after the Allow permission are not applied.
- B. Group members are denied any Amazon EC2 permissions in the us-east-1 region unless they are logged in with multi-factor authentication (MFA).
- C. Group members are allowed the ec2:StopInstances and ec2:TerminateInstances permissions for all regions when logged in with multi-factor authentication (MFA); group members are permitted any other Amazon EC2 action.
- ✅ D. Group members are allowed the ec2:StopInstances and ec2:TerminateInstances permissions for the us-east-1 region only when logged in with multi-factor authentication (MFA); group members are permitted any other Amazon EC2 action within the us-east-1 region.

**Reason:** IAM evaluates every statement in a policy, and an explicit Deny always overrides an Allow, but the deny here is narrowly scoped to only StopInstances/TerminateInstances without MFA — it doesn't touch other EC2 actions, which remain allowed (but still restricted to us-east-1 by the first statement). This rules out A (statements are always all evaluated), B (the deny is action-specific, not blanket), and C (the allow statement's region restriction to us-east-1 still applies to all EC2 actions, including the MFA-protected ones).

---

## Question 475

**Full Question:** A company has an Amazon S3 bucket that contains critical data. The company must protect the data from accidental deletion. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two S3 features best protect a bucket's data against accidental deletion?

**Options:**
- ✅ A. Enable versioning on the S3 bucket.
- ✅ B. Enable MFA delete on the S3 bucket.
- C. Create a bucket policy on the S3 bucket.
- D. Enable default encryption on the S3 bucket.
- E. Create a lifecycle policy for the objects in the S3 bucket.

**Reason:** Versioning preserves prior object versions (a delete just adds a delete marker, so data can be restored), and MFA Delete adds a required second authentication factor before a version or the bucket's versioning state can be permanently changed, making accidental deletion much harder. A bucket policy can be overridden by a privileged/root user, encryption only protects confidentiality (not deletion), and a misconfigured lifecycle policy could actually cause unwanted deletions.

---

## Question 476

**Full Question:** A company wants to give a customer the ability to use on-premises Microsoft Active Directory to download files that are stored in Amazon S3. The customer's application uses an SFTP client to download the files. Which solution will meet these requirements with the least operational overhead and no changes to the customer's application?

**Short Question:** Least-overhead way to let an SFTP client authenticate with on-premises Active Directory and download files from S3?

**Options:**
- ✅ A. Set up AWS Transfer Family with SFTP for Amazon S3. Configure integrated Active Directory authentication.
- B. Set up AWS Database Migration Service (AWS DMS) to synchronize the on-premises client with Amazon S3. Configure integrated Active Directory authentication.
- C. Set up AWS DataSync to synchronize between the on-premises location and the S3 location by using AWS IAM Identity Center (AWS Single Sign-On).
- D. Set up a Windows Amazon EC2 instance with SFTP to connect the on-premises client with Amazon S3. Integrate AWS Identity and Access Management (IAM).

**Reason:** AWS Transfer Family is a fully managed SFTP endpoint backed by S3 with built-in Active Directory integration, requiring no client-side changes. DMS and DataSync aren't SFTP servers, and a self-managed EC2 SFTP server adds unnecessary operational overhead.

---

## Question 477

**Full Question:** A solutions architect configured a VPC that has a small range of IP addresses. The number of Amazon EC2 instances that are in the VPC is increasing, and there is an insufficient number of IP addresses for future workloads. Which solution resolves this issue with the least operational overhead?

**Short Question:** Simplest way to fix IP address exhaustion in an existing VPC?

**Options:**
- ✅ A. Add an additional IPv4 CIDR block to increase the number of IP addresses and create additional subnets in the VPC. Create new resources in the new subnets by using the new CIDR.
- B. Create a second VPC with additional subnets. Use a peering connection to connect the second VPC with the first VPC. Update the routes and create new resources in the subnets of the second VPC.
- C. Use AWS Transit Gateway to add a transit gateway and connect a second VPC with the first VPC. Update the routes of the transit gateway and VPCs. Create new resources in the subnets of the second VPC.
- D. Create a second VPC. Create a site-to-site VPN connection between the first VPC and the second VPC by using a VPN hosted solution on Amazon EC2 and a virtual private gateway. Update the route between VPCs to route traffic through the VPN. Create new resources in the subnets of the second VPC.

**Reason:** Adding a secondary IPv4 CIDR block to the existing VPC is a native, simple feature that instantly gives more IP space, with all resources able to communicate by default. The other options involve a second VPC and peering, transit gateway, or VPN setups that add unnecessary complexity and overhead.

---

## Question 478

**Full Question:** A company has developed a new video game as a web application. The application is in a three-tier architecture in a VPC with Amazon RDS for MySQL in the database layer. Several players will compete concurrently online. The game's developers want to display a top 10 scoreboard in near real time and offer the ability to stop and restore the game while preserving the current scores. What should a solutions architect do to meet these requirements?

**Short Question:** Best way to power a near real-time top-10 leaderboard while preserving scores across stop/restore?

**Options:**
- A. Set up an Amazon ElastiCache for Memcached cluster to cache the scores for the web application to display.
- ✅ B. Set up an Amazon ElastiCache for Redis cluster to compute and cache the scores for the web application to display.
- C. Place an Amazon CloudFront distribution in front of the web application to cache the scoreboard in a section of the application.
- D. Create a read replica on Amazon RDS for MySQL to run queries to compute the scoreboard and serve the read traffic to the web application.

**Reason:** Redis supports sorted sets, which are ideal for computing a real-time leaderboard with very low latency, and it offers persistence (snapshots and AOF) so scores survive a stop/restart. Memcached lacks sorted sets and persistence, CloudFront isn't suited to highly dynamic data, and sorting queries on a relational database read replica are too slow for near real-time results.

---

## Question 479

**Full Question:** A company uses AWS Organizations with all features enabled and runs multiple Amazon EC2 workloads in the AP Southeast 2 region. The company has a service control policy (SCP) that prevents any resources from being created in any other region. A security policy requires the company to encrypt all data at rest. An audit discovers that employees have created Amazon Elastic Block Store (Amazon EBS) volumes for EC2 instances without encrypting the volumes. The company wants any new EC2 instances that any IAM user or root user launches in AP Southeast 2 to use encrypted EBS volumes. The company wants a solution that will have minimal effect on employees who create EBS volumes. Which combination of steps will meet these requirements? (Choose two.)

**Short Question:** Which two steps enforce EBS encryption org-wide (including for root users) with minimal impact on users?

**Options:**
- A. In the Amazon EC2 console, select the EBS encryption account attribute and define a default encryption key.
- B. Create an IAM permission boundary. Attach the permission boundary to the root organizational unit (OU). Define the boundary to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- ✅ C. Create an SCP. Attach the SCP to the root organizational unit (OU). Define the SCP to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- D. Update the IAM policies for each account to deny the ec2:CreateVolume action when the ec2:Encrypted condition equals false.
- ✅ E. In the organization's management account, specify the default EBS volume encryption setting.

**Reason:** An SCP applied at the root OU enforces the deny on all principals, including root users, giving true preventive enforcement, while centrally specifying default EBS encryption in the management account makes encryption automatic so it doesn't burden employees. Permission boundaries and per-account IAM policies don't apply to root users and aren't centrally managed.

---

## Question 480

**Full Question:** A company wants to use an Amazon RDS for PostgreSQL DB cluster to simplify time-consuming database administrative tasks for production database workloads. The company wants to ensure that its database is highly available and will provide automatic failover support in most scenarios in less than 40 seconds. The company wants to offload reads off of the primary instance and keep costs as low as possible. Which solution will meet these requirements?

**Short Question:** Which RDS for PostgreSQL setup gives sub-40-second failover plus cost-effective read offloading?

**Options:**
- A. Use an Amazon RDS Multi-AZ DB instance deployment. Create one read replica and point the read workload to the read replica.
- B. Use an Amazon RDS Multi-AZ DB cluster deployment. Create two read replicas and point the read workload to the read replicas.
- C. Use an Amazon RDS Multi-AZ DB instance deployment. Point the read workload to the secondary instances in the Multi-AZ pair.
- ✅ D. Use an Amazon RDS Multi-AZ DB cluster deployment. Point the read workload to the reader endpoint.

**Reason:** A Multi-AZ DB cluster deployment provides fast failover (typically 35-40 seconds) and includes two readable standby instances accessible via the managed reader endpoint, which load-balances read traffic without extra cost. A Multi-AZ DB instance deployment fails over in 1-2 minutes and its standby can't serve reads, and adding separate read replicas on top of a cluster's built-in readable standbys is redundant.
