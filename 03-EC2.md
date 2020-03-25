# EC2

Amazon Elastic Compute Cloud is a wer service that provides resizable compute capacity in the cloud. Amazon EC2 reduces the time required to obtain and boot new server instances to minut4es, allowing you quickly scale capacity, both up and down, as your computinf requirements change.

## Pricing Models

### On Demandd

Allows tou pay a fixed rate by the hour (or by second) with no commitment.

#### Useful for

- Users that want the low cost and flexibility of Amazon EC2 without any up-front payment of long-term commitment
- Applications with short term, spiky, or unpredictable workloads that cannot be interrupted
- Applications being developed or tested on AWS EC2 for the first time.

### Reserved

Provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. Contract Terms are 1 ou 3 Year Terms.

#### Reserved Price Types

- Standard Reserved Instances: these offer up to 75% off on demand instances. The more you pay up front and the longer the contract, the greater the disconunt
- Comvertible Reserved Instances: these offer up to 54% off on demand capability to change the attributes od RI as long as the exchange result in the creation of Reserved Instances of Equeal or Greater Value.
- Scheduled Reserved Instances: these are available to lanch within the time windows you reserve. This option allows you to match your capacity reservation to a predictable recurriing schedule that only requires a fraction of day, a week, or a month.

#### Useful for

- Applications with steady steady state or predictable usage
- Applications that reserver capacity
- Users able to make upfront payments to reduce their total computins costs even further

### Spot

Enables you to bid whatever price you want for instance capacity, providing for even greates savings if your applications have flexible start and end times.

#### Useful for

- Applications that have flexible star and end times.
- Applications that are only feasible at very low compute prices
- Users with urgent computing needs for large amounts of additional capacity

### Dedicated Host

Physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound sotfware licenses.

#### Useful for

- Regulatory requirements that may not support multi-tenant virtualization
- Great for licensing which does not suporte multi-tetancy or cloud deployments
- Can be purchased On-Demand (hourly)

## Security Groups

A security group acts as a virtual firewall that controls the traffic for one or more instances.

- All Indound traffic is blocked by default
- All outbound traffic is allowed
- If you create an indound rule allowing traffic in, that traffic is automatically allowed back out again.
- Changes to security groups take effect immediately
- You can have any number of EC2 instances within a security  group
- You can have multiple security groups attached to EC2 Instances
- You can specity allow rules, but not deny rules
- Security Groups are STATEFUL
- Uou cannot block specific IP addresses using Security Groups, instead use Network Access Control Lists.

## Elastic Block Store (EBS)

Provides persistent block storage volumes for use with EC2 instances in the Cloud. EBS volume is automatically replicated within its availability Zone to protect you from component failure, offering high availability and durability.

- Volumes exist on EBS. Think of EBS as virtual hard disk.
- Snapshots exist on S3. Think of snapshots as a photograph of the disk ( the first cration takes time)
- Snapshots are incremental - this means that only the blocks that have changed since your last snapshot are moved to S3.
- To create a snapshot for Amazon EBS volumes that serve as root devices, you should stop the instance before taking the snapshot(However you can take a snap while the instance is running)
- You can created AMI's from snapshots
- You can change EBS volumes sizes on the fly, including changing the size and storage type.
- Volumes will ALWAYS be in the same availability zone as the EC2 instance.
- To move an EC2 volume from one AZ to another:
  1. Take a snapshot of it
  2. Create an AMI from the snapshot
  3. Then use the AMI to launch the instance in a new AZ.
- To Move as EC2 volume from one region to another:
  1. Take a snapshot of it
  2. Create an AMI from the snapshot
  3. Then copy the AMI from one region to the other.
  4. Then use the copied AMI to launch the new EC2 instance in the new region

### EBS Types

#### Gerenal Purpose (SSD)

- Description: general purpose SSd volume that balances price and performance for a wide variety of transational workloads
- Use Case: **Most work loads**
- API Name: gp2
- Volume Size: 1GiB - 16 TiB
- Max IOPS/Volume: 16.000

#### Provisioned IOPS (SSD)

- Description: Highest-performance SSD volume designed for mission-critical applications
- Use Case: **Databases**
- API Name: io1
- Volume Size: 4 GiB - 16
- Max IOPS/Volume: 64.000

#### Throughput Optimised Hard Disk Drive (HDD)

- Description: Low cost HDD volume desined for frequently accessed, thoughput-intensive workloads
- Use Case: **Big Data & Data Warehouses**
- API Name: st1
- Volume Size: 500 GiB - 16 TiB
- Max IOPS/Volume: 500

#### Cold Hard Disk Drive (HDD)

