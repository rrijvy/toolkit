# AWS SAA-C03 Real Exam Questions & Answers — Part 11 (Q251–Q275)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 11](https://www.youtube.com/watch/yLBSTeFVs9E)

---

## Question 251

**Full Question:** A company hosts a website on Amazon EC2 instances behind an Application Load Balancer (ALB). The website serves static content. Website traffic is increasing, and the company is concerned about a potential increase in cost. Which solution should the company use to reduce costs while serving the static content?

**Short Question:** Cheapest way to serve growing static-content traffic from EC2/ALB without costs spiraling?

**Options:**
- ✅ A. Create an Amazon CloudFront distribution to cache static files at edge locations
- B. Create an Amazon ElastiCache cluster. Connect the ALB to the ElastiCache cluster to serve cached files
- C. Create an AWS WAF web ACL and associate it with the ALB. Add a rule to the web ACL to cache static files
- D. Create a second ALB in an alternative AWS Region. Route user traffic to the closest Region to minimize data transfer costs

**Reason:** CloudFront is a CDN that caches static content at edge locations close to users, cutting the number of requests and data transferred from the EC2/ALB origin, which lowers cost. ElastiCache is for database caching (not static web content), WAF has no caching capability, and a second ALB in another Region adds cost and complexity instead of reducing it.

---

## Question 252

**Full Question:** A company runs its application on an Oracle database. The company plans to quickly migrate to AWS because of limited resources for database backup administration and data center maintenance. The application uses third-party database features that require privileged access. Which solution will help the company migrate the database to AWS most cost-effectively?

**Short Question:** Fastest, cheapest way to move an Oracle DB needing privileged-access third-party features to AWS?

**Options:**
- A. Migrate the database to Amazon RDS for Oracle. Replace third-party features with cloud services
- ✅ B. Migrate the database to Amazon RDS Custom for Oracle. Customize the database settings to support third-party features
- C. Migrate the database to an Amazon EC2 Amazon Machine Image (AMI) for Oracle. Customize the database settings to support third-party features
- D. Migrate the database to Amazon RDS for PostgreSQL by rewriting the application code to remove dependency on Oracle APEX

**Reason:** Amazon RDS Custom for Oracle gives OS- and database-level (privileged) access needed by the third-party features while still being a managed service, balancing speed and low operational overhead. Standard RDS lacks privileged access, self-managed EC2 has the highest operational burden, and migrating to PostgreSQL requires a slow, costly application rewrite.

---

## Question 253

**Full Question:** A company is building a new web-based customer relationship management application. The application will use several Amazon EC2 instances that are backed by Amazon Elastic Block Store (Amazon EBS) volumes behind an Application Load Balancer (ALB). The application will also use an Amazon Aurora database. All data for the application must be encrypted at rest and in transit. Which solution will meet these requirements?

**Short Question:** Which AWS services correctly provide encryption at rest and in transit for this EC2/EBS/Aurora/ALB app?

**Options:**
- A. Use AWS Key Management Service (AWS KMS) certificates on the ALB to encrypt data in transit. Use AWS Certificate Manager (ACM) to encrypt the EBS volumes and Aurora database storage at rest
- B. Use the AWS root account to log in to the AWS Management Console. Upload the company's encryption certificates. While in the root account, select the option to turn on encryption for all data at rest and in transit for the account
- ✅ C. Use AWS Key Management Service (AWS KMS) to encrypt the EBS volumes and Aurora database storage at rest. Attach an AWS Certificate Manager (ACM) certificate to the ALB to encrypt data in transit
- D. Use BitLocker to encrypt all data at rest. Import the company's TLS certificate keys to AWS Key Management Service (AWS KMS). Attach the KMS keys to the ALB to encrypt data in transit

**Reason:** AWS KMS is the correct tool for encrypting storage (EBS and Aurora) at rest, and ACM certificates attached to the ALB handle TLS termination for encryption in transit — option C matches each service to its correct purpose. Option A swaps the services' roles, option B relies on a nonexistent single "encrypt everything" root toggle (and misuses the root account), and option D uses BitLocker (not applicable to EBS) and incorrectly attaches KMS keys directly to an ALB.

