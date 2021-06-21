## Section_IAM
*Users & Group*
![](./images/IAM_Users_groups.png)

*Permissions*
![](./images/IAM_Permissions.png)

*Policy Structure*
![](./images/IAM_Policy_Structure.png)

*IAM Roles for Services*

`To understand`

we're going to create what's called an IAM Role.So these IAM role will be just like a user,but they are intended to be used not by physical people, but instead they will be used by AWS services.So what does that mean? It's a bit confusing.So for example,we are going to an EC2 Instance create throughout this course,An EC2 Instance is just like a virtual server,and we'll see this in the next section.But so this EC2 Instance may want to
perform some actions on AWS and to do so,we need to give permissions to our EC2 Instance.To do so, we're going to create an IAM Role and together
they're going to make one entity.And together, once the EC2 Instance is trying to access some information from AWS,then it will use the IAM Role.
And if the permission assigned to the IAM Role is correct,then we're going to get access to the call we are trying to make.

![](./images/IAM_Roles_for_AWS_Services.png)

*IAM Guidelines & Best Practices*
> • Don’t use the root account except for AWS account setup
> 
> • One physical user = One AWS user
>
> • Assign users to groups and assign permissions to groups
> 
> • Create a strong password policy
> 
> • Use and enforce the use of Multi Factor Authentication (MFA)
> 
> • Create and use Roles for giving permissions to AWS services
> 
> • Use Access Keys for Programmatic Access (CLI / SDK)
> 
> • Audit permissions of your account with the IAM Credentials Report
>
> • Never share IAM users & Access Keys

*Summary*
> Users: mapped to a physical user, has a password for AWS Console
> 
> Groups: contains users only
> 
> Policies: JSON document that outlines permissions for users or groups
> 
> Roles: for EC2 instances or AWS services
> 
> Security: MFA + Password Policy
> 
> AWS CLI: manage your AWS services using the command-line
> 
> AWS SDK: manage your AWS services using a programming language
> 
> Access Keys: access AWS using the CLI or SDK
> 
> Audit: IAM Credential Reports & IAM Access Advisor

## Section3_IAM+EC2+ENI+IP
__Regions__

__Availability Zones__

> Each region has many availability zones(usually 3, min is 2, max is 6).

__IAM(Identity and Access Management)__

*Introduction*
![](./images/IAM_Introduction.png)

> IAM has a global view

> Permissions are governed by Policies (JSON)

> It’s best to give users the minimal amount of permissions they need to perform their job (least privilege principles)

> One IAM User per PHYSICAL PERSON

> One IAM Role per Application

> IAM credentials should NEVER BE SHARED

__EC2__

*What host is right for me?*

> On demand
> - coming and staying in resort
whenever we like, we pay the full price
> 
> Reserved
> - like planning ahead and if we plan to
stay for a long time, we may get a good
discount
> 
> Spot instances
> - the hotel allows people to bid
for the empty rooms and the highest bidder
keeps the rooms. You can get kicked out at any
time
>
> Dedicated Hosts
> - We book an entire building
of the resort

*EC2 Pricing*

> EC2 instances prices (per hour) varies based on these parameters:
> - Region you’re in
> - Instance Type you’re using
> - On-Demand vs Spot vs Reserved vs Dedicated Host
> - Linux vs Windows vs Private OS (RHEL, SLES, Windows SQL)
>
> You are billed by the second, with a `minimum of 60 seconds.`
> 
> You also pay for other factors such as storage, data transfer, fixed IP public addresses, load balancing
> 
> You do `not` pay for the instance if the instance is `stopped`

__Security Group__

Security Groups are the fundamental of network security in AWS
and they control how traffic is allowed into or out of our EC2 Machines.

*Inbound/Outbound traffic*
![](./images/SecurityGroup_Inbound_Outbound_Traffic.png)

> They regulate: 1.Access to Ports 2.Authorised IP ranges – IPv4 and IPv6 3.Control of inbound network (from other to the instance) 4.Control of outbound network (from the instance to other)"
>
> `It’s good to maintain one separate security group for SSH access`
>
> If your application is not accessible (time out), then it’s a security group issue
> 
> If your application gives a “connection refused“ error, then it’s an application error or it’s not launched
> 
> All inbound traffic is blocked by default
>
> All outbound traffic is authorised by default

