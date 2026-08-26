# AWS SAA-C03 Real Exam Questions & Answers — Part 14 (Q326–Q350)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 14](https://www.youtube.com/watch/UlabsVqXi34)

---

## Question 326

**Full Question:** A company needs to connect several VPCs in the US East 1 region that span hundreds of AWS accounts. The company's networking team has its own AWS account to manage the cloud network. What is the most operationally efficient solution to connect the VPCs?

**Short Question:** What's the most efficient way to centrally connect hundreds of VPCs across many AWS accounts in one region?

**Options:**
- A. Set up VPC peering connections between each VPC. Update each associated subnet's route table.
- B. Configure a NAT gateway and an internet gateway in each VPC to connect each VPC through the internet.
- ✅ C. Create an AWS Transit Gateway in the networking team's AWS account. Configure static routes from each VPC.
- D. Deploy VPN gateways in each VPC. Create a transit VPC in the networking team's AWS account to connect to each VPC.

**Reason:** AWS Transit Gateway acts as a central hub-and-spoke router, letting hundreds of VPCs connect through one managed gateway instead of a full mesh; VPC peering doesn't scale (grows quadratically), routing over the internet is insecure, and the self-managed transit VPC/VPN pattern is high-overhead and outdated.

---

## Question 327

**Full Question:** A company stores confidential data in an Amazon Aurora PostgreSQL database in the AP Southeast 3 region. The database is encrypted with an AWS Key Management Service (AWS KMS) customer managed key. The company was recently acquired and must securely share a backup of the database with the acquiring company's AWS account in AP Southeast 3. What should a solutions architect do to meet these requirements?

**Short Question:** How do you securely share an encrypted Aurora snapshot with another AWS account?

**Options:**
- A. Create a database snapshot. Copy the snapshot to a new unencrypted snapshot. Share the new snapshot with the acquiring company's AWS account.
- ✅ B. Create a database snapshot. Add the acquiring company's AWS account to the KMS key policy. Share the snapshot with the acquiring company's AWS account.
- C. Create a database snapshot that uses a different AWS managed KMS key. Add the acquiring company's AWS account to the KMS key alias. Share the snapshot with the acquiring company's AWS account.
- D. Create a database snapshot. Download the database snapshot. Upload the database snapshot to an Amazon S3 bucket. Update the S3 bucket policy to allow access from the acquiring company's AWS account.

**Reason:** Since the snapshot inherits the original customer-managed KMS key, the receiving account needs permission granted via the KMS key policy (not an alias) to use that key, plus the shared snapshot itself; decrypting the data is not an option since it's confidential, and Aurora snapshots can't be downloaded directly.

---

## Question 328

**Full Question:** A company needs to retain its AWS CloudTrail logs for three years. The company is enforcing CloudTrail across a set of AWS accounts by using AWS Organizations from the parent account. The CloudTrail target S3 bucket is configured with S3 versioning enabled. An S3 lifecycle policy is in place to delete current objects after 3 years. After the fourth year of use of the S3 bucket, the S3 bucket metrics show that the number of objects has continued to rise. However, the number of new CloudTrail logs that are delivered to the S3 bucket has remained consistent. Which solution will delete objects that are older than 3 years in the most cost-effective manner?

**Short Question:** How do you actually get rid of old CloudTrail log objects in a versioned S3 bucket when the object count keeps growing despite a lifecycle rule?

**Options:**
- A. Configure the organization's centralized CloudTrail to expire objects after 3 years.
- ✅ B. Configure the S3 lifecycle policy to delete previous versions as well as current versions.
- C. Create an AWS Lambda function to enumerate and delete objects from Amazon S3 that are older than 3 years.
- D. Configure the parent account as the owner of all objects that are delivered to the S3 bucket.

**Reason:** In a versioned bucket, deleting the current object just adds a delete marker and keeps the old content as a "previous version," so the lifecycle rule must also target noncurrent versions to actually free up space; CloudTrail has no built-in expiration feature, a custom Lambda solution is unnecessary overhead, and object ownership settings don't affect deletion/lifecycle behavior.

---

## Question 329

