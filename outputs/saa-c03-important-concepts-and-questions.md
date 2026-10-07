# SAA-C03 Important Concepts and Repeated Questions

Prepared on 2026-10-08.

This file is the focused study map: repeated concepts, likely question patterns, and original questions created from the GitHub resources reviewed so far. The ranking is not an official AWS exam distribution. It is a practical priority estimate based on:

- Official AWS SAA-C03 domain weights.
- Repeated topic coverage across 9 public GitHub prep repositories.
- Repeated weak-area and final-review notes from repos where authors tracked practice performance or reported passing the exam.
- Exam-style traps and service-comparison patterns that appear across multiple repos.

No real exam questions or dumps were used.

## GitHub Resources Used

| Source | Why It Was Useful |
|---|---|
| https://github.com/RonitSachdev/aws-saa-c03-guides | Exam-focused topic guides, question patterns, traps, and pocket cards. |
| https://github.com/ChathurangaVKD/AWS-Certified-Solutions-Architect-Associate-SAA-C03 | Full module set, quick references, practice-review weak areas, and exam-day edge cases. |
| https://github.com/jordisantamaria/aws-solutions-architect-lab | Decision trees, cheat sheets, Terraform lab structure, and domain-aligned practice areas. |
| https://github.com/sv222/AWS-Solutions-Architect-Associate-Exam-2026 | Broad service reference guide and updated service list. |
| https://github.com/yashsinghviwork/aws-saa-c03-notes | Passed SAA-C03 in February 2026 with 823/1000; notes emphasize concepts and gotchas. |
| https://github.com/LuaGR/aws-saa-c03-notes | Passed SAA-C03 in January 2026 with 812/1000; structured notes across core domains. |
| https://github.com/devesh-talreja/aws-saa-complete-cheatsheet | Exam-focused, comparison-heavy cheat sheet used for revision before passing. |
| https://github.com/yoneshmurugan/100-days-aws-SAA-C03 | Passed SAA-C03 on February 14, 2026 with 823/1000; useful study-roadmap topic ordering. |
| https://github.com/dave-mccollough/aws-solution-architect-study-notes | Older but still useful SAA-C03 notes with IAM/S3/EC2/EBS/database exam tips. |

## Reddit and Developer Community Resources Added

| Source | Community Signal |
|---|---|
| https://www.reddit.com/r/AWSCertifications/comments/189qv7k/passed_saac03_exam_feedback/ | Pass report highlighting EMR transient clusters, FSx for Lustre, Global Accelerator vs Route 53, CloudHSM/KMS, OpenSearch, iSCSI hybrid storage, and selected ML services. |
| https://www.reddit.com/r/AWSCertifications/comments/1r73smm/cleared_aws_solutions_architect_associate_saac03/ | Pass report emphasizing scenario-based thinking, HA, VPC, IAM, storage choices, database tradeoffs, cost, Well-Architected, and hybrid connectivity. |
| https://www.reddit.com/r/AWSCertifications/comments/1s37w7o/a_framework_for_eliminating_wrong_answers_on/ | Strategy post focused on eliminating plausible-but-wrong answers by identifying the primary constraint. |
| https://forum.freecodecamp.org/t/how-i-structured-a-6-week-study-plan-for-aws-saa-c03-what-actually-moved-the-needle/799377 | Community study plan stressing official domain weights, hands-on labs, and wrong-answer review by constraint. |
| https://dev.to/datanestdigital/aws-sa-associate-study-guide-aws-solutions-architect-associate-exam-guide-saa-c03-2igc | Developer-community guide organized around the four official domains, service comparisons, Well-Architected, cost, security, and hands-on labs. |
| https://tutorialsdojo.com/aws-certified-solutions-architect-associate-exam-saa-c03-study-path/ | Community-favored prep path with common comparison topics such as DataSync vs Storage Gateway, RDS Multi-AZ vs read replicas, Direct Connect vs VPN, Config vs CloudTrail, SG vs NACL, and Route 53 routing policies. |
| https://buildplane.ai/blog/aws-solutions-architect-associate-study-guide | Developer tooling/community article emphasizing architecture judgment, decision matrices, and end-to-end diagrams. |
| https://cterpening.github.io/certification-study-library/guides/SAA-C03-aws-certified-solutions-architect-associate/ | Independent study guide mapping objectives to decisions, failure modes, constraints, and worked architecture thinking. |
| https://main.d1m6t5n8e4tvia.amplifyapp.com/certifications/aws-solutions-architect-associate/ | Architect-reviewed study guide listing domain topics and practical tradeoffs. |
| https://certcoach.in/blog/100-aws-saa-fails-what-trips-people-up | Analysis of common Reddit failure patterns: service definitions are not enough, timing matters, and close-cousin services cause misses. |

Community source note: I did not use dump-oriented content or comments that claim to provide real exam questions. The practice questions here are original and concept-based.

## Additional Extensive Research Pass Sources

| Source | Added Signal |
|---|---|
| https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Sample-Questions.pdf | Official AWS sample-question patterns: NAT Gateway route design, EC2 hibernation, movable secondary ENIs, S3 CORS, SQS decoupling, Auto Scaling warmup, and Aurora read endpoints. |
| https://aws.amazon.com/certification/certification-prep/ | Official AWS prep path and Skill Builder references; used as the clean anchor for practice-question style rather than third-party dumps. |
| https://pages.awscloud.com/GLOBAL_TRAINCERT_takethechallenge_resourcehub.html | AWS challenge/resource hub; used to reinforce official preparation order and exam-domain orientation. |
| https://www.youtube.com/watch?v=c3Cn4xYfxJY | FreeCodeCamp/ExamPro SAA-C03 course outline; useful for long-tail services that can appear at use-case level. |
| https://kwizza.ca/certifications/aws/solutions-architect-associate-saa-c03/study-guide | Domain-weighted study guidance and exam-day strategy: read qualifiers, eliminate hard-constraint violations, and study by missed explanations. |
| https://cloudninja.pro/cheat-sheets/solutions-architect | 2026 cheat-sheet coverage of comparison-heavy services, including warm pools, AppSync, AppFlow, Lake Formation, MemoryDB, MSK, Firewall Manager, RAM, and Directory Service. |
| https://techexamlexicon.com/aws/saa-c03/cheat-sheet/ | Updated quick reference for exam format, decision traps, architecture patterns, and high-yield close-cousin services. |
| https://www.reddit.com/r/AWSCertifications/comments/1rn7z4g/aws_cloud_solutions_architect_associate_saac03_45/ | 45-day Reddit plan emphasizing service cheat sheets, qualifiers, elimination, and practice-exam review. |
| https://www.reddit.com/r/AWSCertifications/comments/1d5kkuw/aws_certified_solutions_architect_associate/ | Reddit resource thread reinforcing official guide first, Skill Builder/sample questions, Tutorials Dojo-style practice, and reviewing incorrect/guessed answers. |

