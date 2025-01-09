
# t3rn Node Installation

An essential actor of the t3rn ecosystem is the executor. Executor are off-chain participants that can generate yield by executing transactions that where triggered by an on-chain transaction on the t3rn circuit. Once the transaction is finalized an inclusion proof is generated, which is then submitted to the t3rn blockchain, prooving that the transaction was executed correctly. This in turn unlocks the reward to the executor.

## Installation Steps

### 1. System Update
Update your system packages:

```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Required Dependencies
Install the `figlet` utility:

```bash
sudo apt-get install figlet -y
```

### 3. Create Service User
Create a dedicated user for running the t3rn node:

```bash
sudo useradd -r -s /bin/false t3rn
sudo mkdir -p /home/t3rn
```

### 4. T3rn Binary Installation
Download and set up the t3rn executor:

```bash
cd /home/t3rn
wget https://github.com/t3rn/executor-release/releases/download/v0.33.0/executor-linux-v0.33.0.tar.gz
tar -xzvf executor-linux-v0.33.0.tar.gz
rm -rf executor-linux-v0.33.0.tar.gz
```

Set proper permissions:

```bash
sudo chown -R t3rn:t3rn /home/t3rn
```

### 5. Create Systemd Service
Create a new service file:

```bash
sudo nano /etc/systemd/system/t3rn.service
```

Add the following configuration:

```ini
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
Environment=EXECUTOR_MAX_L3_GAS_PRICE=300
Environment=EXECUTOR_PROCESS_PENDING_ORDERS_FROM_API=true
Environment=PRIVATE_KEY_LOCAL=**your_private_key_here**

# Working directory
WorkingDirectory=/home/t3rn/executor/executor/bin

[Install]
WantedBy=multi-user.target
```

### 6. Configure the Service

- Replace `**your_private_key_here**` with your actual EVM wallet private key.
- Save and exit (CTRL+X, then Y, then Enter).

### 7. Start the Service

Reload systemd to recognize the new service:

```bash
sudo systemctl daemon-reload
```

Enable service to start on boot:

```bash
sudo systemctl enable t3rn
```

Start the service:

```bash
sudo systemctl start t3rn
```

### 8. Monitor the Node

#### Check Service Status

```bash
sudo systemctl status t3rn
```

#### View Logs

- View all logs:

```bash
sudo journalctl -u t3rn
```

- Follow logs in real-time:

```bash
sudo journalctl -u t3rn -f
```

## Service Management

Common Commands:

- **Stop the service**: 

```bash
sudo systemctl stop t3rn
```

- **Restart the service**: 

```bash
sudo systemctl restart t3rn
```

- **Disable autostart**: 

```bash
sudo systemctl disable t3rn
```