**Full Question:** A company provides an online service for posting video content and transcoding it for use by any mobile platform. The application architecture uses Amazon Elastic File System (Amazon EFS) Standard to collect and store the videos so that multiple Amazon EC2 Linux instances can access the video content for processing. As the popularity of the service has grown over time, the storage costs have become too expensive. Which storage solution is most cost effective?

**Short Question:** What's the cheapest way to store and process shared video files across multiple EC2 instances instead of using pricey EFS?

**Options:**
- A. Use AWS Storage Gateway for files to store and process the video content.
- B. Use AWS Storage Gateway for volumes to store and process the video content.
- C. Use Amazon EFS for storing the video content. Once processing is complete, transfer the files to Amazon Elastic Block Store (Amazon EBS).
- ✅ D. Use Amazon S3 for storing the video content. Move the files temporarily over to an Amazon Elastic Block Store (Amazon EBS) volume attached to the server for processing.

**Reason:** Amazon S3 is much cheaper than EFS for storing large volumes of video files, and EC2 instances can download individual files to a local EBS volume for fast processing then upload results back; Storage Gateway options are meant for hybrid on-premises connectivity (not native cloud storage), and option C still relies on expensive EFS for the initial storage.

---

## Question 330

**Full Question:** A company is developing a mobile gaming app in a single AWS region. The app runs on multiple Amazon EC2 instances in an Auto Scaling group. The company stores the app data in Amazon DynamoDB. The app communicates by using TCP traffic and UDP traffic between the users and the servers. The application will be used globally. The company wants to ensure the lowest possible latency for all users. Which solution will meet these requirements?

**Short Question:** What's the best way to minimize latency for a globally-used gaming app that needs both TCP and UDP support?

**Options:**
- A. Use AWS Global Accelerator to create an accelerator. Create an Application Load Balancer (ALB) behind an accelerator endpoint that uses Global Accelerator integration and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the ALB.
- ✅ B. Use AWS Global Accelerator to create an accelerator. Create a Network Load Balancer (NLB) behind an accelerator endpoint that uses Global Accelerator integration and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the NLB.
- C. Create an Amazon CloudFront content delivery network (CDN) endpoint. Create a Network Load Balancer (NLB) behind the endpoint and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the NLB. Update CloudFront to use the NLB as the origin.
- D. Create an Amazon CloudFront content delivery network (CDN) endpoint. Create an Application Load Balancer (ALB) behind the endpoint and listening on the TCP and UDP ports. Update the Auto Scaling group to register instances on the ALB. Update CloudFront to use the ALB as the origin.

**Reason:** AWS Global Accelerator routes traffic over the AWS private backbone to minimize latency and supports both TCP and UDP, and a Network Load Balancer (Layer 4) is needed because it can handle UDP traffic, unlike an Application Load Balancer (Layer 7, HTTP/HTTPS only); CloudFront is also unsuitable since it's built for HTTP/S content caching, not generic TCP/UDP traffic.

---

## Question 331

**Full Question:** A company needs to store its accounting records in Amazon S3. The records must be immediately accessible for one year and then must be archived for an additional 9 years. No one at the company, including administrative users and root users, must be able to delete the records during the entire 10-year period. The records must be stored with maximum resiliency. Which solution will meet these requirements?

**Short Question:** Which S3 setup gives 1 year of immediate access, 9 years of archival, and unbreakable (even by root) 10-year deletion protection with max resiliency?

**Options:**
- A. Store the records in S3 Glacier for the entire 10-year period. Use an access control policy to deny deletion of the records for a period of 10 years.
- B. Store the records by using S3 Intelligent-Tiering. Use an IAM policy to deny deletion of the records after 10 years. Change the IAM policy to allow deletion.
- ✅ C. Use an S3 lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after one year. Use S3 Object Lock in compliance mode for a period of 10 years.
- D. Use an S3 lifecycle policy to transition the records from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after one year. Use S3 Object Lock in governance mode for a period of 10 years.