Second-pass source note: I used these to broaden coverage beyond the first GitHub/community pass. I still avoided dump-style pages, copied questions, and comments advertising "real exam questions."

## Repeat Signal Scan

I scanned local Markdown copies of the 9 repositories above for concept-family keywords. These counts are directional only because larger repos naturally produce more mentions.

| Concept Family | Repos Mentioning It | Markdown Files Hit | Keyword Mentions |
|---|---:|---:|---:|
| Compute / Scaling | 9/9 | 378 | 7973 |
| S3 / Storage | 9/9 | 312 | 7365 |
| Databases | 9/9 | 269 | 4959 |
| IAM / Security | 9/9 | 245 | 4391 |
| VPC / Networking | 9/9 | 242 | 4184 |
| Messaging / Integration | 9/9 | 199 | 3529 |
| DR / HA | 9/9 | 271 | 3246 |
| DNS / Edge | 9/9 | 212 | 2881 |
| Monitoring / Governance | 9/9 | 202 | 2870 |
| Analytics | 9/9 | 152 | 1876 |

## Study Priority Ranking

## Community Overlay: What Changed After Reddit and Dev Community Review

The community sources mostly confirmed the GitHub ranking, but added a few extra study angles:

1. Scenario constraints matter more than memorized definitions.
   - Repeated advice: identify whether the question is asking for lowest cost, least operational overhead, highest availability, fastest migration, strongest security, or best performance.
   - Practice pattern: for every wrong answer, write down the missed constraint.

2. Hands-on labs help with service selection.
   - High-value labs repeatedly mentioned: S3 static site with CloudFront and private origin access, VPC with public/private subnets and NAT, and Auto Scaling behind an ALB with health checks.

3. Close-cousin services are a major failure source.
   - RDS Multi-AZ vs Read Replica.
   - Security Group vs NACL.
   - Gateway Endpoint vs Interface Endpoint.
   - SQS vs SNS vs Kinesis.
   - CloudWatch vs CloudTrail vs Config.
   - Direct Connect vs Site-to-Site VPN.
   - CloudFront vs Global Accelerator vs Route 53.

4. Community pass reports add a few "do not ignore" services.
   - EMR transient clusters.
   - FSx for Lustre Scratch vs Persistent.
   - OpenSearch for analytics/search.
   - Storage Gateway Volume Gateway for iSCSI.
   - Comprehend Medical, Transcribe, and SageMaker AI at use-case level.
   - IAM Identity Center and multi-account governance.

5. Timing strategy is part of preparation.
   - 65 questions in 130 minutes means 2 minutes per question.
   - A practical rule from community failure analysis: if a question is not yielding around 90 seconds, pick the best option, flag it, move on, and revisit.

## Second-Pass Concept Additions

These concepts were added after the extra official/community review. They are not all equal weight, but they close gaps that repeatedly appear in official sample questions, cheat sheets, and community final-review notes.

1. Official AWS sample-question patterns to practice carefully.
   - NAT Gateway must be in a public subnet, and private subnet route tables need a default route to it for outbound internet access.
   - EC2 hibernation preserves in-memory state by writing memory to the EBS root volume before shutdown.
   - A secondary ENI can be moved to a standby instance in the same Availability Zone to preserve a private IP path.
   - S3 CORS is required for browser-based cross-origin requests.
   - Auto Scaling instance warmup or warm pools matter when bootstrap time causes poor scale-out behavior.
   - Aurora replicas and reader endpoints separate read traffic from write traffic.

2. Governance and organization services to know at use-case level.
   - AWS Backup centralizes backup policy across supported services.
   - Service Catalog provides governed self-service infrastructure products.
   - AWS Artifact provides compliance reports and agreements.
   - AWS Health Dashboard gives account-specific service health and planned-maintenance events.
   - AWS Resource Access Manager shares supported resources across accounts.
   - AWS Firewall Manager centrally manages WAF and related policies across AWS Organizations.

3. Data, analytics, and integration services that are easy to overlook.
   - AppSync = managed GraphQL APIs and real-time/mobile sync patterns.
   - AppFlow = SaaS-to-AWS data movement without building custom connectors.
   - Lake Formation = data lake governance and fine-grained access control.
   - Kinesis Data Firehose = managed delivery of streaming data to supported destinations.
   - Amazon MSK = managed Apache Kafka.
   - MemoryDB = Redis-compatible durable in-memory database, not just a cache.

4. Security service distinctions that show up in comparison-style questions.
   - Inspector = vulnerability scanning for workloads and container images.
   - Detective = investigation and relationship graphs after findings.
   - WAF = layer 7 web protection such as SQL injection, XSS, rate-based rules, and geo restrictions.
   - Directory Service = managed Microsoft AD and directory integration patterns.

5. More Route 53 and S3 traps.
   - Geolocation routes by country/continent/user geography.
   - Geoproximity routes by distance to resources and supports bias.
   - S3 object key is the object's name/path; metadata is descriptive system or user-defined attributes.
   - S3 Bucket Keys reduce KMS request volume and cost for heavy SSE-KMS workloads.

