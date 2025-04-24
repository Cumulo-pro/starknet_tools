# 📊 Juno Node Metrics for Grafana

This document describes the Prometheus-exported metrics from a **Juno node**, with suggestions for visualizing them in **Grafana**.

---

## 🔹 `sync_best_known_block_number`

- **Description**: Represents the highest block number known to the node — essentially the latest height the network has reached according to this node.
- **Type**: `gauge`
- **Example**: `1.344464e+06` (1,344,464)
- **Grafana Panel**: `Stat`
- **Unit**: `none` or `short`
- **Suggested Title**: `Best Known Block`

---

## 🔹 `sync_blockchain_height`

- **Description**: Indicates the height of the most recent block that the node has successfully synced. If this number is lower than `sync_best_known_block_number`, the node is still catching up.
- **Type**: `gauge`
- **Example**: `1.194557e+06` (1,194,557)
- **Grafana Panel**: `Stat` or `Gauge`
- **Unit**: `none` or `short`
- **Suggested Title**: `Node Sync Height`

---

## 🔹 `sync_blocks`

- **Description**: A cumulative counter of blocks synchronized by the node since it started running.
- **Type**: `counter`
- **Example**: `1307`
- **Grafana Panel**: `Time Series`
- **Transform**: `rate(sync_blocks[5m])`
- **Suggested Title**: `Blocks Synced per Minute`

---

## 🔹 `sync_reorganisations`

- **Description**: Tracks how many blockchain reorganizations the node has experienced. A reorganization occurs when the node replaces part of its chain with an alternative chain that is longer or otherwise valid.
- **Type**: `counter`
- **Example**: `0`
- **Grafana Panel**: `Stat`
- **Suggested Title**: `Chain Reorganizations`

---


