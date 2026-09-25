# Incident 02: Network Connectivity Failure

## Environment
- Host operating system: Windows 10
- Virtualisation: VirtualBox
- Virtual machine management: Vagrant
- Guest operating system: Ubuntu 24.04
- Web server: Nginx
- Host HTTP port: 8080
- Guest HTTP port: 80

## Symptoms
After disabling port forwarding in the Vagrantfile and reloading the virtual machine, I attempted to access the Nginx webpage from my Windows browser at http://localhost:8080.

The webpage failed to load, although the Ubuntu virtual machine was running.

## Investigation
To determine whether the problem was caused by Nginx or network connectivity, I connected to the Ubuntu virtual machine using SSH.

I ran:

    curl -I http://localhost

The command returned HTTP/1.1 200 OK, confirming that Nginx was responding successfully to local HTTP requests.

Because Nginx worked inside Ubuntu but could not be accessed from Windows, I narrowed the investigation to the connection between the Windows host and the virtual machine.

Port forwarding had been deliberately disabled in the Vagrantfile before this test.


## Root Cause

Port forwarding had been deliberately disabled in the Vagrantfile. Consequently, requests sent to port 8080 on my Windows host were no longer forwarded to port 80 on the Ubuntu virtual machine.

The Nginx service itself was working, as confirmed by the successful HTTP 200 OK response inside Ubuntu.

## Resolution

1. Opened the Vagrantfile and re-enabled port forwarding from Windows port 8080 to Ubuntu port 80.
2. Saved the Vagrantfile.
3. Executed `vagrant reload` to apply the updated configuration.
4. Opened `http://localhost:8080` in my Windows browser.
5. Confirmed that my webpage was accessible again.

## Lessons Learned

- A website being unreachable does not necessarily mean its web server has failed.
- Testing an application locally helps distinguish application failures from networking failures.
- Port forwarding enables my Windows host to access a service inside the Ubuntu virtual machine.
- Changes to Vagrant networking configuration must be applied before testing them.
- Troubleshooting should begin with evidence rather than immediately reinstalling or restarting services.