### Priority 1: VPC and Networking

Why it is high weight:
- Repeated across every source.
- Appears under security, resilience, performance, and cost.
- Many SAA questions are really networking questions hidden inside application scenarios.

Must know:
- Public vs private subnets, route tables, IGW, NAT Gateway, egress-only IGW.
- Security Groups vs NACLs.
- Gateway Endpoint vs Interface Endpoint.
- VPC Peering vs Transit Gateway vs PrivateLink.
- Direct Connect vs VPN, and VPN over Direct Connect for encryption.
- VPC Flow Logs and DNS settings for VPC peering.

Question patterns:
- "Private subnet needs S3 or DynamoDB without NAT cost" = Gateway Endpoint.
- "Block specific IP" = NACL or WAF depending on layer.
- "Many VPCs / transitive routing" = Transit Gateway.
- "Expose service privately to another account without full VPC access" = PrivateLink.
- "Dedicated predictable on-prem connectivity" = Direct Connect.

### Priority 2: S3 and Storage

Why it is high weight:
- Repeated across all repos, frequently linked to security, cost, performance, and migration.
- Common traps include lifecycle minimums, encryption, replication, access control, and performance optimization.

Must know:
- S3 Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant, Glacier Flexible, Glacier Deep Archive.
- Lifecycle policies and minimum storage duration.
- Versioning, Object Lock, replication, presigned URLs, bucket policies, Block Public Access.
- Multipart upload, Transfer Acceleration, byte-range fetches.
- EBS vs EFS vs FSx vs S3.
- Snow Family, DataSync, Transfer Family, Storage Gateway.

Question patterns:
- "Unknown access pattern" = S3 Intelligent-Tiering.
- "Critical infrequent access" = Standard-IA, not One Zone-IA.
- "WORM / compliance retention" = S3 Object Lock compliance mode.
- "Shared Linux file system" = EFS.
- "Shared Windows SMB with AD" = FSx for Windows.
- "Petabyte offline transfer" = Snow Family.

### Priority 3: IAM, Security, and Encryption

Why it is high weight:
- Officially the largest SAA-C03 domain is Design Secure Architectures.
- Repos repeat IAM, KMS, SCPs, Secrets Manager, WAF/Shield, Cognito, and security monitoring.

Must know:
- IAM users, groups, roles, policies, resource policies, permission boundaries, STS.
- Explicit deny beats allow; implicit deny is default.
- SCPs limit maximum permissions but do not grant access.
- KMS vs CloudHSM vs Secrets Manager vs Parameter Store.
- Cognito User Pools vs Identity Pools.
- WAF vs Shield vs Network Firewall vs NACL.
- GuardDuty, Macie, Inspector, Security Hub, CloudTrail, Config.

Question patterns:
- "Temporary AWS credentials for app users" = Cognito Identity Pools.
- "Application user sign-in" = Cognito User Pools.
- "Automatic DB credential rotation" = Secrets Manager.
- "Dedicated HSM / strict HSM control" = CloudHSM.
- "Find PII in S3" = Macie.
- "Detect threats from account/network logs" = GuardDuty.

### Priority 4: Databases and Caching

Why it is high weight:
- Every source stresses RDS Multi-AZ vs Read Replicas, Aurora, DynamoDB, and ElastiCache.
- Many scenario questions ask for performance, availability, or cost through a database choice.

Must know:
- RDS Multi-AZ vs Read Replicas.
- Aurora, Aurora Serverless, Aurora Global Database.
- DynamoDB capacity modes, partition/sort key, DAX, Streams, Global Tables, TTL, GSI vs LSI.
- ElastiCache Redis vs Memcached.
- Redshift vs Athena vs DynamoDB vs RDS.

Question patterns:
- "Relational HA in one Region" = RDS Multi-AZ or Aurora Multi-AZ.
- "Read scaling" = Read Replica.
- "Multi-Region relational low RPO/RTO" = Aurora Global Database.
- "NoSQL active-active multi-Region" = DynamoDB Global Tables.
- "DynamoDB microsecond reads" = DAX.
- "Simple multi-threaded cache" = Memcached; persistence/HA/complex types = Redis.

### Priority 5: EC2, Load Balancing, Auto Scaling, and Compute Choice

Why it is high weight:
- Compute appeared with the highest keyword count in the scan.
- Questions often test choosing between EC2, Lambda, Fargate, Batch, ECS/EKS, and Elastic Beanstalk.

Must know:
- EC2 instance families and pricing models.
- Auto Scaling policies: target tracking, step, scheduled, predictive.
- ALB vs NLB vs Gateway Load Balancer.
- Placement groups: cluster, spread, partition.
- Lambda limits, concurrency, versions/aliases.
- ECS/EKS/Fargate launch tradeoffs.
- Batch for long-running batch jobs.

Question patterns:
- "Path-based HTTP routing" = ALB.
- "Static IP / TCP / UDP / ultra-low latency" = NLB.
- "Traffic inspection appliance" = GWLB.
- "Interruptible batch / lowest cost" = Spot.
- "Stable 1-3 year workload" = Reserved Instances or Savings Plans.
- "Short event-driven task under 15 minutes" = Lambda.

### Priority 6: Messaging, Decoupling, and Event-Driven Architecture

Why it is high weight:
- Repeated heavily in question-pattern repos and course-note repos.
- SAA questions commonly hide decoupling requirements inside multi-service workflows.

Must know:
- SQS Standard vs FIFO.
- SNS pub/sub and SNS to SQS fan-out.
- EventBridge for event buses, SaaS events, scheduled events.
- Step Functions for orchestration, retries, branching.
- Kinesis Data Streams vs Firehose vs Managed Apache Flink.
- Amazon MQ for existing ActiveMQ/RabbitMQ protocols.

