# AWS overview

#### What is cloud computing ?

> Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go
> pricing. Instead of buying, owning, and maintaining physical data centers and servers, you can
> access technology services, such as computing power, storage, and databases, on an as-needed basis
> from a cloud provider like Amazon Web Services (AWS).

Types of cloud.

1. Public cloud (eg. aws, gcp, azure, etc.)
2. Private cloud (on-premise)
3. Hybrid cloud (public + on-premise)

Six advantages of using cloud computing

1. Trade capital expense for variable expense
2. Benefit from massive economies of scale
3. Stop guessing capacity
4. Increase speed and agility
5. Stop spending money running and maintaining data centers
6. Go global in minute

---

### Compute services

---

#### EC2 Compute *(Elastic compute cloud)*

![EC2 dashboard](/images/ec2_dashboard.png)

- **EC2** is similar to a virtual server, which you can provision on demand, and you don't have to
  manage the underlying hardware.

#### EC2 instance types

When you launch an instance, the instance type that you specify determines the hardware of the host
computer used for your instance. Each instance type offers different compute, memory, and storage
capabilities, and is grouped in an instance family based on these capabilities. Select an instance
type based on the requirements of the application or software that you plan to run on your instance.

#### EC2 naming

![EC2 instance naming](/images/ec2_instance_naming.png)

The following are the additional capabilities indicated by the instance type names:

[Additional information](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)

- a – AMD processors
- g – AWS Graviton processors
- i – Intel processors
- d – Instance store volumes
- n – Network optimization
- b – Block storage optimization
- e – Extra storage or memory
- z – High frequency

#### Connection to EC2 instances

- EC2 instance connect ( you need to allow traffic from port 22 in security group )
- Session Manager (you don't have to allow traffic on port 22 instead you need to configure
  appropriate role and agent for your ec2 instance)
- SSH Client ( you need to allow traffic from port 22 in security group )
- EC2 Serial Console

#### Instance Lifecycle

An Amazon EC2 instance transitions through different states from the moment you launch it through to
its termination.

![EC2 instance lifecycle](/images/ec2_instance_lifecycle.png)

![EC2 lifecycle differences](/images/ec2_lifecycle_differences.png)
**Important**

1. When you restart an ec2 instance then the public ip associated with the instance might change,
   however private ip remains constant.
2. When you stop, hibernate, or terminate an instance, every block of storage in the instance store
   is reset. Therefore, your data cannot be accessed through the instance store of another instance.
3. You are charged per second and the minimum is 60 secs, i.e. even if you run and EC2 instance for
   30 seconds you will be charged for 60 seconds.

#### Purchase Options

1. On-demand
2. Reserved instance
3. Spot instance
4. Saving plan ( monetary commitment )
    - EC2 Instance Savings Plans
    - Compute Savings Plans
    - SageMaker Savings Plans
5. Dedicate instance (single tenancy)
6. Dedicated host (single tenancy)

---

### AWS Lambda

AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any
type of application or backend service without provisioning or managing servers. You can trigger
Lambda from over 200 AWS services and software as a service (SaaS) applications, and only pay for
what you use.

![File processing example using lambda](/images/aws_lambda_example.png)

#### Lambda Pricing

You are charged based on the number of requests for your functions and the duration
it takes for your code to execute.

Lambda counts a request each time it starts executing in response to an event notification trigger,
such as from Amazon Simple Notification Service (SNS) or Amazon EventBridge, or an invoke call, such
as from Amazon API Gateway, or via the AWS SDK, including test invokes from the AWS Console.

Duration is calculated from the time your code begins executing until it returns or otherwise
terminates, rounded up to the nearest 1 ms*. The price depends on the amount of memory you allocate
to your function. In the AWS Lambda resource model, you choose the amount of memory you want for
your function, and are allocated proportional CPU power and other resources. An increase in memory
size triggers an equivalent increase in CPU available to your function.

##### Things to remember:

