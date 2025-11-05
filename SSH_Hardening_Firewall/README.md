# SSH Hardening and Firewall implementation

• Project Overview

This document outlines the essential steps taken to secure the OpenSSH server (sshd) on a Linux machine (Lubuntu) by moving from insecure password-based authentication to highly secure key-based authentication.

The primary goal is to prevent brute-force attacks by disabling direct root access and forbidding all password logins.

# Phase 1: Implement Firewall (UFW)

The Uncomplicated Firewall (UFW) should be configured to deny all incoming traffic by default, except for the SSH service, which is necessary for remote management. Perform these steps while logged in locally or before disabling password login, as a mistake could lock you out.

UFW Configuration

Set Default Policy to Deny Incoming Traffic:

*sudo ufw default deny incoming*
*sudo ufw default allow outgoing*


Allow SSH Access (Port 22):

Allow connections on the standard SSH port
*sudo ufw allow ssh*


Enable the Firewall:

Enable the firewall. You will be prompted to confirm.
*sudo ufw enable*


Check Status:

Verify the firewall is active and rules are loaded
sudo ufw status 

# Phase 2: Disable Direct Root Login

The default root user is a major target for attackers. This step disables the ability to log in as root directly via SSH.

Configuration Change

Edit the main SSH configuration file:

*sudo nano /etc/ssh/sshd_config*


Find the line below and ensure it is uncommented (no #) and set to no:

PermitRootLogin no


Apply Changes

Restart the SSH service to enforce the new rule:

*sudo systemctl restart ssh*


# Phase 3: Enable Key-Based Authentication

Key-based authentication uses a cryptographic key pair (a public key on the server and a private key on your local machine) to verify your identity, making the connection virtually impossible to crack compared to a password.

Generate Key Pair (On Local Machine)

Generate a strong Ed25519 key pair. This is where you set your passphrase.

Generate the key pair, prompting for a secure passphrase
*ssh-keygen -t ed25519*


Copy Public Key to Server (On Local Machine)

Install the public key onto the server for your regular user (your_username). You will need to enter your user's login password one last time for this step.

*ssh-copy-id your_username@192.168.1.9*


# Phase 4: Disable ALL Password Login

Once key-based login is confirmed to be working, this is the final, essential step to eliminate the weakest link: password authentication.

Configuration Change

Edit the SSH configuration file again:

*sudo nano /etc/ssh/sshd_config*


Find or add the following lines and ensure they are uncommented and set to no:

*PasswordAuthentication no*
*ChallengeResponseAuthentication no*

Apply and Lock Down

Restart the SSH service. At this point, the server is locked down and will only accept valid SSH keys.

*sudo systemctl restart ssh*


How to Connect After Hardening

Due to strict hardening, the client may not automatically find the key. You must use the -i flag to explicitly point to the private key file.

If you encounter Permission denied (publickey) like i did, the error indicated that the server was refusing all non-key logins, but the client wasn't automatically finding your new key.

You have to
• Explicitly uses your private key file (id_ed25519).
• You will be prompted for your SSH key's passphrase.
• *ssh -i ~/.ssh/id_ed25519 your_username@192.168.1.9*
