# AWS SAA-C03 GitHub Repo Tips and Original Practice Questions

Prepared on 2026-10-08.

This file collects recurring AWS Certified Solutions Architect - Associate (SAA-C03) topics and exam-style tips found across public GitHub study repos, then turns them into original practice questions. These are not exam dumps and are not copied from the repos.

## Sources Checked

- Official AWS exam guide: https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html
- Official AWS technologies and concepts: https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-technologies-concepts.html
- Official AWS in-scope services: https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-03-in-scope-services.html
- RonitSachdev/aws-saa-c03-guides: https://github.com/RonitSachdev/aws-saa-c03-guides
- ChathurangaVKD/AWS-Certified-Solutions-Architect-Associate-SAA-C03: https://github.com/ChathurangaVKD/AWS-Certified-Solutions-Architect-Associate-SAA-C03
- jordisantamaria/aws-solutions-architect-lab: https://github.com/jordisantamaria/aws-solutions-architect-lab
- sv222/AWS-Solutions-Architect-Associate-Exam-2026: https://github.com/sv222/AWS-Solutions-Architect-Associate-Exam-2026
- Also found during scan: https://github.com/yashsinghviwork/aws-saa-c03-notes
- Additional source added: https://github.com/LuaGR/aws-saa-c03-notes
- Additional source added: https://github.com/devesh-talreja/aws-saa-complete-cheatsheet
- Additional source added: https://github.com/yoneshmurugan/100-days-aws-SAA-C03
- Additional source added: https://github.com/dave-mccollough/aws-solution-architect-study-notes

## Community Sources Added

- Reddit pass report: https://www.reddit.com/r/AWSCertifications/comments/189qv7k/passed_saac03_exam_feedback/
- Reddit pass report and strategy: https://www.reddit.com/r/AWSCertifications/comments/1r73smm/cleared_aws_solutions_architect_associate_saac03/
- Reddit scenario elimination framework: https://www.reddit.com/r/AWSCertifications/comments/1s37w7o/a_framework_for_eliminating_wrong_answers_on/
- freeCodeCamp Forum 6-week study plan: https://forum.freecodecamp.org/t/how-i-structured-a-6-week-study-plan-for-aws-saa-c03-what-actually-moved-the-needle/799377
- DEV Community SAA-C03 guide: https://dev.to/datanestdigital/aws-sa-associate-study-guide-aws-solutions-architect-associate-exam-guide-saa-c03-2igc
- Tutorials Dojo SAA-C03 study path: https://tutorialsdojo.com/aws-certified-solutions-architect-associate-exam-saa-c03-study-path/
- BuildPlane SAA-C03 study guide: https://buildplane.ai/blog/aws-solutions-architect-associate-study-guide
- Certification Study Library SAA-C03 guide: https://cterpening.github.io/certification-study-library/guides/SAA-C03-aws-certified-solutions-architect-associate/
- FactualMinds SAA-C03 guide: https://main.d1m6t5n8e4tvia.amplifyapp.com/certifications/aws-solutions-architect-associate/
- CertCoach analysis of common AWS SAA failure patterns: https://certcoach.in/blog/100-aws-saa-fails-what-trips-people-up

Community source note: I skipped dump-style content and comments that advertise "real exam questions." The questions below are original practice questions derived from repeated concepts, not copied from community posts.

## Additional Research Pass Sources

- Official AWS SAA-C03 sample questions: https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Sample-Questions.pdf
- AWS Certification prep page / Skill Builder prep references: https://aws.amazon.com/certification/certification-prep/
- AWS Certified Solutions Architect Challenge resource hub: https://pages.awscloud.com/GLOBAL_TRAINCERT_takethechallenge_resourcehub.html
- freeCodeCamp / ExamPro SAA-C03 full course outline: https://www.youtube.com/watch?v=c3Cn4xYfxJY
- Kwizza SAA-C03 study guide: https://kwizza.ca/certifications/aws/solutions-architect-associate-saa-c03/study-guide
- CloudNinja SAA-C03 cheat sheet page: https://cloudninja.pro/cheat-sheets/solutions-architect
- Tech Exam Lexicon SAA-C03 cheat sheet: https://techexamlexicon.com/aws/saa-c03/cheat-sheet/
- Reddit 45-day plan and exam strategy: https://www.reddit.com/r/AWSCertifications/comments/1rn7z4g/aws_cloud_solutions_architect_associate_saac03_45/
- Reddit SAA-C03 resources thread: https://www.reddit.com/r/AWSCertifications/comments/1d5kkuw/aws_certified_solutions_architect_associate/

Second-pass source note: I used official AWS and community/training references to identify topics and traps. I did not copy third-party question text or answers into this file.

## Official Exam Anchor

The SAA-C03 exam is organized around four scored domains:

| Domain | Weight |
|---|---:|
| Design Secure Architectures | 30% |
| Design Resilient Architectures | 26% |
| Design High-Performing Architectures | 24% |
| Design Cost-Optimized Architectures | 20% |

Practical implication: security is the biggest domain, but the exam usually asks security through architecture scenarios: IAM, network controls, encryption, secure connectivity, data protection, and least-privilege design.

## High-Priority Topics Repeated Across Repos

Study these first because they appear repeatedly in GitHub topic indexes, decision trees, cheat sheets, and trap lists.

1. VPC networking
   - Public vs private subnets, route tables, Internet Gateway, NAT Gateway, egress-only Internet Gateway.
   - Security Groups vs NACLs.
   - VPC endpoints, especially Gateway Endpoints for S3 and DynamoDB.
   - VPC peering vs Transit Gateway vs PrivateLink.
   - Direct Connect vs Site-to-Site VPN.

2. IAM and security
   - IAM users, groups, roles, policies, resource policies, permission boundaries, SCPs.
   - Explicit deny wins over allow.
   - Use roles instead of long-term access keys.
   - Root user should not be used for daily tasks; enable MFA.
   - KMS vs CloudHSM vs Secrets Manager vs Systems Manager Parameter Store.

3. S3 and storage selection
   - S3 storage classes and lifecycle policies.
   - S3 versioning, Object Lock, replication, encryption, presigned URLs.
   - Object vs block vs file storage: S3 vs EBS vs EFS vs FSx.
   - Snow Family, DataSync, Transfer Family, and Storage Gateway for migration and hybrid storage.

