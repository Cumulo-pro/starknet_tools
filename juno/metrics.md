# 📊 Juno Node Metrics for Grafana

This document describes the Prometheus-exported metrics from a **Juno node**, with suggestions for visualizing them in **Grafana**.

---


## 📑 Table of Contents

1. [🔹 `sync_best_known_block_number` – Best Known Block](#-sync_best_known_block_number)
2. [🔹 `sync_blockchain_height` – Node Sync Height](#-sync_blockchain_height)
3. [🔹 `sync_blocks` – Blocks Synced per Minute](#-sync_blocks)
4. [🔹 `sync_reorganisations` – Chain Reorganizations](#-sync_reorganisations)
5. [🔹 `sync_timers` – Sync Operation Timings](#-sync_timers)
6. [🔹 `blockchain_reads` – Blockchain Read Operations](#-blockchain_reads)
7. [🔹 `rpc_server_requests_latency` – RPC Request Latency](#-rpc_server_requests_latency)
8. [🔹 `rpc_http_requests` – Total HTTP Requests](#-rpc_http_requests)
9. [🔹 `rpc_server_requests` – RPC Server Request Counts](#-rpc_server_requests)
10. [🔹 `process_resident_memory_bytes` – Resident Memory Usage](#-process_resident_memory_bytes)
11. [🔹 `process_virtual_memory_bytes` – Virtual Memory Usage](#-process_virtual_memory_bytes)
12. [🔹 `process_virtual_memory_max_bytes` – Max Virtual Memory](#-process_virtual_memory_max_bytes)
13. [🔹 `process_start_time_seconds` – Process Start Time & Uptime](#-process_start_time_seconds)

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

## 🔹 `blockchain_reads`

This `counter` metric tracks the number of **read operations** performed on the blockchain by method type. It provides insight into which types of data are being accessed most frequently by the Juno node.

---

### 📂 Labels

- **`method`**: Indicates the specific type of read operation being counted.

---

### 🧠 Method Breakdown

#### • `BlockHeaderByNumber`
- **Description**: Number of times a block header was requested by its block number.
- **Count**: `1307`

#### • `Head`
- **Description**: Number of times the current head of the chain was read.
- **Count**: `1`

#### • `HeadState`
- **Description**: Accesses to the current state at the head of the chain.
- **Count**: `1322`

#### • `HeadsHeader`
- **Description**: Reads of multiple headers at once.
- **Count**: `79`

#### • `Height`
- **Description**: Reads requesting the current blockchain height.
- **Count**: `3`

---

## 🔹 `rpc_server_requests_latency`

This `histogram` metric captures the **latency (in seconds)** of RPC requests to the node, broken down by method and API version.

It helps identify slow endpoints and optimize performance of JSON-RPC interactions, particularly in Starknet-based environments.

---

### 🏷️ Labels

- **`method`**: The specific RPC method invoked.
- **`version`**: The API version used in the request path (e.g., `/v0_8`).

---

### 🔍 Example: `method="starknet_blockHashAndNumber"` / `version="/v0_8"`

- **Total Requests**: `1`
- **Total Duration**: `~0.0128s`

This means only one RPC call was made for this method in the scrape window, and it completed in under ~13 milliseconds.

---

### 📦 Bucket Distribution

| Bucket (≤ seconds) | Count |
|--------------------|-------|
| 0.005              | 0     |
| 0.01               | 0     |
| 0.025              | 1     |
| 0.05 - 10          | 1     |
| `+Inf`             | 1     |

---

## 🔹 `rpc_http_requests`

This `counter` metric tracks the **total number of HTTP requests** received by the RPC server.

It is a high-level indicator of how much traffic the node is receiving via HTTP, without breaking it down by method or path.

---

### 🧠 Metric Details

- **Type**: `counter`
- **Example**: `1`

---

## 🔹 `rpc_server_requests`

This `counter` metric tracks the **number of processed RPC requests**, broken down by `method` and `version`.

It’s useful for identifying which specific RPC methods are being called most often.

---

### 🏷️ Labels

- **`method`**: RPC method being called (e.g., `starknet_blockHashAndNumber`)
- **`version`**: API version path (e.g., `/v0_8`)

---

## 🔹 `process_resident_memory_bytes`

This `gauge` metric reports the **resident set size (RSS)** of the process in bytes — the portion of memory occupied in RAM.

---

### 🧠 Metric Details

- **Type**: `gauge`
- **Example**: `1.4558208e+09` (≈ 1.46 GB)
- **Use Case**: Monitor how much physical memory your node process is using.
- **Grafana Panel**: `Stat`, `Gauge`, or `Time Series`
- **Unit**: `bytes` or `bytes (binary)`
- **Suggested Title**: `Resident Memory Usage`

---

## 🔹 `process_virtual_memory_bytes`

This `gauge` metric tracks the **total virtual memory** the process has allocated, including memory not yet loaded into RAM.

---

### 🧠 Metric Details

- **Type**: `gauge`
- **Example**: `3.505983488e+09` (≈ 3.5 GB)
- **Use Case**: Monitor the full memory footprint of your process, including swapped or reserved memory.
- **Grafana Panel**: `Gauge` or `Time Series`
- **Unit**: `bytes` or `bytes (binary)`
- **Suggested Title**: `Virtual Memory Usage`

---

## 🔹 `process_virtual_memory_max_bytes`

This `gauge` metric shows the **maximum amount of virtual memory** available to the process. Typically system-defined.

---

### 🧠 Metric Details

- **Type**: `gauge`
- **Example**: `1.8446744073709552e+19` (Huge value — ~18.4 exabytes)
- **Use Case**: Useful for understanding memory limits imposed by the OS or container environment.
- **Grafana Panel**: `Stat`
- **Unit**: `bytes`
- **Suggested Title**: `Max Virtual Memory`

---

## 🔹 `process_start_time_seconds`

This `gauge` metric shows the **Unix timestamp** (in seconds) when the process started.

---

### 🧠 Metric Details

- **Type**: `gauge`
- **Example**: `1.74551753275e+09` (equivalent to a specific UTC datetime)
- **Use Case**: Helps track uptime or identify unexpected restarts.
- **Grafana Panel**: `Stat`
- **Suggested Query** (for human-readable uptime):
  ```promql
  time() - process_start_time_seconds
