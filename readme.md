* Your company has a product which involves data transfer to individual partners. Each partner has unique data, stored in dedicated S3 buckets.
Which permissions implementation would best minimize the risk of any partner accessing inappropriate data?
  - A. S3 bucket policy applied to each bucket with cross-account permissions
  - B. S3 bucket ACL + object ACLS with cross-account permissions
  - C. IAM user in the company account, credentials delivered to partner 
  - D.IAM role in the company account, cross-account trust with partner

  A
  
* You've been asked to fulfill a company requirement to evaluate compliance against individual controls for business continuity, security, PCI, and GDPR.
Which AWS services can be used to verify compliance with these controls? (pick two)
  - A. AWS CloudTrail
  - B. AWS Lambda
  - C. AWS Config
  - D. AWS Detective
  - E. AWS Security Hub
    
  CE 

* An application is deployed to EC2 instances in a private VPC subnet in the us-west-2 region. There is a functional requirement to access S3 buckets deployed into the ap-southeast-1 region. There is a security requirement for the end-to-end traffic to remain private.
Which combination of steps can meet the functional and security requirements? (pick three)
  - A. Deploy a VPC Gateway endpoint into the us-west-2 VPC.
  - B. Deploy a VPC in the ap-southeast-1 region with an S3 Interface endpoint.
  - C.Configure a route table entry in the us-west-2 VPC to direct S3 traffic to the Gateway endpoint.
  - D. Configure a VPC peering connection between the us-west-2 and ap-southeast-1 VPCs.
  - E. Configure the application to use the ap-southeast-1 region when accessing S3 buckets.
  - F. Configure the application to use the ap-southeast-1 Interface endpoint DNS when accessing S3 buckets.
  
  BDF - This solution is more complicated. S3 endpoints, whether they are Gateway or Interface, cannot route traffic to cross-region buckets.
Furthermore, Gateway endpoints cannot accept traffic from a remote VPC via a peering connection, so the Interface endpoint is the only functional option. This creates an ENI to accept the S3 traffic, with an associated DNS entry which can be used from the remote VPC. The traffic remains private, meeting the security requirement.

* A DevOps team has been asked to provide recommendations to minimize the operational overhead of EC2 fleet OS security governance. The scope includes several regions and AWS accounts, all within the same AWS Organization, with plans for consistent growth over time.
Which of these recommendations would best meet the requirement?
  - A. Implement SSM Patch Manager for all EC2 instances with Patch Groups as appropriate per OS and environment
  - B. Implement third party OS patch management and deploy to all regions and accounts
  - C. Deploy applications using containers onto EC2 instances provisioned in Auto Scaling groups and refresh instances regularly from updated AMIs
  - D. Deploy applications using containers onto manually provisioned EC2 instances and refresh instances regularly from updated AMIs

  C - option A will work but need to implement in every account and all regions. option B is similar to A and not very scalable. option D needs manully provision new EC2 instances

* Your company stores sensitive data in S3. There is a requirement to ensure the data is encrypted at rest in the most secure method possible.
Which of the following encryption configurations would meet the requirement?
  - A. SSE-S3
  - B. SSE-KMS
  - C. SSE-C
  - D. Client-side encryption

  D - option A use AWS-256 with AWS owning the entire chain of trust (root CA, master key, data key). option B, AWS only own Root CA, the rest is owned by customer. option C means customer own the whole chain of trust. option A, B and C means AWS perfoms encryption in S3.


* An on-premises application stores data containing PII on an EFS file system, reachable through a site-to-site VPN. There is a requirement for each file to have certain fields tokenized as quickly as possible to avoid the raw data being easily accessed. The tokenization must be completed entirely within AWS.
Which of the following workflows would meet the requirements?
  - A. Deploy a Lambda function which accesses EFS directly and uses KMS to encrypt the fields inline in each file.
  - B. Deploy a Lambda function which accesses EFS directly. Generate a random string to replace each field, then extract the plaintext from each field. Encrypt the field using KMS and insert into a DynamoDB table with the random string as the partition key.
  - C. Use an EMR cluster to access the EFS filesystem and use KMS to encrypt the fields inline in each file.
  - D. Deploy an AWS Batch job which accesses EFS directly. Generate a random string to replace each field, then extract the plaintext from each field. Encrypt the field using KMS and insert into a DynamoDB table with the random string as the partition key.

  B - option A and C does not to tokenization. option D does not do it in real time


* AWS has emailed an abuse notice to your root account identifying a compromised EC2 instance. Your security team wants to prevent the instance from performing any further activity while not affecting the running OS or other workloads.
Which of the following actions would meet the requirement?
  - A. Create a security group with no inbound or outbound rules. Replace the compromised instance's security group with the newly created one.
  - B. Create a security group with no inbound or outbound rules. Attach the newly created security group to the compromised instance.
  - C. Modify the subnet network ACL and block all outbound traffic.
  - D. Modify the subnet network ACL and block all inbound and outbound traffic.
 
  A - option B will not work as the effective security group rules are the union of all rules. From AWS doc, if there is more than one rule for a specific port, we apply the most permissive rule. For example, if you have a rule that allows access to TCP port 22 (SSH) from IP address 203.0.113.1 and another rule that allows access to TCP port 22 from everyone, everyone has access to TCP port 22. option C andn D will impact othe running instances.


* You've been asked to design the monitoring of an internal application which runs on an EC2 instance in a private subnet, listening on port 8080. This monitoring will be used to calculate availability of the application and be used for business continuity purposes as well as automated failover.
Which of the following recommendations will meet the requirement ir the most reliable manner? (pick two)
    - A. Write a script to run via cron every minute which checks the listener port and uploads as a custom CloudWatch metric.-
    - B. Configure the CloudWatch agent to push a metric which tests the listener.
    - C. Provision a CloudWatch Synthetics canary to test the listener port.
    - D. Configure a CloudWatch alarm from the appropriate metric. Configure a Route 53 Health Check with the alarm as the source.
    - E. Configure a Route 53 Health Check with the CloudWatch metric as the source.

    BC - option A use shell script is not very reliable. Option E, route 53 health checks can not use metric as source but can use alarm as source, but internal application can use a better apprach than route 53

* An application has a requirement for cross-region recovery of a DynamoDB table in case of disaster. The requirements include being able to recover any deleted items within a certain retention window. The table in the source region has DynamoDB Streams enabled.
What recommendation should be made to meet the requirement?
  - A. Configure the global table option and add the disaster recovery region.
  - B. Provision a Lambda function triggered by the DynamoDB stream. Parse the stream entries and propagate Create and Update operations only to the remote table.
  - C. Provision a Lambda function triggered by the DynamoDB stream. Parse the stream entries and propagate all operations to the remote table.
  - D. Provision an EventBridge rule to capture all table events. Forward the events to a Lambda function which parses the events and applies them to the remote table.  

  B - option A willl fully recover and reflect deleted items and wont recover deleted items. option C and D is like option A but not meet the requirement for recovering deleted items

* A stateful eCommerce EC2 application has been deployed using Auto Scaling behind an Application Load Balancer. During scaling, customers are complaining about their shopping carts suddenly being empty.
What steps could be taken to ensure shopping cart data is retained during EC2 scaling events?
  - A. Change the Auto Scaling group to a steady state group (same minimum, maximum, desired instances) to match the peak traffic load.
  - B. Enable duration-based stickiness in the EC2 Target group to ensure all requests from the same client are routed to the same target.
  - C. Enable application-based stickiness in the EC2 Target group to ensure all requests from the same client are routed to the same target.
  - D. Migrate the shopping cart data to a DynamoDB table and reference the table instead of the local application cache.
 
  D - option A means not scale at all but cost lot more money. option B and C does not account for scaling in activities. only option D convert the app from statefull to stateless

