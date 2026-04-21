# ◎ Progress Hub

> Track your goals, crush your milestones.

**Progress Hub** is a full-stack goal-tracking app built with React and Node.js/Express. Create goals, break them into milestones, visualize progress with animated rings, and stay on top of deadlines — all in a sleek dark UI.

![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white&style=flat-square)
![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=node.js&logoColor=white&style=flat-square)
![Express](https://img.shields.io/badge/Express-4.x-000000?logo=express&logoColor=white&style=flat-square)
![License](https://img.shields.io/badge/License-MIT-7c6dfa?style=flat-square)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎯 Goal Management | Create, edit, and delete goals with title, description, category, color, and target date |
| 📊 Progress Rings | Animated SVG rings showing completion percentage per goal |
| ✅ Milestones | Break each goal into checkable steps; progress auto-calculates as you tick them off |
| 📅 Deadline Tracking | Countdown shows days remaining; highlights overdue goals in red |
| 📈 Stats Dashboard | Live summary of total, completed, in-progress goals and average progress |
| 🔍 Search & Filter | Filter by status (All / In Progress / Completed / Not Started) and search by title |
| 🎨 Custom Colors & Categories | 8 accent colors and 6 categories (Work, Health, Learning, Finance, Personal, Travel) |
| 📱 Responsive | Works on desktop and mobile |

---

## 🖥️ Tech Stack

```
Frontend          Backend           Tooling
─────────         ─────────         ────────
React 18          Node.js 18+       concurrently
CSS3 (vanilla)    Express 4         GitHub Actions CI
Custom hooks      REST API
SVG animations    In-memory store*
```

> \* Swap the in-memory store for MongoDB or PostgreSQL for production — see [Production](#-production-deployment).

---

## 🚀 Quick Start

### Prerequisites

- **Node.js** 18 or higher — [nodejs.org](https://nodejs.org)
- **npm** 9 or higher (comes with Node)
- **Git**

### 1. Clone

```bash
git clone https://github.com/YOUR_USERNAME/progress-hub.git
cd progress-hub
```

### 2. Install dependencies

```bash
npm run install:all
```

This installs packages for the root, `frontend/`, and `backend/` in one command.

### 3. Start

```bash
npm run dev
```

| Service | URL |
|---|---|
| React app | http://localhost:3000 |
| Express API | http://localhost:5000 |

The frontend proxies `/api` requests to the backend automatically (configured in `frontend/package.json`).

---

## 📁 Project Structure

```
progress-hub/
│
├── backend/
│   ├── server.js          # Express app + all API routes
│   └── package.json
│
├── frontend/
│   ├── public/
│   │   └── index.html
│   └── src/
│       ├── components/
│       │   ├── GoalCard.jsx       # Card shown in the grid
│       │   ├── GoalDetail.jsx     # Side panel with milestones + progress slider
│       │   ├── GoalModal.jsx      # Create / edit modal form
│       │   ├── ProgressRing.jsx   # Animated SVG ring
│       │   └── StatsBar.jsx       # Summary stats in sidebar
│       ├── hooks/
│       │   └── useGoals.js        # Data fetching + state management
│       ├── utils/
│       │   └── api.js             # Fetch wrapper for all API calls
│       ├── styles/
│       │   ├── global.css         # CSS variables, resets, animations
│       │   └── app.css            # All component styles
│       ├── App.jsx                # Root component + layout logic
│       └── index.js               # React entry point
│
├── .github/
│   └── workflows/
│       └── ci.yml                 # GitHub Actions: install + build on push
│
├── .gitignore
├── package.json                   # Root scripts (dev, build, install:all)
└── README.md
```

---

## 🔌 API Reference

Base URL: `http://localhost:5000/api`

### Goals

| Method | Endpoint | Description | Body |
|---|---|---|---|
| `GET` | `/goals` | List all goals | — |
| `GET` | `/goals/:id` | Get single goal | — |
| `POST` | `/goals` | Create a goal | `{ title, description?, category?, color?, targetDate?, milestones? }` |
| `PUT` | `/goals/:id` | Replace a goal | Full goal object |
| `DELETE` | `/goals/:id` | Delete a goal | — |

### Progress & Milestones

| Method | Endpoint | Description | Body |
|---|---|---|---|
| `PATCH` | `/goals/:id/progress` | Set progress manually | `{ progress: 0–100 }` |
| `PATCH` | `/goals/:id/milestones/:milestoneId` | Toggle milestone done/undone | — |

### Stats

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/stats` | Returns `{ total, completed, inProgress, notStarted, avgProgress }` |

#### Example: Create a goal

```bash
curl -X POST http://localhost:5000/api/goals \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Read 12 books this year",
    "category": "learning",
    "color": "#4ECDC4",
    "targetDate": "2024-12-31",
    "milestones": ["Finish book 1", "Finish book 2", "Finish book 3"]
  }'
```

---

## 🏗️ Production Deployment

### 1. Add a real database

Replace the in-memory `goals` array in `backend/server.js` with a proper database. Example with **MongoDB + Mongoose**:

```bash
cd backend && npm install mongoose dotenv
```

```js
// backend/server.js — add at top
require('dotenv').config();
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGO_URI);
```

### 2. Environment variables

Create `backend/.env`:

```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/progresshub
# MongoDB Atlas:
# MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/progresshub
```

### 3. Build the frontend

```bash
npm run build
```

Serve `frontend/build/` statically from Express by adding this to `backend/server.js`:

```js
const path = require('path');
app.use(express.static(path.join(__dirname, '../frontend/build')));
app.get('*', (req, res) =>
  res.sendFile(path.join(__dirname, '../frontend/build/index.html'))
);
```

### 4. Deploy options

| Platform | Notes |
|---|---|
| **Railway** | Connect GitHub repo, add `MONGO_URI` env var, one-click deploy |
| **Render** | Free tier supports Node + MongoDB Atlas |
| **Fly.io** | Great for containerized deploys |
| **Vercel + Railway** | Frontend on Vercel, API + DB on Railway |

---

## 🤝 Contributing

1. Fork the repo
2. Create a branch — `git checkout -b feat/my-feature`
3. Commit — `git commit -m "feat: add my feature"`
4. Push — `git push origin feat/my-feature`
5. Open a Pull Request

---

## 📄 License

MIT © 2024 — free to use, modify, and distribute.
