# Arclight

A Rust CLI that lets developers spin up local development environments with a single command. Pick your services, and Arclight handles the rest — generating Docker Compose files from opinionated presets and displaying live metrics in a TUI dashboard.

## Vision

Developers shouldn't have to hand-write Docker Compose files for common setups. Arclight provides a catalog of service presets (Postgres, Redis, RabbitMQ, etc.) with sensible defaults. You pick what you need, Arclight generates the compose config, and gives you a live dashboard to monitor everything.

## Core Concepts

### Service Presets
Each supported service is a preset with:
- Docker image (always `latest` for simplicity)
- Default ports, volumes, and environment variables
- Healthcheck configuration
- Links to official documentation
- Code examples for connecting from common languages

### TOML → YAML Pipeline
Arclight uses TOML as the source of truth for stack configuration. When you run `arc up`, it generates a Docker Compose YAML file from the TOML config. Users never edit YAML directly.

### Stack Versioning
Configurations are versioned so you can maintain multiple stack profiles (e.g., `minimal` with just a DB, `full` with the whole kitchen sink). Switch between them for different testing scenarios.

## CLI Commands

| Command | Description |
|---|---|
| `arc init` | Interactive setup — pick services from the catalog |
| `arc up` | Generate compose YAML from TOML and start services, launch TUI dashboard |
| `arc down` | Stop and tear down all services |
| `arc docs <service>` | Show documentation links and connection examples for a service |
| `arc status` | Quick check on running services without the full TUI |

## TUI Dashboard Layout

```
┌──────────────────────────┬──────────────────────────────────┐
│  Containers              │  Metrics / Graphs                │
│                          │                                  │
│  ● postgres    running   │  CPU ▁▂▃▅▇▅▃▂▁▃▅▇  45%         │
│  ● redis       running   │  MEM ▃▃▄▄▅▅▅▄▄▃▃▃  220MB       │
│  ● rabbitmq    running   │  NET ▁▁▂▁▁▃▂▁▁▁▂▁  12KB/s      │
│    > selected            │                                  │
│                          ├──────────────────────────────────┤
│                          │  Logs (postgres)                 │
│  Services: 3/3 healthy   │                                  │
│  Profile: testing        │  ready for connections           │
│  Uptime: 00:14:32        │  listening on 5432               │
│                          │  checkpoint complete             │
└──────────────────────────┴──────────────────────────────────┘
```

- **Left panel**: Container list with status indicators. Select a container to view its details.
- **Right top**: Live CPU, memory, and network graphs (CloudWatch-style) for the selected container.
- **Right bottom**: Streaming logs for the selected container.

## Architecture

```
┌─────────┐     ┌──────────┐     ┌───────────┐     ┌─────────────┐
│   CLI   │────▶│  Config  │────▶│ Generator │────▶│   Runtime   │
│ (clap)  │     │  (TOML)  │     │  (YAML)   │     │ (docker cli)│
└─────────┘     └──────────┘     └───────────┘     └──────┬──────┘
                                                          │
                                                   ┌──────▼──────┐
                                                   │     TUI     │
                                                   │  (ratatui)  │
                                                   └─────────────┘
```

| Layer | Responsibility |
|---|---|
| **CLI** | Argument parsing, command dispatch (`clap`) |
| **Config** | TOML parsing, stack versioning, profile management |
| **Presets** | Service catalog — images, ports, env vars, docs, examples |
| **Generator** | Transforms TOML config into Docker Compose YAML |
| **Runtime** | Wraps `docker compose` commands, reads container stats via Docker API |
| **TUI** | Live dashboard with container list, metrics graphs, and log streaming (`ratatui`) |

## Example TOML Config

```toml
[arclight]
version = "1"
name = "my-project"

[profiles.default]
services = ["postgres", "redis"]

[profiles.full]
services = ["postgres", "redis", "rabbitmq", "minio"]
```

## Initial Service Catalog

- PostgreSQL
- Redis
- MongoDB
- RabbitMQ
- MinIO (S3-compatible storage)
- MySQL
- Kafka

*More to be added over time.*

## Tech Stack

- **Language**: Rust (2024 edition)
- **CLI framework**: `clap`
- **TUI framework**: `ratatui` + `crossterm`
- **Config**: `toml` + `serde`
- **YAML generation**: `serde_yaml`
- **Docker interaction**: `bollard` (Docker Engine API) or shelling out to `docker compose`

## Status

Early development. This is a personal project built for fun and learning.
