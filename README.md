# CarRent Pro — Automation-Centric Car Rental Management System

A full-stack car rental management system driven by Playwright automation, Jira integration, and an AI-assisted development workflow.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + Vite + TypeScript + Tailwind CSS |
| Backend | Node.js + Express + TypeScript |
| Database | SQLite (better-sqlite3) |
| Testing | Playwright + TypeScript |
| Integration | Jira REST API v3 |
| Workflow | Claude Code + ts-node scripts |

## Quick Start

### 1. Install & Setup

```bash
npm install
npm run setup
```

This installs all dependencies across backend, frontend, and tests, installs Playwright browsers, and creates your `.env` file.

### 2. Configure Environment

Edit `.env` with your settings (Jira credentials are optional):

```env
PORT=3001
CORS_ORIGIN=http://localhost:5173
# Jira (optional)
JIRA_BASE_URL=https://your-org.atlassian.net
JIRA_EMAIL=you@company.com
JIRA_API_TOKEN=your_token
JIRA_PROJECT_KEY=CRM
```

### 3. Run the Application

```bash
npm run dev
```

- Frontend: http://localhost:5173
- Backend API: http://localhost:3001/api

### 4. Run Tests

```bash
npm test                # Headless
npm run test:headed     # With browser UI
npm run test:report     # Open HTML report
```

### 5. Full Automation Workflow

```bash
npm run workflow
```

Runs: install → start servers → execute tests → create Jira bugs on failure.

---

## Project Structure

```
├── app/
│   ├── backend/              # Express API + SQLite
│   │   └── src/
│   │       ├── server.ts     # Entry point
│   │       ├── database/     # SQLite setup + seeding
│   │       ├── models/       # TypeScript interfaces
│   │       ├── routes/       # cars.ts, rentals.ts
│   │       └── middleware/   # Error handler
│   └── frontend/             # React + Vite + Tailwind
│       └── src/
│           ├── App.tsx
│           ├── api/          # Axios client
│           ├── components/   # Reusable UI components
│           ├── pages/        # Home, Rentals, History, Admin
│           └── types/        # Shared TypeScript types
├── tests/                    # Playwright framework
│   ├── playwright.config.ts
│   ├── specs/                # Test scenarios
│   ├── pages/                # Page Object Models
│   └── helpers/              # Utils + Jira reporter
├── jira/                     # Jira REST API integration
│   ├── jira-service.ts       # Core API client
│   └── create-bug.ts         # Bug creation from failed tests
├── scripts/                  # Automation orchestration
│   ├── run-workflow.ts       # Full CI-style workflow
│   ├── setup.ts              # One-time setup
│   ├── logger.ts             # Structured logging
│   └── process-manager.ts    # Server start/stop
├── database/                 # SQLite database file (auto-created)
├── reports/                  # Test reports + logs
├── screenshots/              # Playwright failure screenshots
├── .env.example
└── package.json
```

---

## API Reference

### Cars

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/cars` | List all cars (supports `?brand=&model=&category=&status=`) |
| GET | `/api/cars/available` | Available cars only |
| GET | `/api/cars/:id` | Get car by ID |
| POST | `/api/cars` | Add new car |
| PUT | `/api/cars/:id` | Update car |
| DELETE | `/api/cars/:id` | Delete car |
| POST | `/api/cars/:id/rent` | Rent a car |
| POST | `/api/cars/:id/return` | Return a rented car |

### Rentals

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/rentals` | Active rentals |
| GET | `/api/rentals/history` | All rental history |

---

## Playwright Test Scenarios

| # | Scenario | Page |
|---|----------|------|
| 1 | View Available Cars | Home |
| 2 | Search Cars by Brand | Home |
| 3 | Search Cars by Category | Home |
| 4 | Reset Search | Home |
| 5 | Add New Car | Admin |
| 6 | Edit Car Details | Admin |
| 7 | Delete Car | Admin |
| 8 | Rent a Car | Home → Modal |
| 9 | Return a Car | Rentals |
| 10 | Verify Rental Persistence After Refresh | History |

---

## Jira Automation Workflow

```
Test Run
  ↓
Failed Tests → reports/failed-tests.json
  ↓
npm run bugs:create
  ↓
Jira Bug Created: [AUTO][PLAYWRIGHT] Failed Scenario - <Name>
  (duplicate check prevents re-creation within 7 days)
```

Bug tickets include:
- Scenario name + expected vs actual
- Full stack trace
- Screenshot path
- Reproduction steps
- Environment info

---

## Application Features

**User**
- Browse all cars with status badges
- Search/filter by brand, model, category, status
- Rent available cars (fills customer info)
- Return rented cars (auto-calculates cost)
- View active rentals with live cost estimate
- View full rental history table

**Admin**
- Fleet statistics (total / available / rented)
- Add new car with form validation
- Edit car details inline
- Delete cars (blocked if actively rented)
- Toggle car status manually

---

## Definition of Done

- [x] Application runs locally (`npm run dev`)
- [x] No TypeScript errors
- [x] Build passes (`npm run build`)
- [x] Playwright tests execute headless (`npm test`)
- [x] HTML report generated automatically
- [x] Screenshots captured on failure
- [x] Failed tests create Jira bugs (when configured)
- [x] `.env.example` included
- [x] README generated
- [x] SQLite database auto-seeded with 8 sample cars
- [x] Clean folder structure with SOLID principles
- [x] Reusable components and utilities
