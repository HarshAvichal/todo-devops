# CI/CD Pipeline Workflow

This document visualizes the CI/CD pipelines implemented for the Todo App.

---

## Overview

We have implemented **two CI/CD pipelines** for comparison:

| Pipeline | Tool | Trigger | Deployment |
|----------|------|---------|------------|
| Pipeline 1 | Jenkins | Manual/Webhook | Vercel Production |
| Pipeline 2 | GitHub Actions | Push to `main` | Artifact Upload |

---

## Jenkins Pipeline (7 Stages)

```
┌─────────────────────────────────────────────────────────────┐
│                     DEVELOPER                                │
│  1. Write code → 2. Commit → 3. Push to GitHub               │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                   JENKINS SERVER                             │
│                                                              │
│  Pipeline triggered → Reads Jenkinsfile → Executes stages    │
└─────────────────────────┬───────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ STAGE 1      │  │ STAGE 2      │  │ STAGE 3      │
│ Checkout     │→ │ Install Deps │→ │ Lint Code    │
│              │  │              │  │              │
│ checkout scm │  │ npm ci       │  │ npm run lint │
└──────────────┘  └──────────────┘  └──────────────┘
                                           │
        ┌──────────────────────────────────┘
        ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ STAGE 4      │  │ STAGE 5      │  │ STAGE 6      │
│ Build App    │→ │ Test Build   │→ │ Archive      │
│              │  │              │  │              │
│ npm run build│  │ Verify dist/ │  │ Save artifacts│
└──────────────┘  └──────────────┘  └──────────────┘
                                           │
                                           ▼
                                   ┌──────────────┐
                                   │ STAGE 7      │
                                   │ Deploy       │
                                   │              │
                                   │ Vercel Prod  │
                                   └──────────────┘
                                           │
                          ┌────────────────┴────────────────┐
                          ▼                                 ▼
                   ┌─────────────┐                   ┌─────────────┐
                   │  ✅ SUCCESS │                   │  ❌ FAILURE │
                   └─────────────┘                   └─────────────┘
```

### Jenkins Stages Detail

| Stage | Command | Purpose |
|-------|---------|---------|
| 1. Checkout | `checkout scm` | Pull latest code from GitHub |
| 2. Install Dependencies | `npm ci` | Install Node.js packages |
| 3. Lint Code | `npm run lint` | Check code quality with ESLint |
| 4. Build Application | `npm run build` | Create production build (dist/) |
| 5. Test Build | `ls dist/` | Verify build directory exists |
| 6. Archive Artifacts | `archiveArtifacts` | Save build for future reference |
| 7. Deploy to Vercel | `npx vercel --prod` | Deploy to Vercel Production |

---

## GitHub Actions Pipeline (6 Steps)

```
┌─────────────────────────────────────────────────────────────┐
│                     DEVELOPER                                │
│  1. Write code → 2. Commit → 3. Push to main                 │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                   GITHUB ACTIONS                             │
│                                                              │
│  Trigger: Push to main OR Pull Request to main               │
│  Runner: ubuntu-latest                                       │
└─────────────────────────┬───────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ STEP 1       │  │ STEP 2       │  │ STEP 3       │
│ Checkout     │→ │ Setup Node   │→ │ Install Deps │
│              │  │              │  │              │
│ checkout@v4  │  │ Node.js 20   │  │ npm ci       │
└──────────────┘  └──────────────┘  └──────────────┘
                                           │
        ┌──────────────────────────────────┘
        ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ STEP 4       │  │ STEP 5       │  │ STEP 6       │
│ ESLint       │→ │ Build        │→ │ Upload       │
│              │  │              │  │              │
│ npm run lint │  │ npm run build│  │ Artifact     │
└──────────────┘  └──────────────┘  └──────────────┘
                                           │
                                           ▼
                                   ┌──────────────┐
                                   │  ✅ COMPLETE │
                                   │  Artifact    │
                                   │  Available   │
                                   └──────────────┘
```

### GitHub Actions Steps Detail

| Step | Action/Command | Purpose |
|------|----------------|---------|
| 1. Checkout | `actions/checkout@v4` | Clone repository |
| 2. Setup Node.js | `actions/setup-node@v4` | Install Node.js 20 with npm cache |
| 3. Install Dependencies | `npm ci \|\| npm install` | Install packages |
| 4. Run ESLint | `npm run lint` | Code quality check (non-blocking) |
| 5. Build Vite App | `npm run build` | Create production build |
| 6. Upload Artifact | `actions/upload-artifact@v4` | Save dist/ folder |

---

## Pipeline Comparison

```
┌────────────────────┬─────────────────────┬─────────────────────┐
│      Aspect        │      JENKINS        │   GITHUB ACTIONS    │
├────────────────────┼─────────────────────┼─────────────────────┤
│ Hosting            │ Self-hosted         │ Cloud (GitHub)      │
│ Configuration      │ Jenkinsfile         │ YAML workflow       │
│ Stages/Steps       │ 7 stages            │ 6 steps             │
│ Build Time         │ ~45-60 seconds      │ ~26 seconds         │
│ Deployment         │ Vercel Production   │ Artifact only       │
│ Lint Behavior      │ Fails on error      │ Continue on error   │
│ Artifacts          │ Jenkins storage     │ GitHub storage      │
└────────────────────┴─────────────────────┴─────────────────────┘
```

---

## Workflow Triggers

### Jenkins
- Manual trigger from Jenkins UI
- Webhook from GitHub (if configured)
- Poll SCM (scheduled checks)

### GitHub Actions
- Automatic on push to `main` branch
- Automatic on pull request to `main` branch

---

## Build Outputs

Both pipelines produce the same output:

```
dist/
├── index.html           # Entry point
├── assets/
│   ├── index-[hash].js  # Bundled JavaScript
│   └── index-[hash].css # Bundled CSS
└── vite.svg             # Static assets
```

---

## Success/Failure Handling

### Jenkins
```
post {
    success → "✅ Pipeline completed successfully!"
    failure → "❌ Pipeline failed!"
    always  → "🧹 Pipeline execution completed"
}
```

### GitHub Actions
```
✅ Green checkmark → All steps passed
❌ Red X → Build or critical step failed
⚠️ Yellow → Lint warnings (continues due to continue-on-error)
```

---

*Project: Todo App with Pomodoro Timer*
*Tools: Jenkins, GitHub Actions, Vite, React, Vercel*
