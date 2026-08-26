# AWS SAA-C03 Real Exam Questions & Answers — Part 10 (Q226–Q250)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 10](https://www.youtube.com/watch/LhVZ5K03m3c)

---

## Question 226

**Full Question:** A company is using Amazon Route 53 latency-based routing to route requests to its UDP-based application for users around the world. The application is hosted on redundant servers in the company's on-premises data centers in the United States, Asia, and Europe. The company's compliance requirements state that the application must be hosted on premises. The company wants to improve the performance and availability of the application. What should a solutions architect do to meet these requirements?

**Short Question:** What's the best way to boost performance and availability of a global, on-premises, UDP-based app without moving it off-premises?

**Options:**
- ✅ A. Configure three Network Load Balancers (NLBs) in the three AWS Regions to address the on-premises endpoints. Create an accelerator by using AWS Global Accelerator and register the NLBs as its endpoints. Provide access to the application by using a name that points to the accelerator DNS.
- B. Configure three Application Load Balancers (ALBs) in the three AWS Regions to address the on-premises endpoints. Create an accelerator by using AWS Global Accelerator and register the ALBs as its endpoints. Provide access to the application by using a name that points to the accelerator DNS.
- C. Configure three Network Load Balancers (NLBs) in the three AWS Regions to address the on-premises endpoints. In Route 53, create a latency-based record that points to the three NLBs and use it as an origin for an Amazon CloudFront distribution. Provide access to the application by using a name that points to the CloudFront DNS.
- D. Configure three Application Load Balancers (ALBs) in the three AWS Regions to address the on-premises endpoints. In Route 53, create a latency-based record that points to the three ALBs and use it as an origin for an Amazon CloudFront distribution. Provide access to the application by using a name that points to the CloudFront DNS.

**Reason:** Since the app uses UDP, only a Network Load Balancer (Layer 4) can handle the traffic — ALBs (Layer 7) and CloudFront both only support HTTP/HTTPS. AWS Global Accelerator can route UDP traffic over AWS's private global network to on-premises NLB endpoints, improving both latency and failover speed.

---

## Question 227

**Full Question:** A company runs an application using Amazon ECS. The application creates resized versions of an original image and then makes Amazon S3 API calls to store the resized images in Amazon S3. How can a solutions architect ensure that the application has permission to access Amazon S3?

**Short Question:** What's the correct, secure way to give an ECS-hosted application permission to call S3 APIs?

**Options:**
- A. Update the S3 role in AWS IAM to allow read/write access from Amazon ECS and then relaunch the container.
- ✅ B. Create an IAM role with S3 permissions and then specify that role as the task role in the task definition.
- C. Create a security group that allows access from Amazon ECS to Amazon S3 and update the launch configuration used by the ECS cluster.
- D. Create an IAM user with S3 permissions and then relaunch the Amazon EC2 instances for the ECS cluster while logged in as this account.

**Reason:** The AWS best practice is to attach an IAM role with the needed S3 permissions as the ECS task role in the task definition, which automatically supplies the container with temporary, secure credentials. Security groups only control network traffic (not IAM authorization), and using long-term IAM user credentials on EC2 instances is an insecure anti-pattern.

---

## Question 228

**Full Question:** A company runs demonstration environments for its customers on Amazon EC2 instances. Each environment is isolated in its own VPC. The company's operations team needs to be notified when RDP or SSH access to an environment has been established. Which solution meets these requirements?

**Short Question:** How do you get notified whenever someone establishes an SSH or RDP connection to EC2 instances in a VPC?

**Options:**
- A. Configure Amazon CloudWatch Application Insights to create AWS Systems Manager OpsItems when RDP or SSH access is detected.
- B. Configure the EC2 instances with an IAM instance profile that has an IAM role with the AmazonSSMManagedInstanceCore policy attached.
- ✅ C. Publish VPC flow logs to Amazon CloudWatch Logs. Create required metric filters. Create an Amazon CloudWatch metric alarm with a notification action for when the alarm is in the ALARM state.
- D. Configure an Amazon EventBridge rule to listen for events of type "EC2 Instance State-change Notification." Configure an Amazon Simple Notification Service (Amazon SNS) topic as a target. Subscribe the operations team to the topic.

