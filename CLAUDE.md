# Arclight — Project Context

## What is this?
A Rust CLI that generates Docker Compose files from service presets and displays a live TUI dashboard. Space/sci-fi themed.

## Roles
- **Wes**: Developer — does most of the coding
- **Claude**: PM/TPM — design guidance, task management, code review, unblocking

## Key Design Decisions
- TOML config is source of truth → generates Docker Compose YAML
- Service presets with sensible defaults (always `latest` image tags for now)
- Stack versioning via named profiles in TOML
- TUI dashboard: CloudWatch-style layout — container list (left), metrics graphs (right top), logs (right bottom)
- CLI binary name: `arc`

## Tech Stack
- **CLI**: `clap`
- **TUI**: `ratatui` + `crossterm`
- **Config**: `toml` + `serde`
- **YAML generation**: `serde_yaml`
- **Docker interaction**: `bollard` or shelling out to `docker compose`
- **Interactive prompts**: `dialoguer` or `inquire`

## Architecture Layers
CLI → Config (TOML) → Generator (YAML) → Runtime (docker compose) → TUI (ratatui)

## Reference Docs
- `README.md` — full design doc with TUI mockup and architecture diagram
- `TASKS.md` — phased task list with checkboxes