- Lambda is suitable for short workloads which should not last for more than 15 minutes.
- You do not manage any underlying hardware, autoscaling or provisioning.
- Cost-effective when compared to EC2, since you are only paying for what you use.

---

### ECR

---

### ECS

---

### EKS

---

### Fargate

---

### Elastic Bean Stalk

---

### LightSail

---

### VPC (Virtual private cloud)

A VPC is an isolated portion of the AWS cloud that has resources defined and restricted for use by a
customer. It is a region specific service, i.e. it
cannot span across more than one region.

There is a soft limit of 5 VPC per region.

- VPC endpoints are used to connect your resources inside VPC to other aws resources over private
  connection.
- VPC Peering is used connect two VPC together (can also reside in different account or different
  regions). Both the VPC must have difference address range. VPC
  peering is not transitive.

```
i.e if we connect vpc a with b and b with c. The resources in vpc a can access resources in vpc b but not in vpc c.
VPC-A -> VPC-B 
VPC-B -> VPC-C 
VPC-A ❌ VPC-C 
```

In order to avoid creating multiple vpc peering connection you can use transit gateway which create
a hub and spoke network to connect multiple vpc together.

- Virtual Private Gateway is used to connect your on-premise data center to AWS cloud over public
  internet.
- Another approach to connect your on-premise data to AWS is by using **Direct Connect**, here your
  on-premise data center connect to the AWS over an actual physical connection provided by an Amazon
  Partner, thus avoid the public internet completely.

---

## IAM

### What is AWS IAM?

AWS IAM (Identity and Access Management) is a web service that helps you securely control access to AWS resources for your users. It enables you to manage users and their level of access to the AWS console and other AWS services.

### Policies and Permissions

You manage access in AWS by creating policies and attaching them to IAM identities (users, groups of users, or roles) or AWS resources. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. AWS evaluates these policies when an IAM principal (user or role) makes a request. Permissions in the policies determine whether the request is allowed or denied. Most policies are stored in AWS as JSON documents.

### Policy types

1. Identity-based policies Attach managed and inline policies to IAM identities (users, groups to which users belong, or roles). Identity-based policies grant permissions to an identity.

2. Resource-based policies: Attach inline policies to resources. The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies. Resource-based policies grant permissions to the principal that are specified in the policy. Principals can be in the same account as the resource or in other accounts.

3. Permissions boundaries: Use a managed policy as the permissions boundary for an IAM entity (user or role). That policy defines the maximum permissions that identity-based policies can grant to an entity, but does not grant permissions. Permission boundaries do not define the maximum permissions that a resource-based policy can grant to an entity.

4. Organisations SCPs – Use an AWS Organisations service control policy (SCP) to define the maximum permissions for account members of an organisation or organisational unit (OU). SCPs limit permissions that identity-based policies or resource-based policies grant to entities (users or roles) within the account but do not grant permissions.

5. Access control lists (ACLs): Use ACLs to control which principals in other accounts can access the resource to which the ACL is attached. ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document structure. ACLs are cross-account permission policies that grant permissions to the specified principal. ACLs cannot grant permissions to entities within the same account.

6. Session policies: Pass advanced session policies when you use the AWS CLI or AWS API to assume a role or a federated user. Session policies limit the permissions that the role's or user's identity-based policies grant to the session. Session policies limit permissions for a created session, but do not grant permissions. For more information, see Session Policies.

### What is the difference between “identities” and “entities” ?

Identities refer to the users or principals who are authenticated to access AWS resources, while entities refer to the AWS resources themselves, such as EC2 instances or S3 buckets.

#### Users

These are the actual users in your organisation, and you can directly assign permission to them.

#### Groups

An IAM user group is a collection of IAM users. User groups let you specify permissions for multiple users, which can make it easier to manage the permissions for those users.

You might wonder what will happen if you create two groups, provide permission for a resource in one group and deny it in another, and add the same user to both groups. In such scenarios, the explicit deny takes precedence over the allow.

