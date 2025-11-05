# Linux Live Environment: Initial Setup and Service Deployment Check

• Project Overview

This project documents the foundational steps taken during a controlled deployment simulation within a non-persistent Linux Live Session. The goal was to establish a secure administrative user, verify logging, and deploy a critical remote access service (OpenSSH), explicitly documenting the results, including the initial connection failure requiring troubleshooting.

This lab demonstrates proficiency in command-line interface (CLI) operations, user management, log analysis, and service installation in a temporary environment.

• Phase 1: Environment Preparation and System Status

  The project began by preparing the non-persistent environment.
  
  - Media Preparation
  
    Place the Lubuntu ISO image onto a USB flash drive.
  
  - Live Boot
  
    Boot the system and select the option to use the environment without installing, ensuring a clean slate for the session.

• Phase 2: Secure User Management and Privilege Verification
  
  A critical step in system administration is creating a dedicated, non-root administrative account, adhering to the Principle of Least Privilege.
  
  - Creating the Dedicated sysadmin User
  
  The user is added with full privilege capabilities.
  
  *Create a new user named 'sysadmin' and prompt for password details*
  *sudo adduser sysadmin*


  - Verifying User Context and Elevated Privileges

   After creation, the user context is switched, and sudo capabilities are confirmed.

  *Switch user to the newly created account*
  *su - sysadmin*
  
  *Verify the user's ability to run privileged commands*
  *sudo whoami*
  
  Expected output: root (demonstrates successful privilege escalation)

- Checking Authentication Logs

  A proactive check of authentication logs provides a baseline security status for the session.
  
   *Search the authentication log for evidence of failed password attempts
    sudo grep "failed password" /var/log/auth.log*
    *Note: In a fresh Live Session, this typically returns a clean output.* 


• Phase 3: Service Deployment and Documented Failure

  The final step was the deployment of the OpenSSH server and an immediate functional test.
  
 - Installing OpenSSH Server
  
  The necessary package is installed using the system's package manager.
  
  *Update package lists
  sudo apt update*
  
  *Install the OpenSSH server package
  sudo apt install openssh-server -y*


-  Local Connection Test and Documenting Failure

  We attempt to connect to the SSH service from the same machine to verify it is running and accessible on the loopback interface (localhost).
  
  *Attempt to SSH into the local machine as the sysadmin user
  ssh sysadmin@localhost*
  
  *Result: Connection cannot be established (e.g., "ssh: connect to host localhost port 22: Connection refused")*


Conclusion and Next Steps

The recorded failure ("localhost can't be established") documents a key troubleshooting scenario. SSH was implemented but I encountered a service avaliability failure. In a real deployment, the next steps would involve:

Service Status Check: Verifying that the ssh service is running using sudo systemctl status ssh.

Firewall Check: Using sudo ufw status to check if a default DENY rule (common in some desktop environments) is preventing the loopback connection on port 22, even locally.

Persistence: Recognizing that all changes made in the Live Session (including user creation and service installation) will be lost upon reboot, requiring the process to be repeated on a persistent, installed environment.
