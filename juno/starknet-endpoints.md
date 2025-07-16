# Starknet Public Endpoints by Cumulo

This page provides access to Cumulo's public Starknet endpoints, along with clear instructions to test their availability and functionality.

---

## 📡 Available Endpoints

| Type               | URL                                       |
|--------------------|-------------------------------------------|
| JSON-RPC (HTTPS)   | `https://rpc.starknet.cumulo.com.es`      |
| WebSocket (WSS)    | `wss://ws.starknet.cumulo.com.es`         |

---

## ✅ Test the JSON-RPC Endpoint

You can use `curl` to test if the RPC is responding correctly:

```bash
curl -X POST https://rpc.starknet.cumulo.com.es \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"starknet_blockHashAndNumber","params":[],"id":1}'
```

Expected output (example):

```json
{
  "jsonrpc": "2.0",
  "result": {
    "block_hash": "0x...",
    "block_number": 1594275
  },
  "id": 1
}
```

---

## 🧪 Test the WebSocket Endpoint

### 1. Install `wscat` globally

```bash
sudo npm install -g wscat
```

### 2. Connect to the WebSocket endpoint

```bash
wscat -c wss://ws.starknet.cumulo.com.es
```

Expected output:

```
Connected (press CTRL+C to quit)
>
```

### 3. Send a JSON-RPC message

Paste this into the open `wscat` session:

```json
{"jsonrpc":"2.0","method":"starknet_blockHashAndNumber","params":[],"id":1}
```

You should receive a response like:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "block_hash": "0x...",
    "block_number": 1594275
  },
  "id": 1
}
```

---

## 👨‍💻 Maintained by Cumulo

For more information, dashboards, or validator documentation, visit [cumulo.pro](https://cumulo.pro) or check our open-source tools on [GitHub @Cumulo-pro](https://github.com/Cumulo-pro).