Question patterns:
- "Decouple producer and worker" = SQS.
- "Ordered exactly-once queue" = SQS FIFO.
- "Same message to multiple independent consumers" = SNS plus SQS queues.
- "Event routing rules / schedule" = EventBridge.
- "Workflow with state, retry, branching" = Step Functions.
- "Replayable real-time stream" = Kinesis Data Streams.

### Priority 7: DNS, Edge, and Global Traffic

Why it is high weight:
- CloudFront, Route 53, and Global Accelerator appear repeatedly in final-review and weak-area notes.
- These questions are comparison-heavy.

Must know:
- Route 53 routing policies: simple, weighted, latency, failover, geolocation, geoproximity, multivalue, IP-based.
- Alias vs CNAME, especially zone apex.
- CloudFront origins, OAC/OAI, cache behaviors, signed URLs/cookies, field-level encryption.
- CloudFront ACM certificate region is us-east-1.
- CloudFront vs Global Accelerator.

Question patterns:
- "Apex domain to ALB/CloudFront" = Alias record.
- "Active-passive DNS" = Route 53 failover.
- "A/B or canary DNS split" = weighted routing.
- "HTTP caching at edge" = CloudFront.
- "Static global IPs / TCP or UDP acceleration" = Global Accelerator.

### Priority 8: Monitoring, Audit, and Governance

Why it is high weight:
- Repeated as a confusion set: CloudWatch vs CloudTrail vs Config vs Trusted Advisor.
- Practice review sources flagged CloudWatch agent and default metrics as common misses.

Must know:
- CloudWatch metrics, logs, alarms, agent, dashboards.
- CloudTrail management events vs data events.
- AWS Config compliance and resource configuration history.
- X-Ray for distributed tracing.
- Trusted Advisor for best-practice checks.
- Systems Manager Session Manager, Run Command, Patch Manager, Parameter Store.

Question patterns:
- "CPU metrics and alarms" = CloudWatch.
- "Memory or disk utilization on EC2" = CloudWatch agent.
- "Who called what API" = CloudTrail.
- "Resource configuration changed or non-compliant" = Config.
- "Trace latency across microservices" = X-Ray.

### Priority 9: Cost Optimization

Why it is high weight:
- Official domain is 20%, and many wrong answers are working but too expensive.
- Repos repeatedly emphasize cost qualifiers.

Must know:
- Spot, Reserved Instances, Savings Plans, On-Demand.
- S3 lifecycle and storage classes.
- NAT Gateway cost vs Gateway Endpoint.
- Compute Optimizer, Cost Explorer, Budgets, CUR.
- Right sizing and managed/serverless tradeoffs.
- Cross-AZ, cross-Region, and internet egress costs.

Question patterns:
- "Can be interrupted" = Spot.
- "Stable 24/7 usage" = Reserved or Savings Plan.
- "Private S3 access at lower cost than NAT" = Gateway Endpoint.
- "Unpredictable DynamoDB traffic" = On-Demand capacity.
- "Unknown S3 access pattern" = Intelligent-Tiering.

### Priority 10: Disaster Recovery and High Availability

Why it is high weight:
- Official resilience domain is 26%.
- Repos consistently repeat Multi-AZ vs Multi-Region and RTO/RPO strategy mapping.

Must know:
- Backup and restore, pilot light, warm standby, active-active/multi-site.
- RTO vs RPO.
- Multi-AZ vs Multi-Region.
- Route 53 failover.
- Aurora Global Database and DynamoDB Global Tables.
- S3 CRR/SRR and backups.

Question patterns:
- "Cheapest DR, hours acceptable" = backup and restore.
- "Core components running, scale on disaster" = pilot light.
- "Scaled-down full environment" = warm standby.
- "Near-zero downtime" = active-active/multi-site.

## Highest-Yield Confusions

| Confusion | Short Rule |
|---|---|
| RDS Multi-AZ vs Read Replica | Multi-AZ = HA/failover; Read Replica = read scaling. |
| Security Group vs NACL | SG = stateful allow-only instance level; NACL = stateless allow/deny subnet level. |
| Gateway vs Interface Endpoint | Gateway = S3/DynamoDB route table, no hourly charge; Interface = ENI/PrivateLink for most services. |
| CloudFront vs Global Accelerator | CloudFront = HTTP caching/CDN; GA = static Anycast IPs and TCP/UDP acceleration. |
| SQS vs SNS vs Kinesis | Queue vs pub/sub notification vs replayable stream. |
| EventBridge vs Step Functions | Event routing vs workflow orchestration. |
| User Pool vs Identity Pool | Authentication vs temporary AWS credentials. |
| Secrets Manager vs Parameter Store | Built-in rotation vs cheaper configuration store. |
| KMS vs CloudHSM | Managed key service vs customer-controlled dedicated HSM. |
| Athena vs Redshift | Serverless ad-hoc SQL over S3 vs data warehouse for recurring analytics. |
| EMR vs Glue | EMR = managed big-data frameworks with more control; Glue = serverless ETL/catalog-heavy workflows. |
| FSx Lustre Scratch vs Persistent | Scratch = temporary high performance; Persistent = durable high-performance file system. |
| Transcribe vs Polly vs Translate | Speech-to-text vs text-to-speech vs language translation. |
| Comprehend Medical vs Comprehend | Medical/clinical text entities vs general NLP. |
| IAM Identity Center vs IAM Users | Centralized workforce SSO vs individual AWS identities/credentials. |
| Geolocation vs Geoproximity | Geolocation = user geography; Geoproximity = distance to resources with optional bias. |
| AppSync vs API Gateway | AppSync = GraphQL/data sync patterns; API Gateway = REST/HTTP/WebSocket API front door. |
| AppFlow vs DataSync | AppFlow = SaaS data integrations; DataSync = file/object transfer and migration. |
| Inspector vs GuardDuty vs Detective | Inspector scans vulnerabilities; GuardDuty detects threats; Detective investigates relationships. |
| MemoryDB vs ElastiCache Redis | MemoryDB = durable Redis-compatible database; ElastiCache Redis = cache/low-latency data store. |

