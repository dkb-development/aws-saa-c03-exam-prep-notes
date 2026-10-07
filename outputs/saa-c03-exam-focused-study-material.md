# AWS SAA-C03 Exam-Focused Study Material

Prepared on 2026-10-08.

This study material is built from the researched GitHub, Reddit, official AWS, and developer-community question patterns already collected in the companion files. It is not a general AWS theory book. It is focused on how SAA-C03 usually asks scenario questions: requirement, constraint, trap, and best-fit service.

No exam dumps or copied real exam questions are used. All examples are original and concept-based.

## Four Exam Sections

| Section | Official Domain | Exam Weight | Status |
|---|---|---:|---|
| 1 | Design Secure Architectures | 30% | Complete in this file |
| 2 | Design Resilient Architectures | 26% | Complete in this file |
| 3 | Design High-Performing Architectures | 24% | Complete in this file |
| 4 | Design Cost-Optimized Architectures | 20% | Complete in this file |

## How To Read SAA-C03 Questions

Before choosing an answer, identify the real qualifier:

| Question qualifier | What AWS usually wants |
|---|---|
| Most secure | Least privilege, private access, encryption, no public exposure |
| Least operational overhead | Managed service, serverless, AWS-native automation |
| Most highly available | Multi-AZ, managed failover, health checks, decoupling |
| Lowest cost | Remove idle capacity, use lifecycle policies, avoid NAT/data-transfer waste |
| Best performance | Caching, read replicas, right load balancer, regional/edge acceleration |
| Fastest migration | Minimal app change, managed migration service, compatibility first |

The SAA trap is that multiple answers may work technically. The correct answer is the one that satisfies all requirements with the best match to the qualifier.

# Section 1: Design Secure Architectures

Exam weight: 30%.

This is the largest SAA-C03 domain. Security questions are rarely pure definitions. They usually appear as architecture decisions:

- Who or what needs access?
- Should the access be public or private?
- Where should encryption happen?
- Which AWS service gives the least-privilege or lowest-operations answer?
- Is the question asking for prevention, detection, investigation, or compliance evidence?

## 1. IAM And Access Control

### What The Exam Is Testing

The exam wants you to know how AWS permissions are granted, limited, and evaluated.

Core rules:

- IAM policies grant permissions to identities such as users, groups, and roles.
- Resource policies grant access directly on resources such as S3 buckets, KMS keys, SQS queues, and Lambda functions.
- IAM roles provide temporary credentials through AWS STS.
- Explicit deny always wins.
- Implicit deny is the default when no policy allows the action.
- SCPs limit maximum permissions in AWS Organizations but do not grant access.
- Permission boundaries limit what an IAM principal can ever do but do not grant access by themselves.

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| EC2 instance needs AWS API access | IAM role attached to instance profile |
| Lambda function needs S3 or DynamoDB access | Lambda execution role |
| App should not store long-term access keys | IAM role / temporary credentials |
| Users from another AWS account need access | Cross-account IAM role or resource policy |
| Organization must block actions in all member accounts | SCP |
| Developer can create roles but must not exceed limits | Permission boundary |
| Workforce users need SSO into many AWS accounts | IAM Identity Center |
| Mobile/web users need application sign-in | Cognito User Pool |
| Mobile/web users need temporary AWS credentials | Cognito Identity Pool |

### Exam-Related Examples

Example 1: EC2 app needs S3 access.

The wrong answer is often "store access keys on the instance." The exam-preferred answer is an IAM role attached to the EC2 instance profile. This avoids hardcoded credentials and uses temporary credentials automatically.

Example 2: Organization wants to prevent all accounts from disabling CloudTrail.

Use an SCP in AWS Organizations. The SCP can deny CloudTrail-disabling actions across accounts or organizational units. Remember: the SCP does not grant CloudTrail permissions. It only limits what accounts can do.

Example 3: A developer has an allow policy for `s3:GetObject`, but another applicable policy explicitly denies it.

The request is denied. Explicit deny beats allow. Do not overthink identity policy vs resource policy if an explicit deny is present.

Example 4: A third-party company needs access to one S3 bucket.

Good answers usually use a cross-account IAM role, an S3 bucket policy, or both. Bad answers create IAM users with long-term access keys in your account unless the question gives a very specific reason.

### Common IAM Traps

- SCPs do not grant permissions.
- Permission boundaries do not grant permissions.
- IAM groups cannot be used as principals in resource policies.
- Roles are preferred over access keys for AWS compute.
- Root user should not be used for daily operations.
- MFA is commonly expected for root and sensitive human access.
- For application users, Cognito is usually better than IAM users.

## 2. Secure Network Architecture

### What The Exam Is Testing

SAA security questions often hide inside VPC design. You must decide whether traffic should be public, private, outbound-only, or completely AWS-private.

Core concepts:

- Public subnet: route table has a route to an Internet Gateway.
- Private subnet: no direct route to an Internet Gateway.
- NAT Gateway: allows private subnet resources to initiate outbound IPv4 internet access.
- Egress-only Internet Gateway: outbound-only IPv6 access.
- Gateway VPC Endpoint: private route-table based access to S3 and DynamoDB.
- Interface VPC Endpoint: private ENI-based access to many AWS services using PrivateLink.
- PrivateLink: expose a private service to other VPCs/accounts without VPC peering.
- VPC Peering: private VPC-to-VPC connectivity, non-transitive.
- Transit Gateway: hub-and-spoke connectivity for many VPCs and on-premises networks.

### Security Groups vs NACLs

| Feature | Security Group | Network ACL |
|---|---|---|
| Level | Instance/ENI level | Subnet level |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and deny |
| Return traffic | Automatically allowed | Must be explicitly allowed |
| Common exam use | Permit app/database traffic | Block a specific IP or subnet-level filter |

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Private EC2 instances need outbound internet patches | NAT Gateway in public subnet plus route from private subnet |
| Private instances need S3/DynamoDB without NAT | Gateway VPC Endpoint |
| Private access to most AWS services | Interface VPC Endpoint |
| Block a known malicious IP at subnet boundary | NACL deny rule |
| Web app needs SQL injection/XSS/rate-limit protection | AWS WAF |
| Need network firewalling between VPC subnets | AWS Network Firewall |
| Many VPCs need transitive connectivity | Transit Gateway |
| Private SaaS-style service access across accounts | PrivateLink |
| Dedicated predictable private connectivity to on-premises | Direct Connect |
| Encrypted connection to on-premises quickly | Site-to-Site VPN |
| Need encryption over Direct Connect | VPN over Direct Connect |

### Exam-Related Examples

Example 1: Private EC2 instances must download software patches but cannot be reachable from the internet.

Use a NAT Gateway in a public subnet and update private subnet route tables to send internet-bound traffic to the NAT Gateway. Do not place the NAT Gateway in the private subnet. Do not assign public IPs to the private instances.

Example 2: Private EC2 instances must access S3 securely and cost-effectively.

Use an S3 Gateway VPC Endpoint. NAT Gateway can technically work, but it sends traffic through a charged NAT path and is not the best answer when the question asks for private or cost-effective S3 access.

Example 3: A security team wants to block one malicious IP address before it reaches instances.

Use a NACL deny rule if the requirement is subnet-level IP blocking. Security Groups cannot create deny rules. If the threat is an HTTP-layer attack such as SQL injection, use AWS WAF instead.

Example 4: A provider wants customers to access one private service without full VPC connectivity.

Use AWS PrivateLink. VPC peering gives broader network-level connectivity and is not the best fit for controlled service exposure.

### Common Network Security Traps

- A route to an Internet Gateway makes a subnet public only when instances also have public IPs.
- NAT Gateway allows outbound internet access, not inbound access from the internet.
- Gateway endpoints are only for S3 and DynamoDB.
- Interface endpoints use ENIs and security groups.
- VPC peering is non-transitive.
- Security Groups are stateful; NACLs are stateless.
- Security Groups cannot deny traffic.
- Direct Connect is private but not encrypted by default.

## 3. Encryption And Key Management

### What The Exam Is Testing

The exam tests which encryption option matches control, operations, and compliance requirements.

Core services:

- AWS KMS: managed key service integrated with many AWS services.
- Customer managed KMS key: you control key policy, rotation settings, grants, and usage.
- AWS managed key: managed by AWS for a service in your account.
- CloudHSM: dedicated HSM cluster where you manage keys and HSM control.
- AWS Certificate Manager: TLS certificates for supported AWS services.
- Secrets Manager: encrypted secret storage with rotation.
- Systems Manager Parameter Store: configuration and secrets storage, commonly cheaper, rotation is not the main built-in feature.

### S3 Encryption Choices

| Requirement | Choose |
|---|---|
| Simple server-side encryption managed by S3 | SSE-S3 |
| Server-side encryption with KMS audit/control | SSE-KMS |
| Reduce KMS request cost for heavy S3 SSE-KMS use | S3 Bucket Keys |
| Customer supplies encryption key on every request | SSE-C |
| Data must be encrypted before sending to S3 | Client-side encryption |
| Compliance retention/WORM | S3 Object Lock compliance mode |

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Need managed encryption integrated with AWS services | KMS |
| Need key usage audit through CloudTrail | KMS |
| Need dedicated HSMs controlled by customer | CloudHSM |
| Need automatic DB credential rotation | Secrets Manager |
| Need low-cost app config value | Parameter Store |
| Need TLS certificate for ALB/API Gateway/CloudFront | ACM |
| CloudFront custom domain certificate | ACM certificate in us-east-1 |
| Existing unencrypted EBS/RDS needs encryption | Snapshot, copy with encryption, restore |

### Exam-Related Examples

Example 1: A company requires encryption at rest for S3 and wants key access logged and controlled.

Use SSE-KMS with a customer managed KMS key. KMS integrates with CloudTrail and key policies. SSE-S3 encrypts but gives less customer control.

Example 2: A compliance workload requires customer-controlled dedicated HSMs.

Choose CloudHSM. KMS is managed and integrated, but CloudHSM is the exam answer when dedicated HSM ownership and direct HSM control are the central requirement.

Example 3: An application needs database passwords rotated every 30 days with minimal custom code.

Choose Secrets Manager. Parameter Store can store secure strings, but Secrets Manager is the usual answer for built-in rotation workflows.

Example 4: A CloudFront distribution uses a custom domain.

The ACM certificate must be in `us-east-1`. This is a common exam detail.

### Common Encryption Traps

- KMS key policy matters. IAM permissions alone may not be enough.
- CloudHSM is not the default answer unless the question emphasizes dedicated HSM/customer control.
- Existing unencrypted EBS/RDS resources usually require snapshot-copy-restore patterns to encrypt.
- S3 Object Lock is for retention/compliance, not ordinary encryption.
- SSE-C means the customer supplies the key with requests; AWS does not store the key.
- Secrets Manager beats Parameter Store when automatic rotation is required.

## 4. Application Identity And User Access

### What The Exam Is Testing

The exam separates workforce identity, application user identity, and AWS service access.

| Need | Choose |
|---|---|
| Employees sign in to AWS accounts centrally | IAM Identity Center |
| External web/mobile users sign up and sign in | Cognito User Pool |
| External users need temporary AWS credentials | Cognito Identity Pool |
| App running on EC2/Lambda/ECS needs AWS access | IAM role |
| Cross-account app/service access | IAM role with trust policy |

### Exam-Related Examples

Example 1: A mobile app lets users sign in, then upload files directly to S3 using temporary credentials.

Use Cognito User Pool for sign-in and Cognito Identity Pool for temporary AWS credentials. The User Pool authenticates the user. The Identity Pool provides AWS credentials through IAM roles.

Example 2: A company uses workforce SSO and needs centralized access to many AWS accounts.