**Reason:** VPC Flow Logs capture accepted connections on specific ports (22 for SSH, 3389 for RDP); sending them to CloudWatch Logs lets you build metric filters and an alarm that notifies the team (e.g., via SNS) when such a connection occurs. CloudWatch Application Insights monitors application health, the SSM policy just enables Session Manager as an SSH/RDP alternative, and EC2 instance state-change events only cover start/stop/terminate — none of these detect actual SSH/RDP connections.

---

## Question 229

**Full Question:** A company is launching an application on AWS. The application uses an Application Load Balancer (ALB) to direct traffic to at least two Amazon EC2 instances in a single target group. The instances are in an Auto Scaling group for each environment. The company requires a development environment and a production environment. The production environment will have periods of high traffic. Which solution will configure the development environment most cost-effectively?

**Short Question:** What's the cheapest way to right-size a dev environment (behind an ALB and Auto Scaling group) without touching production?

**Options:**
- A. Reconfigure the target group in the development environment to have only one EC2 instance as a target.
- B. Change the ALB balancing algorithm to least outstanding requests.
- C. Reduce the size of the EC2 instances in both environments.
- ✅ D. Reduce the maximum number of EC2 instances in the development environment's Auto Scaling group.

**Reason:** Since Auto Scaling manages target group membership, manually removing an instance from the target group would just get overridden as the group restores its desired capacity, and the load balancing algorithm doesn't change how many instances run. Lowering the max (and likely min/desired) instance count in the dev environment's Auto Scaling group directly limits running instances and cuts costs there without risking production's ability to handle high traffic.

---

## Question 230

**Full Question:** A company has a popular gaming platform running on AWS. The application is sensitive to latency because latency can impact the user experience and introduce unfair advantages to some players. The application is deployed in every AWS Region. It runs on Amazon EC2 instances that are part of Auto Scaling groups configured behind Application Load Balancers (ALBs). A solutions architect needs to implement a mechanism to monitor the health of the application and redirect traffic to healthy endpoints. Which solution meets these requirements?

**Short Question:** What's the best way to route global gaming traffic to the nearest healthy regional endpoint with minimal latency?

**Options:**
- ✅ A. Configure an accelerator in AWS Global Accelerator. Add a listener for the port that the application listens on and attach it to a regional endpoint in each Region. Add the ALB as the endpoint.
- B. Create an Amazon CloudFront distribution and specify the ALB as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.
- C. Create an Amazon CloudFront distribution and specify Amazon S3 as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.
- D. Configure an Amazon DynamoDB database to serve as the data store for the application. Create a DynamoDB Accelerator (DAX) cluster to act as the in-memory cache for DynamoDB hosting the application data.

**Reason:** AWS Global Accelerator is built for exactly this scenario — it provides static IPs, continuously health-checks regional ALB endpoints, and routes players over AWS's private global network to the nearest healthy endpoint, minimizing latency and jitter. CloudFront is a caching CDN for HTTP/HTTPS content (not ideal for real-time transactional traffic), and DynamoDB/DAX addresses database performance, not network routing or health-based failover.

---

## Question 231

**Full Question:** A solutions architect needs to design a new microservice for a company's application. Clients must be able to call an HTTPS endpoint to reach the microservice. The microservice also must use AWS Identity and Access Management (IAM) to authenticate calls. The solutions architect will write the logic for this microservice by using a single AWS Lambda function that is written in Go 1.x. Which solution will deploy the function in the most operationally efficient way?

**Short Question:** What's the most operationally efficient way to expose a single Lambda function as an HTTPS endpoint secured with IAM authentication?

**Options:**
- ✅ A. Create an Amazon API Gateway REST API. Configure the method to use the Lambda function. Enable IAM authentication on the API.
- B. Create a Lambda function URL for the function. Specify AWS IAM as the authentication type.
- C. Create an Amazon CloudFront distribution. Deploy the function to Lambda@Edge. Integrate IAM authentication logic into the Lambda@Edge function.
- D. Create an Amazon CloudFront distribution. Deploy the function to CloudFront Functions. Specify AWS IAM as the authentication type.