4. Compute selection
   - EC2, Auto Scaling Groups, Lambda, Fargate, ECS, EKS, Elastic Beanstalk, Batch.
   - Lambda timeout and event-driven use cases.
   - Spot vs Reserved Instances vs Savings Plans vs On-Demand.
   - Placement groups for low latency, fault isolation, or distributed big-data workloads.

5. Databases
   - RDS Multi-AZ vs RDS Read Replicas.
   - Aurora, Aurora Serverless, Aurora Global Database.
   - DynamoDB capacity modes, DAX, Global Tables, Streams, GSI vs LSI.
   - ElastiCache Redis vs Memcached.
   - Redshift vs Athena vs DynamoDB vs RDS.

6. Application integration and decoupling
   - SQS Standard vs FIFO.
   - SNS fan-out, often paired with SQS for durable independent consumers.
   - EventBridge for event routing and schedules.
   - Step Functions for workflow orchestration.
   - Kinesis Data Streams vs Firehose.

7. Resilience and disaster recovery
   - Multi-AZ for availability inside a Region.
   - Multi-Region for Regional failure tolerance.
   - Backup and restore, pilot light, warm standby, active-active/multi-site.
   - Route 53 failover, health checks, cross-Region replication.

8. Performance and edge services
   - CloudFront vs Global Accelerator.
   - ALB vs NLB vs Gateway Load Balancer.
   - Caching layers: CloudFront, ElastiCache, DAX.
   - Read replicas for read scaling.

9. Monitoring and governance
   - CloudWatch for metrics/logs/alarms.
   - CloudTrail for API auditing.
   - AWS Config for configuration compliance.
   - Trusted Advisor, Compute Optimizer, Cost Explorer, Budgets, CUR.

10. Cost optimization
   - Match purchasing model to workload predictability.
   - Use lifecycle policies and right-sized storage tiers.
   - Prefer managed/serverless services when the question emphasizes low operational overhead.
   - Watch network transfer cost, NAT Gateway cost, cross-AZ traffic, and unused resources.

## Service Selection Tips

Use these as quick decision rules when reading scenario questions.

| If the question says... | Usually think... |
|---|---|
| Private subnet needs private S3 access | S3 Gateway VPC Endpoint |
| Block one malicious IP address | NACL or WAF, depending on layer; Security Groups cannot deny |
| URL path or host-based routing | ALB |
| Static IP for load balancer | NLB |
| Third-party firewall appliance | Gateway Load Balancer |
| Global HTTP caching | CloudFront |
| Global static IPs or non-HTTP acceleration | Global Accelerator |
| Read scaling for relational DB | Read Replica |
| Automatic failover for relational DB | Multi-AZ |
| Relational, high scale, MySQL/PostgreSQL compatible | Aurora |
| Key-value with very high scale and low latency | DynamoDB |
| DynamoDB microsecond reads | DAX |
| Data warehouse and BI analytics | Redshift |
| Ad-hoc SQL over S3 | Athena |
| Database migration | DMS, and SCT if changing engines |
| File migration to AWS | DataSync |
| Physical transfer for huge datasets | Snow Family |
| Durable async decoupling | SQS |
| Same event to multiple independent consumers | SNS plus SQS queues |
| Event routing or cron-like scheduled events | EventBridge |
| Multi-step workflow with retries/branching | Step Functions |
| Automatically rotate database credentials | Secrets Manager |
| Store low-cost config values | Systems Manager Parameter Store |
| Dedicated HSM or strict HSM control | CloudHSM |
| PII discovery in S3 | Macie |
| Threat detection from logs | GuardDuty |
| Vulnerability scanning | Inspector |
| Central security findings dashboard | Security Hub |

## Common Exam Traps To Memorize

- RDS Multi-AZ standby is for failover, not read traffic. Use read replicas for read scaling.
- Security Groups are stateful and allow-only. NACLs are stateless and can allow or deny.
- CNAME records are not used at the zone apex; Route 53 Alias records solve AWS-target apex routing.
- NAT Gateway can reach S3, but an S3 Gateway Endpoint is private and usually cheaper.
- Lambda is not for jobs longer than 15 minutes. Consider Fargate, Batch, ECS, or Step Functions.
- EFS is for Linux/NFS shared file storage. Windows shared file storage points to FSx for Windows.
- VPC peering is non-transitive. Use Transit Gateway for hub-and-spoke or many VPCs.
- Direct Connect is private connectivity but not encrypted by default. Use VPN over Direct Connect if encryption is required.
- Existing unencrypted RDS or EBS resources are not simply toggled into encryption; snapshot, copy/encrypt, then restore.
- DynamoDB LSI must be created with the table. GSI can be added later.
- SNS alone is not durable storage for offline consumers. Use SNS to SQS for durable fan-out.
- CloudFront is for HTTP/HTTPS caching. Global Accelerator is for static Anycast IPs and TCP/UDP acceleration.
- Parameter Store can store secrets, but Secrets Manager is the exam answer for built-in rotation.
- For "least operational overhead," prefer managed/serverless services when they satisfy requirements.
- For "most cost-effective," choose the cheapest option that still meets every stated requirement.

## Original Practice Questions

### Domain 1: Design Secure Architectures

1. A company has an application running on EC2 instances in private subnets. The instances must download patches from Amazon S3 without sending traffic through the public internet or paying NAT Gateway data processing charges. What should a solutions architect choose?
   - A. Internet Gateway
   - B. NAT Gateway in each Availability Zone
   - C. S3 Gateway VPC Endpoint
   - D. Site-to-Site VPN

2. A security team needs to block traffic from a known malicious IP address before it reaches instances in a subnet. Which control is most appropriate?
   - A. Security Group inbound deny rule
   - B. Network ACL deny rule
   - C. IAM explicit deny
   - D. Route 53 failover policy

3. A mobile app allows users to sign in with a username and password, then upload files directly to an S3 bucket with temporary AWS credentials. Which Cognito combination fits best?
   - A. User Pool for authentication and Identity Pool for temporary AWS credentials
   - B. Identity Pool only
   - C. User Pool only
   - D. IAM users for each mobile user