Use IAM Identity Center. Do not create IAM users in every account.

Example 3: An application in Account A must access a DynamoDB table in Account B.

Use a cross-account IAM role or resource policy where supported. The key idea is temporary, trusted access rather than sharing long-term credentials.

### Common Identity Traps

- Cognito User Pool is not the same as Identity Pool.
- IAM Identity Center is for workforce access, not customer sign-up for your app.
- IAM users are not the preferred answer for applications running on AWS services.
- Roles are assumed and temporary. Users are long-term identities.

## 5. Data Access Protection

### What The Exam Is Testing

These questions usually focus on preventing accidental public access, protecting sensitive data, or allowing temporary limited access.

### S3 Security Rules

| Scenario phrase | Choose |
|---|---|
| Prevent public S3 access broadly | S3 Block Public Access |
| Give temporary access to a private S3 object | Presigned URL |
| Enforce HTTPS-only access to S3 | Bucket policy condition denying non-TLS |
| Keep objects immutable for compliance | S3 Object Lock compliance mode |
| Replicate encrypted objects cross-Region | S3 CRR plus KMS permissions |
| Browser app needs cross-origin S3 access | S3 CORS |
| CloudFront should be only path to S3 | Origin Access Control plus restrictive bucket policy |

### Exam-Related Examples

Example 1: A private document should be downloadable by one user for 10 minutes.

Use a presigned URL. Do not make the bucket public. Do not create an IAM user for the external person.

Example 2: A static website uses S3 as a private origin behind CloudFront.

Use CloudFront Origin Access Control and a bucket policy that allows access only from the CloudFront distribution. Public bucket ACLs are not the secure answer.

Example 3: A browser script from one domain must call S3 on another domain.

Configure S3 CORS. This is about browser security headers, not bucket encryption or versioning.

Example 4: A regulatory archive must prevent deletion even by admins during the retention period.

Use S3 Object Lock in compliance mode. Governance mode is weaker because privileged users can bypass it.

### Common Data Protection Traps

- Bucket policy and IAM policy can both matter.
- Public access can come from ACLs or policies, so Block Public Access is a broad guardrail.
- Presigned URLs grant temporary access without changing object visibility.
- CORS does not grant IAM permissions. It only allows browser cross-origin behavior.
- OAC is the modern CloudFront-to-S3 private-origin pattern.

## 6. Threat Detection, Audit, And Compliance

### What The Exam Is Testing

Know whether the requirement is logging, monitoring, compliance state, threat detection, vulnerability scanning, sensitive data discovery, investigation, or evidence.

| Requirement | Service |
|---|---|
| Who called which AWS API | CloudTrail |
| Object-level S3 API activity | CloudTrail data events |
| Metrics, logs, alarms | CloudWatch |
| Resource configuration history/compliance | AWS Config |
| Threat detection from logs and behavior | GuardDuty |
| Sensitive data discovery in S3 | Macie |
| Vulnerability scanning for EC2/ECR/Lambda | Inspector |
| Central findings dashboard | Security Hub |
| Investigation graph after findings | Detective |
| Compliance reports and agreements | Artifact |
| Account-specific AWS service events | AWS Health Dashboard |

### Exam-Related Examples

Example 1: Security needs to know which IAM principal deleted an S3 object.

Enable CloudTrail data events for S3. CloudTrail management events alone may not capture object-level activity.

Example 2: The team wants to know whether S3 buckets are publicly readable and track compliance over time.

Use AWS Config rules. Config tracks resource configuration and compliance history.

Example 3: The company wants to detect suspicious activity from VPC Flow Logs, DNS logs, and CloudTrail events.

Use GuardDuty. GuardDuty is threat detection. It is not a vulnerability scanner.

Example 4: The company wants to find personally identifiable information in S3.

Use Macie. Do not choose GuardDuty or Inspector for PII discovery.

Example 5: The team needs to scan EC2 instances and container images for software vulnerabilities.

Use Inspector. Do not choose GuardDuty, because GuardDuty detects threats rather than scanning packages/images for vulnerabilities.

Example 6: After a GuardDuty alert, analysts need to investigate relationships among users, IPs, roles, and resources.

Use Detective. It helps investigation after findings; it is not the primary detector.

### Common Monitoring And Security Traps

- CloudTrail = API audit.
- CloudWatch = metrics/logs/alarms.
- Config = configuration compliance/history.
- GuardDuty = threat detection.
- Inspector = vulnerability scanning.
- Macie = sensitive data in S3.
- Security Hub = aggregate findings.
- Detective = investigate relationships and root cause.
- Artifact = compliance documents.

## 7. Edge And Perimeter Protection

### What The Exam Is Testing

The exam often asks what protects an internet-facing app from specific types of attacks.

| Threat or requirement | Choose |
|---|---|
| SQL injection / XSS | AWS WAF |
| Rate-based request blocking | AWS WAF |
| Country-based web request blocking | AWS WAF |
| DDoS protection baseline | AWS Shield Standard |
| Advanced DDoS protection and response | AWS Shield Advanced |
| Central WAF policy across accounts | AWS Firewall Manager |
| Private S3 origin through CDN only | CloudFront OAC |
| Network-layer inspection in VPC | AWS Network Firewall |
| Third-party inspection appliance at scale | Gateway Load Balancer |

### Exam-Related Examples

Example 1: A public application receives SQL injection attempts.

Use AWS WAF on CloudFront, ALB, API Gateway, or another supported front door. Security Groups and NACLs do not understand SQL injection patterns.

Example 2: A company wants all accounts in AWS Organizations to follow central WAF rules.

Use AWS Firewall Manager. It centrally manages WAF and related security policies across accounts.

Example 3: A company needs managed DDoS protection beyond the default.

Use Shield Advanced. Shield Standard is automatic, but Advanced is the answer when the question mentions enhanced DDoS protection, cost protection, or response support.

### Common Perimeter Traps

- WAF is layer 7. It protects HTTP/S apps from web exploits.
- NACL is subnet-level network filtering, not web request inspection.
- Security Groups cannot block based on HTTP request content.
- Shield is DDoS-focused, not SQL injection-focused.
- Firewall Manager manages policies centrally; it is not itself the packet inspection engine.

## Section 1 Rapid Revision Table

| If the question says... | Think... |
|---|---|
| App on EC2 needs AWS permissions | IAM role / instance profile |
| Lambda needs AWS permissions | Execution role |
| Organization-wide guardrail | SCP |
| Prevent developer privilege escalation | Permission boundary |
| Temporary object access | S3 presigned URL |
| Private S3 behind CloudFront | OAC plus bucket policy |
| Private subnet needs S3 without NAT | S3 Gateway Endpoint |
| Private subnet needs outbound internet | NAT Gateway in public subnet |
| Browser cross-origin request to S3 | S3 CORS |
| Automatic secret rotation | Secrets Manager |
| Dedicated HSM | CloudHSM |
| PII in S3 | Macie |
| API audit | CloudTrail |
| Configuration compliance | AWS Config |
| Threat detection | GuardDuty |
| Vulnerability scanning | Inspector |
| Investigate findings | Detective |
| Central findings | Security Hub |
| SQL injection/XSS/rate limiting | AWS WAF |
| Advanced DDoS | Shield Advanced |
| Central WAF across accounts | Firewall Manager |
| Workforce SSO | IAM Identity Center |
| App user sign-in | Cognito User Pool |
| Temporary AWS creds for app users | Cognito Identity Pool |

## Section 1 Mini Exam Drill

1. EC2 instances in private subnets need to access S3 without public internet access or NAT charges. What should you use?
   - A. Internet Gateway
   - B. S3 Gateway VPC Endpoint
   - C. Public IP addresses
   - D. NAT Gateway only

2. A web app must block SQL injection attempts. What should you use?
   - A. Security Group
   - B. NACL
   - C. AWS WAF
   - D. AWS Config

3. A company wants to prevent all AWS accounts in an OU from disabling CloudTrail. What should be used?
   - A. IAM group
   - B. SCP
   - C. Security Group
   - D. CloudFront

4. A mobile app needs user sign-in and temporary credentials for S3 upload. What should be used?
   - A. Cognito User Pool and Identity Pool
   - B. IAM users for each customer
   - C. CloudTrail and Config
   - D. AWS Artifact

5. A company needs to discover PII in S3 buckets. What should be used?
   - A. GuardDuty
   - B. Macie
   - C. Inspector
   - D. Detective

6. A company needs to scan container images and EC2 instances for software vulnerabilities. What should be used?
   - A. Macie
   - B. Inspector
   - C. CloudTrail
   - D. IAM Identity Center

7. A CloudFront distribution needs a custom TLS certificate. Where must the ACM certificate be created?
   - A. Any Region
   - B. Same Region as the S3 bucket
   - C. us-east-1
   - D. Same Region as the viewer

8. A database password must rotate automatically with minimal code. What should be used?
   - A. KMS only
   - B. Secrets Manager
   - C. Parameter Store standard parameter only
   - D. CloudHSM only

9. A public S3 bucket must become private, but selected users need temporary downloads. What should be used?
   - A. Public ACLs
   - B. Presigned URLs
   - C. Internet Gateway
   - D. S3 Inventory

10. A company needs a central dashboard for findings from GuardDuty, Inspector, and Macie. What should be used?
   - A. Security Hub
   - B. Systems Manager Session Manager
   - C. CloudFront
   - D. Route 53

### Answers

1. B. Gateway endpoints provide private S3 access without NAT.
2. C. WAF handles layer 7 web attacks.
3. B. SCPs enforce organization-level guardrails.
4. A. User Pool authenticates; Identity Pool provides temporary AWS credentials.
5. B. Macie discovers sensitive data in S3.
6. B. Inspector scans workloads and images for vulnerabilities.
7. C. CloudFront requires ACM certificates in us-east-1.
8. B. Secrets Manager is the built-in rotation answer.
9. B. Presigned URLs provide temporary access to private objects.
10. A. Security Hub aggregates security findings.

## Section 1 Final Memory Sheet

Memorize these before attempting security questions:

- Role beats access keys for AWS compute.
- Explicit deny always wins.
- SCP limits, but does not grant.
- Permission boundary limits, but does not grant.
- User Pool authenticates app users.
- Identity Pool gives temporary AWS credentials.
- IAM Identity Center is workforce SSO.
- Security Group is stateful allow-only.
- NACL is stateless allow/deny.
- Gateway endpoint is for S3/DynamoDB.
- Interface endpoint is PrivateLink-based.
- NAT Gateway gives outbound internet to private IPv4 resources.
- Direct Connect is not encrypted by default.
- KMS is managed key service.
- CloudHSM is dedicated customer-controlled HSM.
- Secrets Manager is for rotation.
- Macie finds sensitive data in S3.
- GuardDuty detects threats.
- Inspector scans vulnerabilities.
- Detective investigates findings.
- Security Hub aggregates findings.
- WAF blocks web attacks.
- Shield protects from DDoS.

# Section 2: Design Resilient Architectures

Exam weight: 26%.

This section is about keeping applications available, recoverable, and fault-tolerant. The exam usually asks resilience through scenario wording:

- Can the architecture survive an Availability Zone failure?
- Can it survive a Regional failure?
- What happens if one component becomes slow or unavailable?
- What is the required RTO and RPO?
- Should traffic fail over automatically?
- Should messages be lost, retried, or isolated?
- Which option improves availability without adding unnecessary complexity?

## 1. Resilience Mental Model

### What The Exam Is Testing

Resilience is not only "use more servers." The exam wants you to understand where failures happen and which AWS pattern handles that failure.

