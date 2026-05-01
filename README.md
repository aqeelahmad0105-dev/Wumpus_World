# Wumpus_World
<div align="center">

```
██╗    ██╗██╗   ██╗███╗   ███╗██████╗ ██╗   ██╗███████╗    ██╗    ██╗ ██████╗ ██████╗ ██╗     ██████╗ 
██║    ██║██║   ██║████╗ ████║██╔══██╗██║   ██║██╔════╝    ██║    ██║██╔═══██╗██╔══██╗██║     ██╔══██╗
██║ █╗ ██║██║   ██║██╔████╔██║██████╔╝██║   ██║███████╗    ██║ █╗ ██║██║   ██║██████╔╝██║     ██║  ██║
██║███╗██║██║   ██║██║╚██╔╝██║██╔═══╝ ██║   ██║╚════██║    ██║███╗██║██║   ██║██╔══██╗██║     ██║  ██║
╚███╔███╔╝╚██████╔╝██║ ╚═╝ ██║██║     ╚██████╔╝███████║    ╚███╔███╔╝╚██████╔╝██║  ██║███████╗██████╔╝
 ╚══╝╚══╝  ╚═════╝ ╚═╝     ╚═╝╚═╝      ╚═════╝ ╚══════╝     ╚══╝╚══╝  ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═════╝ 
```

# ◈ Wumpus World — Knowledge-Based Agent

**A fully interactive, browser-based simulation of the classic Wumpus World problem,**  
**powered by a propositional logic reasoning engine with Resolution Refutation.**