4. A database password must rotate automatically every 30 days with minimal custom code. Which service should be used?
   - A. AWS KMS
   - B. AWS Secrets Manager
   - C. Systems Manager Parameter Store standard parameter
   - D. AWS CloudHSM

5. A workload has a compliance requirement for dedicated, single-tenant HSMs where the customer controls the HSM cluster. Which service matches this requirement?
   - A. AWS KMS AWS managed key
   - B. AWS KMS customer managed key
   - C. AWS CloudHSM
   - D. AWS Secrets Manager

6. A company wants to find sensitive personal data stored in S3 buckets. Which service should be selected?
   - A. Amazon GuardDuty
   - B. Amazon Macie
   - C. AWS Config
   - D. Amazon Inspector

7. A developer gives an EC2 application long-term IAM access keys for S3 access. What is the better architecture?
   - A. Store keys in user data
   - B. Store keys in an encrypted AMI
   - C. Attach an IAM role to the EC2 instance profile
   - D. Put access keys in Parameter Store without rotation

8. An organization wants to prevent member accounts from creating public S3 buckets, but still let account teams manage normal resources. What should be used?
   - A. AWS Organizations SCP
   - B. Security Group
   - C. Route 53 policy
   - D. EC2 placement group

### Domain 2: Design Resilient Architectures

9. A production web tier must survive an Availability Zone failure. Which design is best?
   - A. One EC2 instance in one public subnet
   - B. Multiple EC2 instances in one private subnet
   - C. Auto Scaling group across at least two AZs behind an ALB
   - D. One larger EC2 instance behind an Elastic IP

10. A relational database needs automatic failover inside the same Region. It does not need read scaling. What should be enabled?
   - A. RDS Multi-AZ
   - B. RDS Read Replica only
   - C. DynamoDB DAX
   - D. S3 Cross-Region Replication

11. A workload uses three VPCs today and may expand to dozens of VPCs plus on-premises connectivity. Transitive routing is required. Which service fits best?
   - A. VPC Peering mesh
   - B. Transit Gateway
   - C. Gateway VPC Endpoint
   - D. Egress-only Internet Gateway

12. A company wants active-passive disaster recovery for a web application. DNS should route to the secondary site only when the primary fails health checks. What should be used?
   - A. Route 53 failover routing
   - B. Route 53 weighted routing only
   - C. CloudFront signed URLs
   - D. VPC Flow Logs

13. A team wants the same order event to be processed independently by billing, fulfillment, and analytics. A failure in analytics must not block billing. Which design is best?
   - A. One SQS queue with all three consumers
   - B. SNS topic fan-out to three SQS queues
   - C. One Lambda function that calls three services synchronously
   - D. Kinesis Firehose directly to all services

14. An application has temporary spikes and needs to process jobs asynchronously. Failed messages should be isolated after repeated attempts. Which services and feature should be used?
   - A. SQS with a dead-letter queue
   - B. SNS without subscriptions
   - C. CloudTrail with Insights
   - D. Route 53 multivalue answer routing

15. A company needs a disaster recovery strategy with the lowest cost and can tolerate hours of RTO and RPO. Which strategy fits?
   - A. Multi-site active-active
   - B. Warm standby
   - C. Pilot light
   - D. Backup and restore

16. An application must withstand a Regional outage for a relational database and has aggressive RPO/RTO goals. Which database feature is most suitable?
   - A. Aurora Global Database
   - B. RDS Single-AZ
   - C. ElastiCache Memcached
   - D. EBS Multi-Attach

### Domain 3: Design High-Performing Architectures

17. A global website serves mostly static images and JavaScript files. Users need low-latency downloads worldwide. What should be used?
   - A. CloudFront
   - B. Direct Connect
   - C. NAT Gateway
   - D. AWS Batch

18. A gaming application uses UDP and requires two static global IP addresses for allow-listing by corporate customers. Which service is best?
   - A. CloudFront
   - B. Global Accelerator
   - C. Route 53 CNAME
   - D. S3 Transfer Acceleration

19. A web application needs URL path-based routing to different target groups. Which load balancer should be used?
   - A. Application Load Balancer
   - B. Network Load Balancer
   - C. Gateway Load Balancer
   - D. Classic Load Balancer

20. A TCP service needs ultra-low latency and static IP addresses. Which load balancer is best?
   - A. Application Load Balancer
   - B. Network Load Balancer
   - C. Gateway Load Balancer
   - D. CloudFront distribution

21. A DynamoDB application is read-heavy and needs microsecond read latency for eventually consistent reads. What should be added?
   - A. DAX
   - B. RDS Read Replica
   - C. ElastiCache Memcached in front of RDS
   - D. Redshift Spectrum

22. A relational application has heavy read traffic for reporting. It does not need faster failover. What should be added?
   - A. Multi-AZ standby
   - B. Read Replica
   - C. S3 Object Lock
   - D. Gateway Load Balancer

23. A company wants to run SQL queries directly against data stored in S3 for occasional ad-hoc analysis without managing servers. What should be used?
   - A. Athena
   - B. Redshift provisioned cluster
   - C. RDS MySQL
   - D. DynamoDB

24. A team needs to process clickstream events in real time with custom consumer applications and replay data for multiple consumers. Which service is most appropriate?
   - A. Kinesis Data Streams
   - B. Kinesis Data Firehose only
   - C. AWS DMS
   - D. AWS Transfer Family

25. A Lambda function is used for a video encoding job that runs for 25 minutes. What is the best change?
   - A. Increase Lambda timeout to 25 minutes
   - B. Use Fargate, ECS, or AWS Batch for the long-running job
   - C. Put the job behind CloudFront
   - D. Use S3 Intelligent-Tiering

26. A high-performance computing workload needs very low network latency between EC2 instances. Which placement group is best?
   - A. Cluster placement group
   - B. Spread placement group
   - C. Partition placement group
   - D. No placement group

### Domain 4: Design Cost-Optimized Architectures

27. A batch workload can be interrupted and restarted. The company wants the lowest EC2 compute cost. Which pricing option fits best?
   - A. On-Demand Instances
   - B. Spot Instances
   - C. Dedicated Hosts
   - D. Capacity Reservations only