__IPV4_IPV6__

*Public IP*
> Public IP means the machine can be identified on the internet (WWW)
>
> Must be unique across the whole web (not two machines can have the same public IP).
> 
> Can be geo-located easily

*Private IP*

> Private IP means the machine can only be identified on a private network only
>
> The IP must be unique across the private network
> 
> BUT two different private networks (two companies) can have the same IPs.
> 
> Machines connect to WWW using a `NAT` + internet gateway (a proxy)
> 
> Only a specified range of IPs can be used as private IP

*NAT(Network address translation)*
> Translates Private IPs to Public IPs and vice versa
![](./images/NAT3.png)

*Elastic IPs - Try to avoid using Elastic IP*
> When you stop and then start an EC2 instance, it can change its public IP. If you need to have a fixed public IP for your instance, you need an Elastic IP. An Elastic IP is a public IPv4 IP you own as long as you don’t delete it.


*Launch EC2*
> By default, your EC2 machine comes with:
> - A private IP for the internal AWS Network
> - A public IP, for the WWW.
>
> When we are doing SSH into our EC2 machines:
> - We can’t use a private IP, because we are not in the same network
> - We can only use the public IP.
>
> If your machine is stopped and then started, the public IP can change


__ENI(Elastic Network Interfaces)__

ENI is a logical component in a VPC that represents a
virtual network card

*Diagram*
![](./images/ENI.png)

__AMI_is_Region_Locked__

![](./images/AMI_is_region_locked.png)

## Section4_ELB+ASG+Scalability
__High Availability & Scalability For EC2__
> Vertical Scaling: Increase instance size (= scale up / down)
> - From: t2.nano - 0.5G of RAM, 1 vCPU
> - To: u-12tb1.metal – 12.3 TB of RAM, 448 vCPUs
>
> Horizontal Scaling: Increase number of instances (= scale out / in)
> - Auto Scaling Group
> - Load Balancer
>
> High Availability: Run instances for the same application across multi AZ (at least 2 AZs)
> - Auto Scaling Group multi AZ
> - Load Balancer multi AZ

__Load Balancer__

> Spread load across multiple downstream instances
>
> Expose a `single point of access (DNS)` to your application
>
> Seamlessly handle failures of downstream instances
>
> Do regular health checks to your instances
>
> Provide SSL termination (HTTPS) for your websites
>
> Enforce stickiness with cookies
>
> High availability across zones
>
> Separate public traffic from private traffic

__Types of load balancer on AWS__

> AWS has 3 kinds of managed Load Balancers
> - Classic Load Balancer (v1 - old generation) – 2009
>   - HTTP, HTTPS, TCP
>
> - Application Load Balancer (v2 - new generation) – 2016
>   - HTTP, HTTPS, WebSocket
>
> - Network Load Balancer (v2 - new generation) – 2017
>   - TCP, TLS (secure TCP) & UDP
>
> Overall, it is recommended to use the newer / v2 generation load balancers as they
provide more features
>
> You can setup `internal` (private) or `external` (public) ELBs

__Application Load Balancer__
![](./images/Application_Load_Balancer.png)

*ALB - HTTP Based Traffic(`Health checks are at the target group level`)
![](./images/Application_Load_Balancer_http.png)

*Target Groups*
> EC2 instances (can be managed by an Auto Scaling Group) – HTTP
> 
> ECS tasks (managed by ECS itself) – HTTP
> 
> Lambda functions – HTTP request is translated into a JSON event
>
> IP Addresses – must be private IPs
> 
> ALB can route to `multiple target groups`
> 
> `Health checks are at the target group level`
>
> Fixed hostname (XXX.region.elb.amazonaws.com)
> 
> The application servers don’t see the IP of the client directly
> - The true IP of the client is inserted in the header X-Forwarded-For
> - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto) EC2

__Network Load Balancer__
> Network load balancers (Layer 4) allow to:
> - Forward TCP & UDP traffic to your instances
> - Handle millions of request per seconds
> - Less latency ~100 ms (vs 400 ms for ALB)
> 
> NLB has `one static IP per AZ`, and `supports assigning Elastic IP`
(helpful for whitelisting specific IP)
> 
> NLB are used for extreme performance, TCP or UDP traffic
>
> Not included in the AWS free tier

