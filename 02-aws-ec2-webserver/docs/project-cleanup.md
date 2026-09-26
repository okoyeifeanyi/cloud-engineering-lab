# AWS EC2 Project — Resource Cleanup Report

## Project
AWS EC2 Linux Web Server

## Purpose
Document the safe removal of temporary AWS resources after completing the web server deployment and troubleshooting exercises.

## Resources and Cleanup Results

| Resource | Final Status |
|---|---|
| EC2 instance | Terminated |
| Root EBS volume | Deleted |
| Elastic IP | None allocated for the lab |
| AWS budget | USD $5 monthly budget configured |
| Current AWS bill | USD $0.00 at time of review |

## Cleanup Procedure

1. Confirmed that project documentation, the website screenshot and both incident reports had been published to GitHub.
2. Terminated the temporary EC2 instance in the AWS Sydney Region.
3. Verified that the lab's EBS volume had been deleted.
4. Confirmed that no Elastic IP remained allocated for the lab.
5. Accessed AWS Billing and confirmed that the current displayed bill was USD $0.00.

## Cost Management

A USD $5 monthly AWS budget was created for the learning environment.

The bill showed USD $0.00 at the time of review. Because AWS billing data can be delayed, the amount should be checked again after usage has been fully processed.

The budget provides alerts rather than a guaranteed spending limit.

## Lessons Learned

- Terminating an EC2 instance and deleting its storage are separate cleanup considerations.
- Stopped EC2 instances may still have billable attached resources.
- AWS resources should be checked after termination to identify potential ongoing charges.
- Cloud project documentation should include deployment, troubleshooting, security and cleanup procedures.

## Final Project Status

The temporary EC2 instance has been terminated, its EBS volume has been deleted and no Elastic IP remains allocated for this lab. The project documentation is preserved on GitHub.