* A web application is being architected for deployment into AWS. There is a requirement to implement throttling of web requests when the back end latency increases beyond a specific threshold, in order to ensure an optimal user experience.
Which of the below recommendations can meet this requirement?
  - A. Application Load Balancer listener rules
  - B. CloudFront distribution behaviors
  - C. WAF Web ACL rate-based rule
  - D. API Gateway usage plans
  - E. None of these

  E - option A for ALB listern rule does not reject requests. option B cloudfront can only do geo blocking. option C the throutling is configured as a static threshold, and not consider the latency of the backend. optioon D is also a static threshold but not rely on a metric
  

* To improve reliability, you've recommended deploying an application using immutable infrastructure.
Which of the following is NOT a benefit of deploying application infrastructure with immutability?
  - A. Increased security
  - B. Simplified deployments
  - C. Increased performance
  - D. Increased scalability

  C

* As part of a new AWS application architecture, you've been asked to recommend a solution that can balance the needs of two requirements:
1.Oracle Database software with minimum operational overhead
2.Access to the OS and software for regulatory purposes
Which recommendation will meet both requirements?
  - A. RDS with the Oracle Database engine selected
  - B. RDS with the Custom Engine selected
  - C. EC2 with a manual installation of Oracle Database
  - D. AWS does not have any service or feature that can meet both requirements
   
  B

* An loT application is deployed using AWS loT Core, using the MQTT protocol. There is a requirement to provide a single, static IP address, as DNS will not be used. The solution must emphasize high performance.
Which combination of steps will meet the requirements? (pick three)
  - A. Deploy an loT Interface endpoint in a VPC within the same region as the loT Core resource.
  - B. Deploy an NLB in the VPC using the loT Interface endpoint in a target group.
  - C. Deploy an ALB in the VPC using the loT Interface endpoint in a target group.
  - D. Configure a Route 53 Alias record for the loT Interface endpoint.
  - E. Deploy a CloudFront distribution with the ALB endpoint in an origin group.
  - F. Deploy a Global Accelerator with the LB IP address in an origin group.

  ABF - mqtt is not using http or https, this eliminate option C and E


* A workflow requires a Lambda function to be invoked as a target of an EventBridge rule, and will rarely be invoked more than once at the same time. The Lambda function must execute as quickly as possible as a highest priority, but also must optimize for cost.
What recommendations should be made to meet these requirements? (pick three)
  - A. Configure the Lambda function with more memory.
  - B. Configure the Lambda function with less memory.
  - C. Configure the Lambda function with Provisioned Concurrency at a low number.
  - D. Configure the Lambda function with a Provisioned Concurrency at a high number.
  - E. Configure the Lambda function with a timeout at the maximum execution time.
  - F. Configure the Lambda function with a timeout at 95th percentile of maximum execution time.

  ACF

* An application has a requirement for high storage IOPS, in order to accommodate parallel reads and writes of many small data files (approximately 4Kb each).
Which storage solution would NOT be appropriate for a direct performance test of the workload?
  - A. FSx for Lustre provisioned with default configuration
  - B. EBS Provisioned IOPS io2 Block Express volume
  - C. EC2 instance provisioned with an NVMe SSD Instance Store volume
  - D. EFS provisioned with General Purpose performance mode

  A. option A FSx is good with high perfoamcne with low latency, however, the default configuration will place all files below 1 Gb in the same stripe and reduce the overall perfoamcne capacity. option D general purpose performance mode has lower per-option latenct than max I/O

* A new streaming application has performance requirements for low latency and high pps (packets per second). The application is to be deployed into a VPC, using a Network Load Balancer as the front end and EC2 instances in a Cluster Placement Group.
What recommendation should be made to optimize network performance for the application?
  - A. Use a TCP listener on the Network Load Balancer.
  - B. Use a UDP listener on the Network Load Balancer.
  - C. Use a TLS listener on the Network Load Balancer.
  - D. Use a TCP_UDP listener on the Network Load Balancer.

  B. option A tcp session needs to do handshake, udp does not require handshake

* A company has the following AWS migration goals for compute resources:
1.Evolve technology when appropriate. 2.Start with familiar technology and learn from there. 3.Reduce KTLO (Keep The Lights On) activities over time. 4.Agility is top priority, followed by cost optimization.
Which of the following compute cost strategies would be appropriate for the company?
  - A. Purchase 1 year EC2 reservations.
  - B. Purchase 3 year EC2 reservations.
  - C. Purchase 1 year EC2 savings plans.
  - D. Purchase 1 year Compute savings plans.
  D - option ABC only limit to EC2, 3 years is too long. Compte savings plans covers EC2, Fargate and Lambda, but discount is less than EC2 savings plans less than EC2 reversations



* When designing a multi-tier application, all non-production environments must be architected for automatically scaling to zero after hours, on weekends, and on holidays or when otherwise not in use. The application tiers include a web proxy, application runtime, and a database.
Which of the following recommendations will meet the cost optimization requirement? (Pick three)
  - A.Application Load Balancer
  - B. EC2 with nginx using an Auto Scaling group
  - C. Application deployed on EC2 instance using Jenkins
  - D. Application deployed on EC2 using an Auto Scaling group
  - E. RDS database instance
  - F. Aurora Serverless

  BDF - option A has a min cost, option C will not able to scale to 0, option E can not scale to 0

* One of your memory-bound applications needs to have a performance baseline established. The application runs on EC2 instances, and you've been asked to design performance baseline tests that include memory usage metrics from the EC2 Operating System. The memory usage metrics will then be used to recommend the appropriate instance size to ensure cost efficient resource usage.
How can you ensure the memory metrics are collected?
  - A. Use the default CloudWatch metrics for EC2.
  - B. Enable detailed monitoring on the EC2 instance.
  - C. Install/configure the CloudWatch Agent on the EC2 instance.
  - D. Install/configure the CloudWatch Logs Agent on the EC2 instance.

  C

* After migrating an on-premises application to EC2, a company is planning the OS access as the fleet scales. The method must also scale with the number of instances and optimize for operational complexity.
Which recommendation would be appropriate to meet the requirement?
  - A. Use Security Group rules and SSH.
  - B. Use EC2 Serial Console.
  - C. Use Systems Manager Run Command.
  - D. Use Systems Manager Session Manager.

  C 

* Your company's application is using several custom AWS tags that your accounting department would like to generate reports from. The application runs in several AWS accounts that are all part of the same AWS Organization.
What action can ensure that the AWS tags are available as part of the AWS monthly bill?
  - A. Enable User-defined Cost Allocation Tags for each of the AWS tags in the AWS Organizations Management account.
  - B. Enable User-defined Cost Allocation Tags for each of the AWS tags in all of the AWS Organizations member accounts.
  - C. Enable AWS-managed Cost Allocation Tags for each of the AWS tags in the AWS Organizations Management account.
  - D. No action is required, all tags are automatically made available to the Cost Explorer tool and Cost and Usage Reports.

  A
 
* A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents. The average volume of data that the company collects from each site daily is 500 GB. Each site has a high-speed Internet connection.
The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket. The solution must minimize operational complexity.
Which solution meets these requirements?

  - A. Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.
  - B. Upload the data from each site to an S3 bucket in the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket. Then remove the data from the origin S3 bucket.
  - C. Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.
  - D. Upload the data from each site to an Amazon EC2 instance in the closest Region. Store the data in an Amazon Elastic Block Store (Amazon EBS) volume. At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket. Restore the EBS volume in that Region.

  A