<br/>

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![AI](https://img.shields.io/badge/Propositional%20Logic-00F090?style=for-the-badge&logo=probot&logoColor=black)](#)
[![License](https://img.shields.io/badge/License-MIT-purple?style=for-the-badge)](#license)

<br/>

> *"An agent that reasons about the world using logic — not luck."*

</div>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Live Demo](#-live-demo)
- [Features](#-features)
- [How It Works](#-how-it-works)
  - [The Wumpus World Environment](#the-wumpus-world-environment)
  - [Propositional Logic Engine](#propositional-logic-engine)
  - [Resolution Refutation](#resolution-refutation)
  - [Agent Decision Strategy](#agent-decision-strategy)
- [Architecture](#-architecture)
- [Getting Started](#-getting-started)
- [Usage Guide](#-usage-guide)
- [UI Reference](#-ui-reference)
- [Technical Deep Dive](#-technical-deep-dive)
- [Author](#-author)
- [License](#-license)

---

## 🧠 Overview

This project implements a **Knowledge-Based Agent** navigating the classic **Wumpus World** — a foundational problem in Artificial Intelligence originally introduced by Michael Genesereth. The agent must explore a grid-based cave, avoid deadly pits and a lurking Wumpus, and retrieve the gold — all without prior knowledge of the world layout.

Rather than using randomness or brute force, the agent maintains a **propositional logic Knowledge Base (KB)** and applies **Resolution Refutation** at every step to logically infer which cells are safe, which are dangerous, and where to move next.

This is a pure, zero-dependency, single-file implementation — no frameworks, no build tools, no backend. Just HTML, CSS, and vanilla JavaScript.

---

## 🚀 Live Demo

> 🌐 **Deployed via GitHub Pages (branch method) — accessible directly in your browser:**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Wumpus%20World-00F090?style=for-the-badge&logo=github&logoColor=black)](https://basam-babar.github.io/wumpus-world/)

**🔗 https://basam-babar.github.io/wumpus-world/**

No installation needed — just click the link above and the simulation runs instantly in your browser.

> Alternatively, you can clone and run it locally:

```bash
git clone https://github.com/basambabar/wumpus-world.git
cd wumpus-world
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

---

## ✨ Features

| Feature | Description |
|---|---|
| 🧩 **Configurable Grid** | Adjustable world size from 3×3 up to 9×9 via interactive sliders |
| 🤖 **Knowledge-Based Agent** | Reasons using formal propositional logic — not heuristics |
| 🔍 **Resolution Refutation Engine** | Full CNF clause resolution with proof-by-contradiction |
| 📡 **Real-Time Percepts** | Live Breeze and Stench indicators update at each agent position |
| 📊 **Metrics Dashboard** | Tracks inference steps, KB clause count, cells visited, safe/danger sets |
| 📜 **KB Clause Log** | Colour-coded live feed of all clauses added to the Knowledge Base |
| ⏭ **Step / Auto Mode** | Step through decisions manually or let the agent run automatically |
| 🎛 **Speed Control** | Adjust auto-play speed from slow (educational) to fast (benchmarking) |
| 🗺 **World Reveal** | On game end, the full hidden world is revealed with all hazard positions |
| 📱 **Responsive Design** | Adapts cleanly to desktop, tablet, and mobile viewports |
| 🎨 **Terminal Aesthetic** | Orbitron + Share Tech Mono typography, scanline overlay, CRT glow effects |

---

## 🔬 How It Works

### The Wumpus World Environment

The world is a randomly generated `R × C` grid cave with the following rules:

- **Agent** always starts at cell `(0, 0)`, which is guaranteed safe
- **Wumpus** — exactly one, placed randomly (not at origin). Adjacent cells emit a **Stench** percept
- **Pits** — scattered at ~18% density (not at origin or Wumpus cell). Adjacent cells emit a **Breeze** percept
- **Gold** — one piece, placed randomly. Reaching it wins the game
- The agent has **no map** — it perceives only its current cell and reasons about the rest

```
┌─────┬─────┬─────┬─────┬─────┐
│  A  │  ~  │     │  ≋  │     │   A  = Agent (start)
│(0,0)│(0,1)│(0,2)│(0,3)│(0,4)│   ~  = Breeze (pit nearby)
├─────┼─────┼─────┼─────┼─────┤   ≋  = Stench (wumpus nearby)
│     │  ⬤  │     │  ◉  │     │   ⬤  = Pit
│(1,0)│(1,1)│(1,2)│(1,3)│(1,4)│   ◉  = Wumpus
├─────┼─────┼─────┼─────┼─────┤   ◆  = Gold
│  ~  │     │  ◆  │     │     │
│(2,0)│(2,1)│(2,2)│(2,3)│(2,4)│
└─────┴─────┴─────┴─────┴─────┘
```

### Propositional Logic Engine

Every piece of knowledge is stored as a **Conjunctive Normal Form (CNF)** clause — a disjunction of literals. Literals follow this naming convention:

| Symbol | Meaning |
|---|---|
| `P_r_c` | There IS a pit at row `r`, col `c` |
| `!P_r_c` | There is NO pit at row `r`, col `c` |
| `W_r_c` | There IS a Wumpus at row `r`, col `c` |
| `B_r_c` | There IS a breeze at row `r`, col `c` |
| `S_r_c` | There IS a stench at row `r`, col `c` |

**Percept encoding rules** (applied at every visited cell):

```
No Breeze at (r,c)  →  ¬Pit(n₁) ∧ ¬Pit(n₂) ∧ ...    [one unit clause per neighbour]
Breeze at (r,c)     →  Pit(n₁) ∨ Pit(n₂) ∨ ...        [one disjunctive clause]
No Stench at (r,c)  →  ¬Wumpus(n₁) ∧ ¬Wumpus(n₂) ∧ ...
Stench at (r,c)     →  Wumpus(n₁) ∨ Wumpus(n₂) ∨ ...
```

### Resolution Refutation

To answer the query *"Is cell (r,c) safe?"*, the engine applies **proof by contradiction**:

```
1. Goal:       Prove ¬Pit(r,c) and ¬Wumpus(r,c)
2. Negate:     Assume Pit(r,c) (the opposite)
3. Add to KB:  Working set = KB ∪ { Pit(r,c) }
4. Resolve:    Repeatedly apply the resolution rule to all clause pairs
               If complementary literal pair (L, ¬L) found →
                   resolvent = remaining literals from both clauses
5. Check:      If empty clause ⊥ derived → CONTRADICTION
               → Our assumption was false → ¬Pit(r,c) is PROVED ✓
6. Saturate:   If no new clauses emerge → cannot prove → UNKNOWN
```

The resolution rule itself:

```
c₁ = [P(0,2), P(1,1)]      "pit at (0,2) OR pit at (1,1)"
c₂ = [¬P(1,1)]              "no pit at (1,1)"
─────────────────────────────────────────────────────────
resolvent = [P(0,2)]         "therefore: pit at (0,2)"
```

### Agent Decision Strategy

At each step, the agent follows a strict priority order:

```
Priority 1 ── Move to an adjacent unvisited cell already proved SAFE
Priority 2 ── BFS through visited/safe cells to any reachable unvisited safe cell
Priority 3 ── No safe moves available → report STUCK (conservative agent)
```

The agent is **purely conservative** — it will never move to an unknown or inferred-dangerous cell.

---

## 🏗 Architecture

The codebase is organized into 9 clearly separated modules within a single HTML file:

```
wumpus_world.html
│
├── STYLES (CSS)
│   ├── Design tokens (CSS custom properties)
│   ├── Layout system (flexbox, responsive)
│   ├── Component styles (panels, buttons, grid cells)
│   └── Animation keyframes (glow, scanlines, entry effects)
│
└── SCRIPT (JavaScript)
    │
    ├── Part 1 — Propositional Logic Primitives
    │   ├── neg(lit)           — Negate a literal
    │   ├── isTaut(clause)     — Detect tautological clauses
    │   ├── cKey(clause)       — Canonical clause fingerprint
    │   └── tryResolve(c1,c2)  — Core resolution rule
    │
    ├── Part 2 — Knowledge Base
    │   ├── addClause(clause)  — Deduplicated CNF clause insertion
    │   ├── tell(lit)          — Assert a unit fact
    │   ├── ask(literal)       — Resolution refutation query
    │   ├── isSafe(r,c)        — Combined safety check
    │   ├── isDanger(r,c)      — Combined danger check
    │   └── perceive(...)      — Encode percepts into logical rules
    │
    ├── Part 3 — World & Pathfinding
    │   ├── adjacents(r,c,R,C) — Get valid neighbours
    │   ├── buildWorld(R,C)    — Generate random Wumpus World
    │   └── bfsPath(...)       — BFS shortest path through safe cells
    │
    ├── Part 4 — Display Helpers
    │   ├── fmtLit(lit)        — Format literal for human reading
    │   └── fmtClause(clause)  — Format CNF clause as logic notation
    │
    ├── Part 5 — Game State     (all mutable state, single source of truth)
    │
    ├── Part 6 — Game Engine
    │   ├── initGame()         — Start fresh episode
    │   ├── doStep()           — Execute one agent decision cycle
    │   ├── toggleAuto()       — Toggle auto-play
    │   ├── startAutoPlay()    — Begin timed auto-step loop
    │   └── stopAutoPlay()     — Halt auto-play, restore button state
    │
    ├── Part 7 — Rendering
    │   ├── renderGrid()       — Draw full world grid
    │   ├── renderPercepts()   — Update breeze/stench indicator boxes
    │   ├── renderStatus()     — Update agent status & position
    │   ├── renderMetrics()    — Update dashboard numbers
    │   ├── renderKBLog()      — Render colour-coded KB clause feed
    │   ├── renderControls()   — Enable/disable step & auto buttons
    │   └── renderAll()        — Master render orchestrator
    │
    ├── Part 8 — Speed Control  (live speed slider, restarts auto interval)
    └── Part 9 — Init           (initial page render before first game)
```

---

## 🛠 Getting Started

### Prerequisites

None. This project has **zero dependencies**. Any modern browser works:

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

### Installation

```bash
# Clone the repository
git clone https://github.com/basambabar/wumpus-world.git

# Navigate into the project
cd wumpus-world

# Open directly — no build step, no server needed
open index.html
```

---

## 📋 Usage Guide

```
┌─────────────────────────────────────────┐
│  1. Set ROWS and COLS with the sliders  │
│  2. Press  ▶ START GAME                 │
│  3. Press  ⏭ STEP  to advance one move  │
│     — or —                              │
│     Press  ▶▶ AUTO  to run continuously │
│  4. Adjust SPEED slider at any time     │
│  5. Watch the KB Log and Metrics panel  │
│     to observe the reasoning process    │
└─────────────────────────────────────────┘
```

**Game outcomes:**

| Outcome | Condition |
|---|---|
| 🏆 **MISSION SUCCESS** | Agent reaches the gold cell |
| 💀 **AGENT DEAD** | Agent steps into a pit or Wumpus cell |
| ⚠️ **NO PATH** | All reachable safe cells exhausted — agent halts |

---

## 🗺 UI Reference

### Cell States

| Symbol | Color | Meaning |
|---|---|---|
| `◈` | Cyan | Agent's current position |
| `✓` | Green | Visited, confirmed safe |
| `○` | Dark green | Unvisited, inferred safe by KB |
| `?` | Dark grey | Unknown — no inference yet |
| `✕` | Red | Inferred dangerous (pit or Wumpus) |
| `~` | Blue | Breeze percept detected |
| `≋` | Orange | Stench percept detected |
| `⬤` | Red | Pit (revealed on game end) |
| `◉` | Orange | Wumpus (revealed on game end) |
| `◆` | Yellow | Gold |

### KB Clause Log Colours

| Colour | Meaning |
|---|---|
| 🟢 Green | Positive unit fact — *"Pit exists at (r,c)"* |
| 🔴 Red | Negative unit fact — *"No pit at (r,c)"* |
| 🔵 Grey-blue | Disjunctive clause — *"Pit at (r,c) ∨ Pit at (r,c')"* |

---

## 🔧 Technical Deep Dive

### Clause Deduplication

Before adding any clause, the engine computes a **canonical sorted key** (`cKey`) to detect and discard logical duplicates regardless of literal ordering:

```javascript
function cKey(clause) {
  return [...clause].sort().join('|');
}
```

### Tautology Pruning

Clauses containing both `L` and `¬L` are **tautologies** (always true, carry no information) and are discarded immediately:

```javascript
function isTaut(clause) {
  return clause.some(l => clause.includes(neg(l)));
}
```

### Resolution Step Budget

The `ask()` method caps resolution at **2,500 steps per query** to maintain real-time responsiveness. On saturated KBs the engine terminates early with `false` (cannot prove).

### BFS Pathfinding Constraint

The BFS in `bfsPath()` only traverses cells in `visited ∪ safeInferred` — it will **never route through an unknown or dangerous cell**, ensuring the agent never takes risks in transit.

---

## 👤 Author

<div align="center">

**Basam Babar**

*Knowledge-Based Systems · Artificial Intelligence · Frontend Engineering*

[![GitHub](https://img.shields.io/badge/GitHub-@basambabar-181717?style=for-the-badge&logo=github)](https://github.com/basambabar)

</div>

---

## 📄 License

```
MIT License

Copyright (c) 2025 Basam Babar

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

<div align="center">

```
◈ KNOWLEDGE-BASED AGENT · PROPOSITIONAL LOGIC · RESOLUTION REFUTATION · WUMPUS WORLD ◈
```

*Built with logic, not luck.*

</div>