| Failure type | Exam-friendly pattern |
|---|---|
| One EC2 instance fails | Auto Scaling replaces it |
| One target becomes unhealthy | Load balancer health check stops routing to it |
| One Availability Zone fails | Multi-AZ deployment |
| One Region fails | Multi-Region DR or active-active design |
| Database writer fails | Multi-AZ failover / Aurora failover |
| Consumer application fails | Queue buffers work |
| One message repeatedly fails | Dead-letter queue |
| One dependency is slow | Decoupling, retries, backoff, queueing |
| One DNS target fails | Route 53 failover routing with health checks |

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Survive AZ failure | Deploy across multiple AZs |
| Web tier highly available | ALB plus Auto Scaling group across AZs |
| Database automatic failover in one Region | RDS Multi-AZ or Aurora Multi-AZ |
| Read scaling for relational DB | Read Replica, not Multi-AZ standby |
| Regional disaster recovery for relational DB | Aurora Global Database or cross-Region read replica depending requirement |
| NoSQL multi-Region active-active | DynamoDB Global Tables |
| Lowest-cost DR with hours acceptable | Backup and restore |
| Core services running, scale during disaster | Pilot light |
| Reduced-size full environment already running | Warm standby |
| Near-zero downtime, both Regions active | Active-active / multi-site |

### Exam-Related Examples

Example 1: A web app runs on one EC2 instance in one AZ and must survive an AZ failure.

Use an Auto Scaling group spanning at least two AZs behind an ALB. One larger EC2 instance is not resilient. Two instances in the same AZ still fail if that AZ fails.

Example 2: A database needs automatic failover inside one Region but does not need read scaling.

Use RDS Multi-AZ. Do not choose Read Replica just because it creates another database copy. Read replicas are mainly for read scaling and may require manual promotion depending engine and setup.

Example 3: The question says "Regional outage" and "very low RPO/RTO" for a relational database.

Think Aurora Global Database. Multi-AZ only protects against AZ failure inside a Region. It does not solve full Regional outage.

### Common Resilience Traps

- Multi-AZ is not the same as Multi-Region.
- Scaling is not the same as high availability.
- Read replicas are not the default answer for failover.
- Backups are not high availability; they are recovery.
- A queue improves resilience by absorbing failures and spikes, but it does not make processing instant.
- An ALB in front of only one AZ is still a weak architecture.

## 2. Multi-AZ Compute And Load Balancing

### What The Exam Is Testing

Compute resilience usually means spreading stateless application instances across AZs and routing only to healthy targets.

Core services:

- Elastic Load Balancing distributes traffic across targets.
- Auto Scaling replaces failed instances and adjusts capacity.
- Health checks decide which targets receive traffic.
- Launch templates define repeatable EC2 configuration.
- Multi-AZ subnets avoid a single-AZ design.

### ALB, NLB, And Resilience

| Need | Choose |
|---|---|
| HTTP/HTTPS app, path/host routing, health checks | ALB |
| TCP/UDP, static IP, very low latency | NLB |
| Appliance-style traffic inspection | Gateway Load Balancer |

For resilience questions, the load balancer is normally paired with targets in multiple AZs. A load balancer alone is not enough if all targets live in one AZ.

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Replace failed EC2 automatically | Auto Scaling group |
| Stop sending traffic to unhealthy instances | Load balancer health checks |
| Survive one AZ failure for web tier | ALB plus ASG across at least two AZs |
| Stateless web app session loss after replacement | Store session state outside EC2 |
| Initialization takes time and scaling overshoots | Instance warmup or warm pool |
| Need blue/green or rolling deployment resilience | CodeDeploy / deployment strategy with health checks |

### Exam-Related Examples

Example 1: A two-tier app has web servers in one public subnet and a database in one private subnet. It needs high availability.

For the web tier, create public subnets in multiple AZs, put an ALB across those AZs, and run an Auto Scaling group across those AZs. For the database, use RDS Multi-AZ in private subnets across AZs.

Example 2: Instances are replaced by Auto Scaling, but users lose login sessions.

The compute layer is not stateless enough. Store sessions in ElastiCache, DynamoDB, or another external store. Do not rely on instance memory for resilient web sessions.

Example 3: New instances need 5 minutes to start serving traffic, and burst traffic causes timeouts.

Use Auto Scaling warmup, lifecycle hooks, or warm pools depending the options. The key is that instances should not be counted as healthy capacity before they are ready.

### Common Compute Resilience Traps

- Auto Scaling across one AZ does not protect against AZ failure.
- An Elastic IP on one EC2 instance is not high availability.
- Manual recovery is usually not the best answer when automatic health checks are available.
- Stateful app data on local instance storage reduces resilience.
- Instance store data is lost when the instance stops, hibernates, or terminates depending event.

## 3. Database Resilience

### What The Exam Is Testing

Database questions are among the most common resilience traps. The exam heavily tests the difference between failover, read scaling, backup, and cross-Region recovery.

### RDS Multi-AZ vs Read Replica

| Requirement | Choose |
|---|---|
| Automatic failover in same Region | RDS Multi-AZ |
| Standby should not serve read traffic | RDS Multi-AZ |
| Offload reporting/read queries | Read Replica |
| Scale reads horizontally | Read Replica |
| Cross-Region read copy / DR option | Cross-Region Read Replica |
| Improve write performance | Usually not a read replica or Multi-AZ |

### Aurora Resilience

| Requirement | Choose |
|---|---|
| MySQL/PostgreSQL-compatible managed relational DB with high availability | Aurora |
| Separate read traffic from writes | Aurora replicas and reader endpoint |
| Cross-Region relational DR with low RPO/RTO | Aurora Global Database |
| Serverless relational scaling | Aurora Serverless v2, if scenario fits |

### DynamoDB Resilience

| Requirement | Choose |
|---|---|
| Regional managed NoSQL durability | DynamoDB standard table |
| Multi-Region active-active NoSQL | DynamoDB Global Tables |
| Point-in-time restore | DynamoDB PITR |
| Event-driven processing after table changes | DynamoDB Streams |
| Microsecond read cache | DAX |

### Exam-Related Examples

Example 1: Reporting queries are slowing down production writes on RDS.

Use a Read Replica. Multi-AZ is for standby failover, not for serving reporting reads.

Example 2: A production RDS database needs automatic failover inside a Region.

Use RDS Multi-AZ. A snapshot is useful for backup but does not provide automatic failover.

Example 3: A global application requires active-active NoSQL access in multiple Regions.

Use DynamoDB Global Tables. S3 replication or RDS Multi-AZ is not the right service pattern.

Example 4: A relational database must recover from a Regional outage with very low RPO and RTO.

Use Aurora Global Database when available in the options. It is built for low-latency cross-Region replication and faster disaster recovery.

### Common Database Resilience Traps

- RDS Multi-AZ standby is not used for normal read traffic.
- A Read Replica is not the same as a synchronous standby.
- Backups and snapshots help recovery but do not provide immediate high availability.
- DynamoDB Global Tables are for active-active multi-Region NoSQL.
- Aurora Global Database is a strong answer for relational multi-Region DR.
- ElastiCache Redis can support Multi-AZ failover; Memcached is simpler and does not provide the same HA/persistence model.

## 4. Decoupling With Queues, Topics, And Events

### What The Exam Is Testing

Decoupling questions usually describe spikes, slow downstream systems, independent consumers, retry requirements, or failure isolation.

Core idea: if a producer should not wait for a consumer, place a buffer or event layer between them.

### Service Selection

| Requirement | Choose |
|---|---|
| Durable queue between producer and worker | SQS Standard |
| Ordered messages / deduplication | SQS FIFO |
| Fan-out same event to multiple subscribers | SNS |
| Durable fan-out to multiple workers | SNS topic to multiple SQS queues |
| Route events by rule patterns | EventBridge |
| Workflow with state, branching, retries | Step Functions |
| Existing ActiveMQ/RabbitMQ app migration | Amazon MQ |
| Replayable streaming data | Kinesis Data Streams |
| Managed delivery to S3/Redshift/OpenSearch | Kinesis Data Firehose |

### SQS Concepts The Exam Likes

| Concept | Meaning |
|---|---|
| Visibility timeout | Time a message is hidden after a consumer receives it |
| Dead-letter queue | Stores messages that fail repeatedly |
| Long polling | Reduces empty responses and cost |
| Standard queue | High throughput, best-effort ordering, at-least-once delivery |
| FIFO queue | Ordering and deduplication, lower throughput than Standard unless high-throughput FIFO is configured |

### Exam-Related Examples

Example 1: A voting app receives huge bursts. The front end writes directly to RDS, and the database cannot keep up.

Put SQS between the front end and worker fleet. The front end can accept votes quickly, while workers process at a rate the database can handle. This prevents losing requests during spikes.

Example 2: Billing, fulfillment, and analytics must each receive every order event independently.

Use SNS fan-out to separate SQS queues. One queue per consumer isolates failures. If analytics is down, billing can continue.

Example 3: Messages fail repeatedly because of bad payloads.

Use an SQS dead-letter queue. Do not let poison messages block normal processing forever.

Example 4: A payment process has multiple steps, retries, waits, and branching decisions.

Use Step Functions. SQS stores work, but Step Functions orchestrates workflow state.

### Common Decoupling Traps

- SNS alone is not durable storage for offline consumers. Pair SNS with SQS when durable fan-out is required.
- SQS Standard does not guarantee strict ordering.
- FIFO is chosen when ordering/deduplication matters.
- EventBridge is event routing, not a queue replacement for worker backlogs.
- Step Functions is orchestration, not just notification.
- Kinesis Data Streams is for streaming/replayable records, not simple task queues.

## 5. DNS, Health Checks, And Failover

### What The Exam Is Testing

DNS resilience questions often ask how users should be routed when an endpoint, AZ, or Region becomes unhealthy.

### Route 53 Routing Policies

| Requirement | Choose |
|---|---|
| Active-passive failover | Failover routing |
| Split traffic by percentage | Weighted routing |
| Route to lowest-latency Region | Latency-based routing |
| Route by user country/continent | Geolocation routing |
| Route by distance and bias | Geoproximity routing |
| Return multiple healthy records | Multivalue answer |

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Send users to secondary only if primary unhealthy | Route 53 failover routing |
| Active-active across Regions with health checks | Route 53 latency/weighted plus health checks, or Global Accelerator depending service |
| Static Anycast IPs and fast regional failover | Global Accelerator |
| HTTP content delivery with edge caching | CloudFront |
| Apex domain to ALB or CloudFront | Route 53 Alias record |

### Exam-Related Examples

Example 1: A company has a primary web app in one Region and a standby app in another Region. DNS should send traffic to standby only if primary health checks fail.

Use Route 53 failover routing with health checks.

Example 2: Users need static global IPs and fast failover for a TCP application.

Use Global Accelerator. CloudFront is mainly for HTTP/HTTPS caching and edge delivery, not generic TCP/UDP acceleration.

Example 3: A root domain must point to an ALB.

Use a Route 53 Alias A record. Do not use a CNAME at the zone apex.

### Common DNS Resilience Traps

- DNS TTL can affect failover speed.
- Route 53 failover requires health checks.
- CloudFront and Global Accelerator are not interchangeable.
- Weighted routing is not automatically failover unless health checks are configured.
- Alias records are the AWS-friendly answer for apex records pointing to AWS resources.

## 6. Storage, Backup, And Recovery

### What The Exam Is Testing

Storage resilience questions ask about durability, replication, versioning, recovery point, and centralized backup policy.

### S3 Resilience Patterns

| Requirement | Choose |
|---|---|
| Protect against accidental delete/overwrite | S3 Versioning |
| Replicate objects to another Region | S3 Cross-Region Replication |
| Replicate within same Region | S3 Same-Region Replication |
| Compliance retention | S3 Object Lock |
| Unknown access pattern and cost optimization | S3 Intelligent-Tiering |
| Critical infrequent data across multiple AZs | S3 Standard-IA |
| Infrequent data that can be recreated | S3 One Zone-IA |

