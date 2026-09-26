# Incident 01: Nginx Service Failure

## Environment
- Cloud platform: AWS
- Operating system: Ubuntu EC2
- Web server: Nginx
- Access method: SSH using Git Bash
- Application: Static HTML webpage

## Symptoms
During a controlled troubleshooting exercise, I stopped the Nginx service on my EC2 instance. My website became inaccessible from my Windows browser.

## Investigation
I investigated the web-server service using Linux diagnostic commands:

```bash
sudo systemctl status nginx
sudo ss -lntp
curl -I --max-time 5 http://localhost
```

Stopping Nginx was the deliberate change made immediately before the website became unavailable.

## Root Cause
The Nginx service had been deliberately stopped. Consequently, it was no longer available to serve the website.

## Resolution
I restarted Nginx using:

```bash
sudo systemctl start nginx
```

I then verified that the website was accessible again from my Windows browser.

## Lessons Learned
- A running EC2 instance does not guarantee that its applications are available.
- Linux services can be investigated using systemctl.
- Local HTTP tests help isolate application problems from external network problems.
- A recovery should be verified from the user's perspective, not just by restarting a service.