---

## Question 254

**Full Question:** A company previously migrated its data warehouse solution to AWS. The company also has an AWS Direct Connect connection. Corporate office users query the data warehouse using a visualization tool. The average size of a query result returned by the data warehouse is 50 MB, and each web page sent by the visualization tool is approximately 500 KB. Result sets returned by the data warehouse are not cached. Which solution provides the lowest data transfer egress cost for the company?

**Short Question:** How to arrange the visualization tool and Direct Connect link to minimize egress costs for large query results?

**Options:**
- A. Host the visualization tool on premises and query the data warehouse directly over the internet
- B. Host the visualization tool in the same AWS Region as the data warehouse. Access it over the internet
- C. Host the visualization tool on premises and query the data warehouse directly over a Direct Connect connection at a location in the same AWS Region
- ✅ D. Host the visualization tool in the same AWS Region as the data warehouse and access it over a Direct Connect connection at a location in the same Region

**Reason:** Placing the visualization tool in the same Region as the data warehouse means the large 50 MB query results travel for free within AWS, leaving only the small 500 KB web pages to egress to users — and routing that smaller egress over Direct Connect makes it cheaper than internet transfer. The other options all force the large 50 MB result sets to egress over the internet or Direct Connect repeatedly, which is far more expensive.

---

## Question 255

**Full Question:** A company is deploying a new application on Amazon EC2 instances. The application writes data to Amazon Elastic Block Store (Amazon EBS) volumes. The company needs to ensure that all data that is written to the EBS volumes is encrypted at rest. Which solution will meet this requirement?

**Short Question:** Simplest way to guarantee data written to EBS volumes is encrypted at rest?

**Options:**
- A. Create an IAM role that specifies EBS encryption. Attach the role to the EC2 instances
- ✅ B. Create the EBS volumes as encrypted volumes. Attach the EBS volumes to the EC2 instances
- C. Create an EC2 instance tag that has a key of "encrypt" and a value of "true". Tag all instances that require encryption at the EBS level
- D. Create an AWS Key Management Service (AWS KMS) key policy that enforces EBS encryption in the account. Ensure that the key policy is active

**Reason:** EBS encryption is enabled directly at volume creation time, which automatically encrypts all data written to that volume — this is the standard, direct method. IAM roles grant permissions rather than enforce encryption, tags are just metadata with no enforcement power, and a KMS key policy controls key usage but does not itself force new volumes to be created as encrypted.

---

## Question 256

**Full Question:** A solutions architect is designing a company's disaster recovery (DR) architecture. The company has a MySQL database that runs on an Amazon EC2 instance in a private subnet with scheduled backups. The DR design needs to include multiple AWS Regions. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Least-overhead way to add multi-Region disaster recovery for a self-managed MySQL database on EC2?

**Options:**
- A. Migrate the MySQL database to multiple EC2 instances. Configure a standby EC2 instance in the DR Region. Turn on replication.
- B. Migrate the MySQL database to Amazon RDS. Use a Multi-AZ deployment. Turn on read replication for the primary DB instance in different Availability Zones.
- ✅ C. Migrate the MySQL database to an Amazon Aurora global database. Host the primary DB cluster in the primary Region. Host the secondary DB cluster in the DR Region.
- D. Store the scheduled backup of the MySQL database in an Amazon S3 bucket that is configured for S3 Cross-Region Replication (CRR). Use the backup to restore the database in the DR Region.

**Reason:** An Aurora global database is a fully managed, purpose-built cross-Region solution with sub-second replication lag, giving a low RPO and an RTO under a minute with minimal admin effort. Option A is fully self-managed (high overhead), B's Multi-AZ only protects within one Region, and D relies on slow manual backup/restore, giving a poor RTO.

---

## Question 257

