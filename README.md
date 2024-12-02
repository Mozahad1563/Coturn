Setting Up a STUN/TURN Server with coturn
This guide walks you through the steps to install and configure a STUN/TURN server using coturn on Ubuntu.

Prerequisites
A machine running Ubuntu.
Sudo privileges for the user.
Step 1: Update System Packages
Open a terminal.
Run the following command to update the package list:
bash
Copy code
sudo apt update
Step 2: Install coturn
Install coturn using the following command:
bash
Copy code
sudo apt install coturn
Step 3: Stop coturn Service Temporarily
Stop the coturn service to prepare for configuration:
bash
Copy code
sudo systemctl stop coturn
Step 4: Enable coturn Service
Open the coturn default configuration file:
bash
Copy code
sudo nano /etc/default/coturn
Find the line #TURNSERVER_ENABLED=1 and uncomment it by removing the #.
Save the file and exit (press CTRL+O, then CTRL+X).
Step 5: Backup the Default Configuration
Move the existing configuration file to create a backup:
bash
Copy code
sudo mv /etc/turnserver.conf /etc/turnserver.conf.bak
Step 6: Create a New Configuration File
Open a new configuration file:

bash
Copy code
sudo nano /etc/turnserver.conf
Add the following configuration:

plaintext
Copy code
# /etc/turnserver.conf

# Ports
listening-port=3478
tls-listening-port=5349

# Authentication
fingerprint
lt-cred-mech
user=localuser:localpass

# Local network interface (replace with your local IP)
listening-ip=127.0.0.1
relay-ip=127.0.0.1

# Domain and realm (use localhost for local testing)
realm=localhost

# Optional: Quotas
total-quota=50
stale-nonce=600
Save the file and exit (press CTRL+O, then CTRL+X).

Step 7: Configure Firewall
Allow necessary ports through the firewall:
bash
Copy code
sudo ufw allow 3478/tcp
sudo ufw allow 3478/udp
sudo ufw allow 5349/tcp
sudo ufw allow 5349/udp
Step 8: Start and Enable coturn Service
Start the coturn service:

bash
Copy code
sudo systemctl start coturn
Enable coturn to start on boot:

bash
Copy code
sudo systemctl enable coturn
Check the status of the service:

bash
Copy code
sudo systemctl status coturn
Step 9: Test Your STUN/TURN Server
Open Trickle ICE in your browser.
Add your local STUN/TURN server:
STUN: stun:127.0.0.1:3478
TURN: turn:127.0.0.1:3478 (with username localuser and password localpass).
Click Gather Candidates.
Verify:
STUN: Look for candidates with type srflx.
TURN: Look for candidates with type relay.
Notes
Replace 127.0.0.1 with your server's actual IP address for non-local testing.
Adjust quotas and other settings in the /etc/turnserver.conf file based on your requirements.
You now have a functional STUN/TURN server!
