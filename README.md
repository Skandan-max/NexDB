# NexDB  
A high-performance, in-memory key-value data store with TTL support, sorted sets, and efficient event-driven I/O.  

NexDB is **inspired by Redis**, but built entirely from scratch in C++ to explore the design of modern in-memory databases.  
It implements a custom binary protocol, TTL-based expiration, and a non-blocking network server optimized for low-latency operations.  

---

##  Features
- **In-Memory Key-Value Store** – Store and retrieve string values at lightning speed.  
- **Custom Binary Protocol** – Compact request/response format for efficient data transfer.  
- **Supported Data Types**  
  - Strings (`SET`, `GET`, `DEL`)  
  - Sorted Sets (`ZADD`)  
  - Key Enumeration (`KEYS`)  
- **TTL Support** – Millisecond-precision expiration (`PEXPIRE`, `PTTL`).  
- **Non-Blocking I/O** – Event-driven connection handling with `poll()`.  
- **Thread Pool for Cleanup** – Frees large structures without blocking the event loop.  
- **Custom Memory Management** – Lightweight hashtable implementation.

---

##  Architecture Overview
### Server (`server.cpp`)
- Listens on `127.0.0.1:1234`.  
- Parses incoming binary commands and executes corresponding handlers.  
- Uses a custom **type-tagged binary protocol**:  
  - `TAG_NIL` – Null / Not found  
  - `TAG_ERR` – Error  
  - `TAG_STR` – String  
  - `TAG_INT` – 64-bit integer  
  - `TAG_DBL` – Double  
  - `TAG_ARR` – Array  

### Client (`client.cpp`)
- Encodes commands into binary protocol format.  
- Sends to server and decodes responses for human-readable output.


## ⚙️ Installation
### Prerequisites
- C++17 compiler (`g++` or `clang++`)  
- Linux / Unix-like OS

### Build
```bash
# Build server
g++ -std=c++17 -O2 server.cpp common.cpp hashtable.cpp zset.cpp list.cpp heap.cpp thread_pool.cpp -o nexdb_server

# Build client
g++ -std=c++17 -O2 client.cpp -o nexdb_client
```

#### Start server:
```bash
./nexdb_server
```
