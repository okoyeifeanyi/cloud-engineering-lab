# Incident 01 — HTTP Connectivity Failure

## Project
AWS Networking and Security Lab

## Environment

- AWS Region: Sydney (`ap-southeast-2`)
- VPC: `cloud-network-lab-vpc`
- Public subnet: `cloud-public-subnet` (`10.0.1.0/24`)
- EC2 instance: `cloud-network-test-01`
- Security group: `cloud-public-sg`
- Operating system: Ubuntu
- Web server: Nginx

## Incident Summary

During a controlled networking exercise, the inbound HTTP rule on the EC2 instance's security group was removed. New external HTTP connections timed out, although the Nginx service remained active and the web server continued responding locally.

The HTTP rule was restored, and external HTTP access recovered.

## Working Baseline

Before the experiment, the following tests succeeded:

- SSH connection from Windows to the EC2 instance.
- Ubuntu package update and Nginx installation.
- Nginx service status: active.
- Local HTTP request: `HTTP 200 OK`.
- Windows browser: Nginx welcome page displayed.

## Failure Simulation

Removed the inbound HTTP rule allowing TCP port 80 from `0.0.0.0/0` on `cloud-public-sg`.

The SSH rule remained in place.

## Observed Symptoms

- External HTTP connection: timed out.
- Nginx service: active.
- Local HTTP request: `HTTP 200 OK`.

## Diagnosis

Nginx was operational and responded successfully on the instance. The external HTTP failure occurred after the inbound HTTP rule was removed.

The results identified the security-group configuration as the cause of the simulated outage.

## Resolution

Restored the inbound HTTP rule allowing TCP port 80 from `0.0.0.0/0`.

## Recovery Verification

After restoring the rule, external HTTP requests and the Windows browser worked again.

## Lessons Learned

1. Security groups control inbound and outbound network traffic independently of an application's service status.
2. A successful local HTTP test does not prove that a website is externally accessible.
3. Comparing local and external connectivity helps narrow down the cause of an outage.
4. Restoring the relevant security-group rule resolved this controlled incident.

## Status

Resolved. Temporary test-instance cleanup will be recorded separately.