**Reason:** API Gateway is a fully managed service that gives you a managed HTTPS endpoint, native Lambda integration, and a built-in IAM authorization option, plus extra production features like custom domains, throttling, and caching. A Lambda function URL (B) also supports IAM auth but lacks these richer API management features; Lambda@Edge and CloudFront Functions (C, D) are meant for edge content customization, not hosting backend microservices, and don't offer built-in IAM authentication.

---

## Question 232

**Full Question:** A company has an online gaming application that has TCP and UDP multiplayer gaming capabilities. The company uses Amazon Route 53 to point the application traffic to multiple Network Load Balancers (NLBs) in different AWS Regions. The company needs to improve application performance and decrease latency for the online game in preparation for user growth. Which solution will meet these requirements?

**Short Question:** How do you cut latency and boost performance for a global TCP/UDP multiplayer game served by NLBs in multiple regions?

**Options:**
- A. Add an Amazon CloudFront distribution in front of the NLBs. Increase the Cache-Control max-age parameter.
- B. Replace the NLBs with Application Load Balancers (ALBs). Configure Route 53 to use latency-based routing.
- ✅ C. Add AWS Global Accelerator in front of the NLBs. Configure a Global Accelerator endpoint to use the correct listener ports.
- D. Add an Amazon API Gateway endpoint behind the NLBs. Enable API caching. Override method caching for the different stages.

**Reason:** AWS Global Accelerator routes both TCP and UDP traffic over the AWS global network via the nearest edge location, reducing latency and jitter, and it can use NLBs as endpoints. CloudFront (A) and API Gateway (D) are HTTP/S-only services, and ALBs (B) don't support UDP, so none of them work for generic TCP/UDP gaming traffic.

---

## Question 233

**Full Question:** A company has a legacy data processing application that runs on Amazon EC2 instances. Data is processed sequentially, but the order of results does not matter. The application uses a monolithic architecture. The only way that the company can scale the application to meet increased demand is to increase the size of the instances. The company's developers have decided to rewrite the application to use a microservices architecture on Amazon Elastic Container Service (Amazon ECS). What should a solutions architect recommend for communication between the microservices?

**Short Question:** What's the best way for decoupled ECS microservices to pass work items to each other when order doesn't matter?

**Options:**
- ✅ A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Add code to the data producers to send data to the queue. Add code to the data consumers to process data from the queue.
- B. Create an Amazon Simple Notification Service (Amazon SNS) topic. Add code to the data producers to publish notifications to the topic. Add code to the data consumers to subscribe to the topic.
- C. Create an AWS Lambda function to pass messages. Add code to the data producers to call the Lambda function with a data object. Add code to the data consumers to receive a data object that is passed from the Lambda function.
- D. Create an Amazon DynamoDB table. Enable DynamoDB Streams. Add code to the data producers to insert data into the table. Add code to the data consumers to use the DynamoDB Streams API to detect new table entries and retrieve the data.

**Reason:** SQS is a fully managed queue built exactly for decoupling producers from consumers, letting you scale consumers independently while messages persist durably until processed. SNS (B) fans out every message to all subscribers (causing duplicate processing, not task distribution), Lambda-as-broker (C) reinvents queuing/retry logic that SQS already provides, and DynamoDB Streams (D) is meant for reacting to data changes, not for general-purpose task queuing.

---

## Question 234

**Full Question:** A company is building a game system that needs to send unique events to separate leaderboard, matchmaking, and authentication services concurrently. The company needs an AWS event-driven system that guarantees the order of the events. Which solution will meet these requirements?

**Short Question:** Which service can fan out the same event to three separate services at once while strictly preserving event order?

**Options:**
- A. Amazon EventBridge event bus
- ✅ B. Amazon Simple Notification Service (Amazon SNS) FIFO topics
- C. Amazon Simple Notification Service (Amazon SNS) standard topics
- D. Amazon Simple Queue Service (Amazon SQS) FIFO queues

**Reason:** SNS FIFO topics both fan out a message to multiple subscribers (leaderboard, matchmaking, authentication) and strictly preserve message order. EventBridge (A) fans out but doesn't guarantee order, SNS standard topics (C) only offer best-effort ordering, and SQS FIFO queues (D) guarantee order but deliver each message to only one consumer, so they can't fan out to three services at once.

