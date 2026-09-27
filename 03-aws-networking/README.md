# AWS Networking and Security Lab

## Project Overview

This project involves designing and deploying a custom AWS VPC with public and private subnets, an internet gateway, route tables and security groups.

The objective is to understand how AWS resources communicate and how routing and security settings control network access.

## Planned Configuration

| Resource | Planned configuration |
|---|---|
| Region | Sydney (ap-southeast-2) |
| VPC CIDR | 10.0.0.0/16 |
| Public subnet | 10.0.1.0/24 |
| Private subnet | 10.0.2.0/24 |
| Internet gateway | Attached to custom VPC |
| Public route | 0.0.0.0/0 to internet gateway |
| Private route | Local VPC route only |


## Verified VPC and Subnet Deployment

Created a custom VPC named `cloud-network-lab` in the AWS
Sydney Region (`ap-southeast-2`) with IPv4 CIDR block
`10.0.0.0/16`.

Created two subnets within the VPC:

| Setting | Public subnet | Private subnet |
|---|---|---|
| Name | `cloud-public-subnet` | `cloud-private-subnet` |
| IPv4 CIDR | `10.0.1.0/24` | `10.0.2.0/24` |
| Availability Zone | `ap-southeast-2a` | `ap-southeast-2a` |
| State | Available | Available |
| Auto-assign public IPv4 | Enabled | Disabled |

Both subnet CIDR blocks are contained within the VPC
address range and do not overlap.

At this stage, the subnets have been created but custom
internet routing has not yet been configured or tested.
  
## Project Status

Planning stage. Infrastructure deployment and connectivity tests are pending.


## Verified Internet Gateway and Routing

## Security Group and Connectivity Testing

This is where we'll document your successful SSH and Nginx tests and your security-group incident.

### Internet Gateway

Created an internet gateway named `cloud-lab-igw` and attached it to the custom VPC `cloud-network-lab`.

Confirmed that the internet gateway's state is `Attached` and that it is associated with the intended VPC.

### Public Route Table

Created `cloud-public-rt` and explicitly associated it with `cloud-public-subnet` (`10.0.1.0/24`).

Verified the following routes:

| Destination | Target | Purpose |
|---|---|---|
| `10.0.0.0/16` | Local | Communication within the VPC |
| `0.0.0.0/0` | Internet gateway | Default IPv4 route to the internet |

Automatic public IPv4 assignment is enabled on the public subnet.

### Private Route Table

Created `cloud-private-rt` and explicitly associated it with `cloud-private-subnet` (`10.0.2.0/24`).

Verified the following route:

| Destination | Target | Purpose |
|---|---|---|
| `10.0.0.0/16` | Local | Communication within the VPC |

No default IPv4 internet route has been configured for the private subnet. Automatic public IPv4 assignment is disabled.

### Verification Status

Confirmed in the AWS console that the internet gateway is attached, both route tables contain the intended routes, and each route table is associated with its corresponding subnet.

End-to-end connectivity testing is pending. No EC2 test instances have been deployed for this project yet.


## Cost Management and Resource Cleanup

### Temporary EC2 Instance Cleanup

Following the successful public-subnet connectivity test and security-group incident exercise, the temporary EC2 instance was terminated to prevent unnecessary ongoing compute charges.

The following results were verified in AWS:

| Resource | Verified status |
|---|---|
| EC2 instance (`cloud-network-test-01`) | Terminated |
| Root EBS volume | Deleted |
| Elastic IPs allocated to this lab | None |
| Other unexpected billable resources | None found |
| Displayed AWS bill | USD $0.00 |

### Retained Networking Infrastructure

The following resources were retained for the next private-subnet networking exercise:

- Custom VPC: `cloud-network-lab-vpc`
- Public subnet: `cloud-public-subnet`
- Private subnet: `cloud-private-subnet`
- Internet gateway: `cloud-lab-igw`
- Public route table: `cloud-public-rt`
- Private route table: `cloud-private-rt`
- Security group: `cloud-public-sg`

The retained network has a public subnet with a default route to the internet gateway and a private subnet with a local VPC route only.

### Cost Management

A USD $5 monthly AWS budget was configured for the learning account.

At the time of the cleanup review, the AWS bill displayed USD $0.00. AWS billing information may be delayed, so the account should be checked again after the latest usage has been processed.

### Project Status

The public-subnet deployment, SSH and HTTP connectivity tests, deliberate security-group failure, recovery and temporary EC2 cleanup are complete.

Private-subnet connectivity testing remains the next project milestone.
