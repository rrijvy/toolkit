# AWS SAA-C03 Real Exam Questions & Answers — Part 8 (Q176–Q200)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 8](https://www.youtube.com/watch/KG0YEf2EGpw)

---

## Question 176

**Full Question:** A company provides an API interface to customers so the customers can retrieve their financial information. The company expects a larger number of requests during peak usage times of the year. The company requires the API to respond consistently with low latency to ensure customer satisfaction. The company needs to provide a compute host for the API. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Which compute setup gives an API low-latency, spike-tolerant performance with the least operational overhead?

**Options:**
- A. Use an Application Load Balancer and Amazon Elastic Container Service (Amazon ECS)
- ✅ B. Use Amazon API Gateway and AWS Lambda functions with provisioned concurrency
- C. Use an Application Load Balancer and an Amazon Elastic Kubernetes Service (Amazon EKS) cluster
- D. Use Amazon API Gateway and AWS Lambda functions with reserved concurrency

**Reason:** Provisioned concurrency keeps Lambda execution environments pre-warmed, eliminating cold starts and giving consistent low latency in a fully serverless (least-overhead) setup, whereas reserved concurrency only caps/guarantees concurrency without preventing cold starts, and ECS/EKS both require far more infrastructure management.

---

## Question 177

**Full Question:** A solutions architect is creating a new VPC design. There are two public subnets for the load balancer, two private subnets for web servers, and two private subnets for MySQL. The web servers use only HTTPS. The solutions architect has already created a security group for the load balancer allowing port 443 from 0.0.0.0/0. Company policy requires that each resource has the least access required to still be able to perform its tasks. Which additional configuration strategy should the solutions architect use to meet these requirements?

**Short Question:** How should security be configured for the web and database tiers to enforce least privilege in this three-tier VPC?

**Options:**
- A. Create a security group for the web servers and allow port 443 from 0.0.0.0/0; create a security group for the MySQL servers and allow port 3306 from the web server security group
- B. Create a network ACL for the web servers and allow port 443 from 0.0.0.0/0; create a network ACL for the MySQL servers and allow port 3306 from the web server security group
- ✅ C. Create a security group for the web servers and allow port 443 from the load balancer; create a security group for the MySQL servers and allow port 3306 from the web server security group
- D. Create a network ACL for the web servers and allow port 443 from the load balancer; create a network ACL for the MySQL servers and allow port 3306 from the web server security group

**Reason:** Security groups can reference other security groups as a traffic source, letting each tier accept traffic only from the tier directly in front of it (load balancer to web, web to database); network ACLs cannot reference a security group as a source, and opening port 443 to 0.0.0.0/0 on the web tier bypasses the load balancer and violates least privilege.

---

## Question 178

**Full Question:** A company wants to migrate an Oracle database to AWS. The database consists of a single table that contains millions of geographic information system (GIS) images that are high resolution and are identified by a geographic code. When a natural disaster occurs, tens of thousands of images get updated every few minutes. Each geographic code has a single image or row associated with it. The company wants a solution that is highly available and scalable during such events. Which solution meets these requirements most cost-effectively?

**Short Question:** What's the most cost-effective, highly available way to migrate a table of huge GIS images with a very high update rate off Oracle?

**Options:**
- A. Store the images and geographic codes in a database table. Use Oracle running on an Amazon RDS Multi-AZ DB instance
- B. Store the images in Amazon S3 buckets. Use Amazon DynamoDB with the geographic code as the key and the image S3 URL as the value
- C. Store the images and geographic codes in an Amazon DynamoDB table. Configure DynamoDB Accelerator (DAX) during times of high load
- ✅ D. Store the images in Amazon S3 buckets. Store geographic codes and image S3 URLs in a database table. Use Oracle running on an Amazon RDS Multi-AZ DB instance

**Reason:** Large binary images belong in Amazon S3 (scalable, durable, cost-effective), while the database only needs to hold small metadata records (geographic code plus S3 URL), and keeping Oracle on RDS Multi-AZ gives high availability with minimal migration changes; storing images directly in a relational database or DynamoDB is an anti-pattern (DynamoDB also caps items at 400 KB, and DAX only helps reads, not the heavy write load described).

