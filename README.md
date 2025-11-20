# 🧭 Git Workflow Guide (for 4–5 Person Team)

## Overview

Our team uses a **two-branch workflow**:

- **`main`** – Always stable and production-ready.
- **`dev`** – Shared development branch for integration and testing.

Each developer works on **feature branches** created from `dev`.  
All merges into `dev` and `main` are done via **Pull Requests (PRs)**.

---

## 🔧 Branch Structure

```
main
 │
 ├─── dev  ← integration/staging branch
 │     ├── feature/login-page
 │     ├── feature/payment
 │     ├── fix/cart-bug
 │     └── refactor/auth-service
 │
 └───> merged to main when stable (release)
```

---

## 🪜 Step-by-Step Workflow

### 1️⃣ Update your local `dev` branch

Always start from the latest version of `dev`:

```bash
git checkout dev
git pull origin dev
```

---

### 2️⃣ Create a new feature branch

Create a branch for your specific task:

```bash
git checkout -b feature/your-feature-name
```

**Branch naming convention:**
| Type | Prefix | Example |
|------|---------|----------|
| New feature | `feature/` | `feature/user-auth` |
| Bug fix | `fix/` | `fix/login-error` |
| Refactor | `refactor/` | `refactor/order-service` |
| Hotfix (urgent bug on main) | `hotfix/` | `hotfix/checkout-bug` |

---

### 3️⃣ Commit your changes

Write clear and meaningful commit messages following the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
feat: add login validation
fix: correct null pointer issue in checkout
refactor: simplify booking service logic
docs: update README for setup guide
```

---

### 4️⃣ Sync your branch with `dev`

Before pushing or opening a PR, make sure your branch is up to date:

```bash
git checkout dev
git pull origin dev
git checkout feature/your-feature-name
git rebase dev
# OR (if you prefer)
# git merge dev
```

> ✅ Rebase keeps the commit history clean and linear.  
> ⚠️ Use merge if you’re new to rebase.

If conflicts occur:

```bash
# After resolving conflicts
git add .
git rebase --continue
# or
git merge --continue
```

---

### 5️⃣ Push and create a Pull Request

Push your branch:

```bash
git push origin feature/your-feature-name
```

Then go to **GitHub → Open a Pull Request → Base: `dev` ← Compare: `feature/your-feature-name`**.

PR should:

- Be reviewed by **at least one team member**
- Include a **clear description**
- Pass **tests / lint checks** (if CI is enabled)

---

### 6️⃣ Merge to `dev`

Once approved:

- The **reviewer or maintainer** merges it into `dev`
- CI/CD (if any) deploys to staging or test environment

---

### 7️⃣ Merge `dev` → `main` (for release)

When `dev` is stable and fully tested:

```bash
git checkout main
git pull origin main
git merge dev
git push origin main
```

> Optionally tag the release:

```bash
git tag -a v1.0.0 -m "First stable release"
git push origin v1.0.0
```

---

## 🧩 Roles & Responsibilities

| Role                       | Responsibility                                             |
| -------------------------- | ---------------------------------------------------------- |
| **Team Lead / Maintainer** | Reviews PRs, manages merge to `main`, handles conflicts    |
| **Developers**             | Create branches, commit changes, sync with `dev`, open PRs |
| **Tester / QA**            | Validates new features on `dev`, ensures no regression     |
| **Designer / Support**     | Provides assets, documentation, and feedback               |

---

## 🧹 Best Practices

- ✅ Pull from `dev` before creating any new branch
- ✅ Keep commits **small and focused**
- ✅ Test locally before pushing
- ✅ Don’t commit environment files (`.env`, `node_modules`, etc.)
- ✅ Use `.gitignore` properly
- ✅ Avoid pushing directly to `dev` or `main`
- ✅ Review others’ PRs regularly

---

## ⚙️ Optional: Branch Protection Rules (recommended)

- **`main`**: only merge via PR, must pass all checks, at least 1 review
- **`dev`**: PR required, review optional
- Disallow direct push to `main`

---

## 🚀 Example Daily Workflow

```bash
# Morning
git checkout dev
git pull origin dev

# Create your feature branch
git checkout -b feature/payment-ui

# Do your work, commit
git add .
git commit -m "feat: add payment form UI"

# Sync with latest dev before PR
git checkout dev
git pull origin dev
git checkout feature/payment-ui
git rebase dev

# Push and open PR
git push origin feature/payment-ui
# -> Create PR to merge into dev
```

---

**End of File**