---

## Question 235

**Full Question:** A company is migrating its workloads to AWS. The company has transactional and sensitive data in its databases. The company wants to use AWS cloud solutions to increase security and reduce operational overhead for the databases. Which solution will meet these requirements?

**Short Question:** What's the lowest-overhead, most secure way to host a transactional database with sensitive data on AWS?

**Options:**
- A. Migrate the databases to Amazon EC2. Use AWS Key Management Service (AWS KMS) AWS managed key for encryption.
- ✅ B. Migrate the databases to Amazon RDS. Configure encryption at rest.
- C. Migrate the data to Amazon S3. Use Amazon Macie for data security and protection.
- D. Migrate the database to Amazon RDS. Use Amazon CloudWatch Logs for data security and protection.

**Reason:** Amazon RDS is a fully managed relational database service that handles patching, backups, and failover, and it lets you easily turn on encryption at rest with KMS, satisfying both the security and low-overhead requirements. EC2 (A) still leaves the company managing the database itself, S3 (C) is object storage unsuited to transactional workloads, and CloudWatch Logs (D) is a monitoring tool, not an encryption/data-protection feature.

---

## Question 236

**Full Question:** An entertainment company is using Amazon DynamoDB to store media metadata. The application is read-intensive and is experiencing delays. The company does not have staff to handle additional operational overhead and needs to improve the performance efficiency of DynamoDB without reconfiguring the application. What should a solutions architect recommend to meet this requirement?

**Short Question:** What's the fully managed way to speed up reads on a read-heavy DynamoDB table without changing application code?

**Options:**
- A. Use Amazon ElastiCache for Redis
- ✅ B. Use Amazon DynamoDB Accelerator (DAX)
- C. Replicate data by using DynamoDB global tables
- D. Use Amazon ElastiCache for Memcached with auto discovery enabled

**Reason:** DAX is a fully managed, in-memory cache that is API-compatible with DynamoDB, so an application only needs to point to the DAX endpoint instead of DynamoDB directly, with no code changes required. ElastiCache options (A and D) require adding caching logic in the application, and global tables (C) solve cross-region latency, not in-region read performance.

---

## Question 237

**Full Question:** A company is implementing a shared storage solution for a gaming application that is hosted in the AWS Cloud. The company needs the ability to use Lustre clients to access data. The solution must be fully managed. Which solution meets these requirements?

**Short Question:** Which fully managed AWS storage service supports Lustre client access?

**Options:**
- A. Create an AWS DataSync task that shares the data as a mountable file system. Mount the file system to the application server.
- B. Create an AWS Storage Gateway file gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- C. Create an Amazon Elastic File System (Amazon EFS) file system, and configure it to support Lustre. Attach the file system to the origin server. Connect the application server to the file system.
- ✅ D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

**Reason:** Amazon FSx for Lustre is the purpose-built, fully managed AWS service that provides a high-performance file system based on the Lustre protocol. DataSync is a migration tool (not primary storage), Storage Gateway file gateway only supports NFS/SMB, and EFS uses NFS and cannot be configured for Lustre.

---

## Question 238

**Full Question:** A company has a three-tier application on AWS that ingests sensor data from its users' devices. The traffic flows through a Network Load Balancer (NLB), then to Amazon EC2 instances for the web tier, and finally to EC2 instances for the application tier. The application tier makes calls to a database. What should a solutions architect do to improve the security of the data in transit?

**Short Question:** How do you encrypt network traffic in transit for an application behind a Network Load Balancer?

**Options:**
- ✅ A. Configure a TLS listener. Deploy the server certificate on the NLB.
- B. Configure AWS Shield Advanced. Enable AWS WAF on the NLB.
- C. Change the load balancer to an Application Load Balancer (ALB). Enable AWS WAF on the ALB.
- D. Encrypt the Amazon Elastic Block Store (Amazon EBS) volume on the EC2 instances by using AWS Key Management Service (AWS KMS).