**Full Question:** A company's reporting system delivers hundreds of CSV files to an Amazon S3 bucket each day. The company must convert these files to Apache Parquet format and must store the files in a transformed data bucket. Which solution will meet these requirements with the least development effort?

**Short Question:** Least-effort way to automatically convert many daily CSV files in S3 to Parquet format?

**Options:**
- A. Create an Amazon EMR cluster with Apache Spark installed. Write a Spark application to transform the data. Use the EMR File System (EMRFS) to write files to the transformed data bucket.
- ✅ B. Create an AWS Glue crawler to discover the data. Create an AWS Glue extract, transform, and load (ETL) job to transform the data. Specify the transformed data bucket in the output step.
- C. Use AWS Batch to create a job definition with bash syntax to transform the data and output the data to the transformed data bucket. Use the job definition to submit a job. Specify an array job as the job type.
- D. Create an AWS Lambda function to transform the data and output the data to the transformed data bucket. Configure an event notification for the S3 bucket. Specify the Lambda function as the destination for the event notification.

**Reason:** AWS Glue is a serverless, fully managed ETL service purpose-built for this kind of format conversion — a crawler can auto-discover the CSV schema and a Glue job can convert to Parquet with minimal custom code. EMR and AWS Batch both require writing and managing custom transformation code (higher effort), and Lambda's 15-minute timeout makes it unreliable for large/numerous files.

---

## Question 258

**Full Question:** A company uses Amazon S3 to store high-resolution pictures in an S3 bucket. To minimize application changes, the company stores the pictures as the latest version of an S3 object. The company needs to retain only the two most recent versions of the pictures. The company wants to reduce costs, and it has identified the S3 bucket as a large expense. Which solution will reduce the S3 costs with the least operational overhead?

**Short Question:** Lowest-overhead way to automatically keep only the two most recent versions of S3 objects to cut costs?

**Options:**
- ✅ A. Use S3 Lifecycle to delete expired object versions and retain the two most recent versions.
- B. Use an AWS Lambda function to check for older versions and delete all but the two most recent versions.
- C. Use S3 Batch Operations to delete non-current object versions and retain only the two most recent versions.
- D. Deactivate (suspend) versioning on the S3 bucket and retain the two most recent versions.

**Reason:** S3 Lifecycle rules natively support specifying how many non-current versions to retain, making this a no-code, fully managed solution. A Lambda function requires custom code to maintain, S3 Batch Operations is meant for one-time bulk jobs rather than ongoing automation, and suspending versioning does not delete or limit existing old versions at all.

---

## Question 259

**Full Question:** A company runs container applications by using Amazon Elastic Kubernetes Service (Amazon EKS). The company's workload is not consistent throughout the day. The company wants Amazon EKS to scale in and out according to the workload. Which combination of steps will meet these requirements with the least operational overhead? (Choose two.)

**Short Question:** Which two native, low-overhead mechanisms auto-scale an EKS cluster's pods and nodes with fluctuating demand?

**Options:**
- A. Use an AWS Lambda function to resize the EKS cluster.
- ✅ B. Use the Kubernetes Metrics Server to activate horizontal pod autoscaling.
- ✅ C. Use the Kubernetes Cluster Autoscaler to manage the number of nodes in the cluster.
- D. Use Amazon API Gateway and connect it to Amazon EKS.
- E. Use AWS App Mesh to observe network activity.

**Reason:** The Metrics Server feeds the Horizontal Pod Autoscaler to scale pod counts based on CPU/memory, while the Cluster Autoscaler adds or removes worker nodes to match pod demand — together they natively handle scaling at both the pod and infrastructure layers with minimal management. A Lambda-based resizer is a custom high-overhead approach, API Gateway just fronts APIs, and App Mesh is for network observability, not scaling.

---

## Question 260

**Full Question:** A company collects data from a large number of participants who use wearable devices. The company stores the data in an Amazon DynamoDB table and uses applications to analyze the data. The data workload is constant and predictable. The company wants to stay at or below its forecasted budget for DynamoDB. Which solution will meet these requirements most cost-effectively?