### Backup Services And Features

| Requirement | Choose |
|---|---|
| Central backup plans across services | AWS Backup |
| EBS recovery point | EBS snapshot |
| RDS recovery point | Automated backups / snapshots |
| DynamoDB point-in-time restore | DynamoDB PITR |
| File system backup | AWS Backup or service-native backup |
| Prevent backup deletion by admins | AWS Backup Vault Lock |

### Exam-Related Examples

Example 1: A company needs a central way to apply backup policies to EBS, RDS, DynamoDB, and EFS.

Use AWS Backup. Individual service snapshots can work, but AWS Backup is the central policy service.

Example 2: A user accidentally overwrites an S3 object and the company must restore the earlier version.

Use S3 Versioning. Replication alone does not replace versioning for accidental overwrites.

Example 3: S3 objects must be available in another Region for disaster recovery.

Use S3 Cross-Region Replication. Remember that versioning must be enabled for replication.

Example 4: A database can tolerate several hours of downtime and data loss, and the company wants the lowest-cost disaster recovery strategy.

Use backup and restore. This is slower but cheapest.

### Common Backup And Storage Traps

- Backup and restore is not high availability.
- Versioning protects against overwrite/delete mistakes.
- Cross-Region Replication helps Regional DR but does not automatically make an application active-active.
- EFS Standard stores data across multiple AZs; EFS One Zone is lower cost but less resilient.
- AWS Backup centralizes backup policy but does not magically make apps multi-Region active-active.

## 7. Disaster Recovery Strategy Mapping

### What The Exam Is Testing

DR questions almost always give RTO/RPO and cost constraints. Match the strategy to the recovery requirement.

| DR strategy | Cost | RTO/RPO | Exam phrase |
|---|---:|---|---|
| Backup and restore | Lowest | Hours | Cheapest DR, tolerate long recovery |
| Pilot light | Low | Tens of minutes to hours | Core components running, scale during disaster |
| Warm standby | Medium | Minutes | Scaled-down full environment already running |
| Active-active / multi-site | Highest | Near-zero | Both Regions serve production traffic |

### RTO And RPO

| Term | Meaning | Exam interpretation |
|---|---|---|
| RTO | Recovery Time Objective | How long the app can be down |
| RPO | Recovery Point Objective | How much data can be lost |

If the question says "minutes of downtime" or "near-zero data loss," backup and restore is usually wrong. If it says "lowest cost and hours are acceptable," active-active is usually overkill.

### Exam-Related Examples

Example 1: A company can tolerate 12 hours of downtime and wants the cheapest DR plan.

Use backup and restore.

Example 2: A company wants only the critical database and configuration replicated, and it can scale application servers during disaster.

Use pilot light.

Example 3: A company has a smaller full environment already running in another Region and scales it up during disaster.

Use warm standby.

Example 4: A global application must continue with almost no interruption if one Region fails.

Use active-active or multi-site, usually with Route 53/Global Accelerator, data replication, and services designed for multi-Region access.

### Common DR Traps

- Multi-AZ is not DR for Regional failure.
- Snapshots alone do not meet low RTO.
- Active-active is rarely the lowest-cost answer.
- Pilot light is not a full scaled-down running environment; that is warm standby.
- RPO is about data loss, not app startup time.
- RTO is about recovery time, not data replication lag.

## 8. Resilient Application Design Patterns

### What The Exam Is Testing

The exam prefers architectures that remove single points of failure and degrade gracefully.

| Pattern | Why It Helps |
|---|---|
| Stateless compute | Instances can be replaced anytime |
| External session store | Users do not lose session when one instance dies |
| Queue-based buffering | Spikes and temporary failures do not drop work |
| Idempotent processing | Retries do not create duplicate side effects |
| Health checks | Bad targets stop receiving traffic |
| Retry with backoff | Reduces pressure during partial failures |
| Dead-letter queues | Bad messages are isolated |
| Multi-AZ databases | Database tier survives AZ failure |
| Infrastructure as code | Rebuild environments consistently |

### Exam-Related Examples

Example 1: A worker sometimes processes the same SQS message twice.

Design the consumer to be idempotent. SQS Standard provides at-least-once delivery, so duplicates can happen.

Example 2: A monolithic app stores uploaded files on local EC2 disks and loses them when instances are replaced.

Move uploads to S3 or a shared file system such as EFS, depending the requirement. Local instance storage is not resilient application storage.

Example 3: A workload requires shared Linux file storage across multiple AZs.

Use EFS Standard. EFS One Zone is less resilient and should only be chosen when the question accepts single-AZ storage for lower cost.

### Common Design Pattern Traps

- At-least-once delivery means duplicates are possible.
- Stateful EC2 instances are harder to replace.
- Health checks must match real app health, not just open ports.
- A single NAT Gateway can become an AZ dependency for other AZs; resilient VPC designs often use one NAT Gateway per AZ.
- Cross-AZ traffic can improve resilience but may add cost, which can matter in cost questions.

## Section 2 Rapid Revision Table

| If the question says... | Think... |
|---|---|
| Survive AZ failure | Multi-AZ |
| Survive Region failure | Multi-Region DR |
| Web tier HA | ALB plus ASG across AZs |
| Database automatic failover | RDS Multi-AZ / Aurora failover |
| Database read scaling | Read Replica |
| Relational low-RPO cross-Region DR | Aurora Global Database |
| NoSQL active-active multi-Region | DynamoDB Global Tables |
| Burst traffic overwhelms DB | SQS buffer and worker fleet |
| Same event to many independent consumers | SNS to multiple SQS queues |
| Poison messages | Dead-letter queue |
| Ordered queue processing | SQS FIFO |
| Event routing by rules | EventBridge |
| Workflow retries/branching | Step Functions |
| Active-passive DNS | Route 53 failover routing |
| Static global IPs and fast failover | Global Accelerator |
| Restore overwritten S3 object | S3 Versioning |
| Regional copy of S3 objects | S3 Cross-Region Replication |
| Central backup policy | AWS Backup |
| Cheapest DR, hours acceptable | Backup and restore |
| Core components running only | Pilot light |
| Scaled-down full environment | Warm standby |
| Near-zero downtime | Active-active / multi-site |

## Section 2 Mini Exam Drill

1. A web application must remain available if one Availability Zone fails. Which design is best?
   - A. One EC2 instance with an Elastic IP
   - B. Auto Scaling group across multiple AZs behind an ALB
   - C. Two EC2 instances in one subnet
   - D. One larger EC2 instance

2. A relational database needs automatic failover in the same Region but does not need read scaling. What should be enabled?
   - A. RDS Multi-AZ
   - B. RDS Read Replica only
   - C. DynamoDB DAX
   - D. S3 Versioning

3. Reporting queries are slowing down a production RDS database. What should be added?
   - A. Multi-AZ standby
   - B. Read Replica
   - C. NAT Gateway
   - D. AWS WAF

4. A workload needs active-active NoSQL access in multiple Regions. Which service feature fits?
   - A. DynamoDB Global Tables
   - B. RDS Multi-AZ
   - C. EBS snapshots
   - D. S3 One Zone-IA

5. A front-end application receives sudden spikes and the database cannot process writes fast enough. No requests should be lost. What should be used?
   - A. SQS queue with workers
   - B. Direct writes only
   - C. CloudTrail
   - D. Route 53 geolocation

6. Three independent systems must each receive every order event. One system failing must not block the others. Which design is best?
   - A. SNS topic fan-out to separate SQS queues
   - B. One SQS queue shared by all systems
   - C. One Lambda function calling all systems synchronously
   - D. One NAT Gateway

7. DNS should send users to a standby Region only when the primary Region fails health checks. Which Route 53 policy should be used?
   - A. Failover routing
   - B. Simple routing
   - C. Geoproximity routing
   - D. Weighted routing without health checks

8. An application can tolerate several hours of downtime and data loss. The company wants the lowest-cost DR strategy. Which strategy fits?
   - A. Active-active
   - B. Warm standby
   - C. Backup and restore
   - D. Multi-site

9. A company has a smaller full environment already running in another Region and will scale it up during disaster. Which DR strategy is this?
   - A. Backup and restore
   - B. Pilot light
   - C. Warm standby
   - D. No DR

10. A team wants to restore an earlier version of an S3 object after accidental overwrite. What must be enabled?
   - A. S3 Versioning
   - B. S3 Transfer Acceleration
   - C. S3 CORS
   - D. S3 Inventory only

### Answers

1. B. Multi-AZ ASG behind an ALB removes single-AZ and single-instance dependency.
2. A. RDS Multi-AZ provides automatic failover in a Region.
3. B. Read replicas offload read traffic.
4. A. DynamoDB Global Tables support multi-Region active-active NoSQL.
5. A. SQS buffers writes and lets workers process at a controlled rate.
6. A. SNS to separate SQS queues gives durable independent consumers.
7. A. Route 53 failover routing supports active-passive DNS with health checks.
8. C. Backup and restore is lowest cost with slower recovery.
9. C. Warm standby is a scaled-down full environment already running.
10. A. Versioning keeps prior object versions for restore.

## Section 2 Final Memory Sheet

Memorize these before resilience questions:

- Multi-AZ protects against AZ failure.
- Multi-Region protects against Regional failure.
- ALB plus ASG across AZs is the standard web HA pattern.
- RDS Multi-AZ is for failover.
- Read Replica is for read scaling.
- Aurora Global Database is for low-RPO relational cross-Region DR.
- DynamoDB Global Tables are for active-active NoSQL across Regions.
- SQS decouples producers and consumers.
- DLQ isolates repeatedly failing messages.
- SNS plus SQS gives durable fan-out.
- EventBridge routes events by rules.
- Step Functions orchestrates workflows.
- Route 53 failover is active-passive DNS.
- Backup and restore is cheapest but slowest.
- Pilot light keeps core components running.
- Warm standby keeps a smaller full environment running.
- Active-active gives the best continuity but highest cost/complexity.
- S3 Versioning protects against accidental overwrite/delete.
- S3 CRR supports cross-Region object replication.
- AWS Backup centralizes backup policies.

# Section 3: Design High-Performing Architectures

Exam weight: 24%.

This section is about selecting the architecture that meets latency, throughput, scale, and workload-shape requirements. The exam usually asks performance through clues like:

- Users are global and need lower latency.
- Reads are overwhelming a database.
- The workload needs microsecond or millisecond access.
- Traffic spikes at predictable or unpredictable times.
- A batch job exceeds Lambda limits.
- A streaming system needs replay, custom consumers, or simple delivery.
- The question asks for TCP/UDP acceleration, HTTP caching, or static IPs.

## 1. Performance Mental Model

### What The Exam Is Testing

High-performing architecture means choosing the right service for the bottleneck. Do not blindly "scale up." First identify whether the bottleneck is compute, database reads, database writes, network latency, object transfer, queue backlog, or analytics query pattern.