* A company needs the ability to analyze the log files of its proprietary application. The logs are stored in JSON format in an Amazon S3 bucket. Queries will be simple and will run on-demand. A solutions architect needs to perform the analysis with minimal changes to the existing architecture.
What should the solutions architect do to meet these requirements with the LEAST amount of operational overhead?

  - A. Use Amazon Redshift to load all the content into one place and run the SQL queries as needed.
  - B. Use Amazon CloudWatch Logs to store the logs. Run SQL queries as needed from the Amazon CloudWatch console.
  - C. Use Amazon Athena directly with Amazon S3 to run the queries as needed. 
  - D. Use AWS Glue to catalog the logs. Use a transient Apache Spark cluster on Amazon EMR to run the SQL queries as needed.
 
  C

* A company uses AWS Organizations to manage multiple AWS accounts for different departments. The management account has an Amazon S3 bucket that contains project reports. The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations.
Which solution meets these requirements with the LEAST amount of operational overhead?

  - A. Add the aws PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.
  - B. Create an organizational unit (OU) for each department. Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy.
  - C. Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFromOrganization events. Update the S3 bucket policy accordingly.
  - D. Tag each user that needs access to the S3 bucket. Add the aws:PrincipalTag global condition key to the S3 bucket policy.
 
  A

* An application runs on an Amazon EC2 instance in a VPC. The application processes logs that are stored in an Amazon S3 bucket. The EC2 instance needs to access the S3 bucket without connectivity to the internet.
Which solution will provide private network connectivity to Amazon S3?

  - A. Create a gateway VPC endpoint to the S3 bucket.
  - B. Stream the logs to Amazon CloudWatch Logs. Export the logs to the S3 bucket.
  - C. Create an instance profile on Amazon EC2 to allow S3 access.
  - D. Create an Amazon API Gateway API with a private link to access the S3 endpoint.

  A

* A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume. For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer. After completing this change, users reported that, each time they refreshed the website, they could see one subset of their documents or the other, but never all of the documents at the same time.
What should a solutions architect propose to ensure users see all of their documents at once?

  - A. Copy the data so both EBS volumes contain all the documents
  - B. Configure the Application Load Balancer to direct a user to the server with the documents
  - C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS
  - D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server

  C

* A company uses NFS to store large video files in on-premises network attached storage. Each video file ranges in size from 1 MB to 500 GB. The total storage is 70 TB and is no longer growing. The company decides to migrate the video files to Amazon S3. The company must migrate the video files as soon as possible while using the least possible network bandwidth.
Which solution will meet these requirements?

  - A. Create an S3 bucket. Create an IAM role that has permissions to write to the S3 bucket. Use the AWS CLI to copy all files locally to the S3 bucket.
  - B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.
  - C. Deploy an S3 File Gateway on premises. Create a public service endpoint to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.
  - D. Set up an AWS Direct Connect connection between the on-premises network and AWS. Deploy an S3 File Gateway on premises. Create a public virtual interface (VIF) to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.

  B

* A company has an application that ingests incoming messages. Dozens of other applications and microservices then quickly consume these messages. The number of messages varies drastically and sometimes increases suddenly to 100,000 each second. The company wants to decouple the solution and increase scalability.
Which solution meets these requirements?

  - A. Persist the messages to Amazon Kinesis Data Analytics. Configure the consumer applications to read and process the messages.
  - B. Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group to scale the number of EC2 instances based on CPU metrics.
  - C. Write the messages to Amazon Kinesis Data Streams with a single shard. Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB. Configure the consumer applications to read from DynamoDB to process the messages.
  - D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SOS) subscriptions. Configure the consumer applications to process the messages from the queues.

  D
 
* A company is migrating a distributed application to AWS. The application serves variable workloads. The legacy platform consists of a primary server that coordinates jobs across multiple compute nodes. The company wants to modernize the application with a solution that maximizes resiliency and scalability.
How should a solutions architect design the architecture to meet these requirements?

  - A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.
  - B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.
  - C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.
  - D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes.
 
  B

* A company is running an SMB file server in its data center. The file server stores large files that are accessed frequently for the first few days after the files are created. After 7 days the files are rarely accessed.
The total data size is increasing and is close to the company's total storage capacity. A solutions architect must increase the company's available storage space without losing low-latency access to the most recently accessed files. The solutions architect must also provide file lifecycle management to avoid future storage issues.
Which solution will meet these requirements?

  - A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.
  - B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.
  - C. Create an Amazon FSx for Windows File Server file system to extend the company's storage space.
  - D. Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.
 
  B

* A company is building an ecommerce web application on AWS. The application sends information about new orders to an Amazon API Gateway REST API to process. The company wants to ensure that orders are processed in the order that they are received.
Which solution will meet these requirements?

  - A. Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order. Subscribe an AWS Lambda function to the topic to perform processing.
  - B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.
  - C. Use an API Gateway authorizer to block any requests while the application processes an order.
  - D. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order. Configure the SQS standard queue to invoke an AWS Lambda function for processing.

  B

* A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database. The EC2 instances connect to the database by using user names and passwords that are stored locally in a file. The company wants to minimize the operational overhead of credential management.
What should a solutions architect do to accomplish this goal?

  - A. Use AWS Secrets Manager. Turn on automatic rotation.
  - B. Use AWS Systems Manager Parameter Store. Turn on automatic rotation.
  - C. Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key. Migrate the credential file to the S3 bucket. Point the application to the S3 bucket.
  - D. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance. Attach the new EBS volume to each EC2 instance. Migrate the credential file to the new EBS volume. Point the application to the new EBS volume.

  A


* A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB). The web application has static data and dynamic data. The company stores its static data in an Amazon S3 bucket. The company wants to improve performance and reduce latency for the static data and dynamic data. The company is using its own domain name registered with Amazon Route 53.
What should a solutions architect do to meet these requirements?

  - A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.
  - B. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint Configure Route 53 to route traffic to the CloudFront distribution.
  - C. Create an Amazon CloudFront distribution that has the S3 bucket as an origin. Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints. Create a custom domain name that points to the accelerator DNS name. Use the custom domain name as an endpoint for the web application.
  - D. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Create two domain names. Point one domain name to the CloudFront DNS name for dynamic content. Point the other domain name to the accelerator DNS name for static content. Use the domain names as endpoints for the web application.
 
  A

* A company performs monthly maintenance on its AWS infrastructure. During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.
  - B. Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter. Use multi-Region secret replication for the required Regions. Configure Systems Manager to rotate the secrets on a schedule.
  - C. Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled. Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.
  - D. Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys. Store the secrets in an Amazon DynamoDB global table. Use an AWS Lambda function to retrieve the secrets from DynamoDB. Use the RDS API to rotate the secrets.

  A
  
* A company runs an ecommerce application on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. The Auto Scaling group scales based on CPU utilization metrics. The ecommerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance.
The database's performance degrades quickly as application load increases. The application handles more read requests than write transactions. The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability.
Which solution will meet these requirements?

  - A. Use Amazon Redshift with a single node for leader and compute functionality.
  - B. Use Amazon RDS with a Single-AZ deployment Configure Amazon RDS to add reader instances in a different Availability Zone.
  - C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.
  - D. Use Amazon ElastiCache for Memcached with EC2 Spot Instances.
 
  C

* A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC. The company had an inspection server in its on-premises data center. The inspection server performed specific operations such as traffic flow inspection and traffic filtering. The company wants to have the same functionalities in the AWS Cloud.
Which solution will meet these requirements?

  - A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.
  - B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.
  - C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC. 
  - D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC.
 
  C

* A company hosts a data lake on AWS. The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL. The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake. Only the company's management team should have full access to all the visualizations. The rest of the company should have only limited access.
Which solution will meet these requirements?

  - A. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate IAM roles.
  - B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups.
  - C. Create an AWS Glue table and crawler for the data in Amazon S3. Create an AWS Glue extract, transform, and load (ETL) job to produce reports. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.
  - D. Create an AWS Glue table and crawler for the data in Amazon S3. Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL. Generate reports by using Amazon Athena. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

  B


