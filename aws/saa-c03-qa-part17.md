# AWS SAA-C03 Real Exam Questions & Answers — Part 17 (Q401–Q420)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 17](https://www.youtube.com/watch/lIEMW52acNI)

---

## Question 401

**Full Question:** A company has migrated multiple Microsoft Windows Server workloads to Amazon EC2 instances that run in the US West 1 Region. The company manually backs up the workloads to create an image as needed. In the event of a natural disaster in the US West 1 Region, the company wants to recover workloads quickly in the US West 2 Region. The company wants no more than 24 hours of data loss on the EC2 instances. The company also wants to automate any backups of the EC2 instances. Which solutions will meet these requirements with the least administrative effort? (Choose two.)

**Short Question:** Which two low-effort, automated options back up EC2 instances at least every 24 hours and copy the backups to a second region for disaster recovery?

**Options:**
- A. Create an Amazon EC2-backed Amazon Machine Image (AMI) lifecycle policy to create a backup based on tags. Schedule the backup to run twice daily. Copy the image on demand.
- ✅ B. Create an Amazon EC2-backed Amazon Machine Image (AMI) lifecycle policy to create a backup based on tags. Schedule the backup to run twice daily. Configure the copy to the US West 2 Region.
- C. Create a backup vault in US West 1 and in US West 2 by using AWS Backup. Create a backup plan for the EC2 instances based on tag values. Create an AWS Lambda function to run as a scheduled job to copy the backup data to US West 2.
- ✅ D. Create a backup vault by using AWS Backup. Use AWS Backup to create a backup plan for the EC2 instances based on tag values. Define the destination for the copy as US West 2. Specify the backup schedule to run twice daily.
- E. Create a backup vault by using AWS Backup. Use AWS Backup to create a backup plan for the EC2 instances based on tag values. Specify the backup schedule to run twice daily. Copy on demand to US West 2.

**Reason:** B uses Amazon Data Lifecycle Manager to automate scheduled AMI creation with a built-in cross-region copy, and D uses AWS Backup's native cross-region copy feature in the backup plan itself — both are fully automated with no manual steps. A, C, and E all fail because they rely on a manual "copy on demand" step or a custom Lambda function instead of the native automated copy capability.

---

## Question 402

**Full Question:** A company has deployed a serverless application that invokes an AWS Lambda function when new documents are uploaded to an Amazon S3 bucket. The application uses the Lambda function to process the documents. After a recent marketing campaign, the company noticed that the application did not process many of the documents. What should a solutions architect do to improve the architecture of this application?

**Short Question:** How should you fix a serverless S3-to-Lambda pipeline that's losing documents during traffic spikes?

**Options:**
- A. Set the Lambda function's runtime timeout value to 15 minutes.
- B. Configure an S3 bucket replication policy. Stage the documents in the S3 bucket for later processing.
- C. Deploy an additional Lambda function. Load balance the processing of the documents across the two Lambda functions.
- ✅ D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Send the requests to the queue. Configure the queue as an event source for Lambda.

**Reason:** Direct S3-to-Lambda invocation is asynchronous with no durable buffer, so events can be dropped during a traffic spike if processing fails; putting an SQS queue between S3 and Lambda decouples them and durably retains messages until they are successfully processed. Increasing the timeout, using S3 replication, or manually load-balancing across two functions none address the root cause of lost events.

---

## Question 403

**Full Question:** A company that primarily runs its application servers on premises has decided to migrate to AWS. The company wants to minimize its need to scale its internet Small Computer Systems Interface (iSCSI) storage on premises. The company wants only its recently accessed data to remain stored locally. Which AWS solution should the company use to meet these requirements?

**Short Question:** Which AWS Storage Gateway type keeps only recently-accessed iSCSI block data on premises while storing the full data set in the cloud?

**Options:**
- A. Amazon S3 File Gateway
- B. AWS Storage Gateway Tape Gateway
- C. AWS Storage Gateway Volume Gateway (stored volumes)
- ✅ D. AWS Storage Gateway Volume Gateway (cached volumes)