| Bottleneck or goal | Exam-friendly solution |
|---|---|
| Global HTTP content latency | CloudFront |
| Global TCP/UDP acceleration with static IPs | Global Accelerator |
| Relational read pressure | Read Replica / Aurora replicas |
| DynamoDB microsecond reads | DAX |
| Repeated database reads | ElastiCache |
| Huge object uploads | S3 multipart upload |
| Global S3 uploads | S3 Transfer Acceleration |
| Real-time replayable stream | Kinesis Data Streams |
| Managed stream delivery to S3/Redshift/OpenSearch | Kinesis Data Firehose |
| Ad-hoc SQL over S3 | Athena |
| Recurring BI/data warehouse analytics | Redshift |
| Long-running compute job | ECS/Fargate, AWS Batch, or EC2 |
| Short event-driven job | Lambda |

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Static website/images/scripts for global users | CloudFront |
| Static Anycast IPs, TCP/UDP, global failover | Global Accelerator |
| URL path or host routing | ALB |
| TCP/UDP, very low latency, static IP | NLB |
| Third-party firewall appliance | Gateway Load Balancer |
| Read-heavy RDS workload | Read Replica |
| Aurora reads slow writes | Aurora replicas plus reader endpoint |
| DynamoDB read latency must be microseconds | DAX |
| Relational DB repeated reads need caching | ElastiCache Redis or Memcached |
| SQL directly on S3, occasional | Athena |
| BI analytics over structured data at scale | Redshift |

### Exam-Related Examples

Example 1: A global website serves static images and JavaScript to users around the world.

Use CloudFront. It caches HTTP/HTTPS content at edge locations. Do not choose Direct Connect or Global Accelerator unless the question specifically asks for private connectivity, TCP/UDP acceleration, or static Anycast IPs.

Example 2: A gaming application uses UDP and customers need two static global IP addresses for allow-listing.

Use Global Accelerator. CloudFront is not the best answer for generic UDP traffic.

Example 3: A relational database is slow because reporting queries are consuming read capacity.

Use Read Replicas, or Aurora replicas with the reader endpoint for Aurora. Multi-AZ is a failover feature, not a read-scaling feature.

### Common Performance Traps

- Scaling up is often less exam-friendly than using a managed scaling/caching/read-replica pattern.
- Multi-AZ improves availability, not read performance for standard RDS standby.
- CloudFront and Global Accelerator solve different problems.
- DAX is specifically for DynamoDB.
- ElastiCache is a general cache, not a DynamoDB-specific accelerator.
- Lambda has a maximum execution duration; long jobs need a different compute service.

## 2. Load Balancing And Traffic Performance

### What The Exam Is Testing

The exam expects you to map protocol and routing requirements to the right load balancer.

### Load Balancer Selection

| Requirement | Choose |
|---|---|
| HTTP/HTTPS layer 7 routing | Application Load Balancer |
| Host-based or path-based routing | Application Load Balancer |
| WebSocket or HTTP/2 support | Application Load Balancer |
| TCP/UDP/TLS layer 4 load balancing | Network Load Balancer |
| Static IP addresses for load balancer | Network Load Balancer |
| Very low latency, high throughput | Network Load Balancer |
| Appliance fleet for traffic inspection | Gateway Load Balancer |
| Legacy EC2-Classic style support | Classic Load Balancer, only if forced by old scenario |

### Exam-Related Examples

Example 1: `/api` should route to one target group and `/images` to another.

Use ALB because path-based routing is layer 7.

Example 2: A TCP service needs static IPs and very low latency.

Use NLB. ALB does not provide static IPs in the same direct way and is layer 7.

Example 3: Traffic must pass through a third-party firewall appliance fleet.

Use Gateway Load Balancer. It is designed for appliance-style inspection deployments.

### Common Load Balancer Traps

- ALB is not for UDP.
- NLB does not do path-based routing.
- GWLB is not the default web app load balancer.
- Route 53 can distribute DNS, but it does not replace application-layer load balancing.
- CloudFront can sit in front of ALB for caching/edge performance, but it is not the same as ALB.

## 3. Edge Performance: CloudFront, Global Accelerator, Route 53

### What The Exam Is Testing

Global-performance questions often give clues about protocol, caching, and IP requirements.

| Requirement | Choose |
|---|---|
| Cache HTTP/HTTPS content near users | CloudFront |
| Private S3 origin through CDN | CloudFront with OAC |
| Signed URL/cookie for private content | CloudFront signed URL/cookie |
| Dynamic API acceleration with edge network | CloudFront can help for HTTP/S |
| Static global Anycast IPs | Global Accelerator |
| TCP/UDP acceleration | Global Accelerator |
| DNS routing by latency or geography | Route 53 routing policy |
| Apex domain to AWS resource | Route 53 Alias record |

### CloudFront Performance Features

| Feature | Exam meaning |
|---|---|
| Cache behavior | Controls path pattern, origin, methods, TTL, policies |
| Origin Shield | Additional caching layer to reduce origin load |
| OAC | Keeps S3 origin private behind CloudFront |
| Signed URLs/cookies | Restrict access to private content |
| Lambda@Edge / CloudFront Functions | Run lightweight logic at edge; know use-case level |
| Field-level encryption | Protect sensitive fields before they reach origin |

### Exam-Related Examples

Example 1: A global app serves static assets from S3 and users complain about latency.

Use CloudFront with S3 as origin. This is the classic edge-cache performance answer.

Example 2: A global TCP app requires fixed IP addresses for partner allow lists.

Use Global Accelerator. Route 53 DNS names are not the same as static Anycast IPs.

Example 3: Users should be routed to the lowest-latency Region.

Use Route 53 latency-based routing. If the question emphasizes static IPs and fast global failover, shift to Global Accelerator.

### Common Edge Traps

- CloudFront custom TLS certificates must be in `us-east-1`.
- CloudFront is mainly HTTP/HTTPS CDN behavior.
- Global Accelerator is not a cache.
- Route 53 routing policies affect DNS answers, not content caching.
- S3 Transfer Acceleration speeds uploads to S3; it is not the same as CloudFront.

## 4. Database And Cache Performance

### What The Exam Is Testing

The exam heavily tests read scaling, caching, and choosing the right database for access pattern.

### Relational Performance

| Requirement | Choose |
|---|---|
| Read-heavy RDS workload | Read Replica |
| Aurora read/write separation | Aurora replicas and reader endpoint |
| Connection pooling for Lambda/app spikes to RDS | RDS Proxy |
| Multi-Region relational low-latency reads / DR | Aurora Global Database |
| Serverless relational capacity scaling | Aurora Serverless v2 |
| Improve failover, not reads | RDS Multi-AZ |

### DynamoDB Performance

| Requirement | Choose |
|---|---|
| Key-value access at massive scale | DynamoDB |
| Unpredictable traffic, no capacity planning | On-demand capacity |
| Predictable traffic, tune throughput | Provisioned capacity with auto scaling |
| Microsecond eventually consistent reads | DAX |
| Query alternate attributes | GSI |
| Strong alternate query created with table | LSI |
| Multi-Region active-active | Global Tables |
| React to item changes | DynamoDB Streams |

### Cache Selection

| Requirement | Choose |
|---|---|
| Cache relational query results / sessions | ElastiCache |
| Redis data structures, persistence, replication | ElastiCache Redis / Valkey |
| Simple distributed cache, multi-threaded | ElastiCache Memcached |
| Durable Redis-compatible in-memory database | MemoryDB |
| DynamoDB-specific read acceleration | DAX |
| Global HTTP object cache | CloudFront |

### Exam-Related Examples

Example 1: A DynamoDB app has read-heavy access and needs microsecond read latency.

Use DAX. ElastiCache can cache many things, but DAX is purpose-built for DynamoDB.

Example 2: A relational app sees many repeated reads for user profile data.

Use ElastiCache in front of the database. If the question says Redis features, persistence, replication, or sorted sets, choose Redis/Valkey. If it says simple multi-threaded cache, Memcached may fit.

Example 3: An Aurora database has write latency because reads are competing with writes.

Add Aurora replicas and direct reads to the reader endpoint.

Example 4: Lambda functions create too many database connections to RDS.

Use RDS Proxy. It pools and manages connections, improving performance and resilience for spiky serverless access.

### Common Database Performance Traps

- Read Replica improves reads; it does not increase write capacity.
- Multi-AZ improves availability; it is not the answer for reporting read traffic.
- DAX is not for RDS.
- ElastiCache is not a durable primary database unless the question points to MemoryDB for Redis-compatible durability.
- Poor DynamoDB partition-key design can cause hot partitions; choose high-cardinality keys.
- GSI can be added after table creation; LSI must be created with the table.

## 5. Compute Performance And Workload Fit

### What The Exam Is Testing

Compute questions usually ask you to match workload shape to compute model.

| Workload shape | Choose |
|---|---|
| Short event-driven task under Lambda limit | Lambda |
| Long-running containerized task | ECS/Fargate or ECS on EC2 |
| Container without managing servers | Fargate |
| Batch jobs with queues and managed compute | AWS Batch |
| Full control over OS/runtime/placement | EC2 |
| Platform deployment with less management | Elastic Beanstalk |
| Kubernetes requirement | EKS |
| High-performance computing low latency | EC2 cluster placement group, EFA if available |

### Lambda Performance Concepts

| Concept | Exam meaning |
|---|---|
| Timeout limit | Lambda is not for very long-running jobs |
| Memory setting | Also affects CPU allocation |
| Provisioned concurrency | Reduces cold starts for predictable traffic |
| Reserved concurrency | Caps/guarantees concurrency for a function |
| Alias weighted routing | Shift traffic between immutable versions |

### EC2 Placement Groups

| Placement group | Use case |
|---|---|
| Cluster | Low latency / high throughput between instances |
| Spread | Small number of critical instances across distinct hardware |
| Partition | Large distributed systems such as Hadoop/Cassandra/Kafka across partitions |

### Exam-Related Examples

Example 1: A video encoding Lambda runs for 25 minutes.

Move to AWS Batch, ECS/Fargate, or EC2. You cannot simply increase Lambda timeout beyond its service limit.

Example 2: An HPC workload needs very low latency between EC2 instances.

Use a cluster placement group. If EFA is mentioned as an option, it is also relevant for tightly coupled HPC.

Example 3: A big data workload must spread nodes across isolated infrastructure groups.

Use a partition placement group. Cluster placement is for low-latency grouping; spread is for a small number of critical instances.

Example 4: A Lambda deployment should send 10% of traffic to a new function version.

Use a Lambda alias with weighted routing.

### Common Compute Performance Traps

- Lambda is not for long-running jobs.
- Increasing instance size is not always better than Auto Scaling or the right managed service.
- Cluster placement groups can improve latency but may reduce placement flexibility.
- Fargate removes server management but may not fit every specialized host requirement.
- ECS and EKS are not automatically better than Lambda; use workload shape.

## 6. Storage And File Performance

### What The Exam Is Testing

Storage performance questions usually focus on object upload, shared file systems, HPC file systems, and EBS volume fit.

### S3 Performance Patterns

| Requirement | Choose |
|---|---|
| Large object upload | Multipart upload |
| Upload to S3 from global clients | S3 Transfer Acceleration |
| Retrieve only part of large object | Byte-range fetch |
| Query subset of object data | S3 Select, if available in options |
| Event-driven processing after upload | S3 event notifications |

### File And Block Storage Performance

| Requirement | Choose |
|---|---|
| Single-instance block storage | EBS |
| Temporary high IOPS local storage | Instance store |
| Shared Linux NFS file system | EFS |
| Shared Windows SMB with AD | FSx for Windows File Server |
| High-performance Lustre file system for HPC | FSx for Lustre |
| Temporary HPC scratch data | FSx for Lustre Scratch |
| Durable repeated HPC workloads | FSx for Lustre Persistent |

### EBS Exam Clues

| Requirement | Likely choice |
|---|---|
| General purpose SSD | gp3 |
| Highest IOPS / critical database | io2 / io2 Block Express |
| Throughput-oriented large sequential data | st1 |
| Cold, low-cost HDD | sc1 |
| Need EBS data after instance stop | EBS, not instance store |

### Exam-Related Examples

Example 1: A 20 GB file must be uploaded to S3 reliably.

Use multipart upload. Large single PUT uploads are not the right performance/reliability pattern.