* A company is implementing a new business application. The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage. A solutions architect needs to ensure that the EC2 instances can access the S3 bucket.
What should the solutions architect do to meet this requirement?

  - A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.
  - B. Create an IAM policy that grants access to the S3 bucket. Attach the policy to the EC2 instances.
  - C. Create an IAM group that grants access to the S3 bucket. Attach the group to the EC2 instances.
  - D. Create an IAM user that grants access to the S3 bucket. Attach the user account to the EC2 instances.

  A

* An application development team is designing a microservice that will convert large images to smaller, compressed images. When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket.
A solutions architect needs to design a solution that uses durable, stateless components to process the images automatically.
Which combination of actions will meet these requirements? (Choose two.)

  - A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.
  - B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.
  - C. Configure the Lambda function to monitor the S3 bucket for new uploads. When an uploaded image is detected, write the file name to a text file in memory and use the text file to keep track of the images that were processed.
  - D. Launch an Amazon EC2 instance to monitor an Amazon Simple Queue Service (Amazon SQS) queue. When items are added to the queue, log the file name in a text file on the EC2 instance and invoke the Lambda function.
  - E. Configure an Amazon EventBridge (Amazon CloudWatch Events) event to monitor the S3 bucket. When an image is uploaded, send an alert to an Amazon ample Notification Service (Amazon SNS) topic with the application owner's email address for further processing.

  AB

* A company has a three-tier web application that is deployed on AWS. The web servers are deployed in a public subnet in a VPC. The application servers and database servers are deployed in private subnets in the same VPC. The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC. The appliance is configured with an IP interface that can accept IP packets.
A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.
  - B. Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.
  - C. Deploy a transit gateway in the inspection VPConfigure route tables to route the incoming packets through the transit gateway.
  - D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.

  D

* A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS Region. The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes. Modifications to the cloned data must not affect the production environment. The software that accesses this data requires consistently high I/O performance.
A solutions architect needs to minimize the time that is required to clone the production data into the test environment.
Which solution will meet these requirements?

  - A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.
  - B. Configure the production EBS volumes to use the EBS Multi-Attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.
  - C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.
  - D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.
 
  D

* An ecommerce company wants to launch a one-deal-a-day website on AWS. Each day will feature exactly one product on sale for a period of 24 hours. The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Use Amazon S3 to host the full website in different S3 buckets. Add Amazon CloudFront distributions. Set the S3 buckets as origins for the distributions. Store the order data in Amazon S3.
  - B. Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones. Add an Application Load Balancer (ALB) to distribute the website traffic. Add another ALB for the backend APIs. Store the data in Amazon RDS for MySQL.
  - C. Migrate the full application to run in containers. Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic. Store the data in Amazon RDS for MySQL.
  - D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.
 
  D

* A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application. The media files must be resilient to the loss of an Availability Zone. Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern. The solutions architect must minimize the costs of storing and retrieving the media files.
Which storage option meets these requirements?

  - A. S3 Standard
  - B. S3 Intelligent-Tiering
  - C. S3 Standard-Infrequent Access (S3 Standard-IA)
  - D. S3 One Zone-Infrequent Access (S3 One Zone-IA)
 
  B

* A company is storing backup files by using Amazon S3 Standard storage. The files are accessed frequently for 1 month. However, the files are not accessed after 1 month. The company must keep the files indefinitely.
Which storage solution will meet these requirements MOST cost-effectively?

  - A. Configure S3 Intelligent-Tiering to automatically migrate objects.
  - B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.
  - C. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month.
  - D. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month.
 
  B

* A company observes an increase in Amazon EC2 costs in its most recent bill. The billing team notices unwanted vertical scaling of instance types for a couple of EC2 instances. A solutions architect needs to create a graph comparing the last 2 months of EC2 costs and perform an in-depth analysis to identify the root cause of the vertical scaling.
How should the solutions architect generate the information with the LEAST operational overhead?

  - A. Use AWS Budgets to create a budget report and compare EC2 costs based on instance types.
  - B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.
  - C. Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last 2 months.
  - D. Use AWS Cost and Usage Reports to create a report and send it to an Amazon S3 bucket. Use Amazon QuickSight with Amazon S3 as a source to generate an interactive graph based on instance types.
 
  B

* A company is designing an application. The application uses an AWS Lambda function to receive information through Amazon API Gateway and to store the information in an Amazon Aurora PostgreSQL database.
During the proof-of-concept stage, the company has to increase the Lambda quotas significantly to handle the high volumes of data that the company needs to load into the database. A solutions architect must recommend a new design to improve scalability and minimize the configuration effort.
Which solution will meet these requirements?

  - A. Refactor the Lambda function code to Apache Tomcat code that runs on Amazon EC2 instances. Connect the database by using native Java Database Connectivity (JDBC) drivers.
  - B. Change the platform from Aurora to Amazon DynamoDB. Provision a DynamoDB Accelerator (DAX) cluster. Use the DAX client SDK to point the existing DynamoDB API calls at the DAX cluster.
  - C. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using Amazon Simple Notification Service (Amazon SNS).
  - D. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using an Amazon Simple Queue Service (Amazon SQS) queue.
 
  D
  
* A company needs to review its AWS Cloud deployment to ensure that its Amazon S3 buckets do not have unauthorized configuration changes.
What should a solutions architect do to accomplish this goal?

  - A. Turn on AWS Config with the appropriate rules.
  - B. Turn on AWS Trusted Advisor with the appropriate checks.
  - C. Turn on Amazon Inspector with the appropriate assessment template.
  - D. Turn on Amazon S3 server access logging. Configure Amazon EventBridge (Amazon Cloud Watch Events).

  A

* A company is launching a new application and will display application metrics on an Amazon CloudWatch dashboard. The company's product manager needs to access this dashboard periodically. The product manager does not have an AWS account. A solutions architect must provide access to the product manager by following the principle of least privilege.
Which solution will meet these requirements?

  - A. Share the dashboard from the CloudWatch console. Enter the product manager's email address, and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.
  - B. Create an IAM user specifically for the product manager. Attach the CloudWatchReadOnlyAccess AWS managed policy to the user. Share the new login credentials with the product manager. Share the browser URL of the correct dashboard with the product manager.
  - C. Create an IAM user for the company's employees. Attach the ViewOnlyAccess AWS managed policy to the IAM user. Share the new login credentials with the product manager. Ask the product manager to navigate to the CloudWatch console and locate the dashboard by name in the Dashboards section.
  - D. Deploy a bastion server in a public subnet. When the product manager requires access to the dashboard, start the server and share the RDP credentials. On the bastion server, ensure that the browser is configured to open the dashboard URL with cached AWS credentials that have appropriate permissions to view the dashboard.

  A

* A company is migrating applications to AWS. The applications are deployed in different accounts. The company manages the accounts centrally by using AWS Organizations. The company's security team needs a single sign-on (SSO) solution across all the company's accounts. The company must continue managing the users and groups in its on-premises self-managed Microsoft Active Directory.
Which solution will meet these requirements?

  - A. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a one-way forest trust or a one-way domain trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
  - B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
  - C. Use AWS Directory Service. Create a two-way trust relationship with the company's self-managed Microsoft Active Directory.
  - D. Deploy an identity provider (IdP) on premises. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console.
 
  B