## Original Questions By Repeating Concept

### VPC and Networking

1. An EC2 instance in a private subnet must access DynamoDB privately without NAT Gateway charges. What should be configured?
   - A. Interface endpoint for DynamoDB
   - B. Gateway endpoint for DynamoDB
   - C. Internet Gateway
   - D. VPC peering

2. Three VPCs are connected as A-B and B-C with VPC peering. Instances in A must communicate with instances in C. What should the architect do?
   - A. Add a route through VPC B
   - B. Enable transitive peering
   - C. Use Transit Gateway or create direct A-C peering
   - D. Add a NAT Gateway in VPC B

3. A company needs encrypted connectivity from on-premises to AWS immediately. Bandwidth is moderate and setup must finish this week. What is the best choice?
   - A. Direct Connect only
   - B. Site-to-Site VPN
   - C. Global Accelerator
   - D. VPC Gateway Endpoint

4. A SaaS provider wants customers to access one private service without peering entire VPCs or exposing the service publicly. Which pattern fits?
   - A. AWS PrivateLink with NLB and interface endpoints
   - B. Internet Gateway and public ALB
   - C. S3 Gateway Endpoint
   - D. Egress-only Internet Gateway

### S3 and Storage

5. A company stores critical monthly backups that are rarely accessed but must be immediately retrievable. Which S3 class fits best?
   - A. One Zone-IA
   - B. Glacier Deep Archive
   - C. Glacier Instant Retrieval
   - D. Instance Store

6. A regulatory archive must prevent even account administrators from deleting retained objects during the retention period. What should be used?
   - A. S3 Object Lock in compliance mode
   - B. S3 lifecycle expiration
   - C. S3 Transfer Acceleration
   - D. S3 Requester Pays

7. A 20 GB object must be uploaded to S3 reliably and quickly. What upload feature should be used?
   - A. Single PUT request
   - B. Multipart upload
   - C. S3 Select
   - D. CloudWatch agent

8. An on-premises NFS file share must be migrated to S3 while preserving metadata and automating transfer. Which service is best?
   - A. AWS DataSync
   - B. AWS DMS
   - C. Amazon MQ
   - D. AWS Config

### IAM and Security

9. A developer has an IAM allow policy for s3:GetObject, but another applicable policy explicitly denies s3:GetObject. What is the result?
   - A. Allow, because identity policy wins
   - B. Allow, because S3 is global
   - C. Deny, because explicit deny wins
   - D. Allow only from the console

10. A company wants to restrict all member accounts from launching resources outside approved Regions. What should it use?
   - A. SCP in AWS Organizations
   - B. Security Group
   - C. CloudFront cache behavior
   - D. EBS encryption

11. A CloudFront distribution needs an ACM certificate for a custom domain. In which Region must the certificate be created?
   - A. Same Region as the S3 bucket
   - B. Same Region as the ALB
   - C. us-east-1
   - D. Any Region

12. A company needs centralized security findings from GuardDuty, Inspector, and Macie. Which service should aggregate them?
   - A. AWS Security Hub
   - B. AWS Config
   - C. Amazon Athena
   - D. AWS Batch

### Databases and Caching

13. An RDS database is overloaded by reporting queries, but the primary write workload is healthy. What should be added?
   - A. Multi-AZ standby
   - B. Read Replica
   - C. NAT Gateway
   - D. Gateway Load Balancer

14. A DynamoDB table has unpredictable traffic and the team wants to avoid capacity planning. Which mode should be used?
   - A. Provisioned capacity without auto scaling
   - B. On-demand capacity
   - C. RDS Reserved Instance
   - D. Redshift Spectrum

15. A cache must support persistence, Multi-AZ failover, and sorted sets for leaderboards. Which engine fits best?
   - A. Memcached
   - B. Redis
   - C. DAX
   - D. Athena

16. A relational MySQL-compatible application needs cross-Region disaster recovery with very low replication lag and fast failover. What is the best fit?
   - A. Aurora Global Database
   - B. RDS Single-AZ
   - C. DynamoDB Local
   - D. EBS snapshot only

### Compute, Load Balancing, and Scaling

17. An HTTP application needs host-based and path-based routing to multiple microservices. Which load balancer should be used?
   - A. ALB
   - B. NLB
   - C. GWLB
   - D. Route 53 Resolver

18. A service uses UDP and requires static IP addresses. Which load balancer fits best?
   - A. ALB
   - B. NLB
   - C. CloudFront
   - D. Classic Load Balancer

19. A scientific workload needs low latency between EC2 instances for MPI. Which features are likely useful?
   - A. Cluster placement group and EFA
   - B. Spread placement group and SQS
   - C. Gateway Load Balancer and Macie
   - D. CloudTrail and S3 Object Lock

20. A containerized service should run without managing EC2 hosts. Which compute option fits?
   - A. ECS on Fargate
   - B. ECS on self-managed EC2 only
   - C. Dedicated Hosts
   - D. EC2 Instance Store

### Messaging and Event-Driven Architecture

21. A payment workflow needs multiple steps, retries, wait states, and branching logic. Which service should orchestrate it?
   - A. Step Functions
   - B. SNS
   - C. CloudTrail
   - D. S3 Lifecycle

22. A workload needs strict ordering and exactly-once processing for messages. Which queue type should be used?
   - A. SQS Standard
   - B. SQS FIFO
   - C. SNS Standard topic only
   - D. Kinesis Firehose

23. A security finding should trigger a Lambda function when matching a rule pattern. Which service should route the event?
   - A. EventBridge
   - B. DMS
   - C. Redshift
   - D. EBS

24. Existing applications use ActiveMQ protocols and must move to a managed AWS service with minimal code change. What should be selected?
   - A. Amazon MQ
   - B. Amazon SNS
   - C. Amazon SQS FIFO
   - D. AWS Step Functions

