# Setting Up a STUN/TURN Server with coturn
## This guide walks you through the steps to install and configure a STUN/TURN server using coturn on Ubuntu.

## Prerequisites
- A Linux machine (Ubuntu or similar) installed locally or running in a virtual environment.
- Administrative privileges (root or sudo).
- Ensure necessary ports (3478, 5349, etc.) are open in the local firewall (if applicable).
##  Install Coturn
Update the package repository and install coturn:
```
sudo apt update
sudo apt install coturn
```
Stop the service temporarily while configuring:
```
sudo systemctl stop coturn
```
## Configure Coturn
- Enable Coturn Service
Edit the file /etc/default/coturn:
```
sudo nano /etc/default/coturn
```
Uncomment the line:
`TURNSERVER_ENABLED=1`
Save and exit.

## Backup the Default Configuration
Rename the default config file for backup:
```
sudo mv /etc/turnserver.conf /etc/turnserver.conf.bak
```
### Create a New Configuration
Edit a fresh configuration file:
```
sudo nano /etc/turnserver.conf
```
Paste the following basic configuration:  
```
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

# Optional: Enable verbose logging
verbose
```
## Allow Necessary Ports
Enable required ports on your local firewall (if enabled):
```
sudo ufw allow 3478/tcp
sudo ufw allow 3478/udp
sudo ufw allow 5349/tcp
sudo ufw allow 5349/udp
```
## Start Coturn
Start and enable the Coturn service:
```
sudo systemctl start coturn
sudo systemctl enable coturn
```
Check the status to ensure it’s running:
```
sudo systemctl status coturn
````
## Test Your STUN/TURN Server
Using Trickle ICE
`https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/`
Open Trickle ICE in your browser.
#### Add your local STUN/TURN server in the format:
- For STUN: stun:127.0.0.1:3478
- For TURN: turn:127.0.0.1:3478
- (with username localuser and password localpass)
- Click Gather Candidates.
#### Verify:
- STUN: Look for candidates with type srflx.
- TURN: Look for candidates with type relay
  