**Reason:** S3 Object Lock in compliance mode is the only option that truly prevents deletion by any user, including the root user, for the retention period, and the lifecycle transition to S3 Glacier Deep Archive after one year satisfies both the access timeline and low-cost archival; option A starts in Glacier immediately (breaking the immediate-access requirement) and relies on a changeable access policy, B uses a revocable IAM policy instead of Object Lock, and D uses One Zone storage (not maximally resilient) plus governance mode (which authorized users can bypass).

---

## Question 332

**Full Question:** A company uses Amazon EC2 instances and AWS Lambda functions to run its application. The company has VPCs with public subnets and private subnets in its AWS account. The EC2 instances run in a private subnet in one of the VPCs. The Lambda functions need direct network access to the EC2 instances for the application to work. The application will run for at least one year. The company expects the number of Lambda functions that the application uses to increase during that time. The company wants to maximize its savings on all application resources and to keep network latency between the services low. Which solution will meet these requirements?

**Short Question:** How do you get maximum cost savings across both EC2 and growing Lambda usage while keeping low-latency private network access between Lambda and EC2?

**Options:**
- A. Purchase an EC2 instance savings plan. Optimize the Lambda functions' duration and memory usage and the number of invocations. Connect the Lambda functions to the private subnet that contains the EC2 instances.
- B. Purchase an EC2 instance savings plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Connect the Lambda functions to a public subnet in the same VPC where the EC2 instances run.
- ✅ C. Purchase a compute savings plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Connect the Lambda functions to the private subnet that contains the EC2 instances.
- D. Purchase a compute savings plan. Optimize the Lambda functions' duration and memory usage, the number of invocations, and the amount of data that is transferred. Keep the Lambda functions in the Lambda service VPC.

**Reason:** A Compute Savings Plan (unlike an EC2 instance savings plan) covers both EC2 and Lambda usage, satisfying the "savings on all resources" requirement, and connecting Lambda directly into the private subnet where the EC2 instances live gives secure, low-latency access; options A and B wrongly use an EC2-only savings plan (and B also puts Lambda in a less secure public subnet), while D keeps Lambda in the default Lambda service VPC, which cannot reach the EC2 instances in the private subnet at all.

---

## Question 333

**Full Question:** A telemarketing company is designing its customer call center functionality on AWS. The company needs a solution that provides multiple speaker recognition and generates transcript files. The company wants to query the transcript files to analyze the business patterns. The transcript files must be stored for 7 years for auditing purposes. Which solution will meet these requirements?

**Short Question:** Which AWS service combo transcribes calls with speaker identification, stores the transcripts, and lets you SQL-query them for analysis?

**Options:**
- A. Use Amazon Rekognition for multiple speaker recognition. Store the transcript files in Amazon S3. Use machine learning models for transcript file analysis.
- ✅ B. Use Amazon Transcribe for multiple speaker recognition. Use Amazon Athena for transcript file analysis.
- C. Use Amazon Translate for multiple speaker recognition. Store the transcript files in Amazon Redshift. Use SQL queries for transcript file analysis.
- D. Use Amazon Rekognition for multiple speaker recognition. Store the transcript files in Amazon S3. Use Amazon Textract for transcript file analysis.

**Reason:** Amazon Transcribe is the speech-to-text service with a speaker diarization feature that identifies multiple speakers, and it writes transcript files to S3 where Amazon Athena can run SQL queries directly against them; Rekognition (image/video) and Translate (language translation) don't process audio for transcription, and Textract is for extracting text from documents (OCR), not analyzing existing text.

---

## Question 334

**Full Question:** An application that is hosted on Amazon EC2 instances needs to access an Amazon S3 bucket. Traffic must not traverse the internet. How should a solutions architect configure access to meet these requirements?

**Short Question:** How do you let EC2 instances reach an S3 bucket without any traffic going over the public internet?

**Options:**
- A. Create a private hosted zone by using Amazon Route 53.
- ✅ B. Set up a gateway VPC endpoint for Amazon S3 in the VPC.
- C. Configure the EC2 instances to use a NAT gateway to access the S3 bucket.
- D. Establish an AWS Site-to-Site VPN connection between the VPC and the S3 bucket.

