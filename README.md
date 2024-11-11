
# Coturn Docker Setup Guide

This guide provides step-by-step instructions to set up a **Coturn server** with **Docker**.

## Prerequisites
- **Docker** installed on your system.
- You have administrative access to your machine (root or sudo privileges).

---

### 1. Create a Directory for Coturn Configuration
Start by creating a directory to store your Coturn configuration files.

```bash
cd ~/Documents/
mkdir coturn
cd coturn
```

---

### 2. Pull the Coturn Docker Image
Download the latest Coturn Docker image from Docker Hub.

```bash
sudo docker pull coturn/coturn
```

---

### 3. Create the Coturn Configuration File
You need to configure Coturn to work with your network setup. Create the configuration file `turnserver.conf`.

```bash
touch turnserver.conf
```

Open it in a text editor to modify it:

```bash
nano turnserver.conf
```

---

### 4. Edit the `turnserver.conf` File
Add the following configuration to `turnserver.conf`. These settings include listening IP, authentication, and security settings.

```bash
# turnserver.conf - Coturn Configuration

# Listening IP address (0.0.0.0 means all available interfaces)
listening-ip=0.0.0.0

# Listening port for TURN (default: 3478)
listening-port=3478

# Optional: Listening port for TLS (secure TURN)
tls-listening-port=5349

# External/public IP (if behind NAT, specify external IP here)
# external-ip=<YOUR_PUBLIC_IP>

# Fingerprinting (recommended for WebRTC compatibility)
fingerprint

# Authentication type: shared secret
use-auth-secret
static-auth-secret=3f5e8f8a9b2d4c1a3b5d7e9f0a1b2c3d  # Replace with your own generated key for security

# Realm (domain or IP for your TURN server)
realm=localhost

# Limits and quotas
total-quota=100            # Total number of allowed sessions
bps-capacity=0             # No bandwidth limitation

# Security options
stale-nonce
no-loopback-peers
no-multicast-peers

# Optional: TLS configuration
# Uncomment the following if you want to use TLS. Replace with paths to your certificate and key files.
# cert=/etc/coturn/ssl/cert.pem
# pkey=/etc/coturn/ssl/privkey.pem

# Logging settings (optional)
# Uncomment to enable detailed logging for troubleshooting
# log-file=/var/log/turnserver.log
# verbose
# no-stdout-log
```

**Note**:
- Replace `static-auth-secret` with your own generated key for better security.
- If you want to use TLS encryption for TURN connections, uncomment and configure the `cert` and `pkey` options with valid SSL certificates.

---

### 5. Run Coturn Docker Container
After configuring the `turnserver.conf` file, run the Coturn server in a Docker container using the following command:

```bash
sudo docker run -d --network=host     -v /home/youruser/Documents/coturn/turnserver.conf:/etc/coturn/turnserver.conf     coturn/coturn -c /etc/coturn/turnserver.conf
```

**Explanation**:
- `-d`: Run the container in detached mode.
- `--network=host`: Use the host network settings.
- `-v /home/youruser/Documents/coturn/turnserver.conf:/etc/coturn/turnserver.conf`: Mount the local `turnserver.conf` file into the Docker container.
- `coturn/coturn`: The Coturn Docker image.
- `-c /etc/coturn/turnserver.conf`: Use the mounted configuration file inside the container.

This will start the Coturn server, and it will listen on the specified IP and port.

---

### 6. Verify the Coturn Server
Once the container is running, you can verify if the Coturn server is working by checking the logs:

```bash
sudo docker logs <container_id>
```

Find the container ID with:

```bash
sudo docker ps
```

This will show the running containers, and you can use the container ID to check the logs.

---

### Conclusion
By following these steps, you will have successfully set up a **Coturn TURN server** running inside a **Docker container**.