__Cross-Zone Load Balancing__
![](./images/Cross-Zone_Load_Balancing.png)

*Cross-Zone load balancing - Three LB types*
> Classic Load Balancer
> - Disabled by default
> - No charges for inter AZ data if enabled
>
> Application Load Balancer
> - `Always on` (can’t be disabled)
> - No charges for inter AZ data
>
> Network Load Balancer
> - Disabled by default
> - You pay charges ($) for inter AZ data if enabled

__SSL/TLS__

An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption). The load balancer uses an X.509 certificate (SSL/TLS server certificate). You can manage certificates using ACM (AWS Certificate Manager).

*Server Name Indication(SNI)
![](./images/Server_Name_Indication.png)

__ASG(Auto Scaling Group)__

The goal of an Auto Scaling Group (ASG) is to:
> Scale out (add EC2 instances) to match an increased load
> 
> Scale in (remove EC2 instances) to match a decreased load
>
> Ensure we have a minimum and a maximum number of machines running
>
> Automatically Register new instances to a load balancer

*Auto Scaling Alarms*
> It is possible to scale an ASG based on `CloudWatch` alarms
>
> An Alarm monitors a metric (such as Average CPU)
> 
> `Metrics are computed for the overall ASG instances`

Having instances under an ASG means that if they get terminated for whatever reason,
the ASG will automatically `create new ones as a replacement`. Extra safety!

*Scaling Policies*

Target Tracking Scaling
> Most simple and easy to set-up
>
> Example: I want the average ASG CPU to stay at around 40%

Simple / Step Scaling
> When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
>
> When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
> 

Scheduled Actions
> Anticipate a scaling based on known usage patterns
>
> Example: increase the min capacity to 10 at 5 pm on Fridays

__Target Type of target groups__

The following are the possible target types:

1. instance
2. ip
3. lambda

__Sticky sessions for Application Load Balancer__

By default, an Application Load Balancer routes each request independently to a registered target based on the chosen load-balancing algorithm. However, you can use the sticky session feature (also known as session affinity) to enable the load balancer to bind a user's session to a specific target. This ensures that all requests from the user during the session are sent to the same target. This feature is useful for servers that maintain state information in order to provide a continuous experience to clients. To use sticky sessions, the client must support cookies.

Application Load Balancers support both duration-based cookies and application-based cookies. The key to managing sticky sessions is determining how long your load balancer should consistently route the user's request to the same target. Sticky sessions are enabled at the target group level. You can use a combination of duration-based stickiness, application-based stickiness, and no stickiness across all of your target groups.

The content of load balancer generated cookies are encrypted using a rotating key. You cannot decrypt or modify load balancer generated cookies.

For both stickiness types, the Application Load Balancer resets the expiry of the cookies it generates after every request. If a cookie expires, the session is no longer sticky and the client should remove the cookie from its cookie store.

__Target Group vs Auto scaling Group__

Target groups are just a group of Ec2 instances. Target groups are closely associated with ELB and not ASG.

> ELB -> TG - > Group of Instances

We can just use ELB and Target groups to route requests to EC2 instances. With this setup, there is no autoscaling which means instances cannot be added or removed when your load increases/decreases.

> ELB -> TG - > ASG -> Group of Instances

If you want autoscaling, you can attach a TG to ASG which in turn gets associated to ELB. Now with this setup, you get request routing and autoscaling together. `Real world usecases follow this pattern`. If you detach the target group from the Auto Scaling group, the instances are automatically deregistered from the target group

## Section5_EC2_storage_EBS_EFS

__EBS__

An EBS (Elastic Block Store) Volume is a network drive you can attach
to your instances while they run and it allows your instances to persist data

It’s locked to an Availability Zone (AZ)
> An EBS Volume in us-east-1a cannot be attached to us-east-1b

*EBS Volumes come in 4 types*
> GP2 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads -> `Development and test environments`
> 
> IO1 (SSD): Highest-performance SSD volume for mission-critical low-latency or highthroughput workloads -> `MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle`
> 
> ST1 (HDD): Low cost HDD volume designed for frequently accessed, throughputintensive workloads -> `Streaming workloads requiring consistent, fast throughput at a low price\Big data, Data warehouses, Log processing\Apache Kafka\Cannot be a boot volume`
> 
> SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads -> `Scenarios where the lowest storage cost is important/Cannot be a boot volume`

