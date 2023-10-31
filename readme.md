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






  