**Short Question:** Most cost-effective DynamoDB capacity mode for a steady, predictable, budget-conscious workload?

**Options:**
- A. Use provisioned mode and DynamoDB Standard-Infrequent Access (Standard-IA). Reserve capacity for the forecasted workload.
- ✅ B. Use provisioned mode. Specify the read capacity units (RCUs) and write capacity units (WCUs).
- C. Use on-demand mode. Set the read capacity units (RCUs) and write capacity units (WCUs) high enough to accommodate changes in the workload.
- D. Use on-demand mode. Specify the read capacity units (RCUs) and write capacity units (WCUs) with reserved capacity.

**Reason:** For a constant, predictable workload, provisioned mode with explicitly set RCUs/WCUs (optionally with reserved capacity) is the cheapest option since you pay only for exactly the capacity you need. Standard-IA is meant for infrequently accessed data (not this active workload), and on-demand mode charges a higher per-request rate and doesn't even let you specify RCUs/WCUs, making options C and D invalid or costlier.

---

## Question 261

**Full Question:** A company hosts a three-tier e-commerce application on a fleet of Amazon EC2 instances. The instances run in an Auto Scaling group behind an Application Load Balancer (ALB). All e-commerce data is stored in an Amazon RDS for MariaDB Multi-AZ DB instance. The company wants to optimize customer session management during transactions. The application must store session data durably. Which solutions will meet these requirements? (Choose two.)

**Short Question:** Which two options durably manage user session state for a scalable EC2-based e-commerce app?

**Options:**
- ✅ A. Turn on the sticky sessions feature (session affinity) on the ALB
- ✅ B. Use an Amazon DynamoDB table to store customer session information
- C. Deploy an Amazon Cognito user pool to manage user session information
- D. Deploy an Amazon ElastiCache for Redis cluster to store customer session information
- E. Use AWS Systems Manager Application Manager in the application to manage user session information

**Reason:** Sticky sessions (A) bind a user to one instance for simple session handling, while DynamoDB (B) provides a centralized, durable, shared session store that survives instance failure — together they satisfy the durability requirement. Cognito is for authentication (not session storage), ElastiCache is in-memory and not durable, and Systems Manager Application Manager isn't a session-data service.

---

## Question 262

**Full Question:** An e-commerce company wants to launch a one-deal-a-day website on AWS. Each day will feature exactly one product on sale for a period of 24 hours. The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Least-operational-overhead way to build a high-traffic, low-latency daily-deal website?

**Options:**
- A. Use Amazon S3 to host the full website in different S3 buckets. Add Amazon CloudFront distributions. Set the S3 buckets as origins for the distributions. Store the order data in Amazon S3.
- B. Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones. Add an Application Load Balancer (ALB) to distribute the website traffic. Add another ALB for the backend APIs. Store the data in Amazon RDS for MySQL.
- C. Migrate the full application to run in containers. Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic. Store the data in Amazon RDS for MySQL.
- ✅ D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.

**Reason:** Option D is a fully serverless stack — S3 and CloudFront serve static content globally at low latency, while API Gateway, Lambda, and DynamoDB scale elastically to handle millions of requests with no servers to manage. The other options rely on S3 as a database (unsuitable for transactional order data) or on EC2/EKS with RDS, which require more management and don't scale as fast or as effortlessly.

---

## Question 263

**Full Question:** A company wants to send all AWS Systems Manager Session Manager logs to an Amazon S3 bucket for archival purposes. Which solution will meet this requirement with the most operational efficiency?

**Short Question:** Simplest, most efficient way to archive Session Manager logs to S3?

