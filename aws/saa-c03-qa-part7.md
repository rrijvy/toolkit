# AWS SAA-C03 Real Exam Questions & Answers — Part 7 (Q151–Q175)

Source: [AWS Certified Solutions Architect (SAA-C03) | Real Exam Questions & Answers | Part 7](https://www.youtube.com/watch/ruZ0ak64KwQ)

---

## Question 151

**Full Question:** A company is using AWS to design a web application that will process insurance quotes. Users will request quotes from the application. Quotes must be separated by quote type, must be responded to within 24 hours, and must not get lost. The solution must maximize operational efficiency and must minimize maintenance. Which solution meets these requirements?

**Short Question:** Best decoupled, durable, low-maintenance way to route insurance quote requests by type?

**Options:**
- A. Create multiple Amazon Kinesis data streams based on quote type, with each backend group using KCL to pull from its own stream
- B. Create an AWS Lambda function and an SNS topic for each quote type, and subscribe Lambda to its SNS topic
- ✅ C. Create a single SNS topic, subscribe SQS queues to it, and configure SNS message filtering to route messages to the proper queue by quote type
- D. Create multiple Kinesis Data Firehose delivery streams by quote type into an OpenSearch Service cluster

**Reason:** The SNS fan-out pattern with message filtering to SQS queues is fully managed, durable, and requires no per-type infrastructure — meeting both the reliability and low-maintenance requirements.

---

## Question 152

**Full Question:** A gaming company hosts a browser-based application on AWS. The users of the application consume a large number of videos and images that are stored in Amazon S3. This content is the same for all users. The application has increased in popularity and millions of users worldwide are accessing these media files. The company wants to provide the files to the users while reducing the load on the origin. Which solution meets these requirements most cost effectively?

**Short Question:** Cheapest way to serve the same S3 media files to millions of global users while cutting origin load?

**Options:**
- A. Deploy an AWS Global Accelerator in front of the web servers
- ✅ B. Deploy an Amazon CloudFront web distribution in front of the S3 bucket
- C. Deploy an Amazon ElastiCache for Redis instance in front of the web servers
- D. Deploy an Amazon ElastiCache for Memcached instance in front of the web servers

**Reason:** CloudFront caches S3 content at edge locations, serving nearby users directly and reducing S3 load and cost — the correct CDN use case for global static content.

---

## Question 153

**Full Question:** A hospital is designing a new application that gathers symptoms from patients. The hospital has decided to use Amazon SQS and Amazon SNS in the architecture. A solutions architect is reviewing the infrastructure design. Data must be encrypted at rest and in transit. Only authorized personnel of the hospital should be able to access the data. Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

**Short Question:** Which two steps encrypt SNS and SQS at rest/in transit with restricted key access?

**Options:**
- A. Turn on server-side encryption on the SQS components; update the default (AWS-managed) key policy
- ✅ B. Turn on SSE on the SNS components using a KMS customer managed key; apply a key policy restricting key usage to authorized principals
- C. Turn on encryption on the SNS components; update the default key policy; set a condition in the topic policy to allow only TLS connections
- ✅ D. Turn on SSE on the SQS components using a KMS customer managed key; apply a key policy restricting key usage; set a condition in the queue policy to allow only TLS connections
- E. Turn on SSE on the SQS components; apply an IAM policy (not key policy) to restrict key usage

**Reason:** Customer-managed KMS keys with key policies control who can use the key (access control), while queue/topic policy TLS conditions enforce encryption in transit — B covers SNS completely, D covers SQS completely.

---

## Question 154

**Full Question:** A company runs a containerized application on a Kubernetes cluster in an on-premises data center. The company is using a MongoDB database for data storage. The company wants to migrate some of these environments to AWS, but no code changes or deployment method changes are possible at this time. The company needs a solution that minimizes operational overhead. Which solution meets these requirements?

**Short Question:** Migrate on-prem Kubernetes + MongoDB app to AWS with zero code/deployment changes and low overhead?

**Options:**
- A. Amazon ECS with EC2 worker nodes + MongoDB on EC2
- B. Amazon ECS with AWS Fargate + Amazon DynamoDB
- C. Amazon EKS with EC2 worker nodes + Amazon DynamoDB
- ✅ D. Amazon EKS with AWS Fargate + Amazon DocumentDB (with MongoDB compatibility)

**Reason:** EKS preserves the existing Kubernetes deployment method, DocumentDB's MongoDB compatibility avoids code changes, and Fargate removes node management — satisfying every constraint at once.

---

## Question 155

**Full Question:** A company hosts a web application on multiple Amazon EC2 instances. The EC2 instances are in an Auto Scaling group that scales in response to user demand. The company wants to optimize cost savings without making a long-term commitment. Which EC2 instance purchasing option should a solutions architect recommend to meet these requirements?

**Short Question:** Cheapest EC2 purchasing mix for an auto-scaled app with no long-term commitment?

**Options:**
- A. Dedicated instances only
- B. On-Demand instances only
- ✅ C. A mix of On-Demand instances and Spot instances
- D. A mix of On-Demand instances and Reserved instances

**Reason:** Spot instances give deep discounts with no commitment for the elastic (scaling) portion of capacity, while On-Demand covers the steady base — Reserved is ruled out by the no-commitment requirement.

---

## Question 156

**Full Question:** The customers of a finance company request appointments with financial advisers by sending text messages. A web application that runs on Amazon EC2 instances accepts the appointment requests. The text messages are published to an Amazon SQS queue through the web application. Another application that runs on EC2 instances then sends meeting invitations and confirmation emails to the customers. After successful scheduling, this application stores the meeting information in an Amazon DynamoDB database. As the company expands, customers report that their meeting invitations are taking longer to arrive. What should a solutions architect recommend to resolve this issue?

**Short Question:** Fix growing delay in processing an SQS-backed appointment queue?

**Options:**
- A. Add a DynamoDB Accelerator (DAX) cluster in front of the DynamoDB database
- B. Add an Amazon API Gateway API in front of the web application that accepts appointment requests
- C. Add an Amazon CloudFront distribution with the origin set to the web application that accepts appointment requests
- ✅ D. Add an Auto Scaling group for the application that sends meeting invitations, scaling based on SQS queue depth

**Reason:** The bottleneck is processing capacity on the consumer side of the queue — scaling consumer instances based on queue depth directly increases throughput and clears the backlog.

---

## Question 157

**Full Question:** An e-commerce company has an order processing application that uses Amazon API Gateway and an AWS Lambda function. The application stores data in an Amazon Aurora PostgreSQL database. During a recent sales event, a sudden surge in customer orders occurred. Some customers experienced timeouts and the application did not process the orders of those customers. A solutions architect determined that the CPU utilization and memory utilization were high on the database because of a large number of open connections. The solutions architect needs to prevent the timeout errors while making the least possible changes to the application. Which solution will meet these requirements?

**Short Question:** Stop Lambda-caused Aurora connection exhaustion with minimal app changes?

**Options:**
- A. Configure provisioned concurrency for the Lambda function; make the database a global database across multiple regions
- ✅ B. Use Amazon RDS Proxy in front of the database; point the Lambda function at the RDS Proxy endpoint instead of the database endpoint
- C. Create a read replica in a different region; use API Gateway query string parameters to route traffic to the read replica
- D. Migrate from Aurora PostgreSQL to DynamoDB using AWS DMS; modify Lambda to use the DynamoDB table

**Reason:** RDS Proxy pools and shares database connections between many short-lived Lambda invocations, solving connection exhaustion with just an endpoint change — no rewrite required.

---

## Question 158

**Full Question:** A solutions architect has created two IAM policies: Policy 1 and Policy 2. Both policies are attached to an IAM group. A cloud engineer is added as an IAM user to the IAM group. Which action will the cloud engineer be able to perform?

**Short Question:** Which action is allowed when Policy 1 grants broadly and Policy 2 explicitly denies specific actions?

**Options:**
- A. Deleting IAM users
- B. Deleting directories
- ✅ C. Deleting Amazon EC2 instances
- D. Deleting logs from Amazon CloudWatch Logs

**Reason:** Policy 1 allows EC2:* (including terminate) and Policy 2 has no explicit deny for EC2 actions, so it's permitted; the other actions are either never granted or explicitly denied, and an explicit deny always wins.

---

## Question 159

**Full Question:** A company must migrate 20 terabytes of data from a data center to the AWS Cloud within 30 days. The company's network bandwidth is limited to 15 megabits per second and cannot exceed 70% utilization. What should a solutions architect do to meet these requirements?

**Short Question:** Move 20 TB in 30 days over a bandwidth-limited link that can't meet the network deadline?

**Options:**
- ✅ A. Use AWS Snowball
- B. Use AWS DataSync
- C. Use a secure VPN connection
- D. Use Amazon S3 Transfer Acceleration

**Reason:** At 15 Mbps and 70% utilization, a network transfer would take ~185 days — far too slow — so a physical device (Snowball) that bypasses the network entirely is required.

---

## Question 160

**Full Question:** A company has hired a solutions architect to design a reliable architecture for its application. The application consists of one Amazon RDS DB instance and two manually provisioned Amazon EC2 instances that run web servers. The EC2 instances are located in a single availability zone. An employee recently deleted the DB instance and the application was unavailable for 24 hours as a result. The company is concerned with the overall reliability of its environment. What should the solutions architect do to maximize reliability of the application's infrastructure?

**Short Question:** Best fix for single-AZ EC2 + a DB instance that got accidentally deleted?

**Options:**
- A. Delete one EC2 instance and enable termination protection on the other; make the DB instance Multi-AZ with deletion protection
- ✅ B. Make the DB instance Multi-AZ with deletion protection; place EC2 instances behind an ALB in an Auto Scaling group across multiple AZs
- C. Create an additional DB instance plus API Gateway and a Lambda function to write to both DB instances
- D. Place EC2 instances in an Auto Scaling group across multiple AZs using Spot instances; make the DB instance Multi-AZ with deletion protection

**Reason:** Multi-AZ with deletion protection secures the database, and an ALB + Auto Scaling group across AZs removes the EC2 single point of failure — Spot instances are wrong here because interruptions hurt reliability.

---

## Question 161

**Full Question:** A company runs a public three-tier web application in a VPC. The application runs on Amazon EC2 instances across multiple availability zones. The EC2 instances that run in private subnets need to communicate with a license server over the internet. The company needs a managed solution that minimizes operational maintenance. Which solution meets these requirements?

**Short Question:** Fully managed way for private-subnet EC2 to reach the internet with least maintenance?

**Options:**
- A. Provision a NAT instance in a public subnet; route private subnets to it
- B. Provision a NAT instance in a private subnet; route private subnets to it
- ✅ C. Provision a NAT gateway in a public subnet; route private subnets to it
- D. Provision a NAT gateway in a private subnet; route private subnets to it

**Reason:** NAT Gateway is fully AWS-managed (no patching/scaling) and must sit in a public subnet with a route to the internet gateway to function.

---

## Question 162

**Full Question:** An Amazon EC2 instance is located in a private subnet in a new VPC. This subnet does not have outbound internet access, but the EC2 instance needs the ability to download monthly security updates from an outside vendor. What should a solutions architect do to meet these requirements?

**Short Question:** Give a private-subnet EC2 instance outbound-only internet access for updates?

**Options:**
- A. Attach an internet gateway and route the private subnet's default route to it
- ✅ B. Create a NAT gateway in a public subnet; route the private subnet's default route to the NAT gateway
- C. Create a NAT instance in the same private subnet as the EC2 instance; route to it
- D. Attach an internet gateway, plus a NAT instance in the private subnet, routed to the internet gateway

**Reason:** A NAT device must live in a public subnet with a path to the internet gateway; routing the private subnet directly to an internet gateway would make it public instead.

---

## Question 163

**Full Question:** A company wants to migrate its MySQL database from on premises to AWS. The company recently experienced a database outage that significantly impacted the business. To ensure this does not happen again, the company wants a reliable database solution on AWS that minimizes data loss and stores every transaction on at least two nodes. Which solution meets these requirements?

**Short Question:** Reliable AWS MySQL solution that synchronously writes every transaction to 2+ nodes?

**Options:**
- A. Amazon RDS DB instance with synchronous replication to three nodes across three AZs
- ✅ B. Amazon RDS for MySQL DB instance with Multi-AZ enabled for synchronous replication
- C. Amazon RDS for MySQL DB instance with a cross-region read replica synchronously replicating data
- D. Amazon EC2 instance running MySQL, triggering a Lambda function to synchronously replicate to an RDS MySQL instance

**Reason:** RDS Multi-AZ synchronously replicates every committed transaction to a standby in a different AZ before acknowledging the write — read replicas are asynchronous, and RDS has no 3-node sync feature.

---

## Question 164

**Full Question:** A company owns an asynchronous API that is used to ingest user requests and, based on the request type, dispatch requests to the appropriate microservice for processing. The company is using Amazon API Gateway to deploy the API front end and an AWS Lambda function that invokes Amazon DynamoDB to store user requests before dispatching them to the processing microservices. The company provisioned as much DynamoDB throughput as its budget allows, but the company is still experiencing availability issues and is losing user requests. What should a solutions architect do to address this issue without impacting existing users?

**Short Question:** Stop dropping requests when DynamoDB writes are throttled at max budgeted throughput?

**Options:**
- A. Add throttling on API Gateway with server-side throttling limits
- B. Use DynamoDB Accelerator (DAX) and Lambda to buffer writes to DynamoDB
- C. Create a secondary index in DynamoDB for the requests table
- ✅ D. Use an Amazon SQS queue and Lambda to buffer writes to DynamoDB

**Reason:** SQS decouples fast request ingestion from the slower, throughput-limited DynamoDB writes, acting as a durable buffer that absorbs traffic spikes without losing requests.

---

## Question 165

**Full Question:** A company hosts a three-tier web application that includes a PostgreSQL database. The database stores the metadata from documents. The company searches the metadata for key terms to retrieve documents that the company reviews in a report each month. The documents are stored in Amazon S3. The documents are usually written only once, but they are updated frequently. The reporting process takes a few hours with the use of relational queries. The reporting process must not prevent any document modifications or the addition of new documents. A solutions architect needs to implement a solution to speed up the reporting process. Which solution will meet these requirements with the least amount of change to the application code?

**Short Question:** Speed up slow monthly PostgreSQL reporting queries without blocking writes, minimal code change?

**Options:**
- A. New Amazon DocumentDB (MongoDB compatible) cluster with a read replica; scale the read replica for reports
- ✅ B. New Amazon Aurora PostgreSQL cluster with an Aurora replica; issue report queries to the Aurora replica
- C. New Amazon RDS for PostgreSQL Multi-AZ instance; query the secondary (standby) node for reports
- D. New DynamoDB table with fixed write capacity and auto-scaled read capacity for reports

**Reason:** Aurora PostgreSQL is wire-compatible with PostgreSQL (near-zero code change) and Aurora replicas can serve reads, unlike a Multi-AZ standby which is passive and cannot be queried.

---

## Question 166

**Full Question:** A company has a Windows-based application that must be migrated to AWS. The application requires the use of a shared Windows file system attached to multiple Amazon EC2 Windows instances that are deployed across multiple availability zones. What should a solutions architect do to meet this requirement?

**Short Question:** Shared file system for multiple Windows EC2 instances across AZs?

**Options:**
- A. AWS Storage Gateway in Volume Gateway mode, mounted to each Windows instance
- ✅ B. Amazon FSx for Windows File Server, mounted to each Windows instance
- C. Amazon EFS, mounted to each Windows instance
- D. A single Amazon EBS volume attached and mounted to each EC2 instance

**Reason:** FSx for Windows File Server natively provides an SMB shared file system for Windows workloads across AZs; EFS is NFS (Linux-native), EBS can't multi-attach a filesystem this way, and Storage Gateway Volume mode is block storage, not a file share.

---

## Question 167

**Full Question:** A media company uses Amazon CloudFront for its publicly available streaming video content. The company wants to secure the video content that is hosted in Amazon S3 by controlling who has access. Some of the company's users are using a custom HTTP client that does not support cookies. Some of the company's users are unable to change the hard-coded URLs that they are using for access. Which services or methods will meet these requirements with the least impact to the users? (Choose two.)

**Short Question:** Secure CloudFront content for users who can't use cookies AND users who can't change hard-coded URLs?

**Options:**
- ✅ A. Signed cookies
- ✅ B. Signed URLs
- C. AWS AppSync
- D. JSON Web Token (JWT)
- E. AWS Secrets Manager

**Reason:** Signed cookies keep the original URL unchanged (for the hard-coded-URL group) while signed URLs embed auth in the URL itself (for clients without cookie support) — together they cover both groups.

---

## Question 168

**Full Question:** A company is developing a microservices application that will provide a search catalog for customers. The company must use REST APIs to present the front end of the application to users. The REST APIs must access the back-end services that the company hosts in containers in private VPC subnets. Which solution will meet these requirements?

**Short Question:** Connect a public REST API to backend containers running in private VPC subnets?

**Options:**
- A. WebSocket API via API Gateway; ECS in a private subnet; private VPC link
- ✅ B. REST API via API Gateway; ECS in a private subnet; private VPC link
- C. WebSocket API via API Gateway; ECS in a private subnet; security group for access
- D. REST API via API Gateway; ECS in a private subnet; security group for access

**Reason:** The requirement specifies REST (not WebSocket) APIs, and a private VPC Link — not a security group alone — is the actual network path that lets API Gateway reach resources in a private subnet.

---

## Question 169

**Full Question:** A global marketing company has applications that run in the AP Southeast 2 region and the EU West 1 region. Applications that run in a VPC in US West 1 need to communicate securely with databases that run in a VPC in AP Southeast 2. Which network design will meet these requirements?

**Short Question:** Secure cross-region app-to-database connectivity — how to set VPC peering + security group rules correctly?

**Options:**
- A. VPC peering EU West 1 ↔ AP Southeast 2; inbound rule on the EU West 1 app security group allowing the AP Southeast 2 DB security group
- B. VPC peering AP Southeast 2 ↔ EU West 1, route tables updated; inbound rule on AP Southeast 2 DB security group referencing the EU West 1 app security group ID
- ✅ C. VPC peering AP Southeast 2 ↔ EU West 1, route tables updated; inbound rule on AP Southeast 2 DB security group allowing traffic from EU West 1 app server IP addresses
- D. Transit gateway peering attachment between EU West 1 and AP Southeast 2; inbound rule on DB security group referencing the EU West 1 app security group ID

**Reason:** The inbound rule belongs on the database (receiving traffic) side, and cross-region security group rules cannot reference another region's security group ID — only IP/CIDR ranges work across regions.

---

## Question 170

**Full Question:** A company has an application that collects data from IoT sensors on automobiles. The data is streamed and stored in Amazon S3 through Amazon Kinesis Data Firehose. The data produces trillions of S3 objects each year. Each morning, the company uses the data from the previous 30 days to retrain a suite of machine learning (ML) models. Four times each year, the company uses the data from the previous 12 months to perform analysis and train other ML models. The data must be available with minimal delay for up to one year. After one year, the data must be retained for archival purposes. Which storage solution meets these requirements most cost effectively?

**Short Question:** Cheapest S3 tiering for trillions of objects: hot 30 days, warm to 1 year, archive after?

**Options:**
- A. S3 Intelligent-Tiering; lifecycle policy to S3 Glacier Deep Archive after 1 year
- B. S3 Intelligent-Tiering; Intelligent-Tiering auto-moves objects to S3 Glacier Deep Archive after 1 year
- C. S3 Standard-IA; lifecycle policy to S3 Glacier Deep Archive after 1 year
- ✅ D. S3 Standard; lifecycle policy to S3 Standard-IA after 30 days, then to S3 Glacier Deep Archive after 1 year

**Reason:** The access pattern is fully predictable (frequent for 30 days, infrequent to 1 year, archived after), so plain lifecycle transitions beat Intelligent-Tiering's per-object monitoring fee, and Standard-IA avoids high retrieval costs during the frequent-access first 30 days.

---

## Question 171

**Full Question:** A company has an ordering application that stores customer information in Amazon RDS for MySQL. During regular business hours, employees run one-time queries for reporting purposes. Timeouts are occurring during order processing because the reporting queries are taking a long time to run. The company needs to eliminate the timeouts without preventing employees from performing queries. What should a solutions architect do to meet these requirements?

**Short Question:** Stop reporting queries from causing order-processing timeouts, without blocking reporting?

**Options:**
- ✅ A. Create a read replica; move reporting queries to the read replica
- B. Create a read replica; distribute the ordering application across the primary and the read replica
- C. Migrate the ordering application to DynamoDB with on-demand capacity
- D. Schedule the reporting queries for non-peak hours

**Reason:** A read replica isolates the read-heavy reporting workload from the primary instance handling order writes — writes can't go to a read replica, and scheduling would restrict when employees can query, which is disallowed.

---

## Question 172

**Full Question:** A company recently launched a variety of new workloads on Amazon EC2 instances in its AWS account. The company needs to create a strategy to access and administer the instances remotely and securely. The company needs to implement a repeatable process that works with native AWS services and follows the AWS Well-Architected Framework. Which solution will meet these requirements with the least operational overhead?

**Short Question:** Secure, repeatable, low-overhead way to remotely administer EC2 instances using native AWS tools?

**Options:**
- A. Use the EC2 serial console to directly access each instance's terminal
- ✅ B. Attach an appropriate IAM role to each instance; use AWS Systems Manager Session Manager for remote SSH sessions
- C. Create an admin SSH key pair, load the public key on each instance, and deploy a bastion host in a public subnet
- D. Establish an AWS Site-to-Site VPN and have admins SSH directly across the tunnel using SSH keys

**Reason:** Session Manager needs no open inbound ports, bastion hosts, or SSH key distribution — access is controlled purely via IAM roles, which is the lowest-overhead, most auditable native approach.

---

## Question 173

**Full Question:** A company is migrating an on-premises application to AWS. The company wants to use Amazon Redshift as a solution. Which use cases are suitable for Amazon Redshift in this scenario? (Choose three.)

**Short Question:** Which three describe valid Amazon Redshift use cases/features?

**Options:**
- A. Supporting data APIs to access data with traditional, containerized, and event-driven applications
- ✅ B. Supporting client-side and server-side encryption
- ✅ C. Building analytics workloads during specified hours and when the application is not active
- D. Caching data to reduce the pressure on the backend database
- ✅ E. Scaling globally to support petabytes of data and tens of millions of requests per minute
- F. Creating a secondary replica of the cluster by using the AWS Management Console

**Reason:** Redshift is a data warehouse for large-scale analytics with SSE/KMS and client-side encryption support and massive scale via concurrency scaling — it's not a low-latency transactional API backend, not a cache, and doesn't create "replicas" the way RDS does.

---

## Question 174

**Full Question:** A company hosts multiple production applications. One of the applications consists of resources from Amazon EC2, AWS Lambda, Amazon RDS, Amazon SNS, and Amazon SQS across multiple AWS regions. All company resources are tagged with a tag name of "application" and a value that corresponds to each application. A solutions architect must provide the quickest solution for identifying all of the tagged components. Which solution meets these requirements?

**Short Question:** Fastest way to find all resources across services/regions sharing one tag?

**Options:**
- A. Use AWS CloudTrail to generate a list of resources with the application tag
- B. Use the AWS CLI to query each service across all regions to report tagged components
- C. Run a query in Amazon CloudWatch Logs Insights to report on components with the application tag
- ✅ D. Run a query with the AWS Resource Groups Tag Editor to report on resources globally with the application tag

**Reason:** Resource Groups Tag Editor is purpose-built to search tagged resources across regions and services from one console in a few clicks — CloudTrail, CLI scripting, and Logs Insights are all far slower or not designed for this.

---

## Question 175

**Full Question:** A company is running a batch application on Amazon EC2 instances. The application consists of a backend with multiple Amazon RDS databases. The application is causing a high number of reads on the databases. A solutions architect must reduce the number of database reads while ensuring high availability. What should the solutions architect do to meet this requirement?

**Short Question:** Reduce heavy read load on RDS while keeping high availability?

**Options:**
- ✅ A. Add Amazon RDS read replicas
- B. Use Amazon ElastiCache for Redis
- C. Use Amazon Route 53 DNS caching
- D. Use Amazon ElastiCache for Memcached

**Reason:** RDS read replicas directly offload read traffic from the primary and can be promoted for HA; ElastiCache options require application-level caching code changes and Route 53 only caches DNS lookups, not database data.
