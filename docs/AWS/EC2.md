# Elastic Compute Cloud (EC2)

EC2 is a Regional Service and an Infrastructure as a Service (IaaS).

- A Linux-based/Windows-based/Mac-based virtual server that you can provision.
- You are limited to running On-Demand Instances per your vCPU-based On-Demand Instance limit, purchasing 20 Reserved Instances, and requesting Spot Instances per your dynamic Spot limit per region.

## Feature
- The AWS Nitro System is the underlying platform of the next generation of EC2 instances. Traditionally, hypervisors protect the physical hardware and BIOS, virtualize the CPU, storage, networking, and provide a rich set of management capabilities. With the Nitro System, these functions are offloaded to dedicated hardware and software, thereby reducing the costs of your instances in the process. Hence, the Nitro Hypervisor delivers performance that is indistinguishable from bare metal and performs better than its predecessor: the Xen Hypervisor.
- **Instances**: Server environments are called instances.
- **Amazon Machine Images (AMIs)**: Package OS and additional installations in a reusable template.
- **Instance Types**: Various configurations of CPU, memory, storage, and networking capacity for your instances.
  - **General Purpose**: t-type and m-type
  - **Compute Optimized**: c-type
  - **Memory Optimized**: r-type, x-type, and z-type
  - **Storage Optimized**: d-type, h-type, and i-type
  - **Accelerated Computing**: f-type, g-type, and p-type
- **Key Pairs**: Secure login information for your instances.
- **Instance Store Volumes**: Storage volumes for temporary data that are deleted when you STOP or TERMINATE your instance. Note that you can stop an EBS-backed instance but not an Instance Store-backed instance. You can only either start or terminate an Instance Store-backed instance.
- **Elastic Block Store (EBS) Volumes**: Persistent storage volumes for your data (see AWS storage services).
- **Regions and Availability Zones**: Multiple physical locations for deploying your resources, such as instances and EBS volumes (see AWS overview).
- **Security Groups**: A firewall that enables you to specify the protocols, ports, and source IP ranges that can reach your instances (see AWS networking and content delivery).
- **Elastic IP Addresses**: Static IPv4 addresses for dynamic cloud computing (see AWS networking and content delivery).
- **Tags**: Metadata that you can create and assign to your EC2 resources.
- **Virtual Private Clouds (VPCs)**: Virtual networks you can create that are logically isolated from the rest of the AWS cloud, and that you can optionally connect to your own network (see AWS networking and content delivery).
- **User Data**: Add a script that will be run on instance boot.
- **Host Recovery**: Automatically restarts your instances on a new host in the event of an unexpected hardware failure on a Dedicated Host.
- **EC2 Hibernation**: Available for On-Demand and Reserved Instances running on freshly launched M3, M4, M5, C3, C4, C5, R3, R4, and R5 instances running Amazon Linux and Ubuntu 18.04 LTS. You can enable hibernation for your EBS-backed instances at launch. You can then hibernate and resume your instances through the AWS Management Console, or through the AWS SDK and CLI using the existing stop-instances and start-instances commands. Hibernation requires an EC2 instance to be an encrypted EBS-backed instance.

## User Data

- Commands that run when the instance is launched for the first time (doesn't execute for subsequent runs)
- Used to automate dynamic boot tasks (that cannot be done using AMIs)
- Runs with root user privilege

## Instance Classes

1. **General Purpose**: Balance between compute, memory & networking
2. **Compute Optimized**: For compute-intensive tasks
3. **Memory Optimized**: For in-memory databases or distributed web caches
4. **Storage Optimized**: For storage-intensive tasks

## Security Groups

- Only contain Allow rules
- External firewall for EC2 instances
- Can reference a resource by IP or Security Group
- Bound to a VPC (and hence to a region)
- Blocked requests will give a Time Out error

## IAM Roles for EC2 instances

Never enter AWS credentials into the EC2 instance, instead attach IAM Roles to the instances.

## Purchasing Options

1. **On-demand Instances**: Pay per use, no upfront payment
2. **Reserved Instances**: 1 or 3 year commitment
3. **Spot Instances**: Work on a bidding basis
4. **Dedicated Hosts**: Server hardware allocated to a specific company
5. **Dedicated Instances**: Dedicated hardware, billed per instance
6. **On-Demand Capacity Reservations**: Reserve capacity in an AZ

## Elastic IP

- Static Public IP that you own
- Can be attached to an EC2 instance (even when stopped)
- Soft limit of 5 elastic IPs per account

## Placement Groups (Placement Strategies)

1. **Cluster**: Optimize for network
2. **Spread**: Maximize availability
3. **Partition**: Balance of performance and availability

## Elastic Network Interface (ENI)

- Virtual network card that gives a private IP to an EC2 instance
- Can be detached & attached across instances
- Tied to the subnet (and hence to the AZ)

## Instance States

- Stop: EBS root volume preserved
- Terminate: EBS root volume destroyed
- Hibernate: RAM contents saved to EBS root volume

## EC2 Nitro

Newer virtualization technology for EC2 instances with better networking, higher EBS IOPS, and improved security.

## Storage Options

1. Instance Store
2. Elastic Block Storage (EBS)
3. Elastic File System (EFS)

## Monitoring

CloudWatch is used for EC2 Monitoring

## Amazon Machine Image (AMI)

- Pre-packaged instance image
- Bound to a region (can be copied across regions)

## Instance Metadata

URL to fetch metadata about the instance: http://169.254.169.254/latest/meta-data

## Run Command

Systems Manager Run Command lets you remotely manage the configuration of your managed instances.

## Instance Tenancy

- Default: Shared hardware
- Dedicated: Single-tenant hardware
- Host: Dedicated host

## Troubleshooting

Common reasons for immediate instance termination:
- Reached EBS volume limit
- Corrupt EBS snapshot
- Encrypted root EBS volume without proper KMS key permissions
- Missing required part in instance store-backed AMI