28. A production application runs continuously with predictable compute usage for the next 3 years. The company wants discount flexibility across instance families. Which option fits best?
   - A. Savings Plans
   - B. Spot Instances
   - C. On-Demand only
   - D. Instance Store

29. An S3 dataset is frequently accessed for 20 days, then rarely accessed for years. Which cost feature should be used?
   - A. S3 lifecycle policy
   - B. EBS snapshot archive only
   - C. Route 53 weighted routing
   - D. IAM permission boundary

30. A private subnet sends high volumes of traffic to S3 through a NAT Gateway. The architecture works but costs are high. What change should be made?
   - A. Add an S3 Gateway VPC Endpoint
   - B. Add more NAT Gateways in the same AZ
   - C. Replace S3 with EBS
   - D. Use an Internet Gateway directly from the private subnet

31. A company has unpredictable DynamoDB traffic and does not want capacity planning. Which mode should it choose?
   - A. Provisioned capacity with no auto scaling
   - B. On-demand capacity
   - C. RDS Reserved Instance
   - D. Redshift Serverless

32. A development RDS database is not needed for several weeks. Which option can reduce cost while preserving data?
   - A. Stop the DB indefinitely
   - B. Snapshot the DB and delete the running instance, then restore later
   - C. Add a read replica
   - D. Change to Multi-AZ

33. A company needs shared Linux file storage that automatically scales and avoids managing file servers. Which service should it use?
   - A. EFS
   - B. EBS gp3
   - C. FSx for Windows File Server
   - D. Instance Store

34. A company needs a Windows SMB file share integrated with Active Directory. Which service should it use?
   - A. EFS
   - B. FSx for Windows File Server
   - C. S3 Glacier Deep Archive
   - D. EBS io2

35. A company has 500 TB of data and a very slow internet connection. It must move the data to AWS once. Which option is most suitable?
   - A. AWS Snowball Edge
   - B. Kinesis Data Streams
   - C. CloudWatch Logs
   - D. Route 53 Resolver

36. A reporting workload queries petabytes of structured data repeatedly for BI dashboards. Which service is usually best?
   - A. Redshift
   - B. DynamoDB
   - C. SQS
   - D. CloudHSM

## Additional Original Questions From New GitHub Sources

37. A CloudFront distribution uses a custom domain name. The team created an ACM certificate in ap-south-1, but CloudFront cannot use it. What should they do?
   - A. Recreate the certificate in us-east-1
   - B. Recreate the certificate in every Region
   - C. Use an IAM access key instead
   - D. Move the S3 bucket to us-east-1

38. An EC2 Auto Scaling group should scale based on memory utilization. The alarm cannot find memory metrics. What is missing?
   - A. CloudTrail Insights
   - B. CloudWatch agent or custom metric
   - C. Route 53 health check
   - D. S3 server access logging

39. A company stores objects in S3 Standard-IA and deletes many objects after 10 days. Why are costs higher than expected?
   - A. Standard-IA has a minimum storage duration charge
   - B. Standard-IA cannot be deleted
   - C. Standard-IA is always more expensive than Standard
   - D. Standard-IA requires Provisioned IOPS

40. A team wants to use CloudFront to serve private S3 content without making the bucket public. Which configuration should it use?
   - A. Origin Access Control with an S3 bucket policy allowing CloudFront
   - B. Public-read ACLs on all objects
   - C. NAT Gateway in front of S3
   - D. VPC peering to S3

41. An application needs to process DynamoDB table changes with Lambda. Which feature should be enabled?
   - A. DynamoDB Streams
   - B. DAX
   - C. RDS Proxy
   - D. S3 Select

42. A company needs an immutable audit record of financial transactions with cryptographic verification. Which database service is most appropriate?
   - A. Amazon QLDB
   - B. Amazon Neptune
   - C. Amazon Timestream
   - D. Amazon ElastiCache Memcached

43. A workload requires full-text search and log analytics dashboards. Which service fits best?
   - A. Amazon OpenSearch Service
   - B. Amazon SQS
   - C. AWS Backup
   - D. AWS Artifact

44. A company must migrate an Oracle database to PostgreSQL with minimal downtime. Which tools should be used?
   - A. DMS plus Schema Conversion Tool
   - B. DataSync only
   - C. Snowball Edge only
   - D. S3 Transfer Acceleration only

45. An application uses RabbitMQ and the company wants a managed AWS migration path with minimal application changes. Which service should be considered?
   - A. Amazon MQ
   - B. Amazon SNS
   - C. Amazon EventBridge
   - D. AWS Step Functions

46. A user needs direct browser SSH-like access to EC2 instances without opening inbound port 22 or managing SSH keys. Which service helps?
   - A. Systems Manager Session Manager
   - B. AWS Shield
   - C. Amazon Macie
   - D. S3 Object Lambda

47. A company wants to query S3 data with SQL and only pay per query scanned. Which service is best?
   - A. Athena
   - B. Redshift provisioned cluster
   - C. RDS PostgreSQL
   - D. ElastiCache Redis

48. A data lake needs schema discovery and a central metadata catalog for Athena queries. Which AWS service should be used?
   - A. AWS Glue Crawler and Glue Data Catalog
   - B. AWS Batch
   - C. Amazon MQ
   - D. AWS Control Tower

49. An HTTP API needs a lower-cost, lower-latency API Gateway option and does not need all advanced REST API features. Which API type should be selected?
   - A. HTTP API
   - B. REST API only
   - C. WebSocket API
   - D. PrivateLink endpoint service

50. A Lambda deployment needs to shift 10% of traffic to a new immutable function version for validation. What should be used?
   - A. Lambda alias with weighted routing
   - B. Security Group weighted rules
   - C. S3 lifecycle transition
   - D. VPC Flow Logs

51. An EC2 public IP address changes after stop and start. The application needs a stable public IPv4 address. What should be used?
   - A. Elastic IP
   - B. Security Group
   - C. Instance Store
   - D. Placement Group