#### Roles

An IAM role is an IAM identity that you can create in your account that has specific permissions. An IAM role is similar to an IAM user, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. Also, a role does not have standard long-term credentials such as a password or access keys associated with it. Instead, when you assume a role, it provides you with temporary security credentials for your role session.

### IAM best practises

- Follow the least privilege principle.
- Only provide the required permissions to the users. 
- Audit the access using the IAM Credentials Report, CloudTrail, and IAM Access Advisor. 
- Attach roles to the resources instead of using access keys and tokens. 
- Use access keys and tokens for programmatic access.
- Set strong password policies. 
- Enforce multi-factor authentication **(MFA)** usage.

#### Miscellaneous

- AWS trusted advisor looks for security flaws in the configuration components and performance
  issues
  in system, while also looking for underutilized resources.
- DynamoDBa global tables provide a multi-active database that is multi-regional and fully managed.
  Global tables automatically replicate data across your selection of AWS regions.

##### Best practices

- Never store your access key inside ec2 instances, instead create a role with required permissions
  and attach it to the instance.

---

#### AWS well-architected framework

| Pillar                        | Definition                                                                                                                                                                                                                                                                                | Design Principle                                                                                                                                                                                                                                                         |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational Excellence        | The operational excellence pillar focuses on running and monitoring systems, and continually improving processes and procedures. Key topics include automating changes, responding to events, and defining standards to manage daily operations.                                          | 1. Perform operations as code <br/>2. Make frequent, small, reversible changes <br/>3. Refine operations procedures frequently <br/>4. Anticipate failure 5. Learn from all operational failures                                                                         |
| Security Pillar               | The security pillar focuses on protecting information and systems. Key topics include confidentiality and integrity of data, managing user permissions, and establishing controls to detect security events.                                                                              | 1. Implement a strong identity foundation <br/>2. Enable traceability <br/>3. Apply security at all layers <br/>4. Automate security best practices <br/>5. Protect data in transit and at rest<br/> 6. Keep people away from data <br/>7. Prepare for security events   |
| Reliability Pillar            | The reliability pillar focuses on workloads performing their intended functions and how to recover quickly from failure to meet demands. Key topics include distributed system design, recovery planning, and adapting to changing requirements.                                          | 1. Automatically recover from failure <br/>2. Test recovery procedures <br/>3. Scale horizontally to increase aggregate workload availability <br/>4. Stop guessing capacity <br/>5. Manage change through automation                                                    |
| Performance Efficiency Pillar | The performance efficiency pillar focuses on structured and streamlined allocation of IT and computing resources. Key topics include selecting resource types and sizes optimized for workload requirements, monitoring performance, and maintaining efficiency as business needs evolve. | 1. Democratize advanced technologies: Make advanced technology implementation easier for your team <br/>2. Go global in minutes <br/>3. Use serverless architectures <br/>4. Experiment more often <br/>5. Consider mechanical sympathy                                  |
| Cost Optimization Pillar      | The cost optimization pillar focuses on avoiding unnecessary costs. Key topics include understanding spending over time and controlling fund allocation, selecting resources of the right type and quantity, and scaling to meet business needs without overspending.                     | 1. Implement cloud financial management <br/>2. Adopt a consumption model <br/>3. Measure overall efficiency <br/>4. Stop spending money on undifferentiated heavy lifting <br/>5. Analyze and attribute expenditure                                                     |
| Sustainability Pillar         | The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads. Key topics include a shared responsibility model for sustainability, understanding impact, and maximizing utilization to minimize required resources and reduce downstream impacts. | 1. Understand your impact <br/>2. Establish sustainability goals <br/>3. Maximize utilization <br/>4. Anticipate and adopt new, more efficient hardware and software offerings <br/>5. Use managed services <br/>6. Reduce the downstream impact of your cloud workloads |
