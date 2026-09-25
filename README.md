# PRAMAAN — Browser-First Interview Integrity Layer

> *“A fake face can fool a frame. It cannot fool a live interaction.”*

For the comprehensive team context, engineering architecture, and Round 1 demo plan, please see:
👉 **[ROUND_1_KILLER_PROTOTYPE_PLAN.md](file:///d:/musa-ada/Pramaan/ROUND_1_KILLER_PROTOTYPE_PLAN.md)**

---

## Quick Start

### 1. Backend Server (`server/`)
```bash
cd server
npm install
npm run db:migrate
npm run db:seed
npm run dev
```
Starts Express + Socket.IO server at `http://localhost:5001`.

### 2. Frontend Prototype (`pramaan-frontend-prototype/`)
```bash
cd pramaan-frontend-prototype
npm install
npm run dev
```
Starts Next.js prototype at `http://localhost:3000`.

### 3. Automated Verification Engine
```bash
npx tsx verify-all.ts
```
Executes all 28 automated test assertions across authentication, session management, biometric consent, signals, risk scoring, fairness invariants, and privacy guards.
