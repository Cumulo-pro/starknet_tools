# STARKNET ATTESTATION WITH JUNO — TESTNET (SEPOLIA)

Complete guide to run the **Starknet Attestation Validator (staking v2)** using **Juno** on **Sepolia Testnet**, based on the official Nethermind implementation.

---

## Software

- **Repository:** https://github.com/NethermindEth/starknet-staking-v2  
- **Releases:** https://github.com/NethermindEth/starknet-staking-v2/releases  
- **Version used:** `v0.2.8`

---

# 1️⃣ Update / Prepare Environment

Clone the repository and checkout the desired release.

```bash
git clone https://github.com/NethermindEth/starknet-staking-v2.git
cd starknet-staking-v2

git fetch --tags
git checkout v0.2.8
```

---

# 2️⃣ Build the Validator

> Requirements: **Go ≥ 1.22**, `make`, `build-essential`

```bash
make validator
```

Verify version:

```bash
./build/validator --version
```

---

# 3️⃣ Validator Configuration (`config.json`)

Save the configuration file at:

```
/home/cumvelia3-27/starknet-staking-v2/config.json
```

```json
{
  "provider": {
    "http": "http://127.0.0.1:6063/v0_8",
    "ws": "ws://127.0.0.1:6064/v0_8"
  },
  "signer": {
    "operationalAddress": "0xxxx",
    "privateKey": "0xxxx"
  }
}
```

### Secure the configuration file

```bash
sudo chown cumvelia3-27:cumvelia3-27 /home/cumvelia3-27/starknet-staking-v2/config.json
chmod 600 /home/cumvelia3-27/starknet-staking-v2/config.json
```

### Optional: Protect the file from deletion (immutable)

```bash
sudo chattr +i /home/cumvelia3-27/starknet-staking-v2/config.json
```

To remove immutability:

```bash
sudo chattr -i /home/cumvelia3-27/starknet-staking-v2/config.json
```

---

# 4️⃣ systemd Service

Create the service file:

```
/etc/systemd/system/starknet-validator.service
```

```ini
[Unit]
Description=Starknet Attestation Validator
After=network-online.target

[Service]
User=cumvelia3
WorkingDirectory=/home/user/starknet-staking-v2
ExecStart=/home/user/starknet-staking-v2/build/validator \
  --metrics --metrics-host "0.0.0.0" --metrics-port "9096" \
  --config /home/user/starknet-staking-v2/config.json
Restart=always
RestartSec=5
LimitNOFILE=50000

[Install]
WantedBy=multi-user.target
```

Reload and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now starknet-validator
sudo systemctl restart starknet-validator
sudo journalctl -fu starknet-validator
```

Stop service:

```bash
sudo systemctl stop starknet-validator
```

---

## ✅ Successful Attestation Logs (Example)

```text
INFO  validator/validator.go:209  Window to attest to {"start": 1497855, "end": 1497860}
DEBUG validator/dispatcher.go:177 preparing attest transaction for blockhash: 0x20db27c1db61aead225dfcd7b2bddbcef8e3c2d3d186738f3b725527e373e0a
DEBUG validator/dispatcher.go:183 built attest transaction successfully
```

---

# 🧪 WebSocket Test

Verify Juno WS endpoint:

```bash
websocat ws://localhost:6069/v0_8
```

Example request:

```json
{"jsonrpc":"2.0","method":"starknet_blockNumber","params":[],"id":1}
```

---

# 📊 Metrics & Health

- **Metrics:**  
  http://148.72.141.64:9096/metrics

- **Health:**  
  http://148.72.141.64:9096/health

---

# 🗂️ Notes on Older Versions

Older releases (e.g. `v0.1.1`) required similar steps:

- Checkout specific tag
- Build validator
- Configure `config.json`
- Create systemd service

Historical reference dashboards:
- https://gist.github.com/Magicking/d8752f10a3e85e845a032e872445c5ed
- Metrics roadmap: https://www.aligned-stake.com/

---

## ℹ️ Important Operational Notes

- The attestation validator **requires a fully synced Juno node**
- Errors like **`Attestation is out of window`** indicate Juno lag
- Ensure:
  - Low-latency local RPC/WS
  - Accurate system time (NTP)
  - Stable disk I/O

---

🟢 **Cumulo — Starknet Infrastructure & Validation**