- Description: Lowest cost HDD volume designed for less frequently accessed workloads
- Use Case: **File Servers**
- API Name: sc1
- Volume Size: 500 GiB - 16 TiB
- Max IOPS/Volume: 250

#### Magnetic (HDD)

- Description: Previuos generation HDD
- Use Case: **Workloads where data is infrequently accessed**
- API Name: Standard
- Volume Size: 1 GiB - 1 TiB
- Max IOPS/Volume: 40-200

## AMI Types (EBS vs Instance Stores)

- Regions
- Operating system
- Architecture (32-bit or 64-bit)
- Launch Permissions
- Storage for the Root Device (Root Device Volume)
  - Instance Store (EPHEMERAL STORAGE - cannot be stop, only reboot. fails=lose data)
  - EBS Backed Volumes

### EBS vs Instance Stores

All AMI are categorized as either backed by Amazon EBS or backed by instance store.

- **For EBS Volumes**: the root device for an instance launched from the AMI is an AMazon EBS volume created from an Amazon EBS snapshot.
  - Can be stopped, you will not lose the data on thin instance
- **For Instance Store Volumes**: the root device for an instance launched from the AMI is an instance store volume created from a template stored in AMazon S3.
  - Cannot be stopped, if the underlying host fails, you will lose the data on thin instance

 - **Similarity**
  - You can reboot both, you will not lose your data
  - By default, both ROOT volumes will be deletec on termination. However, with EBS volumes you can tell AWS to keep the root device volume.

## ENI vs ENA vs EFA

### ENI - Elastic Network Interface

Essentially a virtual network card fro your EC2 instance, it allow:

- A primaty private IPv4 address from the IPv4 address range of your VPC
- One or more secondary private IPv4 addresses from the IPv4 address range of your VPC
- One Elastic IP address (IPv4) pre private IPv4 address
- One public IPv4 address
- One or more IPv6 addresses
- One or more security groups
- A MAC address
- A source/destination check flag
- A description

#### Senarios for Network Intervaces

- Create a management network
- Use network and secutiry appliances in your VPC
- Create dual-homed instances with workloads/roles on distinct subnets
- Create a low-budget, high-availabilit solution

### EN - Enhanced Networking

Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to tradicional virtualized network interfaces.

Enhanced Networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking, your EC2 instance just have to suport it.

- **Use where you want good network performance**

#### Enhanced Networking Methodology

- **Elastic Network Adapter (ENA)**: which suports network speeds of up to 100 Gbps for supported instances types.

- Intel 82599 **Virtual Function (VF)**: interface, which supports network speeds of up to 10 Gbps for supported instance types. This is typically used on older instances.

  - In any scenario question, you probably want to **choose ENA over VF** if given the option.

### EFA - Elastic Fabric Adapter

A network device that you can attach to your AWS EC2 instance to accelarate High Performance Computing (HPC) and machine learning applications. EFA provides lower and more consistent latency and higher throughput than the TCP transport traditionally used in cloud-based HPC systems. EFA can use OS-bypass, OS-bypass enables HPC and machine learning applications to bypass the operating system kernel and to communicate directly with the EFA device. It makes it a lot faster with a lot lower latency. Not supported with Windoes currently, only Linux.

### Scenarios Useful

#### ENI

For basic networking. Perhaps you need a separate management network to you need a separatemanagement network to your production network or a separate logging network and you need to do this at low cost. In this scenario use multiple ENIs for each network

#### EN(ENA or VF)

For when you need speeds between 10Gbps and 100Gbps. Anywhere you need reliable, high throughput.

#### EFA

For when you need to accelarate High Performance Computing (HPC) and machine learning applications or if you need to do an OS by-pass. If you see a scenario question mentioning HPC or ML and asking what network adaptor you want, choose EFA.

## Encrypted Root Device Volumes (by Snhapshot)

-  Snapshots of encrypted volumes are encrypted automatically.
- Volumes restored from encrypted snapshots are encrypted automatically.
- You can share snapshots, but only if they are unecrypted
- These snapshots can be shared with other AWS accounts or made public
- You can now encrypt root device volumes upon creation of the EC2 instance

### Processe to make a unecrypted root turn in encrypeted
1. Create a snapshot of the unencrypted root device volume
2. Create a copy of the snapshot and select the encrypt option
3. Create an AMI from the encrypted snapshot
4. Use AMI to lounch new encrypeted instances

## CloudWatch

## Working with EC2

### AWS Command Line

### Using IAM Roles

### Ussing Boot Strap

## EC2 Instance Meta Data

## Elastic File System

## FSX for Windows & FSX for Lustre

## EC2 Placement Groups

## AWS WAF

# EC2 Summary

## Quiz
