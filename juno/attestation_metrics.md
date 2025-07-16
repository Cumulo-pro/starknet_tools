
# 📊 Juno Validator Monitoring — Prometheus Metrics

This documentation describes the key Prometheus metrics exposed by a Juno node with validator capabilities, with a focus on synchronization and the attestation process.

## 🔧 Prerequisites

- A running Juno node exposing `/metrics`
- Prometheus configured to scrape it
- Grafana for visualization (optional but recommended)
---

## 📑 Table of Contents

1. [🔧 Prerequisites](#-prerequisites)
2. [📌 Core Node Metrics](#-core-node-metrics)
   - [🔄 Sync Status](#-sync-status)
   - [🧠 Node Info](#-node-info)
3. [🔐 Validator Attestation Metrics](#-validator-attestation-metrics)
   - [🧭 Epoch Tracking](#-epoch-tracking)
   - [⛓ Block Observation](#-block-observation)
   - [✅ Attestation Activity](#-attestation-activity)
4. [📈 Example Grafana Panels](#-example-grafana-panels)
   - [🟢 Last Attestation Time](#-last-attestation-time)
   - [📊 Epoch Progress](#-epoch-progress)
5. [⚠️ Recommended Alerts](#️-recommended-alerts)
6. [👤 Maintainer](#-maintainer)


---

## 📌 Core Node Metrics

### 🔄 Sync Status

| Metric | Description |
|--------|-------------|
| `sync_blockchain_height` | Current height of the local node. |
| `sync_best_known_block_number` | Best known height seen from peers. |
| `sync_blocks` | Number of blocks synced since startup. |
| `sync_reorganisations` | Total number of chain reorgs. Should be 0 in stable operation. |

### 🧠 Node Info

| Metric | Description |
|--------|-------------|
| `juno_info{version="..."}` | Shows the binary version of the node. |
| `rpc_http_requests` | Total HTTP RPC requests handled. Useful for load insight. |

---

## 🔐 Validator Attestation Metrics

These metrics track attestation duties and activity.

### 🧭 Epoch Tracking

| Metric | Description |
|--------|-------------|
| `validator_attestation_current_epoch_id` | ID of the current epoch. |
| `validator_attestation_current_epoch_length` | Total length of the epoch (in blocks). |
| `validator_attestation_current_epoch_starting_block_number` | Block number where the current epoch started. |
| `validator_attestation_current_epoch_assigned_block_number` | Block number assigned to this validator for attestation. |

### ⛓ Block Observation

| Metric | Description |
|--------|-------------|
| `validator_attestation_starknet_latest_block_number` | Last block seen on the Starknet network. Used to compare against assigned duties. |

### ✅ Attestation Activity

| Metric | Description |
|--------|-------------|
| `validator_attestation_attestation_submitted_count` | Number of attestations submitted by this validator. |
| `validator_attestation_attestation_confirmed_count` | Number of attestations confirmed by the network. |
| `validator_attestation_last_attestation_timestamp_seconds` | Unix timestamp of the last successful attestation. |

---

## 📈 Example Grafana Panels

You can create the following panels using the metrics above:

### 🟢 Last Attestation Time

```promql
time() - validator_attestation_last_attestation_timestamp_seconds
```

- **Type:** Stat
- **Unit:** `s`
- **Thresholds:** < 600s (green), 600–1800s (yellow), >1800s (red)

---

### 📊 Epoch Progress

```promql
(
  validator_attestation_starknet_latest_block_number
  - validator_attestation_current_epoch_starting_block_number
)
/ validator_attestation_current_epoch_length * 100
```

- **Type:** Gauge
- **Unit:** `%`

---

## ⚠️ Recommended Alerts

```yaml
# No attestation in the last 30 minutes
- alert: ValidatorNotAttesting
  expr: time() - validator_attestation_last_attestation_timestamp_seconds > 1800
  for: 5m
  labels:
    severity: critical
  annotations:
    summary: "Validator has not attested in the last 30 minutes"

# Assigned block too far behind latest
- alert: AttestationSlotMissed
  expr: validator_attestation_starknet_latest_block_number
        - validator_attestation_current_epoch_assigned_block_number > 3
  for: 2m
  labels:
    severity: warning
  annotations:
    summary: "Validator may have missed an attestation slot"
```

---

## 👤 Maintainer

**Cumulo Pro — Node Infrastructure & Monitoring**  
GitHub: [@cumulo-pro](https://github.com/cumulo-pro)  
Contact: infra@cumulo.pro