52. A big data workload such as Hadoop must spread instances across isolated racks while keeping groups of instances together. Which placement group fits?
   - A. Partition placement group
   - B. Cluster placement group
   - C. Spread placement group only
   - D. No placement group

53. A company needs to detect whether AWS resources drift from required configurations and compliance rules. Which service should it use?
   - A. AWS Config
   - B. CloudTrail only
   - C. CloudFront
   - D. Amazon Kinesis Video Streams

54. A microservices request path has latency across several services. The team needs distributed tracing. Which service should it use?
   - A. AWS X-Ray
   - B. AWS Budgets
   - C. Amazon Macie
   - D. Route 53 Resolver

55. A streaming pipeline should deliver data to S3 with minimal administration and optional Lambda transformation. Which service fits?
   - A. Kinesis Data Firehose
   - B. Kinesis Data Streams with custom consumers only
   - C. Amazon SQS FIFO
   - D. AWS DMS

56. A service must run long-running batch jobs and schedule compute automatically. Lambda duration limits are too short. Which service is most appropriate?
   - A. AWS Batch
   - B. CloudFront Functions
   - C. AWS WAF
   - D. Amazon Route 53

57. A company wants centralized account setup with landing zones, guardrails, and account vending. Which AWS service is designed for this?
   - A. AWS Control Tower
   - B. AWS Global Accelerator
   - C. Amazon Redshift
   - D. AWS DataSync

58. A mobile app has authenticated users who need temporary access to write only to their own S3 prefix. Which approach is best?
   - A. Cognito Identity Pool with IAM roles and scoped permissions
   - B. IAM user access keys embedded in the app
   - C. Public S3 bucket writes
   - D. CloudFront invalidation

59. A company has unpredictable S3 access patterns and wants automatic movement between access tiers without retrieval charges. Which storage class should be used?
   - A. S3 Intelligent-Tiering
   - B. S3 One Zone-IA
   - C. S3 Glacier Deep Archive
   - D. EBS st1

60. A company wants BI dashboards over curated analytics data. Which service is the AWS-native dashboard choice?
   - A. Amazon QuickSight
   - B. Amazon SQS
   - C. AWS KMS
   - D. Amazon Route 53

## Additional Original Questions From Reddit and Developer Community Sources

61. A financial company has one Direct Connect connection to AWS. It needs highly available, consistent private bandwidth if the first Direct Connect location fails. Which design best fits?
   - A. Add a second Direct Connect connection at a different Direct Connect location
   - B. Use one Site-to-Site VPN only
   - C. Add an Internet Gateway to the private subnet
   - D. Use S3 Transfer Acceleration

62. A data processing job runs only for the duration of a nightly EMR workload and should terminate after processing to avoid ongoing costs. Which EMR pattern fits?
   - A. Persistent EMR cluster running all day
   - B. Transient EMR cluster
   - C. RDS Multi-AZ deployment
   - D. CloudFront distribution

63. An HPC workload needs a high-performance file system for temporary scratch data. Long-term replication is not required. Which option fits?
   - A. FSx for Lustre Scratch
   - B. FSx for Windows File Server
   - C. EFS One Zone only
   - D. S3 Glacier Deep Archive

64. An HPC workload needs a durable high-performance file system that persists beyond the job and integrates with S3. Which option fits better than scratch?
   - A. FSx for Lustre Persistent
   - B. Instance Store only
   - C. SQS FIFO
   - D. Route 53 Resolver

65. A company has on-premises iSCSI storage and wants AWS-backed block storage integration for hybrid workloads. Which service family should be considered?
   - A. Storage Gateway Volume Gateway
   - B. AWS Glue Crawler
   - C. Amazon Cognito User Pool
   - D. Amazon SNS

66. A team needs to run Apache Spark analytics and wants managed cluster processing with control over frameworks. Which service is most appropriate?
   - A. Amazon EMR
   - B. AWS WAF
   - C. Amazon Macie
   - D. AWS Certificate Manager

67. A healthcare application needs to extract medical entities from clinical text. Which managed service is most specific?
   - A. Amazon Comprehend Medical
   - B. Amazon Transcribe
   - C. Amazon Polly
   - D. Amazon Translate

68. A call-center analytics workload must convert recorded audio calls to text. Which service should be selected?
   - A. Amazon Transcribe
   - B. Amazon Rekognition
   - C. Amazon Neptune
   - D. Amazon Timestream

69. A company wants to build custom machine-learning models with managed notebooks, training jobs, and deployment options. Which service fits?
   - A. Amazon SageMaker AI
   - B. AWS DataSync
   - C. Amazon MQ
   - D. AWS Shield Standard

70. A solutions architect sees two answer choices that both work. One is self-managed on EC2 and one is a managed AWS service. The question asks for the least operational overhead. Which direction is usually correct?
   - A. Prefer the managed AWS service if it satisfies all requirements
   - B. Prefer self-managed EC2 for more control
   - C. Prefer the option with the most services
   - D. Prefer the cheapest storage class regardless of requirements

71. A scenario asks for the most cost-effective solution, and two options meet all functional requirements. What should be compared next?
   - A. Total cost drivers such as idle capacity, data transfer, storage tier, and management overhead
   - B. Alphabetical order of AWS service names
   - C. Which service was launched most recently
   - D. Which option has the most components

72. A team repeatedly misses practice questions because they choose a service that works but ignores the stated qualifier. Which review method should help most?
   - A. Track misses by constraint such as cost, operational overhead, resilience, performance, and security
   - B. Memorize only service definitions
   - C. Skip explanations for correct answers
   - D. Avoid timed practice

73. A company needs to build a small hands-on project to understand SAA-style architecture decisions. Which project gives the best coverage?
   - A. Static site with S3, CloudFront, and OAC
   - B. One public EC2 instance with SSH open to the world
   - C. One unused EBS volume
   - D. A single IAM user with administrator access and no MFA

74. A company needs to practice private/public subnet design, outbound-only internet for private instances, and route-table behavior. Which hands-on lab fits?
   - A. Build a VPC with public/private subnets and NAT Gateway
   - B. Create a public S3 bucket
   - C. Create only a Route 53 TXT record
   - D. Enable CloudTrail only