**Options:**
- ✅ A. Enable S3 logging in the Systems Manager console. Choose an S3 bucket to send the session data to.
- B. Install the Amazon CloudWatch agent. Push all logs to a CloudWatch log group. Export the logs to an S3 bucket from the group for archival purposes.
- C. Create a Systems Manager document to upload all server logs to a central S3 bucket. Use Amazon EventBridge to run the Systems Manager document against all servers that are in the account daily.
- D. Install an Amazon CloudWatch agent. Push all logs to a CloudWatch log group. Create a CloudWatch Logs subscription that pushes any incoming log events to an Amazon Kinesis Data Firehose delivery stream. Set Amazon S3 as the destination.

**Reason:** Session Manager has a built-in, native option to log directly to an S3 bucket (or CloudWatch Logs) with no agents or extra infrastructure, making option A the most operationally efficient. The other options introduce unnecessary agents, custom documents, or extra services (CloudWatch, EventBridge, Kinesis Data Firehose) that add complexity without added benefit.

---

## Question 264

**Full Question:** A company is storing petabytes of data in Amazon S3 Standard. The data is stored in multiple S3 buckets and is accessed with varying frequency. The company does not know access patterns for all the data. The company needs to implement a solution for each S3 bucket to optimize the cost of S3 usage. Which solution will meet these requirements with the most operational efficiency?

**Short Question:** Most hands-off way to cut S3 storage costs when access patterns are unknown/variable?

**Options:**
- ✅ A. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 Intelligent-Tiering.
- B. Use the S3 Storage Class Analysis tool to determine the correct tier for each object in the S3 bucket. Move each object to the identified storage tier.
- C. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 Glacier Instant Retrieval.
- D. Create an S3 Lifecycle configuration with a rule to transition the objects in the S3 bucket to S3 One Zone-Infrequent Access (S3 One Zone-IA).

**Reason:** S3 Intelligent-Tiering automatically moves objects between access tiers based on actual usage, requiring no manual analysis or management, which makes it the most efficient choice for unpredictable access patterns. Storage Class Analysis only gives recommendations (requiring manual moves), Glacier Instant Retrieval risks high retrieval fees for frequently accessed data, and One Zone-IA sacrifices multi-AZ durability that petabyte-scale data across multiple buckets likely needs.

---

## Question 265

**Full Question:** A company offers a food delivery service that is growing rapidly. Because of the growth, the company's order processing system is experiencing scaling problems during peak traffic hours. The current architecture includes the following: a group of Amazon EC2 instances that run in an Amazon EC2 Auto Scaling group to collect orders from the application, and another group of EC2 instances that run in an Amazon EC2 Auto Scaling group to fulfill orders. The order collection process occurs quickly, but the order fulfillment process can take longer. Data must not be lost because of a scaling event. A solutions architect must ensure that the order collection process and the order fulfillment process can both scale properly during peak traffic hours. The solution must optimize utilization of the company's AWS resources. Which solution meets these requirements?

**Short Question:** How to let order-collection and order-fulfillment tiers scale independently and efficiently without losing data during traffic spikes?

**Options:**
- A. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure each Auto Scaling group's minimum capacity according to peak workload values.
- B. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic that creates additional Auto Scaling groups on demand.
- C. Provision two Amazon Simple Queue Service (Amazon SQS) queues, one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Scale the Auto Scaling groups based on notifications that the queues send.
- ✅ D. Provision two Amazon Simple Queue Service (Amazon SQS) queues, one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Create a metric based on a backlog-per-instance calculation. Scale the Auto Scaling groups based on this metric.

**Reason:** Decoupling the two stages with separate SQS queues lets each tier scale independently, and scaling the fulfillment Auto Scaling group using a backlog-per-instance metric (derived from queue depth divided by instance count) directly reflects actual workload, optimizing resource usage without losing data. Setting minimum capacity to peak levels wastes resources, spinning up new Auto Scaling groups via SNS is needlessly complex, and Auto Scaling groups scale on metrics, not directly on queue notifications.

---

## Question 266