### DNS, Edge, and Global Traffic

25. A root domain example.com must route to an ALB. Which Route 53 record is appropriate?
   - A. CNAME at apex
   - B. Alias A record
   - C. MX record
   - D. TXT record

26. Users in different countries must receive localized content based on their location. Which Route 53 policy fits?
   - A. Weighted
   - B. Geolocation
   - C. Simple
   - D. Failover

27. A global application uses TCP and needs static Anycast IPs plus fast health-based failover. Which service fits?
   - A. Global Accelerator
   - B. CloudFront only
   - C. S3 Transfer Acceleration
   - D. API Gateway private API

28. A private S3 origin should only be accessible through CloudFront. What should be configured?
   - A. Origin Access Control and a restrictive bucket policy
   - B. Public bucket ACLs
   - C. NAT Gateway
   - D. DynamoDB Streams

### Monitoring, Cost, and DR

29. An EC2 Auto Scaling policy must scale based on memory utilization. What is required first?
   - A. CloudWatch agent to publish memory metrics
   - B. CloudTrail data events
   - C. S3 server access logging
   - D. Route 53 health check

30. The team must know which IAM principal deleted an S3 object. What should be enabled?
   - A. CloudTrail data events for S3
   - B. AWS Config only
   - C. Trusted Advisor
   - D. Compute Optimizer

31. A batch job can restart if interrupted and runs for several hours. The company wants the lowest compute cost. What is best?
   - A. Spot Instances
   - B. Dedicated Hosts
   - C. On-Demand only
   - D. Capacity Reservations only

32. An application can tolerate hours of downtime and data loss for disaster recovery. What is the lowest-cost DR strategy?
   - A. Multi-site active-active
   - B. Warm standby
   - C. Pilot light
   - D. Backup and restore

### Community-Sourced Concept Questions

33. A question includes "least operational overhead" and offers a self-managed EC2 database and an RDS option. Both meet the functional requirement. Which is usually better?
   - A. RDS
   - B. Self-managed database on EC2
   - C. Instance Store
   - D. NAT Gateway

34. A question includes "most cost-effective" and several options work. What should you evaluate next?
   - A. Idle capacity, data transfer, storage class, managed-service cost, and commitment discounts
   - B. Which service has the longest name
   - C. Which answer appears first
   - D. Which service has the newest launch date

35. A nightly Spark job should spin up, process data, then terminate to avoid paying for idle cluster time. Which pattern fits?
   - A. Transient EMR cluster
   - B. Always-on EMR cluster
   - C. RDS Multi-AZ
   - D. CloudFront Functions

36. A genomics workload needs a temporary high-throughput file system integrated with S3. The data can be recreated. Which service and deployment type fits?
   - A. FSx for Lustre Scratch
   - B. FSx for Windows File Server
   - C. EFS Standard
   - D. S3 Glacier Deep Archive

37. A company needs durable high-performance Lustre storage for repeated HPC jobs. Which FSx option is more appropriate?
   - A. FSx for Lustre Persistent
   - B. FSx for Lustre Scratch
   - C. Instance Store
   - D. SQS Standard

38. A legacy on-premises application depends on iSCSI block storage. The company wants a hybrid bridge to AWS storage. Which service is most relevant?
   - A. Storage Gateway Volume Gateway
   - B. Amazon EventBridge
   - C. AWS Glue
   - D. AWS WAF

39. A developer needs to identify clinical medical conditions and protected health information in text. Which service is most specific?
   - A. Amazon Comprehend Medical
   - B. Amazon Polly
   - C. Amazon Translate
   - D. Amazon Rekognition

40. A company wants to turn customer-support recordings into searchable text. Which service should be selected?
   - A. Amazon Transcribe
   - B. Amazon Polly
   - C. Amazon Macie
   - D. Amazon QLDB

41. A data science team wants managed notebooks, model training, and model hosting. Which service should be used?
   - A. Amazon SageMaker AI
   - B. Amazon MQ
   - C. AWS DataSync
   - D. AWS Backup

42. A workload needs search over application logs and full-text queries. Which service is the best fit?
   - A. Amazon OpenSearch Service
   - B. Amazon SQS
   - C. Amazon Route 53
   - D. AWS Shield

43. A company has enterprise workforce SSO and wants centralized access across multiple AWS accounts. Which service should it study?
   - A. IAM Identity Center
   - B. S3 Transfer Acceleration
   - C. Amazon Timestream
   - D. AWS Glue DataBrew

44. A candidate keeps missing questions by choosing read replicas for availability. What is the correct distinction?
   - A. Multi-AZ is for availability and failover; Read Replicas are for read scaling
   - B. Read Replicas are synchronous standbys
   - C. Multi-AZ is only for analytics
   - D. Read Replicas are required for every RDS database

45. A practice question asks for "fastest migration" with a heterogeneous database engine change. Which tools are likely relevant?
   - A. DMS and SCT
   - B. CloudFront and WAF
   - C. SQS and SNS
   - D. KMS and CloudHSM only

46. A candidate has 65 questions and 130 minutes. A dense VPC-routing question is still unclear after about 90 seconds. What is the best tactic?
   - A. Choose the best current answer, flag it, and return later
   - B. Spend as long as needed because hard questions are worth more
   - C. Leave it blank
   - D. Restart the exam

47. A team wants a hands-on lab that teaches private origin access and edge caching. Which build is best?
   - A. S3 static site behind CloudFront with OAC
   - B. One public EC2 instance with no load balancer
   - C. One unused IAM group
   - D. One empty DynamoDB table

48. A team wants a hands-on lab that teaches health checks and replacement of unhealthy compute. Which build is best?
   - A. Auto Scaling group behind an ALB with health checks
   - B. S3 bucket with no objects
   - C. One CloudTrail trail only
   - D. One Route 53 TXT record

### Additional Extensive Research Questions