75. A company wants to understand Auto Scaling and load balancer health checks. Which hands-on project is best?
   - A. Auto Scaling group behind an ALB, then deliberately break a health check
   - B. Create an IAM password policy only
   - C. Upload one file to Glacier Deep Archive
   - D. Create an SQS queue with no consumers

76. A question asks for security first. Which answer pattern is usually strongest?
   - A. Least privilege, private network path, encryption, auditability
   - B. Public access for easier debugging
   - C. Long-lived access keys in source code
   - D. Single-AZ design with no backups

77. A workload must ingest streaming events and query aggregated results in near real time. It does not need to keep a relational schema. Which family of services is likely relevant?
   - A. Kinesis, Lambda/Flink, DynamoDB or S3/Athena
   - B. CloudHSM and ACM only
   - C. Route 53 Registrar only
   - D. AWS Artifact only

78. A workload has global users, strict latency needs, and static assets. Which services commonly appear together?
   - A. CloudFront, S3, Route 53, ACM, WAF
   - B. SQS, Macie, Snowmobile, DMS
   - C. Direct Connect, EBS, QLDB, WorkSpaces
   - D. CloudTrail, Config, Artifact only

79. A question involves "close cousin" services. What is the best way to avoid the trap?
   - A. Identify the separating requirement, such as HA vs read scaling or queue vs pub/sub vs stream
   - B. Pick the service you have heard of most often
   - C. Pick the newest service
   - D. Pick the option with the longest description

80. A user must access AWS accounts through enterprise SSO and centralized workforce identity. Which service is usually relevant?
   - A. IAM Identity Center
   - B. Amazon SQS
   - C. Amazon FSx for Lustre
   - D. Amazon Athena

81. A company wants account vending and guardrails for new AWS accounts. Which combination is most relevant?
   - A. AWS Organizations and Control Tower
   - B. CloudFront and Global Accelerator
   - C. Amazon MQ and Amazon MSK
   - D. AWS Glue and Athena

82. A serverless web app needs authentication, an API endpoint, compute, and a NoSQL database. Which combination is common?
   - A. Cognito, API Gateway, Lambda, DynamoDB
   - B. Direct Connect, Snowball, FSx Windows, Redshift
   - C. NLB, EC2 Dedicated Hosts, CloudHSM only
   - D. Route 53 Resolver, EBS st1, Amazon MQ

83. A question is long and dense, and you are unsure after 90 seconds. What is the best exam tactic?
   - A. Make the best choice, flag it, move on, and return later
   - B. Spend 5-7 minutes until completely certain
   - C. Leave it blank
   - D. Restart the exam

84. A practice plan shows repeated misses in "most cost-effective" questions, not service-definition questions. What should you study next?
   - A. Pricing models, storage tier tradeoffs, NAT/data transfer cost, and managed/serverless cost patterns
   - B. Only AWS service launch dates
   - C. Only CLI syntax
   - D. Only programming language runtimes

## Additional Original Questions From Second Research Pass

85. EC2 instances in private subnets need to download operating system patches from the internet, but must not be reachable from the internet. Which two actions are required?
   - A. Create a NAT Gateway in a public subnet
   - B. Route private subnet internet-bound traffic to the NAT Gateway
   - C. Assign public IPs to the private instances
   - D. Route private subnet traffic directly to the Internet Gateway
   - E. Put a NAT Gateway in a private subnet

86. A cost-sensitive workload has long-running EC2 instances that are idle overnight but must resume quickly with in-memory application state. Which EC2 feature can help?
   - A. EC2 hibernation
   - B. EC2 Instance Store only
   - C. Gateway Load Balancer
   - D. S3 Object Lock

87. A failover design needs a detachable network interface that can move between EC2 instances in the same Availability Zone. Which component should be used?
   - A. Secondary ENI
   - B. Primary ENI
   - C. Internet Gateway
   - D. NAT Gateway

88. A browser-hosted JavaScript app loads content from an S3 bucket on a different domain and is blocked by the browser. What S3 feature should be configured?
   - A. CORS
   - B. Transfer Acceleration
   - C. Object Lock
   - D. Requester Pays

89. A web app has a known daily traffic spike. EC2 instances take about one minute to initialize before serving requests. Which Auto Scaling feature helps avoid slow response during scale-out?
   - A. Instance warmup or warm pools with a suitable scaling policy
   - B. S3 Glacier Deep Archive
   - C. CloudTrail data events
   - D. AWS Artifact

90. An Aurora application has write latency because reads are competing with writes. What should the architect do?
   - A. Add Aurora Replicas and use reader endpoints for reads
   - B. Read from the Multi-AZ standby
   - C. Use S3 Select
   - D. Add a NAT Gateway

91. A company wants GraphQL APIs with real-time subscriptions and offline/mobile-friendly synchronization. Which service is most appropriate?
   - A. AWS AppSync
   - B. Amazon MQ
   - C. AWS DMS
   - D. Amazon Athena

92. A business user wants to transfer SaaS application data into S3 and Redshift with minimal custom integration code. Which service fits?
   - A. Amazon AppFlow
   - B. AWS WAF
   - C. Amazon EFS
   - D. AWS CloudHSM

93. A data lake requires centralized permissions, fine-grained access, and governance over data in S3 used by analytics services. Which service is designed for this?
   - A. AWS Lake Formation
   - B. Amazon Route 53
   - C. EC2 Auto Scaling
   - D. AWS Shield Standard

94. A company needs centralized backup policies across EBS, RDS, DynamoDB, and EFS. Which service should be used?
   - A. AWS Backup
   - B. AWS DataSync
   - C. Amazon MQ
   - D. Amazon CloudFront

95. An organization wants approved, preconfigured infrastructure products that teams can launch self-service while staying governed. Which service fits?
   - A. AWS Service Catalog
   - B. AWS Global Accelerator
   - C. Amazon Kinesis Video Streams
   - D. Amazon Inspector

96. A compliance team needs downloadable AWS compliance reports and agreements. Which service is relevant?
   - A. AWS Artifact
   - B. Amazon Macie
   - C. Amazon Timestream
   - D. AWS Batch

97. A team needs account-specific alerts about AWS service events and planned maintenance that may affect their resources. Which service should they check?
   - A. AWS Health Dashboard
   - B. AWS WAF
   - C. Amazon S3 Select
   - D. Amazon Redshift Spectrum

