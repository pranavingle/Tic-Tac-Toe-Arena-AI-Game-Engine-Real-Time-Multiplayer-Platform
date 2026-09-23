# Engineering & Development Rules — Tic-Tac-Toe Arena

> **Document Status**: Active / Living Document  
> **Last Updated**: 2026-09-23  
> **Current Project Phase**: Phase 1 — Project Foundation  

---

## 1. Core Engineering Principles

### Rule 1.1: No False Claims (Mandatory Highest Priority)
- **NEVER** mark a feature, task, or phase as "Implemented", "Tested", or "Verified" unless:
  1. The code actually exists in the repository.
  2. The code has been executed and tested.
  3. Verification output has been confirmed.
- **NEVER** fabricate benchmark metrics, test coverage numbers, or historical commits.
- **Phase 1 CANNOT** be marked COMPLETE merely because files were generated. It must pass all stated validation checks and satisfying the Definition of Done.

### Rule 1.2: Documentation-First & Living Documents
- All major implementation tasks must begin by consulting `docs/PRD.md`, `docs/ARCHITECTURE.md`, `docs/RULES.md`, and `docs/PHASES.md`.
- `docs/PROJECT_STATUS.md` must be updated after every completed development milestone.
- If an architectural or design decision changes during development, the corresponding document in `/docs` must be updated immediately.

---

## 2. Architectural Boundaries & Isolation

### Rule 2.1: Game Engine Independence
- The Game Engine (`/engines/game-engine`) must remain **100% pure TypeScript**.
- It must **NEVER** import or depend on React, Next.js, Express, Socket.IO, Prisma, or DOM APIs.
- The Game Engine is the sole source of truth for game mechanics (turn order, move validity, win/draw state).

### Rule 2.2: AI Engine Independence
- The AI Engine (`/engines/ai-engine`) must remain decoupled from UI rendering and networking protocols.
- It consumes Game Engine state and returns recommended moves alongside execution telemetry.
- Minimax and Alpha-Beta algorithms must be implemented as classical algorithms. Do not refer to them as "Machine Learning".

### Rule 2.3: Server-Authoritative Multiplayer
- The client UI is a presentation layer. It must **NEVER** be trusted for authoritative game state.
- All online multiplayer moves must be transmitted to the backend, validated against a backend Game Engine instance, and broadcast to participants by the server.

---

## 3. TypeScript & Coding Standards

### Rule 3.1: Strict Type Safety
- `strict: true` must be enabled in TypeScript configurations.
- Implicit or explicit `any` types are prohibited unless strictly required for external lib integration (and wrapped appropriately).
- Define explicit interfaces and type aliases for all domain objects (`BoardState`, `Move`, `PlayerSymbol`, `GameResult`, `AIMetric`).

### Rule 3.2: Code Cleanliness & Formatting
- Use explicit, descriptive variable and function names.
- Functional purity is preferred for engine calculation methods.
- Every exported function and class in domain engines must include clear JSDoc comments detailing inputs and return values.

---

## 4. Security & Environment Variables

### Rule 4.1: Secret Protection
- **NEVER** commit real passwords, API keys, JWT secrets, database connection strings with passwords, or private tokens to Git.
- Real secret environment variables belong exclusively in un-tracked `.env` files.
- `.env.example` must contain placeholder documentation for all required variables without revealing sensitive values.

### Rule 4.2: Input Validation & Hashing
- All API endpoint payloads must be validated on the backend prior to processing.
- User passwords must be hashed using `bcrypt` prior to database storage. Plaintext passwords must never be logged or stored.

---

## 5. Testing & Verification Rules

### Rule 5.1: Empirical Verification Requirement
- Code edits are not complete until verified by automated tests or direct command execution.
- Tests must cover critical edge cases (e.g., full board draw, diagonal win, invalid move placement, AI blocking move, unauthorized Socket.IO event).
- Superficial test suites written purely to inflate test counts are strictly prohibited.

---

## 6. Development Workflow Sequence

Every implementation step must follow the disciplined sequence:
```
UNDERSTAND ──► INSPECT ──► PLAN ──► IMPLEMENT ──► TEST ──► VERIFY ──► DOCUMENT ──► UPDATE STATUS
```