---

## Question 179

**Full Question:** An e-commerce company needs to run a scheduled daily job to aggregate and filter sales records for analytics. The company stores the sales records in an Amazon S3 bucket. Each object can be up to 10 GB in size. Based on the number of sales events, the job can take up to an hour to complete. The CPU and memory usage of the job are constant and are known in advance. A solutions architect needs to minimize the amount of operational effort that is needed for the job to run. Which solution meets these requirements?

**Short Question:** What's the lowest-effort way to run a scheduled, up-to-1-hour-long batch job with known/constant CPU and memory needs?

**Options:**
- A. Create an AWS Lambda function that has an Amazon EventBridge notification. Schedule the EventBridge event to run once a day
- B. Create an AWS Lambda function. Create an Amazon API Gateway HTTP API and integrate the API with the function. Create an Amazon EventBridge scheduled event that calls the API and invokes the function
- ✅ C. Create an Amazon ECS cluster with an AWS Fargate launch type. Create an Amazon EventBridge scheduled event that launches an ECS task on the cluster to run the job
- D. Create an Amazon ECS cluster with an Amazon EC2 launch type and an Auto Scaling group with at least one EC2 instance. Create an Amazon EventBridge scheduled event that launches an ECS task on the cluster to run the job

**Reason:** AWS Lambda has a hard 15-minute execution limit, which rules out a job that can run up to an hour, so options A and B fail regardless of how they're invoked; ECS with Fargate runs the long-running containerized job without managing any servers, while the EC2 launch type in option D would still require patching and scaling EC2 instances, adding unnecessary operational overhead.

---

## Question 180

**Full Question:** A solutions architect wants all new users to have specific complexity requirements and mandatory rotation periods for IAM user passwords. What should the solutions architect do to accomplish this?

**Short Question:** What's the native AWS way to enforce password complexity and rotation rules for all IAM users?

**Options:**
- ✅ A. Set an overall password policy for the entire AWS account
- B. Set a password policy for each IAM user in the AWS account
- C. Use third-party vendor software to set password requirements
- D. Attach an Amazon CloudWatch rule to the "create new user" event to set the password with the appropriate requirements

**Reason:** AWS IAM supports a single account-wide password policy that defines minimum length, character complexity, expiration/rotation, and reuse prevention, and it automatically applies to all IAM users including future ones; IAM has no per-user password policy feature, third-party software is unnecessary when a native option exists, and a CloudWatch rule triggered on user creation cannot retroactively enforce password rules since those are enforced only when a password is set or changed.

---

## Question 181

**Full Question:** An e-commerce company is running a multi-tier application on AWS. The front end and backend tiers both run on Amazon EC2, and the database runs on Amazon RDS for MySQL. The backend tier communicates with the RDS instance. There are frequent calls to return identical data sets from the database that are causing performance slowdowns. Which action should be taken to improve the performance of the backend?

**Short Question:** What's the best way to reduce database load when the same query results are requested repeatedly?

**Options:**
- A. Implement Amazon SNS to store the database calls
- ✅ B. Implement Amazon ElastiCache to cache the large data sets
- C. Implement an RDS for MySQL read replica to cache database calls
- D. Implement Amazon Kinesis Data Firehose to stream the calls to the database

**Reason:** Amazon ElastiCache is a managed in-memory caching service that lets the application store frequent query results and serve them instantly without hitting the database again. SNS is a messaging service (not caching), a read replica would still have to re-run the same expensive query each time, and Kinesis Data Firehose is for streaming data ingestion, not query performance.

---

## Question 182

**Full Question:** A retail company has several businesses. The IT team for each business manages its own AWS account. Each team's account is part of an organization in AWS Organizations. Each team monitors its product inventory levels in an Amazon DynamoDB table in the team's own AWS account. The company is deploying a central inventory reporting application into a shared AWS account. The application must be able to read items from all the teams' DynamoDB tables. Which authentication option will meet these requirements most securely?

