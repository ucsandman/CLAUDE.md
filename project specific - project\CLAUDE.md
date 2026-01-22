# PROJECT

*Claude Agent Instructions · Updated YYYY MM DD*

## Context Hygiene

At the start of every response, output `[Context: ~X% used]` and estimate based on conversation length.

Thresholds  
* 0 to 30% Fresh context  
* 30 to 60% Normal  
* 60 to 80% Caution  
* 80% plus Danger  

Never auto compact. When context exceeds 70%, ask:

> Context at ~70%. Want me to compact now, or continue? If we compact, I will preserve the current task state.

Before any compaction, state  
1. What will be preserved  
2. What will be lost  
3. Ask for confirmation  

If the conversation goes sideways, use `Esc Esc` to roll back to a known good checkpoint.

## Overview

This repository contains a modular software system composed of a user interface, backend services, and autonomous processes. The system is designed to support long running workflows, real time coordination, and human in the loop oversight.

No assumptions should be made about the business model, end users, or application domain.

## Core Components

| Component | Description | Implementation |
|---------|------------|----------------|
| thing | thing for thing and thing | Web framework, WebSocket |
| Backend Services | Coordination and state management | Python service |
| thing | thing | Python |
| Public Interface | Optional external facing pages | Web framework |

Legacy components may exist but should not be used.

## System Architecture

```

┌──────────────────────────────────────────────┐
│              Control Interface               │
│        UI, State Views, Interaction           │
│              localhost:3001                  │
└──────────────────────┬───────────────────────┘
│
WebSocket and REST
│
┌──────────────────────▼───────────────────────┐
│               Backend Services               │
│              localhost:8765                  │
└──────────────────────┬───────────────────────┘
│
┌──────────────────────▼───────────────────────┐
│               thing                          │
│                                              │
└──────────────────────┬───────────────────────┘
│
┌──────────────────────▼───────────────────────┐
│               Local Database                 │
│                 SQLite File                  │
└──────────────────────────────────────────────┘

````

## Quick Start

### Single Command

From the project root directory

```bash
python launch.py
````

Optional flags may disable specific subsystems.

This starts

* UI server
* Backend service
* Background processes

Stop with Ctrl plus C.

### Manual Start

```bash
cd backend
python scripts/run_server.py

cd ui
npm install
npm run dev
```

Open `http://localhost:3001`

## Local Endpoints

| Service   | Address                                        |
| --------- | ---------------------------------------------- |
| UI        | [http://localhost:xxxx](http://localhost:xxxx) |
| Backend   | [http://localhost:xxxx](http://localhost:xxxx) |
| WebSocket | ws://localhost:xxxx/ws                         |

## Project Layout

```
project-root/
├── ui/                     # Frontend application
│   ├── app/
│   ├── components/
│   ├── hooks/
│   └── state/
│
├── backend/                # Services and workers
│   ├── processes/
│   ├── api/
│   ├── database/
│   └── scripts/
│
├── data/                   # Local persistence
│   └── app.db
│
├── docs/                   # Internal documentation
└── assets/                 # Non code resources
```

## Database

Type SQLite
Location `data/app.db`

Tables may include

* entities
* events
* state_snapshots
* task_logs

Exact schema is subject to change.

## Process Fleet

| Process     | Responsibility         | Execution  |
| ----------- | ---------------------- | ---------- |
| Worker A    | Data discovery         | Scheduled  |
| Worker B    | Evaluation and scoring | Scheduled  |
| Worker C    | Deep analysis          | On demand  |
| Worker D    | Execution              | Continuous |
| Coordinator | Oversight and routing  | Continuous |

Run processes

```bash
cd backend
python thing.py
python scripts/thing.py
python scripts/thing.py
```

## Configuration

UI environment variables in `ui/.env`

```env
PUBLIC_WS_URL=ws://localhost:XXXX/ws
PUBLIC_API_URL=http://localhost:XXXX
```

Backend environment variables in `backend/.env`

```env
API_KEY=your key here
DATABASE_PATH=../data/app.db
```

## Interface Shortcuts

Press `?` in the UI for the full list.

| Input       | Result             |
| ----------- | ------------------ |
| Ctrl plus K | Command palette    |
| g then 1    | Primary view       |
| g then 2    | Secondary view     |
| Esc         | Close active panel |
| Shift click | Multi select       |

## Operational Notes

Technical

* Prefer 127.0.0.1 over localhost on Windows
* Avoid emoji literals in source files
* Delete build cache if parsing errors occur
* UI requires backend service to function correctly

Process conventions

* All processes inherit from a shared base class
* State changes emit events
* Certain actions require human approval

## Debugging Protocol

For reproducible issues

1. Stop before changing code
2. Identify the delta between working and failing paths
3. Add logging in both paths
4. Compare output before proposing a fix
5. Change one variable at a time
6. Never say fixed, say please test this change

## Documentation Index

* docs/README.md
* docs/specs/
* docs/guides/
* docs/sops/
* docs/testing/
* docs/plans/
* docs/archive/

## Brand and Design

Brand guidelines and asset inventory live here:

`brand/BRAND_GUIDELINES.md`

When editing marketing pages, UI, favicon, metadata, or social preview images, follow the rules in that file first.
