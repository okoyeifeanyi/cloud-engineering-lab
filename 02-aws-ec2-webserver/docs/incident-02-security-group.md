# Incident 02: AWS Security-Group Failure

## Environment
- Cloud platform: AWS
- Infrastructure: Ubuntu EC2 instance
- Web server: Nginx
- Network control: EC2 security group
- Application port: TCP 80

## Symptoms
During a controlled troubleshooting exercise, I removed the inbound HTTP rule from the security group attached to my EC2 instance.

My website became unreachable from my Windows browser.

## Investigation
The exercise involved changing the AWS security group rather than stopping Nginx.

The relevant diagnostic commands were:

```bash
sudo systemctl is-active nginx
curl -I --max-time 5 http://localhost
```

These commands help determine whether the application is running and responding locally, independently of its external accessibility.

## Root Cause
The security group's inbound HTTP rule had been deliberately removed, preventing new external HTTP connections to the EC2 instance on TCP port 80.

## Resolution
I restored the inbound security-group rule:

- Type: HTTP
- Protocol: TCP
- Port: 80
- Source: 0.0.0.0/0

After saving the rule, I refreshed my Windows browser and confirmed that the website was accessible again.

## Lessons Learned
- EC2 security groups control permitted inbound and outbound network traffic.
- An application can remain operational while external network access is blocked.
- Local HTTP checks and external browser tests investigate different parts of the request path.
- Security-group changes should be carefully reviewed and their effects verified.