**Short Question:** What's the most secure way for one central application to read DynamoDB tables that live in many separate AWS accounts?

**Options:**
- A. Integrate DynamoDB with AWS Secrets Manager in the inventory application account. Configure the application to use the correct secret from Secrets Manager to authenticate and read the DynamoDB table. Schedule secret rotation for every 30 days.
- B. In every business account, create an IAM user that has programmatic access. Configure the application to use the correct IAM user access key ID and secret access key to authenticate and read the DynamoDB table. Manually rotate IAM access keys every 30 days.
- ✅ C. In every business account, create an IAM role named BU-RO with a policy that gives the role access to the DynamoDB table and a trust policy to trust a specific role in the inventory application account. In the inventory account, create a role named app_role that allows access to the STS AssumeRole API operation. Configure the application to use app_role and assume the cross-account role BU-RO to read the DynamoDB table.
- D. Integrate DynamoDB with AWS Certificate Manager (ACM). Generate identity certificates to authenticate DynamoDB. Configure the application to use the correct certificate to authenticate and read the DynamoDB table.

**Reason:** Cross-account IAM roles combined with STS AssumeRole give the application temporary, short-lived credentials instead of long-term keys, which is the AWS best practice for cross-account access. Secrets Manager and IAM user access keys both still rely on long-term credentials that carry more risk, and ACM only manages TLS certificates, not IAM authentication or authorization.

---

## Question 183

**Full Question:** A company needs to provide its employees with secure access to confidential and sensitive files. The company wants to ensure that the files can be accessed only by authorized users. The files must be downloaded securely to the employees' devices. The files are stored on an on-premises Windows file server. However, due to an increase in remote usage, the file server is running out of capacity. Which solution will meet these requirements?

**Short Question:** What's the best way to replace an overloaded on-premises Windows file server with a scalable, secure solution for remote employees?

**Options:**
- A. Migrate the file server to an Amazon EC2 instance in a public subnet. Configure the security group to limit inbound traffic to the employees' IP addresses.
- ✅ B. Migrate the files to an Amazon FSx for Windows File Server file system. Integrate the Amazon FSx file system with the on-premises Active Directory. Configure AWS Client VPN.
- C. Migrate the files to Amazon S3 and create a private VPC endpoint. Create a signed URL to allow download.
- D. Migrate the files to Amazon S3 and create a public VPC endpoint. Allow employees to sign on with AWS IAM Identity Center (AWS Single Sign-On).

**Reason:** Amazon FSx for Windows File Server provides a scalable, native Windows file system that can integrate with the existing Active Directory, and AWS Client VPN gives remote employees a secure, encrypted connection for downloading files. Putting the server in a public subnet is risky and doesn't scale with changing employee IPs, S3 doesn't replicate a familiar file-server experience, and "public VPC endpoint" isn't a real AWS concept.

---

## Question 184

**Full Question:** A company needs to transfer 600 terabytes of data from its on-premises network attached storage (NAS) system to the AWS Cloud. The data transfer must be complete within 2 weeks. The data is sensitive and must be encrypted in transit. The company's internet connection can support an upload speed of 100 megabits per second. Which solution meets these requirements most cost-effectively?

**Short Question:** What's the cheapest way to move 600 TB of data to AWS within 2 weeks over just a 100 Mbps internet connection?

**Options:**
- A. Use Amazon S3 multipart upload functionality to transfer the files over HTTPS
- B. Create a VPN connection between the on-premises NAS system and the nearest AWS Region. Transfer the data over the VPN connection.
- ✅ C. Use the AWS Snow Family console to order several AWS Snowball Edge Storage Optimized devices. Use the devices to transfer the data to Amazon S3.
- D. Set up a 10 Gbps AWS Direct Connect connection between the company location and the nearest AWS Region. Transfer the data over a VPN connection into the Region to store the data in Amazon S3.

**Reason:** At 100 Mbps, transferring 600 TB would take over 580 days, which rules out any network-based option such as S3 multipart upload or a VPN. AWS Snowball Edge devices physically ship encrypted data to AWS, bypassing the slow internet link and easily meeting the two-week deadline, while Direct Connect could technically handle the bandwidth but takes weeks or months to provision and is far too costly for a one-time transfer.

