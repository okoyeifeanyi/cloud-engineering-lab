# Incident 02 — Private HTTP Security Group Failure

## Project

AWS Networking and Security Lab

## Environment

- VPC: `cloud-network-lab-vpc`
- Public subnet: `10.0.1.0/24`
- Private subnet: `10.0.2.0/24`
- Public test instance private IP: `10.0.1.56`
- Private application instance private IP: `10.0.2.250`
- Private application server: Python HTTP server on port 80
- Private server security group: `cloud-private-sg`

## Working Baseline

Before the controlled failure:

- SSH access to the public EC2 instance worked.
- SSH access to the private EC2 instance through the public instance worked.
- The private EC2 instance had no public IPv4 address.
- The Python HTTP server was running on the private instance.
- From the public instance, `curl http://10.0.2.250` returned `Hello from the private subnet`.

## Failure Simulation

The inbound HTTP rule on `cloud-private-sg` was removed.

The SSH rule remained unchanged.

## Observed Symptoms

From the public EC2 instance, the following command was run:

`curl --connect-timeout 10 http://10.0.2.250`

The result was:

`curl: (28) Failed to connect to 10.0.2.250 port 80 after 10002 ms: Timeout was reached`

The Python web server on the private instance remained running.

SSH access to the private instance also remained available.

## Diagnosis

The application was still running and SSH connectivity remained functional.

The HTTP failure occurred immediately after removing the inbound port 80 security-group rule.

This isolated the failure to the security-group access layer rather than the application service or VPC routing.

## Resolution

The HTTP rule was restored on `cloud-private-sg` with the following configuration:

- Type: HTTP
- Protocol: TCP
- Port: 80
- Source: `cloud-bastion-sg`

## Recovery Verification

After restoring the HTTP rule, the following command was run again:

`curl http://10.0.2.250`

The result was:

`Hello from the private subnet`

This confirmed that HTTP connectivity between the public and private EC2 instances had been restored.

## Lessons Learned

- Internal application traffic can operate entirely over private VPC addressing.
- A private EC2 instance does not need a public IP address to provide a service to another EC2 instance inside the VPC.
- Security groups can reference another security group as the allowed traffic source.
- Application health and network reachability are separate troubleshooting layers.
- A missing security-group rule can block one type of traffic while other services remain operational.
- Testing the application, network path, and security configuration separately helps identify the actual failure layer.

## Status

Resolved.