**Reason:** Adding a TLS listener with an SSL/TLS certificate on the NLB is the standard way to encrypt traffic in transit between clients and the load balancer. AWS Shield and WAF protect against attacks rather than encrypting traffic (and WAF can't attach to an NLB), switching to an ALB alone doesn't encrypt anything without a TLS listener, and EBS encryption only protects data at rest, not data in transit.

---

## Question 239

**Full Question:** An e-commerce company has noticed performance degradation of its Amazon RDS-based web application. The performance degradation is attributed to an increase in the number of read-only SQL queries triggered by business analysts. A solutions architect needs to solve the problem with minimal changes to the existing web application. What should the solutions architect recommend?

**Short Question:** How do you offload heavy read-only analyst SQL queries from a struggling RDS database with minimal app changes?

**Options:**
- A. Export the data to Amazon DynamoDB and have the business analysts run their queries.
- B. Load the data into Amazon ElastiCache and have the business analysts run their queries.
- ✅ C. Create a read replica of the primary database and have the business analysts run their queries.
- D. Copy the data into an Amazon Redshift cluster and have the business analysts run their queries.

**Reason:** An RDS read replica is a read-only copy purpose-built to absorb heavy read/reporting workloads, letting analysts query it while the primary database is freed up for the web application, with no changes needed to the application itself. DynamoDB doesn't support SQL, ElastiCache isn't built for analytical queries, and Redshift would require building a complex ETL pipeline, which is overkill here.

---

## Question 240

**Full Question:** A solutions architect observes that a nightly batch processing job is automatically scaled up for 1 hour before the desired Amazon EC2 capacity is reached. The peak capacity is the same every night and the batch jobs always start at 1:00 a.m. The solutions architect needs to find a cost-effective solution that will allow the desired EC2 capacity to be reached quickly and allow the Auto Scaling group to scale down after the batch jobs are complete. What should the solutions architect do to meet these requirements?

**Short Question:** What's the cost-effective way to have EC2 capacity ready instantly for a predictable nightly batch job?

**Options:**
- A. Increase the minimum capacity for the Auto Scaling group.
- B. Increase the maximum capacity for the Auto Scaling group.
- ✅ C. Configure scheduled scaling to scale up to the desired compute level.
- D. Change the scaling policy to add more EC2 instances during each scaling operation.

**Reason:** Since the batch job starts at a known, fixed time every night, scheduled scaling can proactively raise capacity right before 1:00 a.m. and scale back down afterward, which is both fast and cost-effective. Raising the minimum keeps unneeded capacity running 24/7, raising the maximum doesn't fix slow reactive scaling, and adding more instances per scaling step is still reactive and risks overprovisioning.

---

## Question 241

**Full Question:** A new employee has joined a company as a deployment engineer. The deployment engineer will be using AWS CloudFormation templates to create multiple AWS resources. A solutions architect wants the deployment engineer to perform job activities while following the principle of least privilege. Which combination of actions should the solutions architect take to accomplish this goal? (Choose two.)

**Short Question:** What two IAM setup steps let a deployment engineer use CloudFormation while following least privilege?

**Options:**
- A. Have the deployment engineer use AWS account root user credentials for performing AWS CloudFormation stack operations
- B. Create a new IAM user for the deployment engineer and add the IAM user to a group that has the PowerUserAccess IAM policy attached
- C. Create a new IAM user for the deployment engineer and add the IAM user to a group that has the AdministratorAccess IAM policy attached
- ✅ D. Create a new IAM user for the deployment engineer and add the IAM user to a group that has an IAM policy that allows AWS CloudFormation actions only
- ✅ E. Create an IAM role for the deployment engineer to explicitly define the permissions specific to the AWS CloudFormation stack, and launch stacks using that IAM role

**Reason:** A dedicated IAM user/group scoped only to CloudFormation actions (D) gives the engineer just what they need, and a separate IAM role assumed by the stack itself (E) lets CloudFormation provision resources without granting the engineer's own user broad permissions — together this is the least-privilege approach, unlike root credentials or the overly broad PowerUserAccess/AdministratorAccess policies.

---

## Question 242

**Full Question:** A company's infrastructure consists of Amazon EC2 instances and an Amazon RDS DB instance in a single AWS Region. The company wants to back up its data in a separate Region. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-effort way to back up both EC2 and RDS to another AWS Region?

**Options:**
- ✅ A. Use AWS Backup to copy EC2 backups and RDS backups to the separate Region
- B. Use Amazon Data Lifecycle Manager (Amazon DLM) to copy EC2 backups and RDS backups to the separate Region
- C. Create Amazon Machine Images (AMIs) of the EC2 instances. Copy the AMIs to the separate Region. Create a read replica for the RDS DB instance in the separate Region
- D. Create Amazon Elastic Block Store (Amazon EBS) snapshots. Copy the EBS snapshots to the separate Region. Create RDS snapshots. Export the RDS snapshots to Amazon S3. Configure S3 Cross-Region Replication (CRR) to the separate Region

**Reason:** AWS Backup is a fully managed, centralized service that natively supports cross-Region backup policies for both EC2 and RDS in one place, whereas DLM only handles EBS snapshots and the other two options require manually managing multiple separate services and steps, adding operational overhead.

---

## Question 243

**Full Question:** A company has a three-tier application for image sharing. The application uses an Amazon EC2 instance for the front-end layer, another EC2 instance for the application layer, and a third EC2 instance for a MySQL database. A solutions architect must design a scalable and highly available solution that requires the least amount of change to the application. Which solution meets these requirements?

**Short Question:** How do you make this single-EC2-instance three-tier app scalable and highly available with minimal rearchitecting?

**Options:**
- A. Use Amazon S3 to host the front-end layer. Use AWS Lambda functions for the application layer. Move the database to an Amazon DynamoDB table. Use Amazon S3 to store and serve users' images
- B. Use load-balanced, Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS DB instance with multiple read replicas to serve users' images
- C. Use Amazon S3 to host the front-end layer. Use a fleet of EC2 instances in an Auto Scaling group for the application layer. Move the database to a memory-optimized instance type to store and serve users' images
- ✅ D. Use load-balanced, Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS Multi-AZ DB instance. Use Amazon S3 to store and serve users' images

**Reason:** Option D keeps the existing application logic nearly unchanged by letting Elastic Beanstalk manage load-balanced, Multi-AZ front-end and application tiers, moves the database to a fully managed Multi-AZ RDS instance for high availability, and correctly stores images in S3 rather than misusing read replicas or database instances as image stores.

---

## Question 244

**Full Question:** A company observes an increase in Amazon EC2 costs in its most recent bill. The billing team notices unwanted vertical scaling of instance types for a couple of EC2 instances. A solutions architect needs to create a graph comparing the last two months of EC2 costs and perform an in-depth analysis to identify the root cause of the vertical scaling. How should the solutions architect generate the information with the least operational overhead?

**Short Question:** What's the easiest built-in tool to graph and deep-dive into an EC2 cost spike caused by instance type changes?

**Options:**
- A. Use AWS Budgets to create a budget report and compare EC2 costs based on instance types
- ✅ B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types
- C. Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last two months
- D. Use AWS Cost and Usage Reports to create a report and send it to an Amazon S3 bucket. Use Amazon QuickSight with Amazon S3 as a source to generate an interactive graph based on instance types

**Reason:** Cost Explorer is purpose-built for visualizing and filtering historical cost data by dimensions like instance type with no setup required, while Budgets is for threshold alerts, the Billing dashboard is too high-level, and Cost and Usage Reports plus QuickSight require significant setup and ongoing management.

---

## Question 245

**Full Question:** A company has deployed a web application on AWS. The company hosts the back-end database on Amazon RDS for MySQL with a primary DB instance and five read replicas to support scaling needs. The read replicas must lag no more than 1 second behind the primary DB instance. The database routinely runs scheduled stored procedures. As traffic on the website increases, the replicas experience additional lag during periods of peak load. A solutions architect must reduce the replication lag as much as possible. The solutions architect must minimize changes to the application code and must minimize ongoing operational overhead. Which solution will meet these requirements?

**Short Question:** How do you cut MySQL read-replica lag on RDS during peak traffic with minimal code changes and low ongoing effort?

**Options:**
- ✅ A. Migrate the database to Amazon Aurora MySQL. Replace the read replicas with Aurora Replicas and configure Aurora Auto Scaling. Replace the stored procedures with Aurora MySQL native functions
- B. Deploy an Amazon ElastiCache for Redis cluster in front of the database. Modify the application to check the cache before the application queries the database. Replace the stored procedures with AWS Lambda functions
- C. Migrate the database to a MySQL database that runs on Amazon EC2 instances. Choose large compute-optimized EC2 instances for all replica nodes. Maintain the stored procedures on the EC2 instances
- D. Migrate the database to Amazon DynamoDB. Provision a large number of read capacity units (RCUs) to support the required throughput and configure on-demand capacity scaling. Replace the stored procedures with DynamoDB Streams

**Reason:** Amazon Aurora MySQL uses a shared, decoupled storage architecture so Aurora Replicas lag only milliseconds behind the primary (versus standard RDS read replica lag), it stays MySQL-compatible so code changes are minimal, and Aurora Auto Scaling handles replica scaling automatically with low operational overhead — unlike adding a cache layer (doesn't fix replica lag and needs app changes), self-managing MySQL on EC2 (raises overhead), or migrating to DynamoDB (requires a full application rewrite).

---

## Question 246

**Full Question:** A company is building a new web-based customer relationship management application. The application will use several Amazon EC2 instances that are backed by Amazon Elastic Block Store (Amazon EBS) volumes behind an Application Load Balancer (ALB). The application will also use an Amazon Aurora database. All data for the application must be encrypted at rest and in transit. Which solution will meet these requirements?

**Short Question:** Which combination of AWS services correctly encrypts EBS/Aurora storage at rest and ALB traffic in transit?

**Options:**
- A. Use AWS Key Management Service (AWS KMS) certificates on the ALB to encrypt data in transit. Use AWS Certificate Manager (ACM) to encrypt the EBS volumes and Aurora database storage at rest.
- B. Use the AWS root account to log in to the AWS Management Console. Upload the company's encryption certificates. While in the root account, select the option to turn on encryption for all data at rest and in transit for the account.
- ✅ C. Use AWS Key Management Service (AWS KMS) to encrypt the EBS volumes and Aurora database storage at rest. Attach an AWS Certificate Manager (ACM) certificate to the ALB to encrypt data in transit.
- D. Use BitLocker to encrypt all data at rest. Import the company's TLS certificate keys to AWS Key Management Service (AWS KMS). Attach the KMS keys to the ALB to encrypt data in transit.

**Reason:** AWS KMS is the correct service for encrypting data at rest (EBS volumes and Aurora storage), while ACM provides the SSL/TLS certificates attached to the ALB to encrypt data in transit — option A reverses these roles, B relies on a nonexistent one-click root account setting (and misuses the root account), and D uses BitLocker (an on-premises tool, not native to EBS) and incorrectly tries to attach KMS keys directly to the ALB instead of using ACM.

---

## Question 247

**Full Question:** A transaction processing company has weekly scripted batch jobs that run on Amazon EC2 instances. The EC2 instances are in an Auto Scaling group. The number of transactions can vary, but the baseline CPU utilization that is noted on each run is at least 60%. The company needs to provision the capacity 30 minutes before the jobs run. Currently, engineers complete this task by manually modifying the Auto Scaling group parameters. The company does not have the resources to analyze the required capacity trends for the Auto Scaling group counts. The company needs an automated way to modify the Auto Scaling group's desired capacity. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the least-effort way to automatically pre-provision Auto Scaling capacity 30 minutes before a recurring weekly batch job, without manually analyzing capacity trends?

**Options:**
- A. Create a dynamic scaling policy for the Auto Scaling group. Configure the policy to scale based on the CPU utilization metric. Set the target value for the metric to 60%.
- B. Create a scheduled scaling policy for the Auto Scaling group. Set the appropriate desired capacity, minimum capacity, and maximum capacity. Set the recurrence to weekly. Set the start time to 30 minutes before the batch jobs run.
- ✅ C. Create a predictive scaling policy for the Auto Scaling group. Configure the policy to scale based on forecast. Set the scaling metric to CPU utilization. Set the target value for the metric to 60%. In the policy, set the instances to pre-launch 30 minutes before the jobs run.
- D. Create an Amazon EventBridge event to invoke an AWS Lambda function when the CPU utilization metric value for the Auto Scaling group reaches 60%. Configure the Lambda function to increase the Auto Scaling group's desired capacity and maximum capacity by 20%.

**Reason:** Predictive scaling uses machine learning to forecast recurring demand and automatically pre-launches instances ahead of time (here, 30 minutes early) without requiring manual trend analysis, whereas dynamic scaling (A) and the EventBridge/Lambda approach (D) are both reactive (they only scale after the CPU threshold is already breached) and scheduled scaling (B) requires manually calculating and maintaining fixed capacity values, which is error-prone given variable transaction volumes.

---

## Question 248

**Full Question:** A company wants to create a mobile app that allows users to stream slow-motion video clips on their mobile devices. Currently, the app captures video clips and uploads the video clips in raw format into an Amazon S3 bucket. The app retrieves these video clips directly from the S3 bucket. However, the videos are large in their raw format. Users are experiencing issues with buffering and playback on mobile devices. The company wants to implement solutions to maximize the performance and scalability of the app while minimizing operational overhead. Which combination of solutions will meet these requirements? (Choose two.)

**Short Question:** Which two managed services best fix buffering/playback issues caused by streaming large raw video files to mobile users, with minimal operational overhead?

**Options:**
- ✅ A. Deploy Amazon CloudFront for content delivery and caching.
- B. Use AWS DataSync to replicate the video files across AWS Regions in other S3 buckets.
- ✅ C. Use Amazon Elastic Transcoder to convert the video files to more appropriate formats.
- D. Deploy an Auto Scaling group of Amazon EC2 instances in Local Zones for content delivery and caching.
- E. Deploy an Auto Scaling group of Amazon EC2 instances to convert the video files to more appropriate formats.

**Reason:** Amazon CloudFront caches content at edge locations to cut latency and buffering, while Amazon Elastic Transcoder converts the large raw videos into mobile-optimized formats to reduce bandwidth needs — both are fully managed with minimal overhead, unlike self-managed EC2-based caching or transcoding (D, E) or DataSync, which is a data transfer tool, not a content-delivery or transcoding solution (B).

---

## Question 249

**Full Question:** A company runs a web application that is backed by Amazon RDS. A new database administrator caused data loss by accidentally editing information in a database table. To help recover from this type of incident, the company wants the ability to restore the database to its state from 5 minutes before any change within the last 30 days. Which feature should the solutions architect include in the design to meet this requirement?

**Short Question:** Which RDS feature lets you restore a database to any specific point in time within the last 30 days?

**Options:**
- A. Read replicas
- B. Manual snapshots
- ✅ C. Automated backups
- D. Multi-AZ deployments

**Reason:** Amazon RDS automated backups combine daily snapshots with continuous transaction logs, enabling point-in-time recovery (PITR) to any second within the retention period (up to 35 days), whereas read replicas and Multi-AZ standbys mirror the primary's data in near real-time (so they'd contain the same bad data) and manual snapshots would require impractically taking one every 5 minutes to meet this requirement.

---

## Question 250

**Full Question:** A company is conducting an internal audit. The company wants to ensure that the data in an Amazon S3 bucket that is associated with the company's AWS Lake Formation data lake does not contain sensitive customer or employee data. The company wants to discover personally identifiable information (PII) or financial information, including passport numbers and credit card numbers. Which solution will meet these requirements?

**Short Question:** Which AWS service automatically scans S3 data to discover sensitive PII and financial information like passport and credit card numbers?

**Options:**
- A. Configure AWS Audit Manager on the account. Select the Payment Card Industry Data Security Standard (PCI DSS) for auditing.
- B. Configure Amazon S3 Inventory on the S3 bucket. Configure Amazon Athena to query the inventory.
- ✅ C. Configure Amazon Macie to run a data discovery job that uses managed identifiers for the required data types.
- D. Use Amazon S3 Select to run a report across the S3 bucket.

**Reason:** Amazon Macie is a fully managed service that uses machine learning and pattern matching with pre-built managed identifiers to automatically discover sensitive data like PII, passport numbers, and credit card numbers within S3 content, whereas Audit Manager only assesses compliance evidence (not file content), S3 Inventory plus Athena only queries object metadata (not content), and S3 Select only retrieves data from individual objects rather than performing bucket-wide automated discovery.