**Full Question:** A company has a multi-tier application deployed on several Amazon EC2 instances in an Auto Scaling group. An Amazon RDS for Oracle instance is the application's data layer, and it uses Oracle-specific PL/SQL functions. Traffic to the application has been steadily increasing, causing the EC2 instances to become overloaded and the RDS instance to run out of storage. The Auto Scaling group does not have any scaling metrics defined and only defines the minimum healthy instance count. The company predicts that traffic will continue to increase at a steady but unpredictable rate before leveling off. What should a solutions architect do to ensure the system can automatically scale for the increased traffic? (Choose two.)

**Short Question:** How do you automatically scale both an overloaded EC2 fleet and a storage-constrained RDS for Oracle database without touching the database engine?

**Options:**
- ✅ A. Configure storage autoscaling on the RDS for Oracle instance
- B. Migrate the database to Amazon Aurora to use autoscaling storage
- C. Configure an alarm on the RDS for Oracle instance for low free storage space
- ✅ D. Configure the Auto Scaling group to use average CPU utilization as the scaling metric
- E. Configure the Auto Scaling group to use average free memory as the scaling metric

**Reason:** RDS Storage Autoscaling automatically grows database storage with no manual work, directly fixing the "running out of storage" problem, while a target-tracking policy on average CPU utilization is the standard built-in Auto Scaling metric that automatically adds/removes EC2 instances as load changes. Aurora migration isn't viable due to Oracle-specific PL/SQL, a CloudWatch alarm alone only notifies rather than acting, and free memory isn't a native EC2 metric (it would need a custom CloudWatch agent, adding overhead).

---

## Question 267

**Full Question:** A company is running a critical business application on Amazon EC2 instances behind an Application Load Balancer. The EC2 instances run in an Auto Scaling group and access an Amazon RDS DB instance. The design did not pass an operational review because the EC2 instances and the DB instance are all located in a single Availability Zone. A solutions architect must update the design to use a second Availability Zone. Which solution will make the application highly available?

**Short Question:** What's the correct way to spread EC2 instances and an RDS database across two Availability Zones for high availability?

**Options:**
- A. Provision a subnet in each Availability Zone. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance with connections to each network.
- B. Provision two subnets that extend across both Availability Zones. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance with connections to each network.
- ✅ C. Provision a subnet in each Availability Zone. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance for Multi-AZ deployment.
- D. Provision a subnet that extends across both Availability Zones. Configure the Auto Scaling group to distribute the EC2 instances across both Availability Zones. Configure the DB instance for Multi-AZ deployment.

**Reason:** A subnet can only exist within a single Availability Zone, so options B and D are architecturally invalid, and a standard RDS instance can't "connect to multiple networks" as in option A. The correct approach is one subnet per AZ (so Auto Scaling can launch instances into each) combined with RDS Multi-AZ deployment, which automatically creates a standby replica in the second AZ and handles failover.

---

## Question 268

**Full Question:** A company is expecting rapid growth in the near future. A solutions architect needs to configure existing users and grant permissions to new users on AWS. The solutions architect has decided to create IAM groups and will add new users to IAM groups based on department. Which additional action is the most secure way to grant permissions to the new users?

**Short Question:** After creating department-based IAM groups, what's the most secure way to actually grant those users their permissions?

**Options:**
- A. Apply service control policies (SCPs) to manage access permissions
- B. Create IAM roles that have least-privilege permission. Attach the roles to the IAM groups.
- ✅ C. Create an IAM policy that grants least-privilege permission. Attach the policy to the IAM groups.
- D. Create IAM roles. Associate the roles with a permissions boundary that defines the maximum permissions.

