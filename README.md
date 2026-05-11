```text
 ██████╗ ██████╗ ██████╗ ██╗████████╗
██╔═══██╗██╔══██╗██╔══██╗██║╚══██╔══╝
██║   ██║██████╔╝██████╔╝██║   ██║
██║   ██║██╔══██╗██╔══██╗██║   ██║
╚██████╔╝██║  ██║██║  ██║██║   ██║
 ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝   ╚═╝
```

*Ephemeral. Elastic. Isolated.*

---

ORBIT is a secure ephemeral cloud compute engine that spins up isolated execution environments on-demand using Docker.

Run code, install dependencies, open interactive terminals, and automatically destroy environments after TTL.

## Features

- **Ephemeral Compute Sessions** - disposable environments per request
- **Docker-Isolated Runtime** - CPU, memory, and process isolation
- **Interactive Terminal** - WebSocket-based live shell access
- **Dynamic Package Installation** - runtime dependency support
- **Auto-Expiry Sessions** - TTL or inactivity-based cleanup
- **Multi-Language Support** - Python, Node.js, Bash
- **API-First Design** - HTTP + WebSocket interfaces

## Core Concepts

- Each session runs in an isolated sandbox environment
- No shared filesystem between sessions
- Stateless API layer responsible only for orchestration
- Fully disposable compute environments
- Horizontally scalable worker architecture

## Quick Start

### Prerequisites
- Docker installed and running
- Go 1.22+
- Python 3.10+

### Run Server

```bash
cd core
go run cmd/server/main.go
```

Server runs at `http://localhost:8080`

### Run Worker Pool

```bash
cd worker
python main.py
```

## API Usage

### Create Session

```json
POST /sessions
{
  "runtime": "python",
  "ttl_seconds": 600
}
```

---

### Execute Code

```json
POST /sessions/{id}/exec
{
  "command": "print(sum(range(100)))"
}
```

---

### WebSocket Terminal

```
ws://localhost:8080/ws/session/{id}
```

Example:

```javascript
const ws = new WebSocket("ws://localhost:8080/ws/session/{id}");

ws.onmessage = (msg) => console.log(msg.data);
ws.send("python3\n");
```

---

## Package Installation

```bash
pip install numpy pandas
```

---

## Example Workflow

```python
session = client.create(runtime="python")

session.exec("print('ORBIT active')")
session.exec("import math; print(math.sqrt(144))")

session.terminal()

session.destroy()
```

---

## System Architecture

```
Client (SDK / CLI / Web)
        |
        v
API Gateway (Go)
        |
        ├── Session Manager
        ├── Scheduler
        v
Redis Queue
        |
        v
Worker Pool (Python)
        |
        v
Docker Execution Layer
        |
        v
Ephemeral Containers
```

---

## Security Model

- Docker namespace isolation
- CPU and memory limits per session
- Non-root container execution
- Optional network isolation (--network=none)
- Automatic container teardown
- Session-level isolation boundaries

---

## Observability

- Session lifecycle tracking
- Execution latency metrics
- Worker health monitoring
- Failure rate tracking
- Container resource usage metrics

---

## Roadmap

### Core Infrastructure
- Warm container pools for faster startup
- Multi-node worker scaling
- Session snapshot and restore

### Developer Experience
- CLI tool (orbit run)
- Python SDK
- TypeScript SDK

### Advanced Features
- Persistent volumes per session
- Real-time log streaming UI
- Multi-language runtime support (Go, Rust, Node)
- Web IDE integration

---

## Use Cases

- AI agent execution runtime
- Secure code sandboxing
- Online IDE backend systems
- Distributed compute job execution
- Interview coding environments

---

## Tech Stack

- Go → API gateway and orchestration layer
- Python → worker execution engine
- Docker → isolation layer
- Redis → queue system
- PostgreSQL → metadata storage
- WebSockets → real-time terminal streaming

---

## Why ORBIT

Most systems:
run code

ORBIT:
creates disposable compute environments on demand

---

## License

MIT
