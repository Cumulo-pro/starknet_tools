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

## 🔹 `sync_timers`

This is a `histogram` metric that tracks the **timing (in seconds)** of different synchronization operations on a Juno node.

It breaks down the observed durations into **buckets**, with each operation labeled by `op` — such as `fetch`, `store`, and `verify`.

---

### 🛠️ Histogram Buckets Breakdown

Each operation includes:

- `sync_timers_bucket{op="X", le="Y"}`: the cumulative count of operations of type `X` that completed in `≤ Y` seconds.
- `sync_timers_count{op="X"}`: total number of observations for operation `X`.
- `sync_timers_sum{op="X"}`: total time spent across all observations for operation `X`.

---

### ⛓️ Operation Types

#### 1. `fetch`
- **Description**: Time taken to fetch blocks or data from peers.
- **Count**: `1324`
- **Total Time**: `3878.64` seconds
- **Grafana Panel**:
  - Type: `Histogram`
  - Legend: `fetch duration`
  - Suggestion: Use `histogram_quantile(0.95, rate(sync_timers_bucket{op="fetch"}[5m]))` to track 95th percentile latency.

#### 2. `store`
- **Description**: Time taken to store blocks or state locally.
- **Count**: `1307`
- **Total Time**: `127.20` seconds
- **Grafana Panel**:
  - Type: `Histogram`
  - Legend: `store duration`
  - Use `histogram_quantile` to track trends and outliers.

#### 3. `verify`
- **Description**: Time spent verifying block validity or consensus.
- **Count**: `1308`
- **Total Time**: `139.98` seconds
- **Grafana Panel**:
  - Type: `Histogram`
  - Legend: `verify duration`
  - Good for debugging slowness or peer issues.

---