---

## Question 185

**Full Question:** A company deploys an application on five Amazon EC2 instances. An Application Load Balancer (ALB) distributes traffic to the instances by using a target group. The average CPU usage on each of the instances is below 10% most of the time, with occasional surges to 65%. A solutions architect needs to implement a solution to automate the scalability of the application. The solution must optimize the cost of the architecture and must ensure that the application has enough CPU resources when surges occur. Which solution will meet these requirements?

**Short Question:** What's the best automated, cost-effective way to scale EC2 instances up and down with CPU load behind an ALB?

**Options:**
- A. Create an Amazon CloudWatch alarm that enters the alarm state when the CPU utilization metric is less than 20%. Create an AWS Lambda function that the CloudWatch alarm invokes to terminate one of the EC2 instances in the ALB target group.
- ✅ B. Create an EC2 Auto Scaling group. Select the existing ALB as the load balancer and the existing target group as the target group. Set a target tracking scaling policy that is based on the ASG average CPU utilization metric. Set the minimum instances to two, the desired capacity to three, the maximum instances to six, and the target value to 50%. Add the EC2 instances to the Auto Scaling group.
- C. Create an EC2 Auto Scaling group. Select the existing ALB as the load balancer, and the existing target group as the target group. Set the minimum instances to two, the desired capacity to three, and the maximum instances to six. Add the EC2 instances to the Auto Scaling group.
- D. Create two Amazon CloudWatch alarms. Configure the first CloudWatch alarm to enter the alarm state when the average CPU utilization metric is below 20%. Configure the second CloudWatch alarm to enter the alarm state when the average CPU utilization metric is above 50%. Configure the alarms to publish to an Amazon Simple Notification Service (Amazon SNS) topic to send an email message. After receiving the message, log in to decrease or increase the number of EC2 instances that are running.

**Reason:** An EC2 Auto Scaling group with a target tracking policy on average CPU utilization automatically adds instances during surges and removes them when demand is low, minimizing cost while guaranteeing capacity. Option A only scales in and never scales out, option C creates an Auto Scaling group with no scaling policy so it can't react to load at all, and option D depends on a human manually acting on an email instead of true automation.

---

## Question 186

**Full Question:** A company is implementing a shared storage solution for a media application that is hosted in the AWS Cloud. The company needs the ability to use SMB clients to access data. The solution must be fully managed. Which AWS solution meets these requirements?

**Short Question:** Which fully managed AWS service provides SMB-based shared file storage?

**Options:**
- A. Create an AWS Storage Gateway Volume Gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- B. Create an AWS Storage Gateway Tape Gateway. Configure tapes to use Amazon S3. Connect the application server to the tape gateway.
- C. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.
- ✅ D. Create an Amazon FSx for Windows File Server file system. Attach the file system to the origin server. Connect the application server to the file system.

**Reason:** Amazon FSx for Windows File Server is a fully managed native Windows file system that natively supports SMB, unlike a Volume Gateway (block storage over iSCSI), a Tape Gateway (backup/archival VTL), or a self-managed EC2 file server (which still requires patching and maintenance).

---

## Question 187

**Full Question:** An application runs on an Amazon EC2 instance in a VPC. The application processes logs that are stored in an Amazon S3 bucket. The EC2 instance needs to access the S3 bucket without connectivity to the internet. Which solution will provide private network connectivity to Amazon S3?

**Short Question:** How can an EC2 instance in a VPC reach S3 privately without internet access?

**Options:**
- ✅ A. Create a gateway VPC endpoint to the S3 bucket.
- B. Stream the logs to Amazon CloudWatch Logs. Export the logs to the S3 bucket.
- C. Create an instance profile on Amazon EC2 to allow S3 access.
- D. Create an Amazon API Gateway API with a PrivateLink to access the S3 endpoint.

