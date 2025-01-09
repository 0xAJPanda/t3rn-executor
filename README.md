# t3rn-



Installation Steps

1. System Update
Update your system packages:
```
sudo apt update
sudo apt upgrade -y
```
2. Install Required Dependencies
```
sudo apt-get install figlet -y
```
3. Create Service User
Create a dedicated user for running the t3rn node:

```
sudo useradd -r -s /bin/false t3rn
sudo mkdir -p /home/t3rn
```

4. T3rn Binary Installation
Download and set up the t3rn executor:
```
cd /home/t3rn
```
```
wget https://github.com/t3rn/executor-release/releases/download/v0.33.0/executor-linux-v0.33.0.tar.gz
```
```
tar -xzvf executor-linux-v0.33.0.tar.gz
```
```
rm -rf executor-linux-v0.33.0.tar.gz
```
Set proper permissions:

```
sudo chown -R t3rn:t3rn /home/t3rn
```

5. Create Systemd Service
Create a new service file:

```
sudo nano /etc/systemd/system/t3rn.service

Add the following configuration:
[Unit]
Description=T3rn Executor Node
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=t3rn
Group=t3rn
ExecStart=/home/t3rn/executor/executor/bin/executor
Restart=always
RestartSec=3
LimitNOFILE=65535

# Environment variables
Environment=NODE_ENV=testnet
Environment=LOG_LEVEL=debug
Environment=LOG_PRETTY=false
Environment=EXECUTOR_PROCESS_ORDERS=true
Environment=EXECUTOR_PROCESS_CLAIMS=true
Environment=ENABLED_NETWORKS=base-sepolia,arbitrum-sepolia,optimism-sepolia,l1rn
Environment=EXECUTOR_MAX_L3_GAS_PRICE=500
Environment=EXECUTOR_PROCESS_PENDING_ORDERS_FROM_API=true
Environment=PRIVATE_KEY_LOCAL=**your_private_key_here**

# Working directory
WorkingDirectory=/home/t3rn/executor/executor/bin

[Install]
WantedBy=multi-user.target
```
6. Configure the Service

Replace your_private_key_here with your actual EVM wallet private key
Save and exit (CTRL+X, then Y, then Enter)

7. Start the Service
Enable and start the t3rn service:

# Reload systemd to recognize the new service

```
sudo systemctl daemon-reload
```

# Enable service to start on boot
```

sudo systemctl enable t3rn
```

# Start the service
```
sudo systemctl start t3rn
```
 8. Monitor the Node

Check Service Status
```
sudo systemctl status t3rn
```

View Logs


# View all logs
```
sudo journalctl -u t3rn
```

# Follow logs in real-time
```
sudo journalctl -u t3rn -f
```
 **Service Management:**

 Common Commands
- Stop the service: sudo systemctl stop t3rn
- Restart the service: sudo systemctl restart t3rn
- Disable autostart: sudo systemctl disable t3rn