98. A company uses Microsoft Active Directory and wants AWS-managed directory services for Windows workloads. Which service family should be reviewed?
   - A. AWS Directory Service
   - B. Amazon AppFlow
   - C. AWS Glue
   - D. Amazon Kinesis

99. A central security team wants to manage WAF rules across many accounts in AWS Organizations. Which service helps centrally manage this?
   - A. AWS Firewall Manager
   - B. AWS Lambda
   - C. Amazon SQS
   - D. AWS Elastic Beanstalk

100. A company needs to share subnets or other supported AWS resources across accounts in the same organization. Which service is most relevant?
   - A. AWS Resource Access Manager
   - B. AWS Artifact
   - C. Amazon Transcribe
   - D. AWS DMS

101. A company must route users based on geographic distance to resources and also bias traffic toward or away from a location. Which Route 53 routing policy fits?
   - A. Geoproximity
   - B. Geolocation
   - C. Simple
   - D. Multivalue answer

102. A company wants to route users based on their country or continent, not necessarily distance to an endpoint. Which Route 53 routing policy fits?
   - A. Geolocation
   - B. Geoproximity
   - C. Weighted
   - D. Simple

103. A team needs to distinguish an S3 object's name from its descriptive system/user-defined attributes. Which pair is correct?
   - A. Object key = object name/path; metadata = descriptive attributes
   - B. Object key = encryption key; metadata = bucket policy
   - C. Object key = version ID; metadata = lifecycle rule
   - D. Object key = IAM user; metadata = IAM role

104. A company wants to reduce KMS request costs for very high-volume S3 SSE-KMS workloads. Which S3 feature can help?
   - A. S3 Bucket Keys
   - B. S3 Object Lock
   - C. S3 Requester Pays
   - D. S3 CORS

105. A workload needs an in-memory Redis-compatible database with durability, not just a cache. Which service should be considered?
   - A. Amazon MemoryDB
   - B. Amazon ElastiCache Memcached
   - C. Amazon Athena
   - D. Amazon Macie

106. An event-driven architecture uses Kafka APIs and the team wants a managed Apache Kafka service. Which service should be selected?
   - A. Amazon MSK
   - B. Amazon MQ
   - C. Amazon SNS
   - D. AWS Step Functions

107. A company wants to send data from a stream directly into S3, Redshift, or OpenSearch with minimal custom consumer management. Which service should be used?
   - A. Kinesis Data Firehose
   - B. Kinesis Data Streams only
   - C. Amazon SQS FIFO
   - D. Amazon Cognito

108. A company needs to scan EC2 instances, Lambda functions, and container images for software vulnerabilities. Which service fits?
   - A. Amazon Inspector
   - B. Amazon GuardDuty
   - C. Amazon Macie
   - D. AWS Config

109. A company wants to investigate the root cause and relationship graph of security findings after GuardDuty alerts. Which service helps with investigation?
   - A. Amazon Detective
   - B. AWS Backup
   - C. AWS Service Catalog
   - D. Amazon Polly

110. A web application requires protection against SQL injection, XSS, geo restrictions, and rate-based blocking. Which service should be used?
   - A. AWS WAF
   - B. AWS Shield Standard only
   - C. AWS Artifact
   - D. Amazon Route 53 Resolver

## Answer Key With Explanations