49. EC2 instances in private subnets must download operating system patches from the internet while remaining unreachable from the internet. Which design is correct?
   - A. Put a NAT Gateway in a public subnet and route private subnet internet traffic to it
   - B. Put an Internet Gateway route directly in the private subnet route table
   - C. Assign public IPs to the private instances
   - D. Put a NAT Gateway in the private subnet

50. EC2 applications keep important session state in memory and must pause for a two-week shutdown, then resume with memory contents restored. Which feature should be used?
   - A. EC2 hibernation
   - B. Instance store snapshots
   - C. Elastic IP reassociation
   - D. Placement groups

51. A monitoring application is reached through a private IPv4 address. If the primary EC2 instance fails, traffic must move quickly to a standby instance in the same Availability Zone. Which option fits?
   - A. Move a secondary ENI with the private IP to the standby instance
   - B. Move the primary ENI to another instance
   - C. Use an Internet Gateway
   - D. Use S3 Transfer Acceleration

52. A browser script hosted on one domain must make authenticated GET requests to objects in an S3 bucket on another domain. What bucket feature is required?
   - A. CORS configuration
   - B. S3 Object Lock
   - C. S3 Inventory
   - D. S3 Batch Operations

53. An Auto Scaling group launches instances that take several minutes to initialize. Scaling metrics should not count initializing instances too early, and scale-out should react better to bursts. What should be configured?
   - A. Instance warmup or a warm pool
   - B. An Elastic IP for every instance
   - C. A single-AZ Auto Scaling group
   - D. S3 lifecycle rules

54. Aurora database writes are becoming slow because reporting reads are heavy. What is the best way to separate reads from writes?
   - A. Add Aurora replicas and use the reader endpoint for read traffic
   - B. Read from the Multi-AZ standby instance directly
   - C. Put a NAT Gateway in front of the database
   - D. Move all reads to CloudTrail

55. A high-volume S3 workload uses SSE-KMS and the team wants to reduce AWS KMS request costs. Which feature can help?
   - A. S3 Bucket Keys
   - B. S3 CORS
   - C. S3 Object Lock
   - D. S3 Requester Pays

56. Which statement correctly distinguishes an S3 object key from object metadata?
   - A. The key is the object name/path; metadata is descriptive attributes
   - B. The key is a KMS key; metadata is an IAM policy
   - C. The key is a lifecycle rule; metadata is a Route 53 record
   - D. The key is a VPC endpoint; metadata is a subnet route

57. A team needs a managed GraphQL API that can combine multiple data sources and support real-time application patterns. Which service should be reviewed?
   - A. AWS AppSync
   - B. Amazon API Gateway REST API only
   - C. Amazon MQ
   - D. AWS Batch

58. A business wants to move data from SaaS applications such as Salesforce into AWS destinations without building and operating custom connectors. Which service fits?
   - A. Amazon AppFlow
   - B. AWS DataSync
   - C. AWS DMS
   - D. AWS Snowball Edge

59. A company needs centralized governance and fine-grained permissions for data in an S3-based data lake. Which service is most relevant?
   - A. AWS Lake Formation
   - B. AWS Elastic Beanstalk
   - C. Amazon Route 53 Resolver
   - D. Amazon Polly

60. A company wants centralized backup policies across supported AWS services and accounts. Which service should be used?
   - A. AWS Backup
   - B. AWS Artifact
   - C. Amazon Athena
   - D. AWS WAF

61. Platform teams want to publish approved infrastructure templates that application teams can launch self-service under governance. Which service fits?
   - A. AWS Service Catalog
   - B. Amazon Global Accelerator
   - C. Amazon Inspector
   - D. AWS Glue

62. A compliance team needs AWS compliance reports and agreements. Which service should they use?
   - A. AWS Artifact
   - B. Amazon Macie
   - C. AWS Batch
   - D. Amazon Timestream

63. A team wants account-specific AWS service-health events and planned-maintenance notifications that may affect its resources. Which service should be checked?
   - A. AWS Health Dashboard
   - B. AWS WAF
   - C. Amazon Redshift Spectrum
   - D. Amazon S3 Select

64. A company needs AWS-managed Microsoft Active Directory integration for Windows workloads. Which service family is relevant?
   - A. AWS Directory Service
   - B. Amazon AppFlow
   - C. Amazon Kinesis
   - D. AWS Glue

65. A central security team must manage WAF rules across many AWS accounts in AWS Organizations. Which service helps?
   - A. AWS Firewall Manager
   - B. AWS Lambda
   - C. Amazon SQS
   - D. AWS Elastic Beanstalk

66. A company wants to share supported resources, such as subnets, across accounts in the same organization. Which service is most relevant?
   - A. AWS Resource Access Manager
   - B. AWS Artifact
   - C. Amazon Transcribe
   - D. AWS DMS

67. Users should receive DNS answers based on their country or continent. Which Route 53 policy fits?
   - A. Geolocation
   - B. Geoproximity
   - C. Weighted
   - D. Simple

68. DNS should route users based on distance to resources and allow biasing traffic toward or away from a location. Which Route 53 policy fits?
   - A. Geoproximity
   - B. Geolocation
   - C. Failover
   - D. Multivalue answer

69. An application needs a Redis-compatible in-memory database with durability, not just a cache. Which service should be considered?
   - A. Amazon MemoryDB
   - B. Amazon ElastiCache Memcached
   - C. Amazon Athena
   - D. Amazon Macie

70. A team uses Kafka APIs and wants a managed Apache Kafka service. Which service should be selected?
   - A. Amazon MSK
   - B. Amazon MQ
   - C. Amazon SNS
   - D. AWS Step Functions

71. A streaming pipeline should deliver records to S3, Redshift, or OpenSearch with minimal custom consumer management. Which service should be used?
   - A. Kinesis Data Firehose
   - B. Kinesis Data Streams only
   - C. Amazon SQS FIFO
   - D. Amazon Cognito