**Reason:** A gateway VPC endpoint routes traffic between the VPC and S3 privately over the AWS network, avoiding any internet or NAT gateway; an instance profile only grants permissions (not network connectivity), and CloudWatch Logs export or an API Gateway/PrivateLink setup don't solve the direct private-access requirement.

---

## Question 188

**Full Question:** A meteorological startup company has a custom web application to sell weather data to its users online. The company uses Amazon DynamoDB to store its data and wants to build a new service that sends an alert to the managers of four internal teams every time a new weather event is recorded. The company does not want this new service to affect the performance of the current application. What should a solutions architect do to meet these requirements with the least amount of operational overhead?

**Short Question:** Least-overhead way to notify four teams of new DynamoDB events without impacting the main app's performance?

**Options:**
- A. Use DynamoDB transactions to write new event data to the table. Configure the transactions to notify internal teams.
- B. Have the current application publish a message to four Amazon Simple Notification Service (Amazon SNS) topics. Have each team subscribe to one topic.
- ✅ C. Enable Amazon DynamoDB Streams on the table. Use triggers to write to a single Amazon Simple Notification Service (Amazon SNS) topic to which the teams can subscribe.
- D. Add a custom attribute to each record to flag new items. Write a cron job that scans the table every minute for items that are new and notifies an Amazon Simple Queue Service (Amazon SQS) queue to which the teams can subscribe.

**Reason:** DynamoDB Streams combined with a Lambda trigger publishing to a single SNS topic is a fully managed, serverless, event-driven pattern that has near-zero impact on table performance; the other options either lack real notification capability, add synchronous latency to the app's write path, or rely on inefficient, high-overhead polling scans.

---

## Question 189

**Full Question:** A company has migrated an application to Amazon EC2 Linux instances. One of these EC2 instances runs several 1-hour tasks on a schedule. These tasks were written by different teams and have no common programming language. The company is concerned about performance and scalability while these tasks run on a single instance. A solutions architect needs to implement a solution to resolve these concerns. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Best low-overhead way to run scalable, hour-long, multi-language scheduled batch tasks currently stuck on one EC2 instance?

**Options:**
- ✅ A. Use AWS Batch to run the tasks as jobs. Schedule the jobs by using Amazon EventBridge (Amazon CloudWatch Events).
- B. Convert the EC2 instance to a container. Use AWS App Runner to create the container on demand to run the tasks as jobs.
- C. Copy the tasks into AWS Lambda functions. Schedule the Lambda functions by using Amazon EventBridge (Amazon CloudWatch Events).
- D. Create an Amazon Machine Image (AMI) of the EC2 instance that runs the tasks. Create an Auto Scaling group with the AMI to run multiple copies of the instance.

**Reason:** AWS Batch is a fully managed service built for scheduled, containerized batch jobs of any language, and it automatically provisions and scales compute; Lambda is ruled out by its 15-minute execution limit, App Runner is meant for HTTP-facing web apps/APIs, and rebuilding queuing/scaling manually with an AMI and Auto Scaling group would mean re-inventing what Batch already provides.

---

## Question 190

**Full Question:** A gaming company uses Amazon DynamoDB to store user information such as geographic location, player data, and leaderboards. The company needs to configure continuous backups to an Amazon S3 bucket with a minimal amount of coding. The backups must not affect availability of the application and must not affect the read capacity units (RCUs) that are defined for the table. Which solution meets these requirements?

**Short Question:** Lowest-code way to continuously back up a DynamoDB table to S3 without touching its provisioned read capacity?

**Options:**
- A. Use an Amazon EMR cluster. Create an Apache Hive job to back up the data to Amazon S3.
- ✅ B. Export the data directly from DynamoDB to Amazon S3 with continuous backups. Turn on point-in-time recovery for the table.
- C. Configure Amazon DynamoDB Streams. Create an AWS Lambda function to consume the stream and export the data to an Amazon S3 bucket.
- D. Create an AWS Lambda function to export the data from the database tables to Amazon S3 on a regular basis. Turn on point-in-time recovery for the table.