Example 2: A genomics/HPC workload needs temporary high-throughput shared storage integrated with S3, and data can be recreated.

Use FSx for Lustre Scratch.

Example 3: A Windows app requires shared SMB file storage integrated with Active Directory.

Use FSx for Windows File Server, not EFS.

### Common Storage Performance Traps

- EFS is for Linux/NFS, not Windows SMB.
- FSx for Lustre is the exam answer for many HPC file-performance scenarios.
- Instance store is fast but ephemeral.
- S3 is object storage, not block storage.
- Multipart upload is a performance and reliability pattern for large S3 objects.

## 7. Streaming And Analytics Performance

### What The Exam Is Testing

Analytics questions usually ask if the workload needs real-time stream processing, simple delivery, ad-hoc SQL, or a data warehouse.

### Streaming Service Selection

| Requirement | Choose |
|---|---|
| Real-time stream with replay and custom consumers | Kinesis Data Streams |
| Managed delivery to S3/Redshift/OpenSearch | Kinesis Data Firehose |
| Real-time stream analytics with SQL/Flink | Amazon Managed Service for Apache Flink |
| Managed Apache Kafka | Amazon MSK |
| Existing broker protocols | Amazon MQ |

### Analytics Service Selection

| Requirement | Choose |
|---|---|
| Ad-hoc SQL directly over S3 | Athena |
| Recurring BI/data warehouse analytics | Redshift |
| Search and log analytics | OpenSearch |
| ETL and metadata catalog | AWS Glue |
| Governed data lake permissions | Lake Formation |
| BI dashboards | QuickSight |
| Big data Spark/Hadoop jobs | EMR |

### Exam-Related Examples

Example 1: A team needs custom consumers to process clickstream events in near real time and replay data.

Use Kinesis Data Streams. Firehose is easier for delivery, but Data Streams is the stronger answer when custom consumers and replay are required.

Example 2: A pipeline should deliver streaming records to S3 with minimal management and optional Lambda transformation.

Use Kinesis Data Firehose.

Example 3: Analysts want occasional SQL queries over files in S3 without managing servers.

Use Athena.

Example 4: The business runs repeated BI queries over structured data at scale.

Use Redshift. Athena is great for ad-hoc serverless queries over S3, but Redshift is the data warehouse pattern.

### Common Analytics Traps

- Kinesis Data Streams is not the same as Firehose.
- Firehose is delivery-focused; Data Streams is consumer/replay-focused.
- Athena queries data in S3 directly.
- Redshift is the data warehouse.
- Glue is ETL/catalog, not the query engine itself.
- OpenSearch is for search/log analytics, not relational transactions.
- MSK is Kafka; Amazon MQ is ActiveMQ/RabbitMQ style broker migration.

## 8. Performance Monitoring Signals

### What The Exam Is Testing

Some performance questions ask what metric or tool is missing before scaling can work.

| Requirement | Choose |
|---|---|
| EC2 CPU scaling | CloudWatch CPU metrics |
| EC2 memory/disk scaling | CloudWatch agent or custom metrics |
| Distributed request latency tracing | X-Ray |
| Application logs and metrics | CloudWatch Logs/Metrics |
| Lambda concurrency visibility | CloudWatch metrics |
| Database performance insights | RDS Performance Insights |
| Right-sizing recommendations | Compute Optimizer |

### Exam-Related Examples

Example 1: Auto Scaling should scale EC2 based on memory, but no memory metric is available.

Install/configure the CloudWatch agent or publish a custom metric. EC2 memory is not a default CloudWatch metric.

Example 2: A microservices app has latency across multiple services and the team needs trace visibility.

Use X-Ray for distributed tracing.

### Common Monitoring Traps

- CloudWatch default EC2 metrics include CPU, network, and disk I/O, but not memory utilization.
- CloudTrail is API audit, not performance monitoring.
- Config is compliance/config history, not request tracing.
- X-Ray traces distributed requests; it is not a general metrics dashboard.

## Section 3 Rapid Revision Table

| If the question says... | Think... |
|---|---|
| Global static website/content | CloudFront |
| Global TCP/UDP, static IPs | Global Accelerator |
| Path/host routing | ALB |
| TCP/UDP or static IP load balancer | NLB |
| Appliance traffic inspection | Gateway Load Balancer |
| RDS read-heavy | Read Replica |
| Aurora read/write separation | Aurora replicas + reader endpoint |
| DynamoDB microsecond reads | DAX |
| Repeated relational reads | ElastiCache |
| Lambda to RDS connection storm | RDS Proxy |
| Long-running job | Batch / ECS / Fargate / EC2 |
| Short event-driven task | Lambda |
| HPC low latency | Cluster placement group / EFA |
| Big data rack-style isolation | Partition placement group |
| Large S3 upload | Multipart upload |
| Global S3 upload acceleration | S3 Transfer Acceleration |
| Shared Linux file system | EFS |
| Shared Windows file system | FSx for Windows |
| HPC file system | FSx for Lustre |
| Real-time stream with replay | Kinesis Data Streams |
| Managed stream delivery | Kinesis Data Firehose |
| Ad-hoc SQL over S3 | Athena |
| Data warehouse | Redshift |
| Search/log analytics | OpenSearch |
| EC2 memory scaling metric | CloudWatch agent |

## Section 3 Mini Exam Drill

1. A global website serves static images and JavaScript. Users need low-latency downloads worldwide. What should be used?
   - A. CloudFront
   - B. Direct Connect
   - C. NAT Gateway
   - D. AWS Batch

2. A UDP gaming application needs static global IP addresses for customer allow lists. Which service is best?
   - A. CloudFront
   - B. Global Accelerator
   - C. Route 53 CNAME only
   - D. S3 Transfer Acceleration

3. A web application needs path-based routing to different target groups. Which load balancer should be used?
   - A. ALB
   - B. NLB
   - C. Gateway Load Balancer
   - D. Classic Load Balancer

4. A TCP service needs very low latency and static IP addresses. Which load balancer is best?
   - A. ALB
   - B. NLB
   - C. CloudFront
   - D. API Gateway

5. A DynamoDB application is read-heavy and needs microsecond read latency for eventually consistent reads. What should be added?
   - A. DAX
   - B. RDS Read Replica
   - C. ElastiCache Memcached for RDS
   - D. Redshift Spectrum

6. Reporting queries are slowing a relational production database. It does not need faster failover. What should be added?
   - A. Multi-AZ standby
   - B. Read Replica
   - C. S3 Object Lock
   - D. Gateway Load Balancer

7. A Lambda function is used for a video encoding job that runs for 25 minutes. What is the best change?
   - A. Increase Lambda timeout to 25 minutes
   - B. Use AWS Batch, ECS/Fargate, or EC2
   - C. Put the job behind CloudFront
   - D. Use Route 53 weighted routing

8. A high-performance computing workload needs very low network latency between EC2 instances. Which placement group is best?
   - A. Cluster placement group
   - B. Spread placement group
   - C. Partition placement group
   - D. No placement group

9. A team needs to process clickstream events in real time with custom consumer applications and replay. Which service fits?
   - A. Kinesis Data Streams
   - B. Kinesis Data Firehose only
   - C. AWS DMS
   - D. AWS Transfer Family

10. A company wants occasional SQL queries directly against data in S3 without managing servers. What should be used?
   - A. Athena
   - B. Redshift provisioned cluster only
   - C. RDS MySQL
   - D. DynamoDB

### Answers

1. A. CloudFront caches HTTP/HTTPS content at edge locations.
2. B. Global Accelerator provides static Anycast IPs and TCP/UDP acceleration.
3. A. ALB supports layer 7 path-based routing.
4. B. NLB supports TCP/UDP, static IPs, and very low latency.
5. A. DAX is the DynamoDB-specific microsecond read cache.
6. B. Read replicas offload relational read traffic.
7. B. Lambda is not for jobs beyond its maximum duration; use batch/container/EC2 compute.
8. A. Cluster placement groups support low-latency, high-throughput EC2 networking.
9. A. Kinesis Data Streams supports custom consumers and replayable streams.
10. A. Athena is serverless SQL over S3.

## Section 3 Final Memory Sheet

Memorize these before performance questions:

- CloudFront = HTTP/HTTPS caching at edge.
- Global Accelerator = static Anycast IPs and TCP/UDP acceleration.
- Route 53 = DNS routing, not caching.
- ALB = layer 7 path/host routing.
- NLB = layer 4 TCP/UDP/static IP/low latency.
- GWLB = inspection appliances.
- RDS Multi-AZ = failover; Read Replica = read scaling.
- Aurora reader endpoint sends reads to Aurora replicas.
- DAX = DynamoDB microsecond reads.
- ElastiCache = cache for repeated reads/session data.
- RDS Proxy = connection pooling for RDS/Aurora.
- Lambda is for short event-driven tasks, not long jobs.
- AWS Batch handles long-running batch workloads.
- Fargate runs containers without host management.
- Cluster placement group = HPC low latency.
- Partition placement group = distributed big-data topology.
- Multipart upload = large S3 object upload.
- Transfer Acceleration = faster global S3 uploads.
- Kinesis Data Streams = real-time stream with replay/custom consumers.
- Firehose = managed stream delivery.
- Athena = SQL over S3.
- Redshift = data warehouse.
- CloudWatch agent is needed for EC2 memory metrics.

# Section 4: Design Cost-Optimized Architectures

Exam weight: 20%.

This section is about choosing the lowest-cost architecture that still satisfies all stated requirements. The exam usually asks cost questions through clues like:

- Workload is steady for 1 or 3 years.
- Workload is interruptible or fault-tolerant.
- Data access becomes rare after a period of time.
- Traffic to S3 is going through a NAT Gateway.
- A database or cluster is idle outside business hours.
- Usage is unpredictable and the team wants no capacity planning.
- Multiple options work, but one has unnecessary idle capacity or operational waste.

## 1. Cost Optimization Mental Model

### What The Exam Is Testing

Cost questions are not asking for the cheapest service in isolation. They ask for the cheapest architecture that still meets requirements for security, durability, availability, performance, and operations.

Always ask:

- Is the workload steady, spiky, or interruptible?
- Is data frequently accessed, rarely accessed, unknown, or archival?
- Is the expensive path caused by NAT Gateway, cross-AZ transfer, cross-Region transfer, or idle compute?
- Does the question say "least operational overhead" or "most cost-effective"? These are related but not identical.
- Does a managed/serverless option remove idle capacity?

### Exam Decision Rules

| Scenario phrase | Choose |
|---|---|
| Interruptible/restartable compute | Spot Instances / Spot Fleet |
| Steady EC2 usage over 1 or 3 years | Savings Plans or Reserved Instances |
| Flexible compute usage across instance families | Compute Savings Plans |
| Specific instance family/Region commitment | EC2 Instance Savings Plans or Reserved Instances |
| Development EC2 idle overnight | Stop instances / scheduled scaling |
| Predictable daily scaling pattern | Scheduled scaling |
| Unknown S3 access pattern | S3 Intelligent-Tiering |
| Data rarely accessed but must be immediately available | S3 Standard-IA or Glacier Instant Retrieval, depending archive wording |
| Data can be recreated and is infrequently accessed | S3 One Zone-IA |
| Long-term archive, hours retrieval acceptable | S3 Glacier Flexible Retrieval |
| Cheapest archive, hours-long retrieval acceptable | S3 Glacier Deep Archive |
| Private S3 access currently through NAT | S3 Gateway VPC Endpoint |
| Unpredictable DynamoDB traffic | On-demand capacity |
| Predictable DynamoDB traffic | Provisioned capacity with auto scaling |

### Exam-Related Examples

Example 1: A nightly batch job can be interrupted and restarted.