* A company provides a Voice over Internet Protocol (VoIP) service that uses UDP connections. The service consists of Amazon EC2 instances that run in an Auto Scaling group. The company has deployments across multiple AWS Regions.
The company needs to route users to the Region with the lowest latency. The company also needs automated failover between Regions.
Which solution will meet these requirements?

  - A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.
  - B. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Use the ALB as an AWS Global Accelerator endpoint in each Region.
  - C. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 latency record that points to aliases for each NLB. Create an Amazon CloudFront distribution that uses the latency record as an origin.
  - D. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 weighted record that points to aliases for each ALB. Deploy an Amazon CloudFront distribution that uses the weighted record as an origin.

  A

* A development team runs monthly resource-intensive tests on its general purpose Amazon RDS for MySQL DB instance with Performance Insights enabled. The testing lasts for 48 hours once a month and is the only process that uses the database. The team wants to reduce the cost of running the tests without reducing the compute and memory attributes of the DB instance.
Which solution meets these requirements MOST cost-effectively?
  
  - A. Stop the DB instance when tests are completed. Restart the DB instance when required.
  - B. Use an Auto Scaling policy with the DB instance to automatically scale when tests are completed.
  - C. Create a snapshot when tests are completed. Terminate the DB instance and restore the snapshot when required.
  - D. Modify the DB instance to a low-capacity instance when tests are completed. Modify the DB instance again when required.
 
  C. option A stop the DB, but still pay for storage price which is more than snapshot on s3

* A company that hosts its web application on AWS wants to ensure all Amazon EC2 instances. Amazon RDS DB instances. and Amazon Redshift clusters are configured with tags. The company wants to minimize the effort of configuring and operating this check.
What should a solutions architect do to accomplish this?

  - A. Use AWS Config rules to define and detect resources that are not properly tagged.
  - B. Use Cost Explorer to display resources that are not properly tagged. Tag those resources manually.
  - C. Write API calls to check all resources for proper tag allocation. Periodically run the code on an EC2 instance.
  - D. Write API calls to check all resources for proper tag allocation. Schedule an AWS Lambda function through Amazon CloudWatch to periodically run the code.

  A

* A development team needs to host a website that will be accessed by other teams. The website contents consist of HTML, CSS, client-side JavaScript, and images.
Which method is the MOST cost-effective for hosting the website?

  - A. Containerize the website and host it in AWS Fargate.
  - B. Create an Amazon S3 bucket and host the website there.
  - C. Deploy a web server on an Amazon EC2 instance to host the website.
  - D. Configure an Application Load Balancer with an AWS Lambda target that uses the Express.js framework.
 
  B  


* A company runs an online marketplace web application on AWS. The application serves hundreds of thousands of users during peak hours. The company needs a scalable, near-real-time solution to share the details of millions of financial transactions with several other internal applications. Transactions also need to be processed to remove sensitive data before being stored in a document database for low-latency retrieval.
What should a solutions architect recommend to meet these requirements?

  - A. Store the transactions data into Amazon DynamoDB. Set up a rule in DynamoDB to remove sensitive data from every transaction upon write. Use DynamoDB Streams to share the transactions data with other applications.
  - B. Stream the transactions data into Amazon Kinesis Data Firehose to store data in Amazon DynamoDB and Amazon S3. Use AWS Lambda integration with Kinesis Data Firehose to remove sensitive data. Other applications can consume the data stored in Amazon S3.
  - C. Stream the transactions data into Amazon Kinesis Data Streams. Use AWS Lambda integration to remove sensitive data from every transaction and then store the transactions data in Amazon DynamoDB. Other applications can consume the transactions data off the Kinesis data stream.
  - D. Store the batched transactions data in Amazon S3 as files. Use AWS Lambda to process every file and remove sensitive data before updating the files in Amazon S3. The Lambda function then stores the data in Amazon DynamoDB. Other applications can consume transaction files stored in Amazon S3.

  C

* A company hosts its multi-tier applications on AWS. For compliance, governance, auditing, and security, the company must track configuration changes on its AWS resources and record a history of API calls made to these resources.
What should a solutions architect do to meet these requirements?

  - A. Use AWS CloudTrail to track configuration changes and AWS Config to record API calls.
  - B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls.
  - C. Use AWS Config to track configuration changes and Amazon CloudWatch to record API calls.
  - D. Use AWS CloudTrail to track configuration changes and Amazon CloudWatch to record API calls.

  B

* A company is preparing to launch a public-facing web application in the AWS Cloud. The architecture consists of Amazon EC2 instances within a VPC behind an Elastic Load Balancer (ELB). A third-party service is used for the DNS. The company's solutions architect must recommend a solution to detect and protect against large-scale DDoS attacks.
Which solution meets these requirements?

  - A. Enable Amazon GuardDuty on the account.
  - B. Enable Amazon Inspector on the EC2 instances.
  - C. Enable AWS Shield and assign Amazon Route 53 to it.
  - D. Enable AWS Shield Advanced and assign the ELB to it.
 
  D

 * A company is building an application in the AWS Cloud. The application will store data in Amazon S3 buckets in two AWS Regions. The company must use an AWS Key Management Service (AWS KMS) customer managed key to encrypt all data that is stored in the S3 buckets. The data in both S3 buckets must be encrypted and decrypted with the same KMS key. The data and the key must be stored in each of the two Regions.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Create an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
  - B. Create a customer managed multi-Region KMS key. Create an S3 bucket in each Region. Configure replication between the S3 buckets. Configure the application to use the KMS key with client-side encryption.
  - C. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
  - D. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with AWS KMS keys (SSE-KMS). Configure replication between the S3 buckets.

  B

* A company recently launched a variety of new workloads on Amazon EC2 instances in its AWS account. The company needs to create a strategy to access and administer the instances remotely and securely. The company needs to implement a repeatable process that works with native AWS services and follows the AWS Well-Architected Framework.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Use the EC2 serial console to directly access the terminal interface of each instance for administration.
  - B. Attach the appropriate IAM role to each existing instance and new instance. Use AWS Systems Manager Session Manager to establish a remote SSH session.
  - C. Create an administrative SSH key pair. Load the public key into each EC2 instance. Deploy a bastion host in a public subnet to provide a tunnel for administration of each instance.
  - D. Establish an AWS Site-to-Site VPN connection. Instruct administrators to use their local on-premises machines to connect directly to the instances by using SSH keys across the VPN tunnel.
 
  B

* A company is hosting a static website on Amazon S3 and is using Amazon Route 53 for DNS. The website is experiencing increased demand from around the world. The company must decrease latency for users who access the website.
Which solution meets these requirements MOST cost-effectively?

  - A. Replicate the S3 bucket that contains the website to all AWS Regions. Add Route 53 geolocation routing entries.
  - B. Provision accelerators in AWS Global Accelerator. Associate the supplied IP addresses with the S3 bucket. Edit the Route 53 entries to point to the IP addresses of the accelerators.
  - C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution.
  - D. Enable S3 Transfer Acceleration on the bucket. Edit the Route 53 entries to point to the new endpoint.
 
  C
  
* A company maintains a searchable repository of items on its website. The data is stored in an Amazon RDS for MySQL database table that contains more than 10 million rows. The database has 2 TB of General Purpose SSD storage. There are millions of updates against this data every day through the company's website.
The company has noticed that some insert operations are taking 10 seconds or longer. The company has determined that the database storage performance is the problem.
Which solution addresses this performance issue?

  - A. Change the storage type to Provisioned IOPS SSD.
  - B. Change the DB instance to a memory optimized instance class.
  - C. Change the DB instance to a burstable performance instance class.
  - D. Enable Multi-AZ RDS read replicas with MySQL native asynchronous replication.

  A