**Reason:** DynamoDB's native point-in-time recovery and export-to-S3 features are managed capabilities that back up data without consuming any table read or write capacity and require no custom code, whereas EMR/Hive, a Lambda-based streams consumer, or a scheduled Lambda scan all either consume RCUs (via scans/reads) or require significant custom coding.

---

## Question 191

**Full Question:** A company needs guaranteed Amazon EC2 capacity in three specific Availability Zones in a specific AWS Region for an upcoming event that will last one week. What should the company do to guarantee the EC2 capacity?

**Short Question:** What's the best way to lock in EC2 capacity in three specific AZs for a one-week event?

**Options:**
- A. Purchase Reserved Instances that specify the Region needed
- B. Create an On-Demand Capacity Reservation that specifies the Region needed
- C. Purchase Reserved Instances that specify the Region and three Availability Zones needed
- ✅ D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed

**Reason:** On-Demand Capacity Reservations can be created per Availability Zone for any duration with no long-term commitment, so creating one in each of the three AZs guarantees capacity for the week and can simply be cancelled afterward. Reserved Instances require a 1- or 3-year commitment (wrong for a one-week event), and a regional-only Capacity Reservation isn't specific enough since capacity reservations are inherently zonal.

---

## Question 192

**Full Question:** A company has an AWS account used for software engineering. The AWS account has access to the company's on-premises data center through a pair of AWS Direct Connect connections. All non-VPC traffic routes to the virtual private gateway. A development team recently created an AWS Lambda function through the console. The development team needs to allow the function to access a database that runs in a private subnet in the company's data center. Which solution will meet these requirements?

**Short Question:** How do you let a Lambda function reach an on-premises database over an existing Direct Connect connection?

**Options:**
- ✅ A. Configure the Lambda function to run in the VPC with the appropriate security group
- B. Set up a VPN connection from AWS to the data center. Route the traffic from the Lambda function through the VPN
- C. Update the route tables in the VPC to allow the Lambda function to access the on-premises data center through Direct Connect
- D. Create an Elastic IP address. Configure the Lambda function to send traffic through the Elastic IP address without an elastic network interface

**Reason:** By default Lambda runs outside the customer's VPC, so it has no access to the VPC's routing to the on-premises network; placing the Lambda function inside the VPC gives it a private IP and lets it use the VPC's existing route tables and Direct Connect path. A new VPN is redundant since Direct Connect already exists, updating route tables doesn't help until Lambda is actually in the VPC, and Elastic IPs are for public internet traffic, not private on-premises connectivity.

---

## Question 193

**Full Question:** A solutions architect needs to securely store a database username and password that an application uses to access an Amazon RDS DB instance. The application that accesses the database runs on an Amazon EC2 instance. The solutions architect wants to create a secure parameter in AWS Systems Manager Parameter Store. What should the solutions architect do to meet this requirement?

**Short Question:** How should an EC2-hosted app securely retrieve encrypted DB credentials stored in Parameter Store?

**Options:**
- ✅ A. Create an IAM role that has read access to the Parameter Store parameter. Allow decrypt access to an AWS Key Management Service (AWS KMS) key that is used to encrypt the parameter. Assign this IAM role to the EC2 instance
- B. Create an IAM policy that allows read access to the Parameter Store parameter. Allow decrypt access to an AWS Key Management Service (AWS KMS) key that is used to encrypt the parameter. Assign this IAM policy to the EC2 instance
- C. Create an IAM trust relationship between the Parameter Store parameter and the EC2 instance. Specify Amazon RDS as a principal in the trust policy
- D. Create an IAM trust relationship between the DB instance and the EC2 instance. Specify Systems Manager as a principal in the trust policy

**Reason:** The EC2 instance needs both ssm:GetParameter and kms:Decrypt permissions, and the correct way to grant an instance permissions is via an IAM role (attached through an instance profile), not by attaching a policy directly to the instance. IAM trust relationships are for defining which principals can assume a role, not for linking a resource like a parameter or database directly to an EC2 instance.

---

## Question 194

**Full Question:** A developer has an application that uses an AWS Lambda function to upload files to Amazon S3 and needs the required permissions to perform the task. The developer already has an IAM user with valid IAM credentials required for Amazon S3. What should a solutions architect do to grant the permissions?

