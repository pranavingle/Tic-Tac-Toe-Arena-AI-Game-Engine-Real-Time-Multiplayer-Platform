# System Architecture — Tic-Tac-Toe Arena

> **Document Status**: Active / Living Document  
> **Last Updated**: 2026-09-23  
> **Current Project Phase**: Phase 1 — Project Foundation  

---

## 1. High-Level Architecture Overview

Tic-Tac-Toe Arena follows a modular, decoupled architecture separated into distinct domain layers. A core design principle is **strict isolation of domain logic**: the Game Engine and AI Engine are completely independent of UI frameworks, HTTP frameworks, and database persistence layers.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FRONTEND (Next.js / React)                      │
│     UI Presentation ── User Input Handling ── Local Board Visualizer    │
└──────────────────┬─────────────────────────────────┬───────────────────┘
                   │ HTTP / REST                     │ WebSockets
                   ▼                                 ▼
┌────────────────────────────────────────────────────────────────────────┐
│                        BACKEND (Express.js / Node.js)                  │
│     Auth Controller ── Room Manager ── Socket.IO Server Sync Coordinator │
└──────────┬──────────────────────┬──────────────────────────┬───────────┘
           │                      │                          │
           ▼                      ▼                          ▼
┌──────────────────┐    ┌──────────────────┐       ┌─────────────────────┐
│   GAME ENGINE    │    │    AI ENGINE     │       │ DATABASE / PRISMA   │
│  (Pure Domain)   │    │ (Minimax / A-B)  │       │ (PostgreSQL Store)  │
└──────────────────┘    └──────────────────┘       └─────────────────────┘
```

> **Note**: In Phase 1, all engines, controllers, database schemas, and UI components are **PLANNED** architecture.

---

## 2. Component Architectural Specifications

### 2.1 Game Engine (`/engines/game-engine`) `[PLANNED]`
- **Responsibility**: Answers *"What happened and what is allowed according to rulebook mechanics?"*
- **Characteristics**:
  - 100% pure, deterministic TypeScript code.
  - Zero external dependencies (No React, Express, Socket.IO, or Prisma).
  - Handles board state representation (3x3 grid array), player turn toggling, move validation, win condition checking (8 winning lines), draw checking, and move history state generation.

### 2.2 AI Engine (`/engines/ai-engine`) `[PLANNED]`
- **Responsibility**: Answers *"What move should the AI select given current board state and depth strategy?"*
- **Characteristics**:
  - Independent of UI rendering and network protocols.
  - Consumes Game Engine state to generate legal move lists.
  - Implements classical decision algorithms:
    - **Minimax Search**: Recursive game tree evaluation.
    - **Alpha-Beta Pruning**: Branch cutoff optimizations.
    - **Heuristic Evaluation**: Static evaluation for depth-limited searches.
    - **Move Ordering**: Priority sorting of center/corner moves to maximize pruning efficiency.
  - **Telemetry**: Measures real-time execution statistics (evaluated nodes, pruned branches, elapsed time ms).

### 2.3 Rating Engine (`/engines/rating-engine`) `[PLANNED]`
- **Responsibility**: Calculates rating adjustments post-match (Elo / Glicko-2) based on opponent ratings and match outcome.

### 2.4 Backend Application (`/backend`) `[PLANNED]`
- **Responsibility**: API routes, user authentication, security validation, WebSocket server coordination, and database transaction dispatching.
- **Key Modules**:
  - **Auth Service**: User registration, bcrypt password hashing, JWT signing/verification.
  - **Room Manager**: In-memory active game session tracker and room code allocator.
  - **Socket Handler**: Real-time event communication channel (`game:join`, `game:move`, `game:state`, `game:over`).
  - **Server Authority**: Every client move is validated against the Game Engine instance on the backend before emitting state changes to players.

### 2.5 Database Layer (`/database`) `[PLANNED]`
- **Technology**: PostgreSQL with Prisma ORM.
- **Entities**:
  - `User`: Accounts, credentials, ratings, match summary counters.
  - `Match`: Historical game records, outcome, duration, game mode.
  - `MoveHistory`: Sequential array of moves for match replay reconstruction.

### 2.6 Frontend Application (`/frontend`) `[PLANNED]`
- **Technology**: Next.js, React, Tailwind CSS, Framer Motion.
- **Responsibility**: UI rendering, board state visualization, user interactions, local game modes, WebSocket connection client.

---

## 3. Communication Patterns & Data Flow

### Local vs AI Match Flow `[PLANNED]`
1. User clicks grid cell in Frontend UI.
2. Local Game Engine validates move and updates UI state.
3. If vs AI, Frontend calls AI Engine (`getBestMove(boardState, difficulty)`).
4. AI Engine computes move + telemetry, returns move to Game Engine.
5. Game Engine updates state, triggers UI win/draw check.

### Real-Time Multiplayer Flow `[PLANNED]`
1. Client sends `game:move` payload to Backend via Socket.IO.
2. Backend validates user JWT, room membership, and turn sequence.
3. Backend passes move to server-side Game Engine instance.
4. If legal, Backend updates room state, saves to database if match finishes, and broadcasts `game:state` to both room participants.
5. If illegal, Backend emits `game:error` payload to originating client without altering room state.

---

## 4. Repository Directory Structure

```
tic-tac-toe-arena/
├── docs/                      # Architectural & control documentation
├── frontend/                  # Presentation layer
├── backend/                   # Service & communication layer
├── engines/                   # Domain logic engines
│   ├── game-engine/           # Core rulebook engine
│   ├── ai-engine/             # Minimax & Alpha-Beta decision engine
│   └── rating-engine/         # Player rating engine
├── database/                  # Schema definition & migrations
├── tests/                     # Test automation suites
├── package.json               # Package configuration
├── tsconfig.json              # TypeScript compilation base
├── .gitignore                 # Version control ignore rules
├── .env.example               # Planned environment configuration template
└── README.md                  # System overview
```