Use Spot Instances or a Spot-based compute option. The important phrase is "interruptible/restartable." Do not choose Dedicated Hosts or On-Demand when lowest cost is the goal.

Example 2: A production service runs 24/7 with stable baseline usage.

Use Savings Plans or Reserved Instances for the steady baseline. Keep On-Demand for bursts if needed.

Example 3: A private subnet sends heavy traffic to S3 through a NAT Gateway and costs are high.

Add an S3 Gateway VPC Endpoint and route S3 traffic privately. NAT technically works, but it adds avoidable processing and data-transfer cost.

### Common Cost Traps

- "Cheapest" is wrong if it violates durability, availability, or retrieval-time requirements.
- Spot is wrong if the workload cannot tolerate interruption.
- One Zone-IA is wrong for critical data that must survive AZ loss.
- Deep Archive is wrong when immediate retrieval is required.
- NAT Gateway is convenient but can become a major cost driver.
- Capacity Reservations reserve capacity but do not provide a discount by themselves.

## 2. Compute Cost Optimization

### What The Exam Is Testing

Compute cost questions focus on matching pricing model to workload predictability and interruption tolerance.

### EC2 Pricing Models

| Requirement | Choose |
|---|---|
| Short-term, no commitment | On-Demand |
| Interruptible, fault-tolerant, restartable | Spot Instances |
| Steady usage commitment | Savings Plans or Reserved Instances |
| Flexible usage across EC2, Fargate, Lambda | Compute Savings Plans |
| Specific instance family in a Region | EC2 Instance Savings Plans |
| Need capacity guarantee | Capacity Reservation |
| Need physical server control/licensing/compliance | Dedicated Host |

### Auto Scaling Cost Patterns

| Scenario phrase | Choose |
|---|---|
| Predictable traffic spike every morning | Scheduled scaling |
| Variable demand around target metric | Target tracking scaling |
| Idle dev/test outside work hours | Stop/start schedule |
| Batch workers needed only when queue has messages | Scale from queue depth |
| New instances initialize slowly | Warm pools, but balance cost because warm capacity can cost money |

### Exam-Related Examples

Example 1: A stateless image-processing fleet can retry jobs if an instance is interrupted.

Use Spot Instances. If the answer uses Auto Scaling with mixed instances and purchase options, even better, because it can combine cost savings and availability.

Example 2: A finance application runs continuously for three years with predictable usage.

Use Savings Plans or Reserved Instances. Spot is not appropriate for steady production workloads that cannot be interrupted.

Example 3: A development EC2 instance is idle every night.

Stop it on a schedule. You still pay for attached EBS volumes, but you stop paying for EC2 instance runtime.

Example 4: A batch job runs once per night, processes data, then has no work.

Use AWS Batch, transient EMR, or scaling worker fleets that terminate after processing. The exam often rewards avoiding idle clusters.

### Common Compute Cost Traps

- Reserved capacity and reserved discount are not the same thing.
- Dedicated Hosts are usually expensive and chosen for licensing/compliance, not general cost savings.
- Spot Instances can be interrupted, so they are not the answer for strict always-on stateful workloads.
- Stopping EC2 saves compute cost but not EBS, Elastic IP, snapshot, or other attached resource costs.
- Overprovisioned Auto Scaling minimum capacity can waste money.
- Graviton can be a cost/performance answer when the workload supports ARM.

## 3. Storage Cost Optimization

### What The Exam Is Testing

Storage cost questions are very common because S3 has many storage classes and lifecycle rules. The exam wants you to map access pattern, retrieval time, durability, and minimum storage duration.

### S3 Storage Class Selection

| Requirement | Choose |
|---|---|
| Frequently accessed data | S3 Standard |
| Unknown/changing access pattern | S3 Intelligent-Tiering |
| Infrequent but critical, immediate access | S3 Standard-IA |
| Infrequent, can recreate, single-AZ acceptable | S3 One Zone-IA |
| Archive with millisecond access | S3 Glacier Instant Retrieval |
| Archive with minutes/hours retrieval | S3 Glacier Flexible Retrieval |
| Lowest-cost long-term archive | S3 Glacier Deep Archive |

### S3 Lifecycle Patterns

| Scenario phrase | Choose |
|---|---|
| Data hot for 30 days then rarely read | Lifecycle transition to IA/archive |
| Old logs kept for compliance | Lifecycle to Glacier class |
| Delete old incomplete multipart uploads | Lifecycle cleanup rule |
| Unknown access pattern | Intelligent-Tiering, not manual lifecycle guessing |
| High-volume SSE-KMS cost | S3 Bucket Keys |

### Minimum Duration Trap

S3 infrequent and archive classes can have minimum storage duration charges. If objects are deleted quickly after moving to Standard-IA, One Zone-IA, or Glacier classes, costs can be higher than expected.

### Exam-Related Examples

Example 1: Objects are accessed frequently for 20 days, then rarely for years.

Use an S3 lifecycle policy. Keep objects in Standard while hot, then transition them to lower-cost classes when access drops.

Example 2: Access pattern is unknown and changes over time.

Use S3 Intelligent-Tiering. The exam likes this answer when the question explicitly says "unknown access pattern."

Example 3: Critical backups are rarely accessed but must be immediately available.

Choose S3 Standard-IA or Glacier Instant Retrieval depending the wording. Avoid One Zone-IA if the data is critical and must tolerate AZ loss.

Example 4: The company deletes Standard-IA objects after 10 days and sees unexpected charges.

Minimum storage duration charges apply. The class was not a good fit for very short-lived data.

### EBS, EFS, And FSx Cost Patterns

| Requirement | Cost-aware choice |
|---|---|
| General EBS SSD workload | gp3 instead of overprovisioned io volumes |
| Highest IOPS database requirement | io2, but only when required |
| Shared Linux file storage with changing access | EFS lifecycle management / IA |
| Shared Windows storage | FSx for Windows, right-sized |
| Temporary HPC scratch data | FSx for Lustre Scratch |
| Durable repeated HPC data | FSx for Lustre Persistent |

### Common Storage Cost Traps

- Do not choose the cheapest storage class if retrieval time or AZ durability requirement fails.
- One Zone-IA is lower cost but lower resilience.
- Deep Archive is cheap but not immediate.
- Lifecycle rules should match access timing and minimum duration rules.
- EBS volumes continue costing money when EC2 instances are stopped.
- Old snapshots, unattached EBS volumes, and incomplete multipart uploads are common hidden costs.

## 4. Network And Data Transfer Cost

### What The Exam Is Testing

Networking cost questions usually hide in architectures that technically work but route traffic through an expensive path.

### Common Network Cost Drivers

| Cost driver | Optimization |
|---|---|
| Private subnets accessing S3 through NAT | S3 Gateway VPC Endpoint |
| Private subnets accessing DynamoDB through NAT | DynamoDB Gateway VPC Endpoint |
| Private access to other AWS services through NAT | Interface VPC Endpoint, if cost fits |
| Cross-AZ traffic between app and database | Keep chatty components in same AZ where possible while preserving HA |
| Cross-Region replication/transfer | Replicate only required data |
| Internet egress from origin | CloudFront caching can reduce repeated origin fetches |
| NAT Gateway per-GB processing | Avoid unnecessary NAT paths |

### Gateway Endpoint vs NAT Gateway

| Requirement | Better answer |
|---|---|
| Private S3 access from VPC | S3 Gateway Endpoint |
| Private DynamoDB access from VPC | DynamoDB Gateway Endpoint |
| General outbound internet for private subnet | NAT Gateway |
| Avoid NAT cost for supported AWS services | VPC endpoint |

### Exam-Related Examples

Example 1: EC2 instances in private subnets download many objects from S3 through NAT Gateway.

Use an S3 Gateway Endpoint. This keeps traffic private and avoids NAT Gateway data processing for S3 traffic.

Example 2: A workload transfers large amounts of data between AZs because app servers and databases are not aligned.

The cost-aware answer may place resources carefully across AZs or reduce chatty cross-AZ traffic while still meeting high-availability requirements. Do not collapse everything into one AZ if the question requires resilience.

Example 3: A global website repeatedly serves the same images from an S3 origin.

CloudFront can improve latency and reduce repeated origin data transfer by caching at edge locations.

### Common Network Cost Traps

- NAT Gateway is not free; both hourly and processing charges can matter.
- "More NAT Gateways" can improve AZ independence, but it is not the cost fix for S3/DynamoDB traffic.
- Interface endpoints have hourly/data charges, so they are not automatically cheaper in every case.
- Cross-AZ and cross-Region data transfer can dominate cost for chatty systems.
- Public internet paths may violate security requirements even if they seem cheaper.

## 5. Database Cost Optimization

### What The Exam Is Testing

Database cost questions focus on matching capacity model and engine to workload pattern.

### DynamoDB Cost Patterns

| Requirement | Choose |
|---|---|
| Unpredictable/spiky traffic, no planning | On-demand capacity |
| Predictable traffic | Provisioned capacity with auto scaling |
| Read-heavy with repeated reads | DAX if DynamoDB-specific microsecond reads are required |
| Remove old items automatically | TTL |
| Avoid over-fetching | Query with good key design, not scans |
| Multi-Region active-active | Global Tables, only when required |

### RDS And Aurora Cost Patterns

| Requirement | Choose |
|---|---|
| Steady long-running RDS usage | RDS Reserved Instance |
| Variable relational capacity | Aurora Serverless v2, if scenario fits |
| Dev/test DB idle temporarily | Stop RDS temporarily |
| Dev/test DB not needed for weeks | Snapshot and delete, then restore later if acceptable |
| Read-heavy workload | Read Replica may be cheaper than scaling primary vertically |
| Too many idle read replicas | Remove unused replicas |

### Analytics Database Cost Patterns

| Requirement | Choose |
|---|---|
| Occasional SQL over S3 | Athena |
| Regular high-volume BI warehouse | Redshift |
| Avoid always-on cluster for intermittent analytics | Serverless option if available and requirements fit |
| Temporary Spark processing | Transient EMR cluster |

### Exam-Related Examples

Example 1: A DynamoDB table has unpredictable traffic and the team wants to avoid capacity planning.

Use on-demand capacity. Provisioned capacity can be cheaper for predictable workloads, but it requires planning.

Example 2: A development RDS database is not needed for several weeks.

Snapshot and delete the database, then restore later if that meets the requirement. Stopping RDS is useful temporarily, but not the best several-week answer because stopped DBs are not meant to remain stopped indefinitely.

Example 3: A nightly Spark job needs a cluster only during processing.

Use a transient EMR cluster that terminates after the job. An always-on cluster wastes money.

Example 4: Analysts need occasional SQL queries over S3 logs.

Use Athena, and reduce scanned data through partitioning/compression/columnar formats if mentioned. Redshift provisioned clusters can be overkill for occasional ad-hoc queries.

### Common Database Cost Traps

- DynamoDB on-demand is simple for unpredictable traffic, but provisioned can be cheaper when predictable.
- Scans are often inefficient and expensive compared with key-based queries.
- Read replicas add cost; use them when they solve read scaling or DR requirements.
- Multi-AZ adds availability, not cost savings.
- Aurora Serverless is not automatically cheapest for every workload; match it to variable usage.
- Redshift is powerful, but idle provisioned clusters cost money.

## 6. Serverless And Managed Service Cost Patterns

### What The Exam Is Testing

Serverless can reduce cost when usage is intermittent because you avoid paying for idle servers. But serverless is not always cheaper at high steady scale.

### Service Fit