**Short Question:** What's the secure way to give a Lambda function permission to upload files to S3?

**Options:**
- A. Add required IAM permissions in the resource policy of the Lambda function
- B. Create a signed request using the existing IAM credentials in the Lambda function
- C. Create a new IAM user and use the existing IAM credentials in the Lambda function
- ✅ D. Create an IAM execution role with the required permissions and attach the IAM role to the Lambda function

**Reason:** Lambda functions get permissions through an IAM execution role, which supplies temporary, automatically-rotated credentials to call other AWS services like S3. Embedding a developer's long-term IAM user credentials in Lambda code (options B and C) is an insecure anti-pattern, and permissions are granted through an identity-based execution role, not a resource policy on the function itself.

---

## Question 195

**Full Question:** A company has a stateless web application that runs on AWS Lambda functions that are invoked by Amazon API Gateway. The company wants to deploy the application across multiple AWS Regions to provide regional failover capabilities. What should a solutions architect do to route traffic to multiple regions?

**Short Question:** What service should route and fail over traffic across a multi-region Lambda/API Gateway application?

**Options:**
- ✅ A. Create Amazon Route 53 health checks for each region. Use an active-active failover configuration
- B. Create an Amazon CloudFront distribution with an origin for each region. Use CloudFront health checks to route traffic
- C. Create a transit gateway. Attach the transit gateway to the API Gateway endpoint in each region. Configure the transit gateway to route requests
- D. Create an Application Load Balancer in the primary region. Set the target group to point to the API Gateway endpoint host names in each region

**Reason:** Route 53 is a global DNS service that can health-check each regional API Gateway endpoint and automatically stop routing traffic to an unhealthy region, which is exactly what's needed for regional failover. CloudFront origin failover only handles primary/secondary origins within one distribution rather than independent regional stacks, Transit Gateway is for private VPC/on-premises networking (not public API endpoints), and an ALB is a regional service that can't serve as a global failover entry point.

---

## Question 196

**Full Question:** A company is developing an e-commerce application that will consist of a load balanced front end, a container-based application, and a relational database. A solutions architect needs to create a highly available solution that operates with as little manual intervention as possible. Which solutions meet these requirements? (Choose two.)

**Short Question:** Which two AWS services give a highly available, low-maintenance database tier and container tier?

**Options:**
- ✅ A. Create an Amazon RDS DB instance in Multi-AZ mode
- B. Create an Amazon RDS DB instance and one or more replicas in another Availability Zone
- C. Create an Amazon EC2 instance-based Docker cluster to handle the dynamic application load
- ✅ D. Create an Amazon Elastic Container Service (Amazon ECS) cluster with a Fargate launch type to handle the dynamic application load
- E. Create an Amazon Elastic Container Service (Amazon ECS) cluster with an Amazon EC2 launch type to handle the dynamic application load

**Reason:** RDS Multi-AZ provides automatic, no-touch failover to a synchronously replicated standby (unlike read replicas, which need manual promotion and use async replication), and ECS with Fargate removes the need to manage any underlying servers, unlike a self-managed EC2 Docker cluster or ECS on EC2.

---

## Question 197

**Full Question:** A company recently migrated its web application to AWS by rehosting the application on Amazon EC2 instances in a single AWS Region. The company wants to redesign its application architecture to be highly available and fault tolerant. Traffic must reach all running EC2 instances randomly. Which combination of steps should the company take to meet these requirements? (Choose two.)

**Short Question:** Which infrastructure layout plus Route 53 routing policy randomly spreads traffic across EC2 instances while staying fault tolerant?

**Options:**
- A. Create an Amazon Route 53 failover routing policy
- B. Create an Amazon Route 53 weighted routing policy
- ✅ C. Create an Amazon Route 53 multivalue answer routing policy
- D. Launch three EC2 instances: two instances in one Availability Zone and one instance in another Availability Zone
- ✅ E. Launch four EC2 instances: two instances in one Availability Zone and two instances in another Availability Zone

