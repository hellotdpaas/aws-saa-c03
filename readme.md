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


  