**Reason:** Cached volumes present iSCSI block storage on premises while storing the full primary data set in Amazon S3 and keeping only a local cache of frequently/recently accessed data, minimizing on-premises storage. File Gateway is file-based (not iSCSI), Tape Gateway is for backup/archival, and stored volumes keep the entire data set on premises (the opposite of what's needed).

---

## Question 404

**Full Question:** A company has a web server running on an Amazon EC2 instance in a public subnet with an Elastic IP address. The default security group is assigned to the EC2 instance. The default network ACL has been modified to block all traffic. A solutions architect needs to make the web server accessible from everywhere on port 443. Which combination of steps will accomplish this task? (Choose two.)

**Short Question:** Which two security group and network ACL changes open HTTPS (port 443) access to a public EC2 web server?

**Options:**
- ✅ A. Create a security group with a rule to allow TCP port 443 from source 0.0.0.0/0.
- B. Create a security group with a rule to allow TCP port 443 to destination 0.0.0.0/0.
- C. Update the network ACL to allow TCP port 443 from source 0.0.0.0/0.
- D. Update the network ACL to allow inbound/outbound TCP port 443 from source 0.0.0.0/0 to destination 0.0.0.0/0.
- ✅ E. Update the network ACL to allow inbound TCP port 443 from source 0.0.0.0/0, and outbound TCP ports 32768–65535 to destination 0.0.0.0/0.

**Reason:** Security groups are stateful, so a single inbound rule allowing port 443 from anywhere (A) is sufficient at that layer; network ACLs are stateless, so they need both an inbound rule for port 443 and an outbound rule for the ephemeral port range (32768–65535) that return traffic actually uses, which only E provides correctly. B specifies an outbound rule when the block is on inbound traffic, C is missing the required outbound rule, and D uses port 443 for the outbound rule instead of the correct ephemeral port range.

---

## Question 405

**Full Question:** A company maintains a searchable repository of items on its website. The data is stored in an Amazon RDS for MySQL database table that contains more than 10 million rows. The database has 2 terabytes of general purpose SSD storage. There are millions of updates against this data every day through the company's website. The company has noticed that some insert operations are taking 10 seconds or longer. The company has determined that the database storage performance is the problem. Which solution addresses this performance issue?

**Short Question:** How do you fix slow RDS MySQL insert performance that's been diagnosed as a storage I/O bottleneck?

**Options:**
- ✅ A. Change the storage type to Provisioned IOPS SSD.
- B. Change the DB instance to a memory optimized instance class.
- C. Change the DB instance to a burstable performance instance class.
- D. Enable Multi-AZ RDS read replicas with MySQL native asynchronous replication.

**Reason:** The bottleneck is explicitly storage I/O, and Provisioned IOPS SSD is designed to deliver consistent, high, low-latency IOPS for sustained transactional workloads, directly solving that problem. Changing the instance class (memory-optimized or burstable) only affects CPU/memory rather than disk I/O, and Multi-AZ/read replicas address availability and read scaling, not write/insert performance.

---

## Question 406

**Full Question:** A company's application is having performance issues. The application is stateful and needs to complete in-memory tasks on Amazon EC2 instances. The company used AWS CloudFormation to deploy infrastructure and used the M5 EC2 instance family. As traffic increased, the application performance degraded. Users are reporting delays when the users attempt to access the application. Which solution will resolve these issues in the most operationally efficient way?

**Short Question:** What's the most efficient fix for a memory-bound, stateful EC2 app that's slowing down as traffic grows?

**Options:**
- A. Replace the EC2 instances with T3 EC2 instances that run in an Auto Scaling group. Make the changes by using the AWS Management Console.
- B. Modify the CloudFormation templates to run the EC2 instances in an Auto Scaling group. Increase the desired capacity and the maximum capacity of the Auto Scaling group manually when an increase is necessary.
- C. Modify the CloudFormation templates. Replace the EC2 instances with R5 EC2 instances. Use Amazon CloudWatch built-in EC2 memory metrics to track the application performance for future capacity planning.
- ✅ D. Modify the CloudFormation templates. Replace the EC2 instances with R5 EC2 instances. Deploy the Amazon CloudWatch agent on the EC2 instances to generate custom application latency metrics for future capacity planning.

**Reason:** R5 instances are memory-optimized, which matches the memory-intensive workload, but CloudWatch does not collect memory metrics by default — the CloudWatch agent must be installed to get that data, making D correct and C factually wrong. A ignores the memory bottleneck and bypasses CloudFormation via the console (causing drift), and B just adds more of the same under-resourced instance type via Auto Scaling, which doesn't fix a stateful memory-bound app.

---

## Question 407

**Full Question:** A company has a web application with sporadic usage patterns. There is heavy usage at the beginning of each month, moderate usage at the start of each week, and unpredictable usage during the week. The application consists of a web server and a MySQL database server running inside the data center. The company would like to move the application to the AWS Cloud and needs to select a cost-effective database platform that will not require database modifications. Which solution will meet these requirements?

**Short Question:** What's the cheapest MySQL-compatible AWS database for a workload with unpredictable, spiky usage?

**Options:**
- A. Amazon DynamoDB
- B. Amazon RDS for MySQL
- ✅ C. MySQL-compatible Amazon Aurora Serverless
- D. MySQL deployed on Amazon EC2 in an Auto Scaling group

**Reason:** Aurora Serverless is MySQL-compatible (so no application changes are needed) and automatically scales database compute up and down — even to zero during idle periods — making it the most cost-effective fit for sporadic traffic. DynamoDB would require a NoSQL rewrite, RDS for MySQL bills for always-on provisioned capacity, and self-managed MySQL on EC2 in an Auto Scaling group is complex and impractical for a stateful database.

---

## Question 408

**Full Question:** A company is developing an application that provides order shipping statistics for retrieval by a REST API. The company wants to extract the shipping statistics, organize the data into an easy-to-read HTML format, and send the report to several email addresses at the same time every morning. Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

**Short Question:** How do you build a serverless daily job that pulls data from a REST API and emails it as an HTML report?

**Options:**
- A. Configure the application to send the data to Amazon Kinesis Data Firehose.
- ✅ B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
- C. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Glue job to query the application's API for the data.
- ✅ D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.
- E. Store the application data in Amazon S3. Create an Amazon Simple Notification Service (Amazon SNS) topic as an S3 event destination to send the report by email.

**Reason:** EventBridge provides the scheduling to trigger a Lambda function on a daily basis, and Lambda is the right lightweight serverless compute to call the API, build the HTML, and hand it off; SES is the purpose-built service for sending that HTML-formatted report to multiple recipients. Kinesis Data Firehose is for streaming ingestion (not report generation), Glue is a heavyweight ETL tool that's overkill here, and the S3/SNS approach doesn't fit since the source is a REST API and SNS isn't designed for HTML email formatting.

---

## Question 409

**Full Question:** An image hosting company stores its objects in Amazon S3 buckets. The company wants to avoid accidental exposure of the objects in the S3 buckets to the public. All S3 objects in the entire AWS account need to remain private. Which solution will meet these requirements?

**Short Question:** What's the strongest way to guarantee every S3 object across an entire AWS account stays private?

**Options:**
- A. Use Amazon GuardDuty to monitor S3 bucket policies. Create an automatic remediation action rule that uses an AWS Lambda function to remediate any change that makes the objects public.
- B. Use AWS Trusted Advisor to find publicly accessible S3 buckets. Configure email notifications in Trusted Advisor when a change is detected. Manually change the S3 bucket policy if it allows public access.
- C. Use AWS Resource Access Manager to find publicly accessible S3 buckets. Use Amazon Simple Notification Service (Amazon SNS) to invoke an AWS Lambda function when a change is detected. Deploy a Lambda function that programmatically remediates the change.
- ✅ D. Use the S3 Block Public Access feature at the account level. Use AWS Organizations to create a service control policy (SCP) that prevents IAM users from changing the setting. Apply the SCP to the account.

**Reason:** S3 Block Public Access at the account level is a proactive, preventive control that stops any bucket from ever becoming public, and locking it down with an SCP ensures no IAM user (not even the root user) can disable it. The other options are reactive or detective controls that only catch and fix exposure after it already happened, and Resource Access Manager isn't even designed to detect public S3 buckets — it's for sharing resources across accounts.

---

## Question 410

**Full Question:** A company is launching a new application and will display application metrics on an Amazon CloudWatch dashboard. The company's product manager needs to access this dashboard periodically. The product manager does not have an AWS account. A solutions architect must provide access to the product manager by following the principle of least privilege. Which solution will meet these requirements?

**Short Question:** How do you give someone without an AWS account least-privilege, read-only access to one CloudWatch dashboard?

**Options:**
- ✅ A. Share the dashboard from the CloudWatch console. Enter the product manager's email address and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.
- B. Create an IAM user specifically for the product manager. Attach the CloudWatch ReadOnlyAccess AWS managed policy to the user. Share the new login credentials with the product manager. Share the browser URL of the correct dashboard with the product manager.
- C. Create an IAM user for the company's employees. Attach the ViewOnlyAccess AWS managed policy to the IAM user. Share the new login credentials with the product manager. Ask the product manager to navigate to the CloudWatch console and locate the dashboard by name in the dashboard section.
- D. Deploy a bastion server in a public subnet. When the product manager requires access to the dashboard, start the server and share the RDP credentials on the bastion server. Ensure that the browser is configured to open the dashboard URL with cached AWS credentials that have appropriate permissions to view the dashboard.

**Reason:** CloudWatch's built-in dashboard sharing feature lets you grant a named external user read-only access to just that one dashboard via a link, without creating any AWS identity for them — a textbook least-privilege solution. Options B and C both violate the "no AWS account" requirement by creating an IAM user, with C also granting an overly broad ViewOnlyAccess policy across nearly all services, and D is an insecure, high-overhead approach involving shared RDP credentials and cached AWS credentials in a browser.

---

## Question 411

**Full Question:** A company is running several business applications in three separate VPCs within the US East 1 region. The applications must be able to communicate between VPCs. The applications also must be able to consistently send hundreds of gigabytes of data each day to a latency sensitive application that runs in a single on-premises data center. A solutions architect needs to design a network connectivity solution that maximizes cost effectiveness. Which solution meets these requirements?

**Short Question:** Most cost-effective way to connect three VPCs to each other and to a single on-premises data center with high data volume and low latency?

**Options:**
- A. Configure three AWS Site-to-Site VPN connections from the data center to AWS. Establish connectivity by configuring one VPN connection for each VPC.
- B. Launch a third-party virtual network appliance in each VPC. Establish an IPsec VPN tunnel between the data center and each virtual appliance.
- C. Set up three AWS Direct Connect connections from the data center to a Direct Connect gateway in US East 1. Establish connectivity by configuring each VPC to use one of the Direct Connect connections.
- ✅ D. Set up one AWS Direct Connect connection from the data center to AWS. Create a transit gateway and attach each VPC to the transit gateway. Establish connectivity between the Direct Connect connection and the transit gateway.

**Reason:** A single Direct Connect connection combined with a transit gateway creates a cost-effective hub-and-spoke design where all VPCs and the on-premises network share one high-performance, low-latency connection; VPN options are too slow for the data volume, and multiple Direct Connect connections are unnecessarily expensive.

---

## Question 412

**Full Question:** A company has an on-premises volume backup solution that has reached its end of life. The company wants to use AWS as part of a new backup solution and wants to maintain local access to all the data while it is backed up on AWS. The company wants to ensure that the data backed up on AWS is automatically and securely transferred. Which solution meets these requirements?

**Short Question:** Best AWS hybrid backup solution that keeps all data locally accessible on-premises while automatically backing it up to AWS?

**Options:**
- A. Use AWS Snowball to migrate data out of the on-premises solution to Amazon S3. Configure on-premises systems to mount the Snowball S3 endpoint to provide local access to the data.
- B. Use AWS Snowball Edge to migrate data out of the on-premises solution to Amazon S3. Use the Snowball Edge file interface to provide on-premises systems with local access to the data.
- C. Use AWS Storage Gateway and configure a cached volume gateway. Run the Storage Gateway software appliance on-premises and configure a percentage of data to cache locally. Mount the gateway storage volumes to provide local access to the data.
- ✅ D. Use AWS Storage Gateway and configure a stored volume gateway. Run the Storage Gateway software appliance on-premises and map the gateway storage volumes to on-premises storage. Mount the gateway storage volumes to provide local access to the data.

**Reason:** Stored volume gateway keeps the entire primary dataset on-premises for full local access while asynchronously backing up point-in-time snapshots to Amazon S3; cached volume gateway fails the "all data local" requirement since it only caches frequently used data locally, and Snowball/Snowball Edge are migration tools, not ongoing backup solutions.

---

## Question 413

**Full Question:** A company has multiple AWS accounts for development work. Some staff consistently use oversized Amazon EC2 instances, which causes the company to exceed the yearly budget for the development accounts. The company wants to centrally restrict the creation of AWS resources in these accounts. Which solution will meet these requirements with the least development effort?

**Short Question:** Least-effort way to centrally block developers across multiple AWS accounts from launching oversized EC2 instances?

**Options:**
- A. Develop AWS Systems Manager templates that use an approved EC2 creation process. Use the approved Systems Manager templates to provision EC2 instances.
- ✅ B. Use AWS Organizations to organize the accounts into organizational units (OUs). Define and attach a service control policy (SCP) to control the usage of EC2 instance types.
- C. Configure an Amazon EventBridge rule that invokes an AWS Lambda function when an EC2 instance is created. Stop disallowed EC2 instance types.
- D. Set up AWS Service Catalog products for the staff to create the allowed EC2 instance types. Ensure that staff can deploy EC2 instances only by using the Service Catalog products.

**Reason:** A service control policy attached at the organizational unit level is a centrally managed, preventive guardrail that denies launching disallowed instance types across all accounts with no custom development, unlike Systems Manager templates or Service Catalog (which can be bypassed) or an EventBridge/Lambda rule (which only reacts after the instance is already created).

---

## Question 414

**Full Question:** A company is developing software that uses a PostgreSQL database schema. The company needs to configure multiple development environments and databases for the company's developers. On average, each development environment is used for half of the 8-hour workday. Which solution will meet these requirements most cost effectively?

**Short Question:** Most cost-effective PostgreSQL-compatible database option for dev environments that are only used about half the workday?

**Options:**
- A. Configure each development environment with its own Amazon Aurora PostgreSQL database.
- B. Configure each development environment with its own Amazon RDS for PostgreSQL Single-AZ DB instance.
- ✅ C. Configure each development environment with its own Amazon Aurora Serverless PostgreSQL-compatible database.
- D. Configure each development environment with its own Amazon S3 bucket by using Amazon S3 Object Select.

**Reason:** Aurora Serverless automatically scales capacity up and down (or pauses) based on demand, so idle development environments cost little to nothing, unlike standard provisioned Aurora or RDS instances that charge continuously whether idle or not; S3 isn't a relational database at all and can't support a PostgreSQL schema.

---

## Question 415

**Full Question:** A company is developing an application to support customer demands. The company wants to deploy the application on multiple Amazon EC2 Nitro-based instances within the same availability zone. The company also wants to give the application the ability to write to multiple block storage volumes on multiple EC2 Nitro-based instances simultaneously to achieve higher application availability. Which solution will meet these requirements?

**Short Question:** Which EBS volume type supports EBS Multi-Attach so multiple Nitro-based EC2 instances can write to the same volume simultaneously?

**Options:**
- A. Use General Purpose SSD (GP3) EBS volumes with Amazon EBS Multi-Attach.
- B. Use Throughput Optimized HDD (ST1) EBS volumes with Amazon EBS Multi-Attach.
- ✅ C. Use Provisioned IOPS SSD (IO2) EBS volumes with Amazon EBS Multi-Attach.
- D. Use General Purpose SSD (GP2) EBS volumes with Amazon EBS Multi-Attach.

**Reason:** EBS Multi-Attach is only supported on Provisioned IOPS SSD volumes (io1/io2), allowing a single volume to attach to multiple Nitro-based instances in the same availability zone; GP3, GP2, and ST1 volumes do not support this feature.

---

## Question 416

**Full Question:** A solutions architect is designing the architecture for a software demonstration environment. The environment will run on Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer (ALB). The system will experience significant increases in traffic during working hours but is not required to operate on weekends. Which combination of actions should the solutions architect take to ensure that the system can scale to meet demand? (Choose two.)

**Short Question:** What two scaling actions handle both unpredictable weekday traffic spikes and a scheduled weekend shutdown for an EC2 Auto Scaling group?

**Options:**
- A. Use AWS Auto Scaling to adjust the ALB capacity based on request rate
- B. Use AWS Auto Scaling to scale the capacity of the VPC internet gateway
- C. Launch the EC2 instances in multiple AWS Regions to distribute the load across Regions
- ✅ D. Use a target tracking scaling policy to scale the Auto Scaling group based on instance CPU utilization
- ✅ E. Use scheduled scaling to change the Auto Scaling group minimum, maximum, and desired capacity to zero for weekends, then revert to the default values at the start of the week

**Reason:** A target tracking policy on CPU utilization handles the unpredictable weekday traffic increases automatically, while scheduled scaling to zero capacity on weekends addresses the predictable, known-in-advance shutdown period; the ALB and internet gateway already scale automatically and are not user-configurable, and multi-region deployment is unnecessary complexity for this single-environment scaling problem.

---

## Question 417

**Full Question:** A company is creating an application that runs on containers in a VPC. The application stores and accesses data in an Amazon S3 bucket. During the development phase, the application will store and access 1 TB of data in Amazon S3 each day. The company wants to minimize costs and wants to prevent traffic from traversing the internet whenever possible. Which solution will meet these requirements?

**Short Question:** What's the cheapest way to give a VPC private, non-internet access to S3 for high-volume daily traffic?

**Options:**
- A. Enable S3 Intelligent-Tiering for the S3 bucket
- B. Enable S3 Transfer Acceleration for the S3 bucket
- ✅ C. Create a gateway VPC endpoint for Amazon S3. Associate this endpoint with all route tables in the VPC
- D. Create an interface endpoint for Amazon S3 in the VPC. Associate this endpoint with all route tables in the VPC

**Reason:** A gateway VPC endpoint routes S3 traffic privately over the AWS network with no hourly charge and no data processing fee, making it the most cost-effective option; an interface endpoint provides similar private connectivity but incurs hourly and per-GB charges, Intelligent-Tiering only affects storage cost (not network path), and Transfer Acceleration actually routes traffic over the public internet.

---

## Question 418

**Full Question:** A company runs an application that receives data from thousands of geographically dispersed remote devices that use UDP. The application processes the data immediately and sends a message back to the device if necessary. No data is stored. The company needs a solution that minimizes latency for the data transmission from the devices. The solution also must provide rapid failover to another AWS Region. Which solution will meet these requirements?

**Short Question:** What's the lowest-latency, fastest-failover way to serve a global UDP-based application across two AWS Regions?

**Options:**
- A. Configure an Amazon Route 53 failover routing policy. Create a Network Load Balancer (NLB) in each of the two Regions. Configure the NLB to invoke an AWS Lambda function to process the data
- ✅ B. Use AWS Global Accelerator. Create a Network Load Balancer (NLB) in each of the two Regions as an endpoint. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the NLB. Process the data in Amazon ECS
- C. Use AWS Global Accelerator. Create an Application Load Balancer (ALB) in each of the two Regions as an endpoint. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the ALB. Process the data in Amazon ECS
- D. Configure an Amazon Route 53 failover routing policy. Create an Application Load Balancer (ALB) in each of the two Regions. Create an Amazon Elastic Container Service (Amazon ECS) cluster with the Fargate launch type. Create an ECS service on the cluster. Set the ECS service as the target for the ALB. Process the data in Amazon ECS

**Reason:** AWS Global Accelerator routes traffic over the AWS private backbone for minimal latency and provides near-instant regional failover, and since the application uses UDP it needs a Network Load Balancer (an ALB only supports HTTP/HTTPS, not UDP); Route 53 failover relies on DNS TTLs and caching, which is far slower than Global Accelerator's failover.

---

## Question 419

**Full Question:** A company is using a centralized AWS account to store log data in various Amazon S3 buckets. A solutions architect needs to ensure that the data is encrypted at rest before the data is uploaded to the S3 buckets. The data also must be encrypted in transit. Which solution meets these requirements?

**Short Question:** How do you make sure log data is already encrypted before it even leaves the client on its way to S3?

**Options:**
- ✅ A. Use client-side encryption to encrypt the data that is being uploaded to the S3 buckets
- B. Use server-side encryption to encrypt the data that is being uploaded to the S3 buckets
- C. Create bucket policies that require the use of server-side encryption with S3-managed encryption keys (SSE-S3) for S3 uploads
- D. Enable the security option to encrypt the S3 buckets through the use of a default AWS Key Management Service (AWS KMS) key

**Reason:** Client-side encryption encrypts the data on the client before it is ever sent, so it stays encrypted in transit and at rest, satisfying the "encrypted before upload" requirement; all the other options (SSE, SSE-S3 bucket policies, default KMS key encryption) are forms of server-side encryption, meaning S3 only encrypts the data after it has already been received.

---

## Question 420

**Full Question:** A company uses AWS Organizations to manage multiple AWS accounts for different departments. The management account has an Amazon S3 bucket that contains project reports. The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations. Which solution meets these requirements with the least amount of operational overhead?

**Short Question:** What's the lowest-maintenance way to restrict an S3 bucket so only accounts inside your AWS Organization can access it?

**Options:**
- ✅ A. Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy
- B. Create an organizational unit (OU) for each department. Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy
- C. Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFromOrganization events. Update the S3 bucket policy accordingly
- D. Tag each user that needs access to the S3 bucket. Add the aws:PrincipalTag global condition key to the S3 bucket policy

**Reason:** The aws:PrincipalOrgID condition key lets one static bucket policy statement automatically grant access to every current and future member account of the organization, requiring no updates as accounts change; the OU-path key adds unnecessary complexity, the CloudTrail approach requires custom automation to react to account events, and tagging every user individually creates a large, ongoing manual burden.