72. A company needs to scan EC2 instances, Lambda functions, and container images for software vulnerabilities. Which service fits?
   - A. Amazon Inspector
   - B. Amazon GuardDuty
   - C. Amazon Macie
   - D. AWS Config

73. After GuardDuty findings, a security team needs relationship graphs and context to investigate likely root cause. Which service helps?
   - A. Amazon Detective
   - B. AWS Backup
   - C. AWS Service Catalog
   - D. Amazon Polly

74. A public web application needs protection from SQL injection, XSS, rate-based abuse, and country-based blocking. Which service should be used?
   - A. AWS WAF
   - B. AWS Shield Standard only
   - C. AWS Artifact
   - D. Amazon Route 53 Resolver

## Answer Key

1. B - Gateway endpoints support S3 and DynamoDB and avoid NAT for those services.
2. C - VPC peering is non-transitive.
3. B - Site-to-Site VPN is encrypted and fast to provision.
4. A - PrivateLink exposes a service privately without full network connectivity.
5. C - Glacier Instant Retrieval is archival storage with millisecond access.
6. A - Compliance mode prevents deletion during retention, even by root/admin.
7. B - Multipart upload is required over 5 GB and recommended for large objects.
8. A - DataSync is for automated file transfer to AWS storage.
9. C - Explicit deny always wins.
10. A - SCPs set preventive guardrails for member accounts/OUs.
11. C - ACM certs for CloudFront must be in us-east-1.
12. A - Security Hub aggregates security findings.
13. B - Read replicas scale read workloads.
14. B - On-demand capacity avoids DynamoDB capacity planning.
15. B - Redis supports persistence, replication, and rich data structures.
16. A - Aurora Global Database is designed for cross-Region relational DR.
17. A - ALB is layer 7 and supports host/path routing.
18. B - NLB supports TCP/UDP and static IPs.
19. A - Cluster placement groups and EFA target HPC low-latency networking.
20. A - Fargate runs containers without EC2 host management.
21. A - Step Functions orchestrates stateful workflows.
22. B - SQS FIFO provides ordering and deduplication semantics.
23. A - EventBridge routes events by rule patterns.
24. A - Amazon MQ supports legacy broker protocols.
25. B - Alias records support AWS targets at the zone apex.
26. B - Geolocation routes by user geography.
27. A - Global Accelerator gives static Anycast IPs and TCP/UDP acceleration.
28. A - OAC plus bucket policy keeps S3 private behind CloudFront.
29. A - EC2 memory is not a default CloudWatch metric.
30. A - S3 object-level API activity requires CloudTrail data events.
31. A - Spot is best for interruptible, restartable jobs.
32. D - Backup and restore is lowest-cost DR with slower recovery.
33. A - Managed services usually win for least operational overhead when requirements are met.
34. A - Cost questions require comparing real cost drivers.
35. A - Transient EMR clusters are created for a job and terminated afterward.
36. A - FSx for Lustre Scratch is high-performance temporary storage.
37. A - FSx for Lustre Persistent is durable for repeated jobs.
38. A - Volume Gateway supports iSCSI block storage patterns.
39. A - Comprehend Medical is specific to clinical/medical text.
40. A - Transcribe converts speech to text.
41. A - SageMaker AI handles managed ML build/train/deploy workflows.
42. A - OpenSearch supports full-text search and log analytics.
43. A - IAM Identity Center is for centralized workforce access across accounts.
44. A - Multi-AZ is HA/failover; Read Replicas scale reads.
45. A - DMS migrates data; SCT helps convert schemas for different engines.
46. A - Time management matters; flag and move prevents one question from consuming the exam.
47. A - This lab combines S3, CloudFront, private origin access, DNS/TLS, and edge caching concepts.
48. A - This lab makes Auto Scaling, ALB routing, health checks, and resilience concrete.
49. A - NAT Gateway belongs in a public subnet; private subnet route tables point internet-bound traffic to it.
50. A - EC2 hibernation restores memory state from the EBS root volume.
51. A - Secondary ENIs can be detached and attached to standby instances in the same AZ.
52. A - S3 CORS enables approved browser-based cross-origin requests.
53. A - Instance warmup and warm pools help Auto Scaling handle slow initialization.
54. A - Aurora replicas and reader endpoints offload read traffic from the writer.
55. A - S3 Bucket Keys reduce AWS KMS request volume for SSE-KMS.
56. A - The object key identifies the object; metadata stores descriptive attributes.
57. A - AppSync is the managed GraphQL service for data-backed APIs and real-time patterns.
58. A - AppFlow is for managed SaaS-to-AWS data flows.
59. A - Lake Formation handles data lake governance and fine-grained permissions.
60. A - AWS Backup centralizes backup plans for supported services.
61. A - Service Catalog enables governed self-service infrastructure products.
62. A - Artifact provides AWS compliance reports and agreements.
63. A - AWS Health Dashboard gives account-specific service-health events.
64. A - Directory Service covers AWS managed directory integration such as Microsoft AD patterns.
65. A - Firewall Manager centrally manages WAF and related protections across accounts.
66. A - RAM shares supported AWS resources across accounts.
67. A - Geolocation routes based on user geography.
68. A - Geoproximity routes based on distance and supports bias.
69. A - MemoryDB is Redis-compatible with durability.
70. A - MSK is AWS's managed Apache Kafka service.
71. A - Firehose is managed delivery to supported analytics/storage destinations.
72. A - Inspector scans workloads and images for software vulnerabilities.
73. A - Detective helps investigate relationships and root cause around findings.
74. A - WAF protects web applications at layer 7.

## How To Use This File

1. Start with Priority 1 through Priority 6 before deep-diving the rest.
2. For every topic, memorize the "Question patterns" more than the service definition.
3. When practicing, write down the qualifier: cost, performance, security, resilience, operational overhead, or migration speed.
4. Revisit the highest-yield confusions daily during the final week.
