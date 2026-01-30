# DPMP v2 — Dual-Pool Mining Proxy -- Umbrel Community App Store Version

**DPMP v2** is a lightweight, high-reliability **Stratum v1 mining proxy** designed to sit between one or more miners and multiple upstream mining pools. 

It enables **deterministic dual-pool scheduling**, advanced difficulty/extranonce handling, and deep observability via Prometheus metrics — while remaining simple to deploy and operate.

This repository contains the **v2 architecture**, which is a ground-up redesign focused on correctness, robustness, and long-running stability.

---

## Quick Install (UmbrelOS)

In Umbrel, go into the App Store, click on the ellipsis at top-right and select "Community App Stores".

Paste in the Community App Store link below:

https://github.com/ckryza/ckryza-umbrel-app-store.git

...and click on the Add button. You will then see Chris's Community App Store.

Click on the Open button, then click on DPMP v2, and click on the Install button.

DPMP v2 will be installed and the icon will appear on your Umbrel home page.

## Ports

DPMP v2 uses ports 3351 (stratum) and 9210 (metrics), 8855 (GUI)

## Logs

Logs live in /data and auto-rotate at 50 MB x 3.

## Upgrade Behavior

Updates via the Community App Store preserve both config and logs.


## What DPMP v2 Does

At a high level, DPMP v2:

- Accepts Stratum connections from miners (acts like a pool)
- Maintains concurrent Stratum connections to **two upstream pools** (acts like a miner)
- Routes jobs and share submissions according to a **scheduler**
- Ensures correct propagation of:
  - difficulty
  - extranonce
  - job IDs
- Exposes detailed runtime metrics for monitoring and analysis
- Logs structured, machine-readable events for debugging and auditing

The proxy is intentionally **transparent**: miners and pools do not need to be modified or aware that DPMP is in the middle.

---

## Core Features

### 🔀 Dual-Pool Scheduling
- Simultaneous connections to Pool **A** and Pool **B**
- Weight-based scheduling (e.g. `50:50`, `70:30`)
- Time-sliced switching with stickiness controls
- Safe handling of pool transitions to avoid stale or invalid submits

### 🎯 Correct Stratum Semantics
- Strict job ownership tracking per pool
- Extranonce consistency enforcement
- Difficulty forwarding with pool-aware gating
- Duplicate and stale share detection
- Graceful handling of miner reconnects

### 📊 First-Class Observability
- Built-in **Prometheus metrics endpoint**
- Tracks:
  - downstream miner connections
  - upstream pool connections
  - message RX/TX counts
  - difficulty state
  - scheduler behavior
- Designed to integrate with Grafana or custom dashboards

### 🧾 Structured Logging
- JSON-formatted logs
- Explicit event types (e.g. `pool_switched`, `share_result`, `job_forwarded`)
- Designed for both human debugging and automated analysis
- Suitable for ingestion into Loki or other log systems

### 🖥 Web UI
- **NiceGUI-based interface** (primary)
- Live config editing, view logs, and status

---

## Architecture Overview

DPMP v2 operates in three distinct roles simultaneously:

1. **Downstream Pool Role**  
   Listens for miner connections and serves Stratum jobs.

2. **Upstream Miner Role**  
   Connects to real pools, subscribes, authorizes, and receives work.

3. **Scheduler / Router**  
   Decides which pool is “active,” forwards jobs, and routes share submissions safely.

The design emphasizes:
- explicit state tracking
- defensive validation
- clear separation of responsibilities

---

## Project Status

- ✅ Actively used in real mining setups
- ✅ Stable for long-running operation
- 🔧 Configuration, install, and GUI documentation intentionally evolving
- 🚧 Future work planned around:
  - richer GUI (NiceGUI)
  - improved dashboards
  - additional scheduling strategies

---

## What This Repo Intentionally Does *Not* Include (Yet)

To keep the repository clean and safe:

- No live configuration files (local `config.json` is ignored)
- No secrets or credentials
- No logs or backups

---

## Who This Is For

DPMP v2 is designed for users who:

- Run one or more ASIC or CPU/GPU miners that do not internally support dual-pool mining (i.e., Avalon Q, Avalon Nano3S, etc.)
- Want to split hash power across pools deterministically, such as mine to a Bitcoin pool and a Bitcoin Cash pool simultaneously
- Care about correctness, observability, and long-term stability
- Prefer transparent tooling over black-box pool logic

---

## License

MIT License

---

## Disclaimer

This software operates at the Stratum protocol level.  
Misconfiguration can result in rejected shares or lost revenue.  
Use at your own risk and validate behavior carefully in your environment.

---

*More documentation coming soon.*
