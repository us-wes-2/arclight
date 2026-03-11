# Arclight — Task List

## Phase 1: Foundation
Get the basic project structure, CLI, and config layer working. No TUI yet — just functional plumbing.

- [ ] **1.1 Project structure** — Set up crate modules: `cli`, `config`, `presets`, `generator`, `runtime`, `tui`
- [ ] **1.2 CLI scaffolding** — Add `clap` with subcommands: `init`, `up`, `down`, `docs`, `status`
- [ ] **1.3 Service presets** — Define the preset data model (image, ports, env vars, volumes, healthcheck, docs URL, code examples). Implement at least Postgres and Redis to start.
- [ ] **1.4 Config layer** — Define the TOML schema. Parse/serialize with `serde`. Support profiles.
- [ ] **1.5 `arc init` flow** — Interactive prompts (using `dialoguer` or `inquire`) to pick services and create the TOML config file.

## Phase 2: Generation & Runtime
Turn config into running containers.

- [ ] **2.1 YAML generator** — Read TOML config + presets → produce a valid `docker-compose.yml`
- [ ] **2.2 `arc up` command** — Generate YAML, then shell out to `docker compose up -d`
- [ ] **2.3 `arc down` command** — Shell out to `docker compose down`
- [ ] **2.4 `arc status` command** — Query running containers and print a table (name, status, ports)
- [ ] **2.5 `arc docs` command** — Look up a service preset and print docs URL + code examples
- [ ] **2.6 Stack versioning** — Support multiple profiles in the TOML, pass `--profile` flag to `arc up`

## Phase 3: TUI Dashboard
The fun part.

- [ ] **3.1 TUI scaffolding** — Set up `ratatui` + `crossterm`, basic app loop with quit handling
- [ ] **3.2 Left panel — container list** — Show running containers with status indicators (●), highlight selected
- [ ] **3.3 Right bottom — log panel** — Stream logs for the selected container
- [ ] **3.4 Right top — metrics graphs** — CPU, memory, network sparklines/graphs for selected container
- [ ] **3.5 Status bar** — Profile name, service count, uptime
- [ ] **3.6 Keyboard navigation** — Arrow keys to select containers, tab to switch panels, `q` to quit
- [ ] **3.7 Auto-launch TUI on `arc up`** — After services start, transition into the dashboard

## Phase 4: Polish & Extras
Nice-to-haves once the core works.

- [ ] **4.1 Expand service catalog** — Add MongoDB, RabbitMQ, MinIO, MySQL, Kafka
- [ ] **4.2 Error handling & UX** — Friendly error messages when Docker isn't running, ports are in use, etc.
- [ ] **4.3 Config validation** — Warn on port conflicts, duplicate services, invalid profiles
- [ ] **4.4 Color theming** — Make the TUI look good. Sci-fi aesthetic if we're feeling it.
- [ ] **4.5 `arc list`** — List all available service presets
- [ ] **4.6 Generated YAML comments** — Add helpful comments to generated compose files (what each service does, how to connect)

## Notes
- Always use `latest` image tags for now — no version pinning yet
- TOML is source of truth, YAML is generated output (could be gitignored)
- Start simple, iterate. Get `arc init` → `arc up` → containers running before touching the TUI