__EFS(Elastic File System)__

![](./images/EFS.png)

> Use cases: content management, web serving, data sharing, Wordpress
>
> Uses security group to control access to EFS
>
> `Compatible with Linux based AMI (not Windows)`

*EBS VS EFS - Elastic Block Storage*
![](./images/EBS_VS_EFS.png)

*EBS VS EFS - Elastic File System*
![](./images/EBS_VS_EFS_2.png)

## Section6_RDS+Aurora+ElastiCache

__RDS(Relational Database Service)__

*Read Replicas*
![](./images/RDS_Read_Replicas.png)

*Read Replicas - use cases*
![](./images/RDS_Read_Replicas_UseCase.png)

*Read Replicas - network cose*
> In AWS there’s a network cost when data goes from one AZ to another
>
> To reduce the cost, you can have your Read Replicas in the same AZ

*Disaster Recovery*
![](./images/RDS_Disaster_Recovery.png)

*RDS - Encryption*
> If the master is not encrypted, the read replicas cannot be encrypted
>
> SSL certificates to encrypt data to RDS in flight
> 
> Provide SSL options with trust certificate when connecting to database
>
> To enforce SSL:
> * PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Parameter Groups)
> * MySQL: Within the DB:
> GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;

*RDS - IAM Authentication*
![](./images/RDS_IAM.png)

*RDS Security Summary*
> Encryption at rest:
> * Is done only when you first create the DB instance
> * or: unencrypted DB => snapshot => copy snapshot as encrypted => create DB from snapshot
> 
> `Your responsibility:`
> * Check the ports / IP / security group inbound rules in DB’s SG
> * In-database user creation and permissions or manage through IAM
> * Creating a database with or without public access
> * Ensure parameter groups or DB is configured to only allow SSL connections
> 
> AWS responsibility:
> * No SSH access
> * No manual DB patching
> * No manual OS patching
> * No way to audit the underlying instance

__Aurora__
> Aurora is a proprietary technology from AWS (not open sourced)
> 
> Postgres and MySQL are both supported as Aurora DB (that means your
drivers will work as if Aurora was a Postgres or MySQL database)
> 
> Aurora is “AWS cloud optimized” and `claims 5x performance improvement
over MySQL on RDS, over 3x the performance of Postgres on RDS`
> 
> Aurora storage automatically grows in increments of 10GB, up to 64 TB.
> 
> Aurora can have 15 replicas while MySQL has 5, and the replication process
is faster (sub 10 ms replica lag)
> 
> Failover in Aurora is instantaneous. It’s HA (High Availability) native.
> 
> `Aurora costs more than RDS (20% more) – but is more efficient`
>
> Possibility to authenticate using IAM token (same method as RDS)

*Aurora Global Database (recommended)*
> 1 Primary Region (read / write)
>
> Up to 5 secondary (read-only) regions, replication lag is
less than 1 second
> 
> Up to 16 Read Replicas per secondary region
> 
> Helps for decreasing latency
> 
> Promoting another region (for disaster recovery) has an
RTO of < 1 minute

__Amazon ElastiCache__

> ElastiCache is to get managed Redis or Memcached
> 
> AWS takes care of OS maintenance / patching, optimizations, setup,
configuration, monitoring, failure recovery and backups
> 
> Using ElastiCache involves heavy application code changes

*DB Cache*
![](./images/ElastiCache_DB_Cache.png)

*Lazy locading/Cache-Aside/Lazy Population*
![](./images/ElastiCache_Lazy_Loading.png)

*Write Through - Add or Update cache when database is updated*
![](./images/ElastiCache_Write_Through.png)

*Final words of wisdom*
> Lazy Loading / Cache aside is easy to implement and works for many
situations as a foundation, especially on the read side
>
> Write-through is usually combined with Lazy Loading as targeted for the
queries or workloads that benefit from this optimization
> 
> Setting a TTL is usually not a bad idea, except when you’re using Writethrough.Set it to a sensible value for your application
> 
> Only cache the data that makes sense (user profiles, blogs, etc…)
>
> Quote: `There are only two hard things in Computer Science: cache
invalidation and naming things`