# 💰 ExpenseTrack

A simple, study-friendly **5-page expense tracker** built with pure **HTML5 & CSS3** only.  
No backend. No frameworks. No JavaScript libraries.

---

## 📁 Project Structure

```
expense-tracker/
│
├── index.html          ← Dashboard (home page)
├── add-expense.html    ← Add new expense form
├── expenses.html       ← All expenses list
├── budget.html         ← Budget planner
├── about.html          ← About the project
├── style.css           ← Single shared stylesheet
│
├── Jenkinsfile         ← Jenkins CI/CD pipeline
└── README.md           ← This file
```

---

## 🚀 How to Run Locally

No server needed! Just open in a browser:

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/expense-tracker.git

# Open in browser
open index.html
# or just double-click index.html
```

---

## 📤 Push to GitHub

```bash
cd expense-tracker

git init
git add .
git commit -m "Initial commit: expense tracker HTML CSS"

git remote add origin https://github.com/YOUR_USERNAME/expense-tracker.git
git branch -M main
git push -u origin main
```

---

## 🔧 Jenkins CI/CD Setup

### Step 1: Install Jenkins
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-17-jdk jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### Step 2: Install Required Jenkins Plugins
Go to: `Manage Jenkins → Plugins → Available`
- ✅ Git Plugin
- ✅ GitHub Plugin
- ✅ Pipeline Plugin

### Step 3: Create a Pipeline Job
1. Click **New Item** → **Pipeline**
2. Name it `expense-tracker`
3. Under **Pipeline**, choose: `Pipeline script from SCM`
4. SCM: `Git`
5. Repository URL: `https://github.com/YOUR_USERNAME/expense-tracker.git`
6. Branch: `*/main`
7. Script Path: `Jenkinsfile`
8. Click **Save**

### Step 4: Enable GitHub Webhook (Auto-build on Push)
1. In Jenkins job: `Configure → Build Triggers`  
   ✅ Check **"GitHub hook trigger for GITScm polling"**

2. In GitHub repo: `Settings → Webhooks → Add webhook`  
   - Payload URL: `http://YOUR_JENKINS_IP:8080/github-webhook/`
   - Content type: `application/json`
   - Events: `Just the push event`
   - Click **Add webhook**

### Step 5: Run the Pipeline
- Click **Build Now** to trigger manually, OR
- Push any commit to GitHub — Jenkins will auto-build!

---

## 🏗 Pipeline Stages

| Stage | What it does |
|-------|-------------|
| **Checkout** | Pulls latest code from GitHub |
| **Validate** | Checks all 5 HTML files + CSS exist |
| **Build** | Zips the project as a build artifact |
| **Archive** | Saves the zip in Jenkins artifacts |
| **Deploy** | Copies files to `/var/www/html/expense-tracker/` |

---

## 📚 CSS Concepts Used (Exam Notes)

| Concept | Usage |
|---------|-------|
| CSS Variables | `--color: value` in `:root`, used with `var()` |
| Flexbox | Navbar, summary strip, form actions |
| CSS Grid | Dashboard cards, budget cards, feature grid |
| Media Queries | `@media (max-width: 768px)` for mobile |
| Box Model | `margin`, `padding`, `border`, `box-sizing` |
| Pseudo-classes | `:hover`, `:active`, `tr:last-child` |
| Transitions | `transition: all 0.2s ease` for smooth effects |
| Progress bars | CSS width + background color only |

---

## 🌐 Pages Overview

| Page | File | Description |
|------|------|-------------|
| Dashboard | `index.html` | Summary cards + bar chart + recent transactions |
| Add Expense | `add-expense.html` | HTML form to record income/expense |
| Expenses | `expenses.html` | Full table with filter bar + pagination |
| Budget | `budget.html` | Category budgets with progress bars |
| About | `about.html` | Project info, tech stack, exam notes |

---

© 2026 ExpenseTrack — Built for learning 🎓