* A company has thousands of edge devices that collectively generate 1 TB of status alerts each day. Each alert is approximately 2 KB in size. A solutions architect needs to implement a solution to ingest and store the alerts for future analysis.
The company wants a highly available solution. However, the company needs to minimize costs and does not want to manage additional infrastructure. Additionally, the company wants to keep 14 days of data available for immediate analysis and archive any data older than 14 days.
What is the MOST operationally efficient solution that meets these requirements?

  - A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.
  - B. Launch Amazon EC2 instances across two Availability Zones and place them behind an Elastic Load Balancer to ingest the alerts. Create a script on the EC2 instances that will store the alerts in an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.
  - C. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster. Set up the Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster to take manual snapshots every day and delete data from the cluster that is older than 14 days.
  - D. Create an Amazon Simple Queue Service (Amazon SQS) standard queue to ingest the alerts, and set the message retention period to 14 days. Configure consumers to poll the SQS queue, check the age of the message, and analyze the message data as needed. If the message is 14 days old, the consumer should copy the message to an Amazon S3 bucket and delete the message from the SQS queue.
 
  A 

* A company's application integrates with multiple software-as-a-service (SaaS) sources for data collection. The company runs Amazon EC2 instances to receive the data and to upload the data to an Amazon S3 bucket for analysis. The same EC2 instance that receives and uploads the data also sends a notification to the user when an upload is complete. The company has noticed slow application performance and wants to improve the performance as much as possible.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Create an Auto Scaling group so that EC2 instances can scale out. Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.
  - B. Create an Amazon AppFlow flow to transfer data between each SaaS source and the S3 bucket. Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.
  - C. Create an Amazon EventBridge (Amazon CloudWatch Events) rule for each SaaS source to send output data. Configure the S3 bucket as the rule's target. Create a second EventBridge (Cloud Watch Events) rule to send events when the upload to the S3 bucket is complete. Configure an Amazon Simple Notification Service (Amazon SNS) topic as the second rule's target.
  - D. Create a Docker container to use instead of an EC2 instance. Host the containerized application on Amazon Elastic Container Service (Amazon ECS). Configure Amazon CloudWatch Container Insights to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.

  B

* A company runs a highly available image-processing application on Amazon EC2 instances in a single VPC. The EC2 instances run inside several subnets across multiple Availability Zones. The EC2 instances do not communicate with each other. However, the EC2 instances download images from Amazon S3 and upload images to Amazon S3 through a single NAT gateway. The company is concerned about data transfer charges.
What is the MOST cost-effective way for the company to avoid Regional data transfer charges?

  - A. Launch the NAT gateway in each Availability Zone.
  - B. Replace the NAT gateway with a NAT instance.
  - C. Deploy a gateway VPC endpoint for Amazon S3.
  - D. Provision an EC2 Dedicated Host to run the EC2 instances.
 
  C

* A company has an on-premises application that generates a large amount of time-sensitive data that is backed up to Amazon S3. The application has grown and there are user complaints about internet bandwidth limitations. A solutions architect needs to design a long-term solution that allows for both timely backups to Amazon S3 and with minimal impact on internet connectivity for internal users.
Which solution meets these requirements?

  - A. Establish AWS VPN connections and proxy all traffic through a VPC gateway endpoint.
  - B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection.
  - C. Order daily AWS Snowball devices. Load the data onto the Snowball devices and return the devices to AWS each day.
  - D. Submit a support ticket through the AWS Management Console. Request the removal of S3 service limits from the account.

  B

* A company has an Amazon S3 bucket that contains critical data. The company must protect the data from accidental deletion.
Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

  - A. Enable versioning on the S3 bucket.
  - B. Enable MFA Delete on the S3 bucket.
  - C. Create a bucket policy on the S3 bucket.
  - D. Enable default encryption on the S3 bucket.
  - E. Create a lifecycle policy for the objects in the S3 bucket.
 
  AB

* A company has a data ingestion workflow that consists of the following:
• An Amazon Simple Notification Service (Amazon SNS) topic for notifications about new data deliveries
• An AWS Lambda function to process the data and record metadata
The company observes that the ingestion workflow fails occasionally because of network connectivity issues. When such a failure occurs, the Lambda function does not ingest the corresponding data unless the company manually reruns the job.
Which combination of actions should a solutions architect take to ensure that the Lambda function ingests all data in the future? (Choose two.)

  - A. Deploy the Lambda function in multiple Availability Zones.
  - B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.
  - C. Increase the CPU and memory that are allocated to the Lambda function.
  - D. Increase provisioned throughput for the Lambda function.
  - E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.

  BE

* A company needs guaranteed Amazon EC2 capacity in three specific Availability Zones in a specific AWS Region for an upcoming event that will last 1 week.
What should the company do to guarantee the EC2 capacity?

  - A. Purchase Reserved Instances that specify the Region needed.
  - B. Create an On-Demand Capacity Reservation that specifies the Region needed.
  - C. Purchase Reserved Instances that specify the Region and three Availability Zones needed.
  - D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed.
 
  D

* A company has an application that provides marketing services to stores. The services are based on previous purchases by store customers. The stores upload transaction data to the company through SFTP, and the data is processed and analyzed to generate new marketing offers. Some of the files can exceed 200 GB in size.
Recently, the company discovered that some of the stores have uploaded files that contain personally identifiable information (PII) that should not have been included. The company wants administrators to be alerted if PII is shared again. The company also wants to automate remediation.
What should a solutions architect do to meet these requirements with the LEAST development effort?

  - A. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Inspector to scan the objects in the bucket. If objects contain PII, trigger an S3 Lifecycle policy to remove the objects that contain PII.
  - B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
  - C. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
  - D. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Email Service (Amazon SES) to trigger a notification to the administrators and trigger an S3 Lifecycle policy to remove the meats that contain PII.

  B

* A company's website uses an Amazon EC2 instance store for its catalog of items. The company wants to make sure that the catalog is highly available and that the catalog is stored in a durable location.
What should a solutions architect do to meet these requirements?

  - A. Move the catalog to Amazon ElastiCache for Redis.
  - B. Deploy a larger EC2 instance with a larger instance store.
  - C. Move the catalog from the instance store to Amazon S3 Glacier Deep Archive.
  - D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.

  D

* A company stores call transcript files on a monthly basis. Users access the files randomly within 1 year of the call, but users access the files infrequently after 1 year. The company wants to optimize its solution by giving users the ability to query and retrieve files that are less than 1-year-old as quickly as possible. A delay in retrieving older files is acceptable.
Which solution will meet these requirements MOST cost-effectively?

  * A. Store individual files with tags in Amazon S3 Glacier Instant Retrieval. Query the tags to retrieve the files from S3 Glacier Instant Retrieval.
  * B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 Lifecycle policies to move the files to S3 Glacier Flexible Retrieval after 1 year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select.
  * C. Store individual files with tags in Amazon S3 Standard storage. Store search metadata for each archive in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Instant Retrieval after 1 year. Query and retrieve the files by searching for metadata from Amazon S3.
  * D. Store individual files in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Deep Archive after 1 year. Store search metadata in Amazon RDS. Query the files from Amazon RDS. Retrieve the files from S3 Glacier Deep Archive.
 
  B

* A company has a production workload that runs on 1,000 Amazon EC2 Linux instances. The workload is powered by third-party software. The company needs to patch the third-party software on all EC2 instances as quickly as possible to remediate a critical security vulnerability.
What should a solutions architect do to meet these requirements?

  - A. Create an AWS Lambda function to apply the patch to all EC2 instances.
  - B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances.
  - C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances.
  - D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances.
 
  D. option B is a good solution, but patch manager does not support thrid party softewares

* A company is developing an application that provides order shipping statistics for retrieval by a REST API. The company wants to extract the shipping statistics, organize the data into an easy-to-read HTML format, and send the report to several email addresses at the same time every morning.
Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

  - A. Configure the application to send the data to Amazon Kinesis Data Firehose.
  - B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
  - C. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Glue job to query the application's API for the data.
  - D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.
  - E. Store the application data in Amazon S3. Create an Amazon Simple Notification Service (Amazon SNS) topic as an S3 event destination to send the report by email.
 
  BD

