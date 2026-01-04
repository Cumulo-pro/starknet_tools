# STARKNET FULL NODE WITH JUNO — TESTNET (SEPOLIA)

Complete guide to deploy, sync, and operate a **Starknet full node using Juno** on **Sepolia Testnet**, including systemd setup, snapshots, and staking process.

---

## Infrastructure

- **Server:** BIG VELIA 3

---

## Validator Information

### Explorer (Validator)
- https://sepolia.voyager.online/staking?validator=0x02ed848a45985f6715f5955368597f1bbc7f140b1d16e04d40ca28a921e59657

### Wallets — Braavos Mon

- **Test staker**  
  `0x02ed848a45985f6715f5955368597f1bbc7f140b1d16e04d40ca28a921e59657`

- **Test operator**  
  `0x06bec7db6f576e9245c948cb5f3a1f9af41ae63dba7535caa0e35164b163ac93`

---

## Explorer Dashboard

- https://sepolia.voyager.online/staking-dashboard

---

## Faucet

- https://starknet-faucet.vercel.app/

---

## Contracts / Chain Info

- https://docs.starknet.io/resources/chain-info/#sepolia_4

---

## Bootstrap Data

### Ethereum RPC (Sepolia)

- Alchemy Dashboard:  
  https://dashboard.alchemy.com/apps/kefeenic80369uzg/networks

- WSS Endpoint:
```text
wss://eth-sepolia.g.alchemy.com/v2/oR8C4R92EtGRr7W4cuxVtg1-JxucfbLL
```

---

# 🧪 Running a Juno Node on Starknet Testnet

## 1) ⚙️ System Requirements (one-time)

```bash
sudo apt update
sudo apt install -y libjemalloc-dev libjemalloc2 pkg-config libbz2-dev unzip
make install-deps
```

---

## 2) 📥 Download Juno v0.15.16

```bash
curl -LOJ https://github.com/NethermindEth/juno/releases/download/v0.15.16/juno-v0.15.16-linux-amd64.zip
unzip juno-v0.15.16-linux-amd64.zip
sudo mv ./juno-v0.15.16-linux-amd64 /usr/local/bin/juno
sudo chmod +x /usr/local/bin/juno
```

### Verify version

```bash
juno -v
```

---

## 3) 📁 Create data directory

```bash
mkdir -p /home/cumvelia3-27/junodata_testnet
chown cumvelia3-27:cumvelia3-27 /home/cumvelia3-27/junodata_testnet
```

---

## 4) 🛠️ Configure systemd service

### File: `/etc/systemd/system/juno-testnet.service`

```ini
[Unit]
Description=Juno Starknet Testnet
After=network.target

[Service]
Type=simple
User=cumvelia3
ExecStart=/usr/local/bin/juno \
  --network sepolia \
  --http \
  --http-port 6063 \
  --http-host 0.0.0.0 \
  --db-path /home/cumvelia3/junodata_testnet \
  --eth-node "wss://eth-sepolia.g.alchemy.com/v2/oR8C4R92EtGRr7W4cuxVtg1-JXucfbLL" \
  --metrics \
  --metrics-port 9093 \
  --metrics-host=0.0.0.0 \
  --ws \
  --ws-port 6064 \
  --ws-host 127.0.0.1 \

Restart=on-failure
RestartSec=10
LimitNOFILE=50000

[Install]
WantedBy=multi-user.target
```

---

## 5) 🚀 Start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable juno-testnet
sudo systemctl restart juno-testnet
sudo journalctl -fu juno-testnet
```

---

# 📦 SNAPSHOT — Juno Sepolia

## 1) ⛔ Stop the service

```bash
sudo systemctl stop juno-testnet
```

## 2) 🧹 Clean data directory

```bash
rm -rf /home/cumvelia3/junodata_testnet/*
```

## 3) 📥 Download and extract snapshot (using screen)

```bash
screen -S juno_snapshot
```

Inside the session:

```bash
wget -O juno_sepolia.tar https://juno-snapshots.nethermind.io/files/sepolia/latest
tar -xvf juno_sepolia.tar -C /home/cumvelia3-27/junodata_testnet
```

Exit screen:
- Close session: `exit`
- Detach: `Ctrl + A` then `D`

Reattach:
```bash
screen -r juno_snapshot
```

---

# 🧪 Staking Process

- Reference guide:  
  https://darkcenobyte.medium.com/guide-for-solo-staker-on-starknet-phase-1-ca217246efde

- Chain info:  
  https://docs.starknet.io/resources/chain-info/#sepolia_4

- Contract on Voyager (write):  
  https://sepolia.voyager.online/contract/0x03745ab04a431fc02871a139be6b93d9260b0ff3e779ad9c8b377183b23109f1?lang=en&theme=dark#writeContract

---

## Staking Contract

```text
0x03745ab04a431fc02871a139be6b93d9260b0ff3e779ad9c8b377183b23109f1
```

---

## Wallets — Braavos Mon

- **Test staker**  
  `0x02ed848a45985f6715f5955368597f1bbc7f140b1d16e04d40ca28a921e59657`

- **Test operator**  
  `0x06bec7db6f576e9245c948cb5f3a1f9af41ae63dba7535caa0e35164b163ac93`

---

## Staking Configuration Parameters

- **Amount (20 STRK)**  
  `200000000000000000000`

- **reward_address**  
  `0x06bec7db6f576e9245c948cb5f3a1f9af41ae63dba7535caa0e35164b163ac93`

- **operational_address**  
  `0x06bec7db6f576e9245c948cb5f3a1f9af41ae63dba7535caa0e35164b163ac93`

- **pool_enabled**  
  `1`

- **commission**  
  `500`

---

## ✅ Successful Staking Transaction

- Hash:  
  `0x1b4d8789f4b295f3cef7c290eef1690355e71be6fa00a77abc72d7578676e0`

- Explorer:  
  https://sepolia.voyager.online/tx/0x1b4d8789f4b295f3cef7c290eef1690355e71be6fa00a77abc72d7578676e0

---

# 🔄 Update Juno

```bash
# Stop the service if running under systemd
sudo systemctl stop juno.service

# Download new version
curl -LOJ https://github.com/NethermindEth/juno/releases/download/v0.15.0-rc.3/juno-v0.15.0-rc.3-linux-amd64.zip

# Unzip the binary
unzip juno-v0.15.0-rc.3-linux-amd64.zip

# Move binary to /usr/local/bin
sudo mv ./juno-v0.15.0-rc.3-linux-amd64 /usr/local/bin/juno

# Set execution permissions
sudo chmod +x /usr/local/bin/juno

# Verify version
juno -v
```
