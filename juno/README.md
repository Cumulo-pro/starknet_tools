# 📡 Juno Node Monitoring — Prometheus & Grafana

This section of the `starknet_tools` repository is dedicated to tracking and analyzing metrics for **Juno nodes** — the Starknet full node written in Go — including optional validator capabilities.

It provides complete resources to help you monitor sync status, validator attestation performance, RPC traffic, and system-level usage. Using **Prometheus and Grafana**, you can maintain visibility and performance over your infrastructure.

---

## 📁 Available Resources

- **[Juno Validator Monitoring — Prometheus Metrics](https://github.com/Cumulo-pro/starknet_tools/blob/main/juno/attestation_metrics.md)**

   This document describes the specific Prometheus metrics exposed by a Juno node when acting as a validator. It includes detailed explanations of attestation epochs, slots, confirmations, and suggested alert rules to track validator uptime and failures.

- **[Juno Node Metrics Documentation](https://github.com/Cumulo-pro/starknet_tools/blob/main/juno/metrics.md)**

   A comprehensive reference for all metrics exported by Juno via `/metrics`. Includes sync state, RPC request counters, memory usage, and histogram metrics like fetch/store/verify. Recommended for both full nodes and validators.

- **[Grafana Dashboard: JUNO Node by Cumulo](https://github.com/Cumulo-pro/starknet_tools/blob/main/juno/JUNO%20Node%20by%20Cumulo-1752653324274.json)**

   A production-ready Grafana dashboard to monitor Juno node activity in real time. It visualizes core sync metrics, validator attestation progress, RPC load, and system stats such as CPU and memory usage. Built and maintained by [Cumulo Pro](https://cumulo.pro).

---

<img width="2116" height="614" alt="image" src="https://github.com/user-attachments/assets/f32ee4b9-d09f-4c1e-b5b1-69005e8cd83a" />


With these resources, you’ll be able to deploy a robust and auditable monitoring system for your Juno nodes, ensuring high availability and visibility over both synchronization and validator responsibilities.


