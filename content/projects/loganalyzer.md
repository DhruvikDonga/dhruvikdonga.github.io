+++
authors = ["Dhruvik Donga"]
title = "Log Analyzer with WASM View"
date = "2026-03-22"
description = "A high-performance, self-contained log monitoring engine built with Rust and WebAssembly. It features real-time file tailing, a multi-threaded ETL pipeline, and a reactive dashboard powered by uPlot"
projectLink = "https://github.com/DhruvikDonga/log_analyzer"
projectName = "loganalyzer"
+++


A high-performance, self-contained log monitoring engine built with **Rust** and **WebAssembly**. It features real-time file tailing, a multi-threaded ETL pipeline, and a reactive dashboard powered by `uPlot`.

### Design Decisions

* **Actor-Based Broadcaster:** To eliminate lock contention, the system uses a Central Hub Pattern. Independent threads for Logs and Metrics "fire and forget" data into an mpsc channel. A dedicated Broadcaster thread owns the WebSocket pool, ensuring one service never blocks another.
* **Non-Blocking History Replay:** New clients receive a 500-item "Replay Cache" (Logs and Metrics) upon connection. This catch-up happens in a decoupled task, allowing the live stream to remain jitter-free for existing users.
* **Linear-Time Parsing:** Log lines are processed in a single pass using a streaming BufReader, ensuring $O(n)$ complexity.
* **WASM-Side Aggregation:** The backend sends lean LogUpdate and MetricUpdate packets. The WASM client performs final time-binning into a BTreeMap, offloading UI computation from the main JS thread.


---
![screen-capture3-ezgif com-speed](https://github.com/user-attachments/assets/16792276-3d81-44fe-9b59-2ccf299e60e5)


## 🏗️ Build Instructions

To create the single, self-contained binary, you must build the frontend assets **before** compiling the Rust application so they can be embedded via `rust_embed`.

### 1. Build WASM Library
```bash
cd wasm_client
wasm-pack build --target web
```

### 2. Build Web UI Assets
This generates the dist folder that the Rust binary will "bake" into its own executable.

```bash
cd site
npm install
npm run build
cd ../..
```

### 3. Build & Run the Application

```bash
cargo build
./target/debug/log_analyzer
```

## Usage
The analyzer supports two modes of operation:

### Mode A: Manual File Selection (Default)

Define your log files in `files.yaml` and run:
```bash
./target/debug/log_analyzer
```

### Mode B: Directory Monitoring

Monitor all files within a specific directory:
```bash
./target/debug/log_analyzer --dir_path /home/dhruvik/log_analyzer/example_logs
```

## Configuration
The analyzer relies on two configuration files located in the project root.

### 1. formats.json
Defines timestamp parsing and error classification.

```JSON
{
  "date_formats": [
    { "name": "Common Log", "format": "[%d/%b/%Y:%H:%M:%S %z]" },
    { "name": "JSON ISO",   "format": "%+" },
    { "name": "Syslog",     "format": "%b %d %H:%M:%S" }
  ],
  "error_indicators": ["error", "404", "500", "panic", "critical", "failed"]
}
```

### 2. files.yaml
Used for Mode A (Manual Selection).

```YAML
files:
  - "/home/dhruvik/log_analyzer/example_logs/access.log"
  - "/home/dhruvik/log_analyzer/example_logs/service.log"
```

## Service Endpoints

| Service    | Address | Description |
| -------- | ------- | ------------- |
| Web UI | http://127.0.0.1:7867/    | The dashboard served from memory |
| WebSocket | ws://127.0.0.1:9001/     | Real-time structured log stream |

## Internal Architecture
The system utilizes four primary parallel execution units:

| Unit    | Technology | Description |
| -------- | ------- | ------------- |
| ETL Worker| ``notify`` + ``mpsc`` | Watches files parses strings into structured JSON, and flags errors. |
| Metrics Worker | ``sysinfo`` | Real-time structured log stream |
| Hub Broadcaster | ``mpsc`` | The "Post Office" that manages history caches and WebSocket broadcasting. |
| Web Server | ``rust_embed`` | Custom multi-threaded server delivering assets directly from RAM. |
| WASM client | ``gloo-net`` + ``serde`` | A high-performance subscriber that deserializes binary streams and manages client-side time-binning. |

## Features
- Zero-I/O UI: Assets are served from RAM for maximum speed.
- Monotonic Sorting: Client-side BTreeMap ensures the X-axis is always sorted correctly.
- Resource Efficient: 500ms throttled updates (via TimeoutFuture) prevent CPU spikes during log bursts.
- Logrotate Friendly: Automatically handles file renames and truncations without dropping the stream.