1. C. Use an S3 Gateway Endpoint for private, route-table based access from VPC to S3.
2. B. NACLs can explicitly deny traffic at the subnet level. Security Groups cannot deny.
3. A. User Pools authenticate users; Identity Pools exchange identities for temporary AWS credentials.
4. B. Secrets Manager supports managed secret storage and automatic rotation workflows.
5. C. CloudHSM provides dedicated HSM control for strict compliance requirements.
6. B. Macie discovers sensitive data such as PII in S3.
7. C. Instance profiles with IAM roles avoid long-term credentials on EC2.
8. A. SCPs set account or OU guardrails in AWS Organizations.
9. C. Multi-AZ Auto Scaling behind an ALB removes single-AZ dependence.
10. A. Multi-AZ is for high availability and automatic failover.
11. B. Transit Gateway supports transitive hub-and-spoke connectivity at scale.
12. A. Route 53 failover routing uses health checks for active-passive DNS.
13. B. SNS to separate SQS queues gives each consumer durable, independent processing.
14. A. SQS decouples work; DLQs isolate messages that repeatedly fail processing.
15. D. Backup and restore is usually lowest cost but has slower recovery.
16. A. Aurora Global Database is designed for low-latency cross-Region relational replication and fast recovery.
17. A. CloudFront caches HTTP/HTTPS content at edge locations.
18. B. Global Accelerator supports static Anycast IPs and TCP/UDP acceleration.
19. A. ALB supports layer 7 routing such as path and host rules.
20. B. NLB is layer 4, supports static IPs, and is built for very low latency.
21. A. DAX is a DynamoDB-specific cache for microsecond read latency.
22. B. Read Replicas scale reads; Multi-AZ is mainly for failover.
23. A. Athena is serverless SQL over S3.
24. A. Kinesis Data Streams supports real-time stream processing and multiple consumers.
25. B. Lambda has a 15-minute maximum duration; use a container or batch service for longer jobs.
26. A. Cluster placement groups are for low-latency, high-throughput EC2 networking.
27. B. Spot is the lowest-cost EC2 option for interruptible workloads.
28. A. Savings Plans are good for steady usage with flexibility.
29. A. Lifecycle policies move objects to lower-cost classes as access changes.
30. A. Gateway Endpoints avoid NAT for S3 and keep traffic private.
31. B. On-demand mode removes capacity planning for variable DynamoDB workloads.
32. B. RDS stop is temporary; for several weeks, snapshot and delete can reduce cost more.
33. A. EFS is managed, scalable Linux/NFS shared storage.
34. B. FSx for Windows File Server provides SMB and Active Directory integration.
35. A. Snowball Edge is built for large offline transfers when networks are slow.
36. A. Redshift is the standard AWS data warehouse choice for repeated BI analytics at scale.
37. A. CloudFront requires ACM certificates in us-east-1.
38. B. EC2 memory is not a default CloudWatch metric; publish it with the CloudWatch agent or a custom metric.
39. A. S3 IA classes have minimum storage duration charges.
40. A. Origin Access Control plus a restrictive bucket policy keeps the bucket private behind CloudFront.
41. A. DynamoDB Streams captures table changes for Lambda processing.
42. A. QLDB is the ledger database for immutable, verifiable history.
43. A. OpenSearch supports full-text search and log analytics dashboards.
44. A. DMS moves data; SCT converts schema for heterogeneous database migration.
45. A. Amazon MQ supports managed ActiveMQ/RabbitMQ-compatible migrations.
46. A. Session Manager gives shell access without inbound SSH ports or SSH keys.
47. A. Athena queries S3 directly and charges by data scanned.
48. A. Glue Crawlers discover schemas and populate the Glue Data Catalog.
49. A. API Gateway HTTP APIs are simpler, lower-latency, and lower-cost than REST APIs when advanced features are not needed.
50. A. Lambda aliases can route weighted traffic to immutable versions.
51. A. Elastic IP provides a stable public IPv4 address.
52. A. Partition placement groups are designed for distributed big-data workloads.
53. A. AWS Config tracks resource configuration and compliance against rules.
54. A. X-Ray provides distributed tracing across services.
55. A. Firehose is managed delivery to destinations such as S3, with optional transformation.
56. A. AWS Batch handles long-running batch jobs beyond Lambda limits.
57. A. Control Tower sets up and governs a multi-account landing zone.
58. A. Cognito Identity Pools provide temporary AWS credentials through IAM roles.
59. A. Intelligent-Tiering is for changing or unknown access patterns.
60. A. QuickSight is AWS's BI dashboard service.
61. A. Redundant Direct Connect at a different location protects against location failure while preserving private consistent bandwidth.
62. B. Transient EMR clusters run for the job and terminate to reduce cost.
63. A. FSx for Lustre Scratch is high performance for temporary workloads without replication needs.
64. A. FSx for Lustre Persistent is durable and better for long-lived HPC file systems.
65. A. Volume Gateway exposes iSCSI block storage backed by AWS storage.
66. A. EMR is the managed Hadoop/Spark big-data platform.
67. A. Comprehend Medical is specialized for medical text entities.
68. A. Transcribe converts speech/audio to text.
69. A. SageMaker AI is for managed custom ML model development and deployment.
70. A. "Least operational overhead" usually points to managed services when requirements are met.
71. A. Cost questions require comparing real cost drivers, not just whether a solution works.
72. A. Community pass reports repeatedly stress reviewing wrong answers by constraint and pattern.
73. A. S3 plus CloudFront plus OAC exercises static hosting, private origin access, edge, DNS/TLS concepts.
74. A. A VPC public/private subnet lab teaches routing, NAT, and subnet isolation.
75. A. ASG plus ALB health checks builds intuition for resilience and scaling.
76. A. Security architecture combines least privilege, private paths, encryption, and audit.
77. A. Streaming architectures commonly combine Kinesis with processing and serving/storage services.
78. A. Global static web patterns commonly combine S3, CloudFront, Route 53, ACM, and WAF.
79. A. The separating requirement is what distinguishes close-cousin services.
80. A. IAM Identity Center handles centralized workforce SSO across AWS accounts.
81. A. Organizations plus Control Tower handles account structure, landing zones, and guardrails.
82. A. Cognito, API Gateway, Lambda, and DynamoDB are a common serverless application stack.
83. A. Time management matters; flag and move prevents hard questions from consuming easy-question time.
84. A. Cost misses usually require studying pricing and cost drivers, not just service definitions.
85. A and B. NAT belongs in a public subnet and private route tables must send internet-bound traffic to it.
86. A. Hibernation saves instance memory to the EBS root volume for faster resume.
87. A. Secondary ENIs can be detached and attached to another instance in the same AZ.
88. A. CORS allows browser-based cross-origin access when configured correctly.
89. A. Warmup/warm pools reduce the impact of slow instance initialization during scale-out.
90. A. Aurora replicas and reader endpoints separate read traffic from writes.
91. A. AppSync is AWS's managed GraphQL service with real-time/mobile patterns.
92. A. AppFlow integrates SaaS applications with AWS data destinations.
93. A. Lake Formation provides data lake governance and fine-grained permissions.
94. A. AWS Backup centralizes backup policies across supported services.
95. A. Service Catalog provides governed self-service portfolios.
96. A. Artifact provides AWS compliance reports and agreements.
97. A. AWS Health Dashboard provides account-specific service health events.
98. A. Directory Service provides managed directory options for Microsoft AD integration.
99. A. Firewall Manager centrally manages WAF and related protections across accounts.
100. A. RAM shares supported AWS resources across accounts.
101. A. Geoproximity supports distance-based routing with bias.
102. A. Geolocation routes by user geography such as country or continent.
103. A. The key identifies the object; metadata stores descriptive attributes.
104. A. S3 Bucket Keys reduce AWS KMS request volume for SSE-KMS.
105. A. MemoryDB is Redis-compatible with durable storage.
106. A. MSK is the managed Apache Kafka service.
107. A. Firehose delivers streaming data to supported destinations with minimal management.
108. A. Inspector scans workloads for vulnerabilities.
109. A. Detective helps investigate and correlate security findings.
110. A. WAF protects web applications at layer 7.

## Final Review Order

1. Read the official AWS exam guide once to understand the domain weights and task statements.
2. Study VPC, IAM, S3, EC2/ASG/ELB, RDS/Aurora, DynamoDB, SQS/SNS/Kinesis, Route 53, CloudFront, and cost tools.
3. Memorize the traps section above.
4. Practice scenario questions by identifying the qualifier: most secure, most resilient, highest performance, lowest cost, least operational overhead.
5. Review missed questions by topic rather than by service name.

## Last-Minute Mental Model

The exam rarely asks "What does this service do?" in isolation. It usually asks "Given these constraints, which architecture best satisfies all requirements?" Read for constraints first: cost, latency, durability, RPO/RTO, operational overhead, security, scale, migration time, and protocol.