| Scenario phrase | Cost-aware choice |
|---|---|
| Short event-driven compute, intermittent traffic | Lambda |
| Always-on high-throughput compute | EC2/ECS with commitments may be cheaper |
| Containerized app with no host management | Fargate |
| Very steady containers at scale | ECS on EC2 with Savings Plans may be cost-effective |
| Simple lower-cost API Gateway option | HTTP API if advanced REST API features are not needed |
| Static website | S3 + CloudFront |
| Serverless auth/API/compute/NoSQL stack | Cognito + API Gateway + Lambda + DynamoDB |

### Exam-Related Examples

Example 1: An HTTP API does not need advanced API Gateway REST API features and needs lower cost/lower latency.

Choose API Gateway HTTP API if available. The exam may contrast REST API and HTTP API.

Example 2: A static website is hosted on EC2 web servers.

Move static content to S3 and CloudFront. Running EC2 for static content is usually unnecessary cost and operations.

Example 3: A high-throughput service runs constantly and predictably.

Serverless may not always be cheapest. A committed compute model with Savings Plans may be more cost-effective if the workload is steady.

### Common Serverless Cost Traps

- "Least operational overhead" often points serverless, but "lowest cost at high steady utilization" may point committed compute.
- Lambda cost depends on requests, duration, and memory.
- API Gateway cost can matter at very high request volume.
- Fargate avoids server management but can cost more than well-utilized EC2 for steady workloads.
- Managed services reduce operations, but the exam still expects cost tradeoff thinking.

## 7. Cost Visibility And Governance Tools

### What The Exam Is Testing

Cost tool questions ask whether you need monitoring, forecasting, alerts, reports, right-sizing, or governance.

| Requirement | Tool |
|---|---|
| View and analyze spend | Cost Explorer |
| Alert when spend exceeds threshold | AWS Budgets |
| Detailed billing data in S3 | Cost and Usage Report |
| EC2/EBS/Lambda right-sizing recommendations | Compute Optimizer |
| Best-practice checks including cost checks | Trusted Advisor |
| Enforce account guardrails | Organizations SCPs / Control Tower |
| Allocate cost by team/project | Cost allocation tags |
| Govern approved deployable products | Service Catalog |

### Exam-Related Examples

Example 1: A team wants an alert when monthly spend is forecasted to exceed a target.

Use AWS Budgets.

Example 2: A company wants to explore historical service spend and forecast future spend.

Use Cost Explorer.

Example 3: A company needs detailed billing records delivered to S3 for analytics.

Use Cost and Usage Report.

Example 4: A team wants recommendations for overprovisioned EC2 instances and EBS volumes.

Use Compute Optimizer.

### Common Cost Tool Traps

- Budgets alerts; Cost Explorer analyzes.
- Cost and Usage Report is the detailed data export.
- Compute Optimizer recommends right-sizing, but it does not enforce changes.
- Tags must be applied consistently to allocate cost accurately.
- Trusted Advisor is broader best-practice checking, not detailed billing analytics.

## 8. Cost-Aware Disaster Recovery And Resilience

### What The Exam Is Testing

Cost optimization cannot ignore resilience requirements. The exam often asks for the lowest-cost DR plan that satisfies RTO/RPO.

| Requirement | Cost-aware DR choice |
|---|---|
| Hours of downtime/data loss acceptable | Backup and restore |
| Low cost, core components only | Pilot light |
| Faster recovery, scaled-down environment | Warm standby |
| Near-zero downtime | Active-active, highest cost |

### Exam-Related Examples

Example 1: The company can tolerate 8 hours of downtime and wants the cheapest DR strategy.

Use backup and restore. Active-active would work but is unnecessarily expensive.

Example 2: The company needs minutes of recovery time and already keeps a smaller full environment running.

Use warm standby. Backup and restore is too slow.

Example 3: The company wants both Regions serving production traffic all the time.

Use active-active/multi-site. It is expensive, so choose it only when requirements justify it.

### Common DR Cost Traps

- Active-active is rarely the lowest-cost answer.
- Backup and restore is cheapest but slowest.
- Warm standby costs more than pilot light.
- Multi-AZ is not a substitute for cross-Region DR.
- Cross-Region replication creates transfer and storage costs, so replicate only what the requirement needs.

## 9. Cost Optimization By Question Pattern

### Pattern 1: "Most Cost-Effective"

When multiple choices work, compare:

- Idle compute.
- Storage class and retrieval requirements.
- NAT and data transfer paths.
- Commitment discounts.
- Managed/service operation cost.
- Minimum storage duration.
- Whether the workload can tolerate interruption.

### Pattern 2: "Least Operational Overhead"

This often points to:

- Managed databases instead of self-managed EC2 databases.
- Serverless services.
- Auto Scaling instead of manual scaling.
- AWS Backup instead of custom backup scripts.
- S3 lifecycle policies instead of manual object movement.

But if the question says "lowest cost" and traffic is steady/high, a well-utilized committed compute option may beat serverless.

### Pattern 3: "Works But Costs Too Much"

Common exam fixes:

- Replace NAT-to-S3 with Gateway Endpoint.
- Move old S3 data with lifecycle policies.
- Use Spot for interruptible batch jobs.
- Stop or terminate idle dev/test resources.
- Use read replicas/caching instead of scaling a primary database vertically.
- Use Athena for occasional S3 queries instead of always-on analytics clusters.
- Use transient EMR instead of always-on EMR.

## Section 4 Rapid Revision Table

| If the question says... | Think... |
|---|---|
| Interruptible/restartable compute | Spot |
| Steady 1-3 year compute usage | Savings Plans / Reserved Instances |
| Need capacity guarantee | Capacity Reservation |
| Licensing/physical server control | Dedicated Host |
| Dev EC2 idle nightly | Stop/start schedule |
| Batch cluster needed only during job | AWS Batch / transient EMR |
| Unknown S3 access pattern | Intelligent-Tiering |
| Critical infrequent access | Standard-IA |
| Re-creatable infrequent data | One Zone-IA |
| Long-term lowest-cost archive | Glacier Deep Archive |
| S3 data hot then cold | Lifecycle policy |
| Deleting IA objects too early | Minimum storage duration charge |
| Private S3 through NAT is expensive | S3 Gateway Endpoint |
| Private DynamoDB through NAT is expensive | DynamoDB Gateway Endpoint |
| Unpredictable DynamoDB traffic | On-demand capacity |
| Predictable DynamoDB traffic | Provisioned + auto scaling |
| Occasional SQL over S3 | Athena |
| Repeated BI warehouse queries | Redshift |
| Cost alert | AWS Budgets |
| Spend analysis | Cost Explorer |
| Detailed billing export | Cost and Usage Report |
| Right-sizing recommendations | Compute Optimizer |
| Reduce S3 SSE-KMS request cost | S3 Bucket Keys |

## Section 4 Mini Exam Drill

1. A batch workload can be interrupted and restarted. The company wants the lowest EC2 compute cost. Which option fits best?
   - A. Dedicated Hosts
   - B. Spot Instances
   - C. On-Demand Instances only
   - D. Capacity Reservations only

2. A service runs continuously with predictable usage for the next three years. What should reduce compute cost?
   - A. Savings Plans or Reserved Instances
   - B. Spot Instances only
   - C. Dedicated Hosts by default
   - D. On-Demand only

3. An S3 dataset is frequently accessed for 30 days, then rarely accessed for years. Which feature should be used?
   - A. S3 lifecycle policy
   - B. Security Group
   - C. NAT Gateway
   - D. AWS WAF

4. Private EC2 instances send high volumes of S3 traffic through a NAT Gateway and costs are high. What should be added?
   - A. S3 Gateway VPC Endpoint
   - B. More NAT Gateways
   - C. Internet Gateway route to the private subnet
   - D. Dedicated Host

5. A DynamoDB table has unpredictable traffic and the team wants no capacity planning. Which mode should be used?
   - A. Provisioned capacity without auto scaling
   - B. On-demand capacity
   - C. RDS Reserved Instance
   - D. Redshift Spectrum

6. A development RDS database is not needed for several weeks, but data must be preserved. Which option can reduce cost most?
   - A. Leave it running
   - B. Snapshot and delete, then restore later if acceptable
   - C. Add Multi-AZ
   - D. Add a read replica

7. A company stores objects in S3 Standard-IA and deletes many after 10 days. Why are costs higher than expected?
   - A. Minimum storage duration charges
   - B. Security Groups are stateful
   - C. Route 53 health checks
   - D. Lambda cold starts

8. Analysts occasionally run SQL queries over log files in S3 and do not want to manage servers. What is most cost-effective?
   - A. Athena
   - B. Always-on Redshift provisioned cluster
   - C. RDS MySQL
   - D. DynamoDB DAX

9. A team wants an alert when monthly AWS spend is forecasted to exceed a limit. Which tool should be used?
   - A. AWS Budgets
   - B. AWS Config
   - C. AWS CloudTrail
   - D. Amazon Macie

10. A high-volume S3 SSE-KMS workload has high KMS request costs. Which feature can help reduce that cost?
   - A. S3 Bucket Keys
   - B. S3 CORS
   - C. S3 Object Lock
   - D. S3 Transfer Acceleration

### Answers

1. B. Spot is the lowest-cost EC2 option for interruptible, restartable workloads.
2. A. Savings Plans or Reserved Instances reduce cost for steady committed usage.
3. A. Lifecycle policies transition data as access patterns change.
4. A. Gateway endpoints avoid NAT processing for S3 traffic.
5. B. On-demand capacity avoids capacity planning for unpredictable DynamoDB traffic.
6. B. For several weeks, snapshot/delete/restore can reduce cost more than keeping the DB running.
7. A. S3 IA/archive classes can have minimum storage duration charges.
8. A. Athena is serverless SQL over S3 and fits occasional ad-hoc queries.
9. A. AWS Budgets sends threshold and forecast alerts.
10. A. S3 Bucket Keys reduce KMS request volume for SSE-KMS workloads.

## Section 4 Final Memory Sheet

Memorize these before cost questions:

- Spot = interruptible/restartable workloads.
- Savings Plans / Reserved Instances = steady committed usage.
- On-Demand = flexibility, no commitment.
- Capacity Reservation = capacity guarantee, not automatic discount.
- Dedicated Host = licensing/compliance, usually not cheapest.
- Stop idle EC2; remember EBS still costs money.
- For idle dev RDS over weeks, snapshot/delete/restore can be cheaper.
- S3 Intelligent-Tiering = unknown/changing access.
- Standard-IA = infrequent but critical immediate access.
- One Zone-IA = infrequent, re-creatable, single-AZ acceptable.
- Glacier Instant = archive with immediate retrieval.
- Glacier Deep Archive = lowest-cost long-term archive.
- Lifecycle policies are the exam answer for hot-to-cold data.
- Minimum duration charges matter for IA/archive classes.
- NAT-to-S3 cost problem = S3 Gateway Endpoint.
- NAT-to-DynamoDB cost problem = DynamoDB Gateway Endpoint.
- DynamoDB on-demand = unpredictable traffic.
- DynamoDB provisioned + auto scaling = predictable traffic.
- Athena = occasional SQL over S3.
- Redshift = recurring data warehouse analytics.
- Budgets = alerts.
- Cost Explorer = analysis and forecasting.
- CUR = detailed billing export.
- Compute Optimizer = right-sizing recommendations.
- S3 Bucket Keys = lower KMS request cost for SSE-KMS.

## Final Review Order For The Full Guide

1. Read the rapid revision table for each section.
2. Memorize the close-cousin distinctions: Multi-AZ vs Read Replica, CloudFront vs Global Accelerator, SG vs NACL, SQS vs SNS vs Kinesis, Athena vs Redshift, Spot vs Savings Plans.
3. Practice the mini drills without checking answers.
4. For every missed question, write the missed qualifier: security, resilience, performance, cost, operational overhead, migration speed, or protocol.
5. Re-read the final memory sheet for each section during the final week.
