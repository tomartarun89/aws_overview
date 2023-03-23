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

#### Purchase Options

1. On-demand
2. Reserved instance
3. Spot instance
4. Saving plan
    - EC2 saving plan
    - Compute saving plan
    - Sagemaker saving plan
5. Dedicate instance (single tenancy)
6. Dedicated host (single tenancy)

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

#### Miscellaneous

- AWS trusted advisor looks for security flaws in the configuration components and performance
  issues
  in system, while also looking for underutilized resources.
- DynamoDBa global tables provide a multi-active database that is multi-regional and fully managed.
  Global tables automatically replicate data across your selection of AWS regions.

##### Best practices

- Never store your access key inside ec2 instances, instead create a role with required permissions
  and attach it to the instance.