* A company wants to migrate its on-premises application to AWS. The application produces output files that vary in size from tens of gigabytes to hundreds of terabytes. The application data must be stored in a standard file system structure. The company wants a solution that scales automatically. is highly available, and requires minimum operational overhead.
Which solution will meet these requirements?

  - A. Migrate the application to run as containers on Amazon Elastic Container Service (Amazon ECS). Use Amazon S3 for storage.
  - B. Migrate the application to run as containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use Amazon Elastic Block Store (Amazon EBS) for storage.
  - C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.
  - D. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic Block Store (Amazon EBS) for storage.

  C

* A company needs to store its accounting records in Amazon S3. The records must be immediately accessible for 1 year and then must be archived for an additional 9 years. No one at the company, including administrative users and root users, can be able to delete the records during the entire 10-year period. The records must be stored with maximum resiliency.
Which solution will meet these requirements?

  - A. Store the records in S3 Glacier for the entire 10-year period. Use an access control policy to deny deletion of the records for a period of 10 years.
  - B. Store the records by using S3 Intelligent-Tiering. Use an IAM policy to deny deletion of the records. After 10 years, change the IAM policy to allow deletion.
  - C. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year. Use S3 Object Lock in compliance mode for a period of 10 years.
  - D. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 year. Use S3 Object Lock in governance mode for a period of 10 years.

  C

* A company runs multiple Windows workloads on AWS. The company's employees use Windows file shares that are hosted on two Amazon EC2 instances. The file shares synchronize data between themselves and maintain duplicate copies. The company wants a highly available and durable storage solution that preserves how users currently access the files.
What should a solutions architect do to meet these requirements?

  - A. Migrate all the data to Amazon S3. Set up IAM authentication for users to access files.
  - B. Set up an Amazon S3 File Gateway. Mount the S3 File Gateway on the existing EC2 instances.
  - C. Extend the file share environment to Amazon FSx for Windows File Server with a Multi-AZ configuration. Migrate all the data to FSx for Windows File Server.
  - D. Extend the file share environment to Amazon Elastic File System (Amazon EFS) with a Multi-AZ configuration. Migrate all the data to Amazon EFS.

  C
  
* A solutions architect is developing a VPC architecture that includes multiple subnets. The architecture will host applications that use Amazon EC2 instances and Amazon RDS DB instances. The architecture consists of six subnets in two Availability Zones. Each Availability Zone includes a public subnet, a private subnet, and a dedicated subnet for databases. Only EC2 instances that run in the private subnets can have access to the RDS databases.
Which solution will meet these requirements?

  - A. Create a new route table that excludes the route to the public subnets' CIDR blocks. Associate the route table with the database subnets.
  - B. Create a security group that denies inbound traffic from the security group that is assigned to instances in the public subnets. Attach the security group to the DB instances.
  - C. Create a security group that allows inbound traffic from the security group that is assigned to instances in the private subnets. Attach the security group to the DB instances.
  - D. Create a new peering connection between the public subnets and the private subnets. Create a different peering connection between the private subnets and the database subnets.
 
  C

* A company has registered its domain name with Amazon Route 53. The company uses Amazon API Gateway in the ca-central-1 Region as a public interface for its backend microservice APIs. Third-party services consume the APIs securely. The company wants to design its API Gateway URL with the company's domain name and corresponding certificate so that the third-party services can use HTTPS.
Which solution will meet these requirements?

  - A. Create stage variables in API Gateway with Name="Endpoint-URL" and Value="Company Domain Name" to overwrite the default URL. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM).
  - B. Create Route 53 DNS records with the company's domain name. Point the alias record to the Regional API Gateway stage endpoint. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region.
  - C. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the same Region. Attach the certificate to the API Gateway endpoint. Configure Route 53 to route traffic to the API Gateway endpoint.
  - D. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region. Attach the certificate to the API Gateway APIs. Create Route 53 DNS records with the company's domain name. Point an A record to the company's domain name.

  C, for a regional api gateway, cetts can live in the same region, only edge-optimised api and cloudfront need certs live in us-east-1

 * A company is running a popular social media website. The website gives users the ability to upload images to share with other users. The company wants to make sure that the images do not contain inappropriate content. The company needs a solution that minimizes development effort.
What should a solutions architect do to meet these requirements?

  - A. Use Amazon Comprehend to detect inappropriate content. Use human review for low-confidence predictions.
  - B. Use Amazon Rekognition to detect inappropriate content. Use human review for low-confidence predictions.
  - C. Use Amazon SageMaker to detect inappropriate content. Use ground truth to label low-confidence predictions.
  - D. Use AWS Fargate to deploy a custom machine learning model to detect inappropriate content. Use ground truth to label low-confidence predictions.

  B

* A company wants to run its critical applications in containers to meet requirements for scalability and availability. The company prefers to focus on maintenance of the critical applications. The company does not want to be responsible for provisioning and managing the underlying infrastructure that runs the containerized workload.
What should a solutions architect do to meet these requirements?

  - A. Use Amazon EC2 instances, and install Docker on the instances.
  - B. Use Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 worker nodes.
  - C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
  - D. Use Amazon EC2 instances from an Amazon Elastic Container Service (Amazon ECS)-optimized Amazon Machine Image (AMI).
 
  C

* A company hosts more than 300 global websites and applications. The company requires a platform to analyze more than 30 TB of clickstream data each day.
What should a solutions architect do to transmit and process the clickstream data?

  - A. Design an AWS Data Pipeline to archive the data to an Amazon S3 bucket and run an Amazon EMR cluster with the data to generate analytics.
  - B. Create an Auto Scaling group of Amazon EC2 instances to process the data and send it to an Amazon S3 data lake for Amazon Redshift to use for analysis.
  - C. Cache the data to Amazon CloudFront. Store the data in an Amazon S3 bucket. When an object is added to the S3 bucket. run an AWS Lambda function to process the data for analysis.
  - D. Collect the data from Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to transmit the data to an Amazon S3 data lake. Load the data in Amazon Redshift for analysis.

  D

* A company has a website hosted on AWS. The website is behind an Application Load Balancer (ALB) that is configured to handle HTTP and HTTPS separately. The company wants to forward all requests to the website so that the requests will use HTTPS.
What should a solutions architect do to meet this requirement?

  - A. Update the ALB's network ACL to accept only HTTPS traffic.
  - B. Create a rule that replaces the HTTP in the URL with HTTPS.
  - C. Create a listener rule on the ALB to redirect HTTP traffic to HTTPS.
  - D. Replace the ALB with a Network Load Balancer configured to use Server Name Indication (SNI).
 
  C

* A company is developing a two-tier web application on AWS. The company's developers have deployed the application on an Amazon EC2 instance that connects directly to a backend Amazon RDS database. The company must not hardcode database credentials in the application. The company must also implement a solution to automatically rotate the database credentials on a regular basis.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Store the database credentials in the instance metadata. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and instance metadata at the same time.
  - B. Store the database credentials in a configuration file in an encrypted Amazon S3 bucket. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and the credentials in the configuration file at the same time. Use S3 Versioning to ensure the ability to fall back to previous values.
  - C. Store the database credentials as a secret in AWS Secrets Manager. Turn on automatic rotation for the secret. Attach the required permission to the EC2 role to grant access to the secret.
  - D. Store the database credentials as encrypted parameters in AWS Systems Manager Parameter Store. Turn on automatic rotation for the encrypted parameters. Attach the required permission to the EC2 role to grant access to the encrypted parameters.

  C

