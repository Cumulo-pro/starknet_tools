# 🧪 Starknet Validator Resources

A curated index of validator and infrastructure resources for the Starknet ecosystem, focused on Juno full nodes and attestation validators aligned with the SNIP-28 staking protocol.

## 🌐 Overview

This repository powers the **Validator Resources** section on the [Starknet Services Page](https://cumulo.pro/services/starknet), displaying detailed, structured data about validator infrastructure and endpoints.

The frontend consumes a single JSON file:

- [`validators.json`](https://github.com/Cumulo-pro/starknet_tools/blob/main/data/validators.json): Validator node endpoints and attestation tooling

Each entry includes public validator data such as name, RPC endpoints, WebSocket URLs, Prometheus metrics, Grafana dashboards, GitHub repos, and monitoring tools.

## ⚙️ How It Works

The data is loaded dynamically by the Cumulo services frontend, offering a clean and searchable UI to explore validator endpoints and monitoring tools.

All resources can be updated via Pull Request, either by contributors or the validators themselves. This ensures timely and decentralized updates to node data across the Starknet ecosystem.

## 🛠️ Contributing

To add or update validator entries:

1. Fork the repo.
2. Edit [`validators.json`](https://github.com/Cumulo-pro/starknet_tools/blob/main/data/validators.json).
3. Submit a Pull Request with a clear summary of your additions or changes.

> Make sure your JSON is valid. You can use [JSONLint](https://jsonlint.com) to verify before committing.

## 📄 JSON Structure Example

```json
{
  "name": "Cumulo Pro",
  "rpc": "http://127.0.0.1:6062",
  "ws": "ws://127.0.0.1:6069",
 ...
}