**Reason:** A gateway VPC endpoint for S3 is built specifically to give a VPC a private route to S3 without touching the public internet; a Route 53 private hosted zone only manages DNS, a NAT gateway routes traffic out to the internet (the opposite of what's needed), and Site-to-Site VPN connects a VPC to an on-premises network, not to an S3 bucket.

---

## Question 335

**Full Question:** A company wants to add its existing AWS usage cost to its operation cost dashboard. A solutions architect needs to recommend a solution that will give the company access to its usage cost programmatically. The company must be able to access cost data for the current year and forecast costs for the next 12 months. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead way to programmatically pull current-year AWS cost data plus a 12-month cost forecast into a dashboard?

**Options:**
- ✅ A. Access usage cost-related data by using the AWS Cost Explorer API with pagination.
- B. Access usage cost-related data by using downloadable AWS Cost Explorer report .csv files.
- C. Configure AWS Budgets actions to send usage cost data to the company through FTP.
- D. Create AWS Budgets reports for usage cost data. Send the data to the company through SMTP.

**Reason:** The AWS Cost Explorer API is built for programmatic access and offers direct operations for both historical cost/usage data and cost forecasts, making it the most automated, low-overhead choice; CSV downloads require manual handling and custom parsing, and AWS Budgets is designed for threshold alerts and human-readable email/report delivery (FTP/SMTP), not programmatic data feeds.

---

## Question 336

**Full Question:** A company uses a payment processing system that requires messages for a particular payment ID to be received in the same order that they were sent. Otherwise, the payments might be processed incorrectly. Which actions should a solutions architect take to meet this requirement? (Choose two.)

**Short Question:** Which two AWS services/configurations guarantee that messages for the same payment ID are processed in the order they were sent?

**Options:**
- A. Write the messages to an Amazon DynamoDB table with the payment ID as the partition key
- ✅ B. Write the messages to an Amazon Kinesis data stream with the payment ID as the partition key
- C. Write the messages to an Amazon ElastiCache for Memcached cluster with the payment ID as the key
- D. Write the messages to an Amazon SQS standard queue. Set the message attribute to use the payment ID
- ✅ E. Write the messages to an Amazon SQS FIFO queue. Set the message group ID to use the payment ID

**Reason:** Kinesis Data Streams preserves record order within a shard when using a consistent partition key, and SQS FIFO queues guarantee strict ordering within a message group when the message group ID is set consistently; DynamoDB and ElastiCache aren't message queues with ordering guarantees, and SQS standard queues only offer best-effort ordering.

---

## Question 337

**Full Question:** A company hosts its application on AWS. The company uses Amazon Cognito to manage users. When users log in to the application, the application fetches required data from Amazon DynamoDB by using a REST API that is hosted in Amazon API Gateway. The company wants an AWS managed solution that will control access to the REST API to reduce development efforts. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-effort, fully managed way to secure an API Gateway REST API using an existing Cognito user pool?

**Options:**
- A. Configure an AWS Lambda function to be an authorizer in API Gateway to validate which user made the request
- B. For each user, create and assign an API key that must be sent with each request. Validate the key by using an AWS Lambda function
- C. Send the user's email address in the header with every request. Invoke an AWS Lambda function to validate that the user with that email address has proper access
- ✅ D. Configure an Amazon Cognito user pool authorizer in API Gateway to allow Amazon Cognito to validate each request

**Reason:** API Gateway has a built-in Cognito user pool authorizer that validates the JWT token issued at login with zero custom code, whereas a Lambda authorizer requires writing and maintaining validation logic, API keys are meant for client identification/throttling rather than user auth, and trusting an email header is easily spoofed and insecure.

---

## Question 338

**Full Question:** A solutions architect must create a disaster recovery (DR) plan for a high volume software as a service (SaaS) platform. All data for the platform is stored in an Amazon Aurora MySQL DB cluster. The DR plan must replicate data to a secondary AWS Region. Which solution will meet these requirements most cost effectively?

**Short Question:** What's the cheapest way to replicate an Aurora MySQL database to a second region for disaster recovery?

**Options:**
- A. Use MySQL binary log replication to an Aurora cluster in the secondary region. Provision 1 DB instance for the Aurora cluster in the secondary region
- ✅ B. Set up an Aurora global database for the DB cluster. When setup is complete, remove the DB instance from the secondary region
- C. Use AWS Database Migration Service (AWS DMS) to continuously replicate data to an Aurora cluster in the secondary region. Remove the DB instance from the secondary region
- D. Set up an Aurora global database for the DB cluster. Specify a minimum of 1 DB instance in the secondary region

**Reason:** Aurora global database lets the secondary region hold a storage-only replica (no running compute instance), so you pay only for storage and replication traffic and can spin up an instance quickly during an actual failover; binary log replication is less performant/native, DMS requires a running target instance so it can't function with the instance removed, and keeping a standing instance in option D adds unnecessary compute cost.

---

## Question 339

**Full Question:** A company wants to allow its users to upload images in an application that is hosted in the AWS Cloud. The company needs a solution that automatically resizes the images so that the images can be displayed on multiple device types. The application experiences unpredictable traffic patterns throughout the day. The company is seeking a highly available solution that maximizes scalability. What should a solutions architect do to meet these requirements?

**Short Question:** What's the most scalable, highly available serverless way to automatically resize user-uploaded images for an app with unpredictable traffic?

**Options:**
- ✅ A. Create a static website hosted in Amazon S3 that invokes AWS Lambda functions to resize the images and store the images in an Amazon S3 bucket
- B. Create a static website hosted in Amazon CloudFront that invokes AWS Step Functions to resize the images and store the images in an Amazon RDS database
- C. Create a dynamic website hosted on a web server that runs on an Amazon EC2 instance. Configure a process that runs on the EC2 instance to resize the images and store the images in an Amazon S3 bucket
- D. Create a dynamic website hosted on an automatically scaling Amazon ECS cluster that creates a resize job in an Amazon SQS queue. Set up an image resizing program that runs on an Amazon EC2 instance to process the resize jobs

**Reason:** S3 event notifications triggering Lambda is a fully serverless, inherently highly available and massively scalable pattern that needs no server management; option B misuses RDS for image storage and CloudFront can't directly invoke Step Functions, option C relies on a single EC2 instance as a single point of failure, and option D still bottlenecks the actual resizing on one non-scalable EC2 instance.

---

## Question 340

**Full Question:** A company has a web application for travel ticketing. The application is based on a database that runs in a single data center in North America. The company wants to expand the application to serve a global user base. The company needs to deploy the application to multiple AWS Regions. Average latency must be less than 1 second on updates to the reservation database. The company wants to have separate deployments of its web platform across multiple regions. However, the company must maintain a single primary reservation database that is globally consistent. Which solution should a solutions architect recommend to meet these requirements?

**Short Question:** Which database solution lets a globally deployed app write reservations in any region with sub-1-second global consistency?

**Options:**
- ✅ A. Convert the application to use Amazon DynamoDB. Use a global table for the reservation table. Use the correct regional endpoint in each regional deployment
- B. Migrate the database to an Amazon Aurora MySQL database. Deploy Aurora read replicas in each region. Use the correct regional endpoint in each regional deployment for access to the database
- C. Migrate the database to an Amazon RDS for MySQL database. Deploy MySQL read replicas in each region. Use the correct regional endpoint in each regional deployment for access to the database
- D. Migrate the application to an Amazon Aurora Serverless database. Deploy instances of the database to each region. Use the correct regional endpoint in each regional deployment to access the database. Use AWS Lambda functions to process event streams in each region to synchronize the databases

**Reason:** DynamoDB global tables provide a fully managed, multi-active, multi-region database where each region writes locally and changes replicate globally in under a second; Aurora and RDS read replica setups are single-master architectures where all writes must go to one primary region (causing high latency for distant users), and a custom Lambda-based sync system is complex, error-prone, and unnecessary when global tables already solve this natively.

---

## Question 341

**Full Question:** A solutions architect is designing a REST API in Amazon API Gateway for a cashback service. The application requires 1 GB of memory and 2 GB of storage for its computation resources. The application will require that the data is in a relational format. Which combination of additional AWS services will meet these requirements with the least administrative effort? (Choose two.)

**Short Question:** Which two AWS services (one compute, one database) back an API Gateway app needing modest compute and relational data, with the least admin effort?

**Options:**
- A. Amazon EC2
- ✅ B. AWS Lambda
- ✅ C. Amazon RDS
- D. Amazon DynamoDB
- E. Amazon Elastic Kubernetes Service (Amazon EKS)

**Reason:** AWS Lambda is serverless and easily handles the modest 1 GB memory / 2 GB storage needs with minimal administrative overhead while integrating natively with API Gateway, and Amazon RDS is a fully managed relational database that satisfies the relational data requirement; EC2 and EKS both require manual infrastructure management, and DynamoDB is NoSQL, not relational.

---

## Question 342

**Full Question:** A company has an on-premises MySQL database used by the global sales team with infrequent access patterns. The sales team requires the database to have minimal downtime. A database administrator wants to migrate this database to AWS without selecting a particular instance type in anticipation of more users in the future. Which service should a solutions architect recommend?

**Short Question:** Which AWS MySQL-compatible service auto-scales compute without picking a fixed instance type, for an infrequently-used, highly-available database?

**Options:**
- A. Amazon Aurora MySQL
- ✅ B. Amazon Aurora Serverless for MySQL
- C. Amazon Redshift Spectrum
- D. Amazon RDS for MySQL

**Reason:** Aurora Serverless automatically starts, stops, and scales compute capacity within a defined min/max range instead of requiring a fixed instance type, which fits an unpredictable, infrequent workload; standard Aurora and RDS for MySQL both require provisioning a specific instance size, and Redshift Spectrum is an analytics data warehouse, not a MySQL-compatible transactional database.

---

## Question 343

**Full Question:** A gaming company is moving its public scoreboard from a data center to the AWS Cloud. The company uses Amazon EC2 Windows Server instances behind an Application Load Balancer to host its dynamic application. The company needs a highly available storage solution for the application. The application consists of static files and dynamic server-side code. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two storage setups give highly-available static file delivery and shared server-side code storage for Windows EC2 instances?

**Options:**
- ✅ A. Store the static files on Amazon S3. Use Amazon CloudFront to cache objects at the edge.
- B. Store the static files on Amazon S3. Use Amazon ElastiCache to cache objects at the edge.
- C. Store the server-side code on Amazon Elastic File System (Amazon EFS). Mount the EFS volume on each EC2 instance to share the files.
- ✅ D. Store the server-side code on Amazon FSx for Windows File Server. Mount the FSx for Windows File Server volume on each EC2 instance to share the files.
- E. Store the server-side code on a general purpose SSD (gp2) Amazon Elastic Block Store (Amazon EBS) volume. Mount the EBS volume on each EC2 instance to share the files.

**Reason:** S3 with CloudFront is the durable, low-latency standard for serving static content globally, and FSx for Windows File Server provides a shared file system over the native Windows SMB protocol that multiple EC2 instances can mount together; EFS uses Linux-native NFS (not suited to Windows), a standard EBS volume can only attach to one instance at a time, and ElastiCache is an in-memory cache, not a CDN.

---

## Question 344

**Full Question:** A social media company runs its application on Amazon EC2 instances behind an Application Load Balancer (ALB). The ALB is the origin for an Amazon CloudFront distribution. The application has more than a billion images stored in an Amazon S3 bucket and processes thousands of images each second. The company wants to resize the images dynamically and serve appropriate formats to clients. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Best low-overhead way to dynamically resize and serve appropriate image formats at massive scale through CloudFront?

**Options:**
- A. Install an external image management library on an EC2 instance. Use the image management library to process the images.
- B. Create a CloudFront origin request policy. Use the policy to automatically resize images and to serve the appropriate format based on the User-Agent HTTP header in the request.
- ✅ C. Use a Lambda@Edge function with an external image management library. Associate the Lambda@Edge function with the CloudFront behaviors that serve the images.
- D. Create a CloudFront response headers policy. Use the policy to automatically resize images and to serve the appropriate format based on the User-Agent HTTP header in the request.

**Reason:** Lambda@Edge runs code at CloudFront edge locations to fetch, resize, and return images on the fly, making it the standard fully managed, serverless pattern for this use case; origin request and response headers policies only control which headers/cookies/query strings are forwarded or added and cannot execute processing logic, and doing this on EC2 requires managing a fleet of servers.

---

## Question 345

**Full Question:** An e-commerce company stores terabytes of customer data in the AWS Cloud. The data contains personally identifiable information (PII). The company wants to use the data in three applications. Only one of the applications needs to process the PII. The PII must be removed before the other two applications process the data. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Best low-overhead way to serve one dataset to three apps where only one app is allowed to see the PII?

**Options:**
- A. Store the data in an Amazon DynamoDB table. Create a proxy application layer to intercept and process the data that each application requests.
- ✅ B. Store the data in an Amazon S3 bucket. Process and transform the data by using S3 Object Lambda before returning the data to the requesting application.
- C. Process the data and store the transformed data in three separate Amazon S3 buckets so that each application has its own custom data set. Point each application to its respective S3 bucket.
- D. Process the data and store the transformed data in three separate Amazon DynamoDB tables so that each application has its own custom data set. Point each application to its respective DynamoDB table.

**Reason:** S3 Object Lambda transforms data on the fly (such as redacting PII) as it's returned from a single source copy in S3, avoiding duplicate data stores and custom infrastructure; a custom proxy layer requires significant build and maintenance effort, and pre-processing into three separate S3 buckets or DynamoDB tables duplicates data and adds ETL/sync overhead.

---

## Question 346

**Full Question:** A company wants to create an application to store employee data in a hierarchical structured relationship. The company needs a minimum latency response to high-traffic queries for the employee data and must protect any sensitive data. The company also needs to receive monthly email messages if any financial information is present in the employee data. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two steps give low-latency hierarchical employee data storage plus automated monthly email alerts on any sensitive financial data found in it?

**Options:**
- A. Use Amazon Redshift to store the employee data in hierarchies. Unload the data to Amazon S3 every month.
- ✅ B. Use Amazon DynamoDB to store the employee data in hierarchies. Export the data to Amazon S3 every month.
- C. Configure Amazon Macie for the AWS account. Integrate Macie with Amazon EventBridge to send monthly events to AWS Lambda.
- D. Use Amazon Athena to analyze the employee data in Amazon S3. Integrate Athena with Amazon QuickSight to publish analysis dashboards and share the dashboards with users.
- ✅ E. Configure Amazon Macie for the AWS account. Integrate Macie with Amazon EventBridge to send monthly notifications through an Amazon Simple Notification Service (Amazon SNS) subscription.

**Reason:** DynamoDB delivers the single-digit-millisecond latency needed for high-traffic hierarchical queries, and a monthly export to S3 provides a safe copy for scanning without hitting the live database; Macie (built to discover sensitive data like financial info) feeding EventBridge directly into an SNS subscription is the simplest, lowest-overhead way to trigger the required monthly email alerts — routing through Lambda (option C) or using Redshift/Athena/QuickSight (options A and D) adds unnecessary steps or the wrong tool for sensitive-data discovery.

---

## Question 347

**Full Question:** A company wants to move from many standalone AWS accounts to a consolidated multi-account architecture. The company plans to create many new AWS accounts for different business units. The company needs to authenticate access to these AWS accounts by using a centralized corporate directory service. Which combination of actions should a solutions architect recommend to meet these requirements? (Choose two.)

**Short Question:** Which two actions set up centralized multi-account management with SSO login from the company's own corporate directory?

**Options:**
- ✅ A. Create a new organization in AWS Organizations with all features turned on. Create the new AWS accounts in the organization.
- B. Set up an Amazon Cognito identity pool. Configure AWS IAM Identity Center (AWS Single Sign-On) to accept Amazon Cognito authentication.
- C. Configure a service control policy (SCP) to manage the AWS accounts. Add AWS IAM Identity Center (AWS Single Sign-On) to AWS Directory Service.
- D. Create a new organization in AWS Organizations. Configure the organization's authentication mechanism to use AWS Directory Service directly.
- ✅ E. Set up AWS IAM Identity Center (AWS Single Sign-On) in the organization. Configure IAM Identity Center and integrate it with the company's corporate directory service.

**Reason:** AWS Organizations is the foundational service for centrally creating and governing many AWS accounts, and AWS IAM Identity Center is the SSO service that layers on top of an organization and can connect to an external corporate directory as its identity source — Cognito is meant for customer-facing app identities (not corporate SSO), and Organizations itself has no built-in authentication mechanism.

---

## Question 348

**Full Question:** A company has an application that is running on Amazon EC2 instances. A solutions architect has standardized the company on a particular instance family and various instance sizes based on the current needs of the company. The company wants to maximize cost savings for the application over the next 3 years. The company needs to be able to change the instance family and sizes in the next 6 months based on application popularity and usage. Which solution will meet these requirements most cost effectively?

**Short Question:** Which 3-year EC2 cost-saving option still lets you freely change instance family and size later?

**Options:**
- ✅ A. Compute Savings Plan
- B. EC2 Instance Savings Plan
- C. Zonal Reserved Instances
- D. Standard Reserved Instances

**Reason:** A Compute Savings Plan applies its discount automatically to any EC2 usage regardless of instance family, size, region, or OS, so the company keeps saving even after switching instance types; the other three options are all locked to a specific instance family (and, for Zonal RIs, a specific Availability Zone), so a change in instance family would forfeit the discount.

---

## Question 349

**Full Question:** A media company collects and analyzes user activity data on premises. The company wants to migrate this capability to AWS. The user activity data store will continue to grow and will be petabytes in size. The company needs to build a highly available data ingestion solution that facilitates on-demand analytics of existing data and new data with SQL. Which solution will meet these requirements with the least operational overhead?

**Short Question:** What's the lowest-overhead, highly available way to ingest ever-growing petabyte-scale activity data and query it on demand with SQL?

**Options:**
- A. Send activity data to an Amazon Kinesis Data Stream. Configure the stream to deliver the data to an Amazon S3 bucket.
- ✅ B. Send activity data to an Amazon Kinesis Data Firehose delivery stream. Configure the stream to deliver the data to an Amazon Redshift cluster.
- C. Place activity data in an Amazon S3 bucket. Configure Amazon S3 to run an AWS Lambda function on the data as the data arrives in the S3 bucket.
- D. Create an ingestion service on Amazon EC2 instances that are spread across multiple Availability Zones. Configure the service to forward data to an Amazon RDS Multi-AZ database.

**Reason:** Kinesis Data Firehose is a fully managed, highly available ingestion pipeline that can load data straight into Amazon Redshift, a petabyte-scale data warehouse purpose-built for high-performance SQL analytics — meeting every requirement with minimal management; option A lacks a SQL query engine, option C uses Lambda (not a SQL analytics tool) for petabyte-scale analysis, and option D relies on a self-managed EC2 service and RDS, which is a transactional (OLTP) database not suited to petabyte-scale analytics.

---

## Question 350

**Full Question:** A company uses AWS Organizations to run workloads within multiple AWS accounts. A tagging policy adds department tags to AWS resources when the company creates tags. An accounting team needs to determine spending on Amazon EC2 consumption. The accounting team must determine which departments are responsible for the costs regardless of AWS account. The accounting team has access to AWS Cost Explorer for all AWS accounts within the organization and needs to access all reports from Cost Explorer. Which solution meets these requirements in the most operationally efficient way?

**Short Question:** What's the correct, most efficient way to activate and use a custom "department" cost allocation tag to see EC2 spend by department across a whole AWS Organization?

**Options:**
- ✅ A. From the organization's management account billing console, activate a user-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name and filter by EC2.
- B. From the organization's management account billing console, activate an AWS-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name and filter by EC2.
- C. From the organization's member account billing console, activate a user-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name and filter by EC2.
- D. From the organization's member account billing console, activate an AWS-defined cost allocation tag named department. Create one cost report in Cost Explorer grouping by tag name and filter by EC2.

**Reason:** Cost allocation tags for an entire organization can only be activated centrally from the management (payer) account, and since "department" is a custom tag the company created (not one AWS generates automatically), it must be activated as a user-defined tag rather than an AWS-defined one.