* A company is deploying a new public web application to AWS. The application will run behind an Application Load Balancer (ALB). The application needs to be encrypted at the edge with an SSL/TLS certificate that is issued by an external certificate authority (CA). The certificate must be rotated each year before the certificate expires.
What should a solutions architect do to meet these requirements?

  - A. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
  - B. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Import the key material from the certificate. Apply the certificate to the ALUse the managed renewal feature to automatically rotate the certificate.
  - C. Use AWS Certificate Manager (ACM) Private Certificate Authority to issue an SSL/TLS certificate from the root CA. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
  - D. Use AWS Certificate Manager (ACM) to import an SSL/TLS certificate. Apply the certificate to the ALB. Use Amazon EventBridge (Amazon CloudWatch Events) to send a notification when the certificate is nearing expiration. Rotate the certificate manually.

  D

* A company runs its infrastructure on AWS and has a registered base of 700,000 users for its document management application. The company intends to create a product that converts large .pdf files to .jpg image files. The .pdf files average 5 MB in size. The company needs to store the original files and the converted files. A solutions architect must design a scalable solution to accommodate demand that will grow rapidly over time.
Which solution meets these requirements MOST cost-effectively?

  - A. Save the .pdf files to Amazon S3. Configure an S3 PUT event to invoke an AWS Lambda function to convert the files to .jpg format and store them back in Amazon S3.
  - B. Save the .pdf files to Amazon DynamoDUse the DynamoDB Streams feature to invoke an AWS Lambda function to convert the files to .jpg format and store them back in DynamoDB.
  - C. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic Block Store (Amazon EBS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the files to .jpg format. Save the .pdf files and the .jpg files in the EBS store.
  - D. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic File System (Amazon EFS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the file to .jpg format. Save the .pdf files and the .jpg files in the EFS store.

  A

* A company has more than 5 TB of file data on Windows file servers that run on premises. Users and applications interact with the data each day.
The company is moving its Windows workloads to AWS. As the company continues this process, the company requires access to AWS and on-premises file storage with minimum latency. The company needs a solution that minimizes operational overhead and requires no significant changes to the existing file access patterns. The company uses an AWS Site-to-Site VPN connection for connectivity to AWS.
What should a solutions architect do to meet these requirements?

  - A. Deploy and configure Amazon FSx for Windows File Server on AWS. Move the on-premises file data to FSx for Windows File Server. Reconfigure the workloads to use FSx for Windows File Server on AWS.
  - B. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to the S3 File Gateway. Reconfigure the on-premises workloads and the cloud workloads to use the S3 File Gateway.
  - C. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to Amazon S3. Reconfigure the workloads to use either Amazon S3 directly or the S3 File Gateway. depending on each workload's location.
  - D. Deploy and configure Amazon FSx for Windows File Server on AWS. Deploy and configure an Amazon FSx File Gateway on premises. Move the on-premises file data to the FSx File Gateway. Configure the cloud workloads to use FSx for Windows File Server on AWS. Configure the on-premises workloads to use the FSx File Gateway.

  D

* A hospital recently deployed a RESTful API with Amazon API Gateway and AWS Lambda. The hospital uses API Gateway and Lambda to upload reports that are in PDF format and JPEG format. The hospital needs to modify the Lambda code to identify protected health information (PHI) in the reports.
Which solution will meet these requirements with the LEAST operational overhead?

  - A. Use existing Python libraries to extract the text from the reports and to identify the PHI from the extracted text.
  - B. Use Amazon Textract to extract the text from the reports. Use Amazon SageMaker to identify the PHI from the extracted text.
  - C. Use Amazon Textract to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.
  - D. Use Amazon Rekognition to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

  C

* A company has an application that generates a large number of files, each approximately 5 MB in size. The files are stored in Amazon S3. Company policy requires the files to be stored for 4 years before they can be deleted. Immediate accessibility is always required as the files contain critical business data that is not easy to reproduce. The files are frequently accessed in the first 30 days of the object creation but are rarely accessed after the first 30 days.
Which storage solution is MOST cost-effective?

  - A. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Glacier 30 days from object creation. Delete the files 4 years after object creation.
  - B. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 30 days from object creation. Delete the files 4 years after object creation.
  - C. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Delete the files 4 years after object creation.
  - D. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Move the files to S3 Glacier 4 years after object creation.
 
* A company hosts an application on multiple Amazon EC2 instances. The application processes messages from an Amazon SQS queue, writes to an Amazon RDS table, and deletes the message from the queue. Occasional duplicate records are found in the RDS table. The SQS queue does not contain any duplicate messages.
What should a solutions architect do to ensure messages are being processed once only?

  - A. Use the CreateQueue API call to create a new queue.
  - B. Use the AddPermission API call to add appropriate permissions.
  - C. Use the ReceiveMessage API call to set an appropriate wait time.
  - D. Use the ChangeMessageVisibility API call to increase the visibility timeout.
 
  D

* A solutions architect is designing a new hybrid architecture to extend a company's on-premises infrastructure to AWS. The company requires a highly available connection with consistent low latency to an AWS Region. The company needs to minimize costs and is willing to accept slower traffic if the primary connection fails.
What should the solutions architect do to meet these requirements?

  * A. Provision an AWS Direct Connect connection to a Region. Provision a VPN connection as a backup if the primary Direct Connect connection fails.
  * B. Provision a VPN tunnel connection to a Region for private connectivity. Provision a second VPN tunnel for private connectivity and as a backup if the primary VPN connection fails.
  * C. Provision an AWS Direct Connect connection to a Region. Provision a second Direct Connect connection to the same Region as a backup if the primary Direct Connect connection fails.
  * D. Provision an AWS Direct Connect connection to a Region. Use the Direct Connect failover attribute from the AWS CLI to automatically create a backup connection if the primary Direct Connect connection fails.
 
  A

* A company is running a business-critical web application on Amazon EC2 instances behind an Application Load Balancer. The EC2 instances are in an Auto Scaling group. The application uses an Amazon Aurora PostgreSQL database that is deployed in a single Availability Zone. The company wants the application to be highly available with minimum downtime and minimum loss of data.
Which solution will meet these requirements with the LEAST operational effort?

  - A. Place the EC2 instances in different AWS Regions. Use Amazon Route 53 health checks to redirect traffic. Use Aurora PostgreSQL Cross-Region Replication.
  - B. Configure the Auto Scaling group to use multiple Availability Zones. Configure the database as Multi-AZ. Configure an Amazon RDS Proxy instance for the database.
  - C. Configure the Auto Scaling group to use one Availability Zone. Generate hourly snapshots of the database. Recover the database from the snapshots in the event of a failure.
  - D. Configure the Auto Scaling group to use multiple AWS Regions. Write the data from the application to Amazon S3. Use S3 Event Notifications to launch an AWS Lambda function to write the data to the database.

  B

* A company's HTTP application is behind a Network Load Balancer (NLB). The NLB's target group is configured to use an Amazon EC2 Auto Scaling group with multiple EC2 instances that run the web service.
The company notices that the NLB is not detecting HTTP errors for the application. These errors require a manual restart of the EC2 instances that run the web service. The company needs to improve the application's availability without writing custom scripts or code.
What should a solutions architect do to meet these requirements?

  - A. Enable HTTP health checks on the NLB, supplying the URL of the company's application.
  - B. Add a cron job to the EC2 instances to check the local application's logs once each minute. If HTTP errors are detected. the application will restart.
  - C. Replace the NLB with an Application Load Balancer. Enable HTTP health checks by supplying the URL of the company's application. Configure an Auto Scaling action to replace unhealthy instances.
  - D. Create an Amazon Cloud Watch alarm that monitors the UnhealthyHostCount metric for the NLB. Configure an Auto Scaling action to replace unhealthy instances when the alarm is in the ALARM state.
 
  C
  
