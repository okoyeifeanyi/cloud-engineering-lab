# Incident 01: Nginx Service Failure

## Environment
- Windows 10 host
- Vagrant and VirtualBox
- Ubuntu 24.04 virtual machine
- Nginx web server

## Symptom
When Nginx stopped, my browser failed to load the webpage.

## Investigation
I ran `sudo systemctl status nginx` and found that the service was inactive.

I also ran `sudo ss -lntp` and observed that nothing was listening on port 80.

## Resolution
I ran `sudo systemctl start nginx` to restart the web server.

## Root Cause
Nginx had been deliberately stopped during the troubleshooting exercise, leaving no web server listening on port 80.

## Lessons Learned
- Use `systemctl` to investigate and manage Linux services.
- Use `ss -lntp` to inspect listening network ports.
- Investigate the service before attempting to restore it.