**Reason:** IAM policies are the correct object to attach directly to IAM groups, so writing a least-privilege policy and attaching it to each group lets every user in that group automatically inherit the right permissions. SCPs only set maximum guardrails at the AWS Organizations level (they don't grant access), and IAM roles cannot be attached to groups at all, which rules out options B and D.

---

## Question 269

**Full Question:** A company is using Amazon CloudFront with its website. The company has enabled logging on the CloudFront distribution, and logs are saved in one of the company's Amazon S3 buckets. The company needs to perform advanced analyses on the logs and build visualizations. What should a solutions architect do to meet these requirements?

**Short Question:** Which AWS services let you run SQL analysis on CloudFront logs sitting in S3 and then build dashboards from the results?

**Options:**
- A. Use standard SQL queries in Amazon Athena to analyze the CloudFront logs in the S3 bucket. Visualize the results with AWS Glue.
- ✅ B. Use standard SQL queries in Amazon Athena to analyze the CloudFront logs in the S3 bucket. Visualize the results with Amazon QuickSight.
- C. Use standard SQL queries in Amazon DynamoDB to analyze the CloudFront logs in the S3 bucket. Visualize the results with AWS Glue.
- D. Use standard SQL queries in Amazon DynamoDB to analyze the CloudFront logs in the S3 bucket. Visualize the results with Amazon QuickSight.

**Reason:** Amazon Athena runs standard SQL directly against data in S3 without needing to load it anywhere, and Amazon QuickSight connects natively to Athena to build BI dashboards, making B the complete, serverless solution. DynamoDB is a NoSQL database and cannot query S3 data with SQL (ruling out C and D), and AWS Glue is an ETL service, not a visualization tool (ruling out A).

---

## Question 270

**Full Question:** A law firm needs to share information with the public. The information includes hundreds of files that must be publicly readable. Modifications or deletions of the files by anyone before a designated future date are prohibited. Which solution will meet these requirements in the most secure way?

**Short Question:** How do you make hundreds of files publicly readable on S3 while guaranteeing nobody can modify or delete them until a future date?

**Options:**
- A. Upload all files to an Amazon S3 bucket that is configured for static website hosting. Grant read-only IAM permissions to any AWS principals that access the S3 bucket until the designated date.
- ✅ B. Create a new Amazon S3 bucket with S3 versioning enabled. Use S3 Object Lock with a retention period in accordance with the designated date. Configure the S3 bucket for static website hosting. Set an S3 bucket policy to allow read-only access to the objects.
- C. Create a new Amazon S3 bucket with S3 versioning enabled. Configure an event trigger to run an AWS Lambda function in case of object modification or deletion. Configure the Lambda function to replace the objects with the original versions from a private S3 bucket.
- D. Upload all files to an Amazon S3 bucket that is configured for static website hosting. Select the folder that contains the files. Use S3 Object Lock with a retention period in accordance with the designated date. Grant read-only IAM permissions to any AWS principals that access the S3 bucket.

**Reason:** S3 Object Lock (which requires versioning) provides a true WORM guarantee that prevents deletion or modification for a set retention period, and pairing it with static website hosting plus a bucket policy correctly delivers public read access. IAM permissions alone (A) can be bypassed by other policies or the root user, the Lambda "fix-it-after" approach (C) is reactive rather than preventative, and Object Lock cannot be applied to a "folder" since it's an object/bucket-level feature (D).

---

## Question 271

**Full Question:** A company has a production workload that runs on 1,000 Amazon EC2 Linux instances. The workload is powered by third-party software. The company needs to patch the third-party software on all EC2 instances as quickly as possible to remediate a critical security vulnerability. What should a solutions architect do to meet these requirements?

**Short Question:** Fastest way to urgently patch a critical vulnerability across 1,000 EC2 instances?

**Options:**
- A. Create an AWS Lambda function to apply the patch to all EC2 instances
- B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances
- C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances
- ✅ D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances

**Reason:** Run Command executes a custom command across all 1,000 instances immediately and simultaneously, making it the fastest option; Patch Manager and maintenance windows are built for routine scheduled patching (not urgent one-off fixes), and a custom Lambda function would be unnecessary complexity when a purpose-built tool already exists.

---

## Question 272

**Full Question:** A solutions architect needs to allow team members to access Amazon S3 buckets in two different AWS accounts: a development account and a production account. The team currently has access to S3 buckets in the development account by using unique IAM users that are assigned to an IAM group that has appropriate permissions in the account. The solutions architect has created an IAM role in the production account. The role has a policy that grants access to an S3 bucket in the production account. Which solution will meet these requirements while complying with the principle of least privilege?

**Short Question:** Best way to let development-account IAM users access an S3 bucket in a production account, using least privilege?

**Options:**
- A. Attach the AdministratorAccess policy to the development account users
- ✅ B. Add the development account as a principal in the trust policy of the role in the production account
- C. Turn off the S3 Block Public Access feature on the S3 bucket in the production account
- D. Create a user in the production account with unique credentials for each team member

**Reason:** Adding the development account as a trusted principal lets its users assume the existing production-account role and get only that role's specific permissions, which is the standard secure cross-account access pattern; the other options either grant excessive access, expose the bucket publicly, or create unnecessary duplicate credentials.

---

## Question 273

**Full Question:** A solutions architect is designing the architecture of a new application being deployed to the AWS Cloud. The application will run on Amazon EC2 On-Demand Instances and will automatically scale across multiple Availability Zones. The EC2 instances will scale up and down frequently throughout the day. An Application Load Balancer (ALB) will handle the load distribution. The architecture needs to support distributed session data management. The company is willing to make changes to code if needed. What should the solutions architect do to ensure that the architecture supports distributed session data management?

**Short Question:** How to manage user session data reliably for an EC2 fleet that scales up and down frequently?

**Options:**
- ✅ A. Use Amazon ElastiCache to manage and store session data
- B. Use session affinity (sticky sessions) of the ALB to manage session data
- C. Use Session Manager from AWS Systems Manager to manage the session
- D. Use the GetSessionToken API operation in AWS Security Token Service (AWS STS) to manage the session

**Reason:** ElastiCache is a fast, centralized in-memory store that any instance can read from or write to, keeping the application stateless and resilient to instances scaling in or out; sticky sessions lose data when an instance terminates, Systems Manager Session Manager is for secure instance access (not app session data), and STS GetSessionToken issues security credentials, not application session storage.

---

## Question 274

**Full Question:** A company hosts a three-tier web application in the AWS Cloud. A Multi-AZ Amazon RDS for MySQL server forms the database layer. Amazon ElastiCache forms the cache layer. The company wants a caching strategy that adds or updates data in the cache when a customer adds an item to the database. The data in the cache must always match the data in the database. Which solution will meet these requirements?

**Short Question:** Which caching strategy keeps the cache and database always in sync on writes?

**Options:**
- A. Implement the lazy loading caching strategy
- ✅ B. Implement the write-through caching strategy
- C. Implement the adding TTL caching strategy
- D. Implement the AWS AppConfig caching strategy

**Reason:** Write-through caching writes to the cache and the database at the same time, guaranteeing they stay synchronized; lazy loading only populates the cache on a cache miss (so it can go stale), a TTL just controls expiration rather than sync, and AWS AppConfig is a configuration management service, not a caching strategy.

---

## Question 275

**Full Question:** A company is moving its on-premises Oracle database to Amazon Aurora PostgreSQL. The database has several applications that write to the same tables. The applications need to be migrated one by one, with a month in between each migration. Management has expressed concerns that the database has a high number of reads and writes. The data must be kept in sync across both databases throughout the migration. What should a solutions architect recommend?

**Short Question:** Best tool combination to migrate a high-transaction Oracle database to Aurora PostgreSQL while keeping both databases in sync during a months-long phased cutover?

**Options:**
- A. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a change data capture (CDC) replication task and a table mapping to select all tables
- B. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a full load plus change data capture (CDC) replication task and a table mapping to select all tables
- ✅ C. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a memory optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select all tables
- D. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a compute optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select the largest tables

**Reason:** This is a heterogeneous migration (Oracle to PostgreSQL) needing the Schema Conversion Tool to convert the schema, paired with DMS running a full load plus CDC task on all tables (not just the largest) to keep both databases synchronized, with a memory-optimized replication instance suited to the high read/write volume; AWS DataSync is for file/object data, not relational database migration.
