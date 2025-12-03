# 📝 Todo App with Productivity Features

A modern, feature-rich todo application built with React and Vite, featuring Pomodoro timer, focus mode, and productivity analytics.

## ✨ Features

- ✅ **Task Management** - Add, edit, delete, and complete todos
- 🎯 **Priority Levels** - Set high, medium, or low priority
- 📁 **Categories** - Organize tasks by category
- 📅 **Due Dates** - Set deadlines for your tasks
- 🔍 **Search & Filter** - Find tasks quickly
- 🍅 **Pomodoro Timer** - Built-in time management
- 🎯 **Focus Mode** - Distraction-free task completion
- 📊 **Analytics** - Track your productivity
- 💾 **Local Storage** - Your data persists automatically

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ installed
- npm package manager

### Installation

```bash
# Clone the repository
git clone https://github.com/HarshAvichal/todo-devops.git
cd todo-devops

# Install dependencies
npm install

# Start development server
npm run dev
```

Open http://localhost:5173 in your browser.

---

## 📜 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run lint` | Run ESLint |
| `npm run preview` | Preview production build |

---

## 🏗️ Tech Stack

| Technology | Purpose |
|------------|---------|
| React 19 | UI Framework |
| Vite | Build Tool |
| ESLint | Code Quality |
| date-fns | Date Manipulation |
| lucide-react | Icons |

---

## 🤖 CI/CD Pipelines

This project implements **two CI/CD pipelines** for comparison:

### Pipeline 1: Jenkins

Self-hosted CI/CD with Vercel deployment.

**Stages:**
1. Checkout - Pull code from GitHub
2. Install Dependencies - `npm ci`
3. Lint Code - `npm run lint`
4. Build Application - `npm run build`
5. Test Build - Verify dist/ exists
6. Archive Artifacts - Save build files
7. Deploy to Vercel - Production deployment

### Pipeline 2: GitHub Actions

Cloud-based CI/CD with artifact upload.

**Steps:**
1. Checkout - Clone repository
2. Setup Node.js - Node 20 with caching
3. Install Dependencies - `npm ci`
4. Run ESLint - Code quality check
5. Build Vite App - `npm run build`
6. Upload Artifact - Save dist/ folder

### Trigger Comparison

| Pipeline | Trigger |
|----------|---------|
| Jenkins | Manual / Webhook |
| GitHub Actions | Push to `main`, PR to `main` |

---

## 📁 Project Structure

```
todo-devops/
├── src/
│   ├── components/
│   │   ├── AddTodo.jsx
│   │   ├── TodoItem.jsx
│   │   ├── TodoList.jsx
│   │   ├── FilterBar.jsx
│   │   ├── StatsPanel.jsx
│   │   ├── PomodoroTimer.jsx
│   │   ├── FocusMode.jsx
│   │   └── ProductivityAnalytics.jsx
│   ├── App.jsx
│   ├── App.css
│   ├── main.jsx
│   └── index.css
├── .github/
│   └── workflows/
│       └── github-actions-ci.yml    # GitHub Actions pipeline
├── Jenkinsfile                       # Jenkins pipeline
├── docker-compose.jenkins.yml        # Jenkins Docker setup
├── setup-jenkins.sh                  # Jenkins setup script
├── vercel.json                       # Vercel configuration
├── PIPELINE_WORKFLOW.md              # Pipeline documentation
├── package.json
└── vite.config.js
```

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [PIPELINE_WORKFLOW.md](./PIPELINE_WORKFLOW.md) | CI/CD pipeline visualization |
| [JENKINS_VS_GITHUB_ACTIONS.md](./JENKINS_VS_GITHUB_ACTIONS.md) | Tool comparison |
| [Jenkinsfile](./Jenkinsfile) | Jenkins pipeline code |
| [github-actions-ci.yml](./.github/workflows/github-actions-ci.yml) | GitHub Actions workflow |

---

## 🎨 Components

### Core Components
- **AddTodo** - Form to add new tasks
- **TodoItem** - Individual task display
- **TodoList** - List of all todos
- **FilterBar** - Search and filter controls
- **StatsPanel** - Quick statistics

### Productivity Components
- **PomodoroTimer** - Time management timer
- **FocusMode** - Distraction-free interface
- **ProductivityAnalytics** - Productivity insights

---

## 🌐 Deployment

**Platform:** Vercel

The app is deployed automatically via Jenkins pipeline when code is pushed to `main` branch.

---

## 🎓 Project Purpose

This project was built for **DevSecOps learning**, comparing:

| Aspect | Jenkins | GitHub Actions |
|--------|---------|----------------|
| Hosting | Self-hosted | Cloud |
| Setup | Complex | Simple |
| Flexibility | High | Medium |
| Maintenance | Required | None |

---

## 👥 Team

- **HarshAvichal** - Jenkins Pipeline & Documentation
- **satyajeetmane01** - GitHub Actions Pipeline

---

## 📄 License

MIT License

---

*Built with React + Vite | CI/CD with Jenkins & GitHub Actions*