**Reason:** A symmetrical 2-2 instance split across two Availability Zones ensures losing one AZ only costs 50% of capacity, and Route 53 multivalue answer routing returns multiple healthy IP addresses so clients randomly pick one, unlike failover (active/passive) or weighted (proportion-based) routing.

---

## Question 198

**Full Question:** A company wants to use artificial intelligence (AI) to determine the quality of its customer service calls. The company currently manages calls in four different languages, including English. The company will offer new languages in the future. The company does not have the resources to regularly maintain machine learning (ML) models. The company needs to create written sentiment analysis reports from the customer service call recordings. The customer service call recording text must be translated into English. Which combination of steps will meet these requirements? (Choose three.)

**Short Question:** Which three managed AWS AI services turn multilingual call audio into English sentiment reports without managing ML models?

**Options:**
- A. Use Amazon Comprehend to translate the audio recordings into English
- B. Use Amazon Lex to create the written sentiment analysis reports
- C. Use Amazon Polly to convert the audio recordings into text
- ✅ D. Use Amazon Transcribe to convert the audio recordings in any language into text
- ✅ E. Use Amazon Translate to translate text in any language to English
- ✅ F. Use Amazon Comprehend to create the sentiment analysis reports

**Reason:** The correct pipeline is Amazon Transcribe (speech-to-text) → Amazon Translate (translate to English) → Amazon Comprehend (sentiment analysis), all fully managed with no ML model maintenance; Comprehend doesn't translate, Lex builds chatbots rather than analyzing sentiment, and Polly converts text to speech, the opposite of what's needed.

---

## Question 199

**Full Question:** A company's web application consists of an Amazon API Gateway API in front of an AWS Lambda function and an Amazon DynamoDB database. The Lambda function handles the business logic and the DynamoDB table hosts the data. The application uses Amazon Cognito user pools to identify the individual users of the application. A solutions architect needs to update the application so that only users who have a subscription can access premium content. Which solution will meet this requirement with the least operational overhead?

**Short Question:** Simplest, lowest-overhead way to restrict premium API content to subscribed users only?

**Options:**
- A. Enable API caching and throttling on the API Gateway API
- B. Set up AWS WAF on the API Gateway API. Create a rule to filter users who have a subscription
- C. Apply fine-grained IAM permissions to the premium content in the DynamoDB table
- ✅ D. Implement API usage plans and API keys to limit the access of users who do not have a subscription

**Reason:** API Gateway usage plans and API keys are purpose-built to gate specific API methods behind different subscription tiers via configuration alone, whereas caching/throttling and WAF aren't designed for subscription-based authorization, and enforcing access at the DynamoDB layer with per-user IAM permissions is complex, doesn't scale, and checks access at the wrong layer.

---

## Question 200

**Full Question:** A company has a web application that is based on Java and PHP. The company plans to move the application from on premises to AWS. The company needs the ability to test new site features frequently. The company also needs a highly available and managed solution that requires minimum operational overhead. Which solution will meet these requirements?

**Short Question:** Best managed, low-overhead way to host a Java/PHP app on AWS while supporting frequent feature testing?

**Options:**
- A. Create an Amazon S3 bucket. Enable static web hosting on the S3 bucket. Upload the static content to the S3 bucket. Use AWS Lambda to process all dynamic content
- ✅ B. Deploy the web application to an AWS Elastic Beanstalk environment. Use URL swapping to switch between multiple Elastic Beanstalk environments for feature testing
- C. Deploy the web application to Amazon EC2 instances that are configured with Java and PHP. Use Auto Scaling groups and an Application Load Balancer to manage the website's availability
- D. Containerize the web application. Deploy the web application to Amazon EC2 instances. Use the AWS Load Balancer Controller to dynamically route traffic between containers that contain the new site features for testing

**Reason:** AWS Elastic Beanstalk is a managed PaaS that automatically provisions EC2, Auto Scaling, and load balancing for high availability with minimal effort, and its built-in blue/green deployment with URL swapping makes feature testing easy, while the S3/Lambda option requires a full app rewrite and the EC2-based options put far more configuration and scaling work on the company.
