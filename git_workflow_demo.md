# Git Workflow Demo for CMH Team

A practical guide to using Git for day-to-day collaboration on R projects.

---

## 1. One-Time Setup

```bash
# Set your identity (required before first commit)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Clone the shared repo to your local machine
git clone https://github.com/your-org/cmh_demo.git
cd cmh_demo
```

---

## 2. The Daily Workflow

```
         remote (GitHub)
              |
    git pull  |  git push
              |
         local repo
              |
    git add   |  git commit
              |
         working files
```

### Step-by-step

```bash
# 1. Always start by pulling the latest changes from the team
git pull origin main

# 2. Make your changes (edit scripts, add data, etc.)
#    e.g., open analysis.R and update it

# 3. Check what changed
git status          # which files are modified
git diff            # what exactly changed

# 4. Stage the files you want to commit
git add analysis.R
git add data/cleaned_data.csv   # add specific files
# OR stage everything at once:
git add .

# 5. Commit with a clear, descriptive message
git commit -m "Add age-adjusted mortality rate calculation"

# 6. Push your commit to GitHub
git push origin main
```

---

## 3. Branching — Safe Collaboration

Branches let teammates work in parallel without breaking each other's work.

```
main        ──────●──────────────────────●──── (stable, shared)
                   \                    /
feature/age-adj     ●──●──●────────────   (your branch)
```

```bash
# Create a new branch for your feature/task
git checkout -b feature/age-adjusted-rates

# Work, commit as usual
git add analysis.R
git commit -m "Draft age-adjusted rate function"

# Push your branch to GitHub
git push origin feature/age-adjusted-rates

# When done, open a Pull Request on GitHub for teammates to review.
# After approval, merge into main.

# Switch back to main and pull the merged result
git checkout main
git pull origin main

# Delete the branch locally once merged
git branch -d feature/age-adjusted-rates
```

---

## 4. Resolving Merge Conflicts

Conflicts happen when two people edit the same lines. Git marks them like this:

```r
<<<<<<< HEAD
rate <- deaths / population * 100000   # your version
=======
rate <- deaths / pop_size * 100000     # teammate's version
>>>>>>> feature/fix-denominator
```

**Steps to resolve:**

```bash
# 1. Open the conflicted file and manually pick the correct version
#    (delete the conflict markers <<<, ===, >>>)

# 2. Stage the resolved file
git add analysis.R

# 3. Complete the merge
git commit -m "Resolve conflict in rate denominator"
```

---

## 5. Useful Everyday Commands

```bash
git log --oneline          # compact commit history
git log --oneline --graph  # visualize branches

git diff HEAD~1            # compare with the previous commit
git show abc1234           # inspect a specific commit

git stash                  # temporarily shelve uncommitted changes
git stash pop              # bring them back

git reset --soft HEAD~1    # undo last commit, keep changes staged
```

---

## 6. .gitignore — What NOT to Track

The `.gitignore` file tells Git to skip certain files. For R projects:

```
# R artifacts
.Rhistory
.RData
.Rproj.user/

# Large data files (share via secure drive instead)
data/raw/
*.csv
*.xlsx

# Credentials — NEVER commit these
*.env
secrets.R
```

---

## 7. Recommended Branch Naming

| Pattern | Example | Use for |
|---|---|---|
| `feature/` | `feature/icd10-mapping` | New analysis or feature |
| `fix/` | `fix/mortality-denominator` | Bug or error correction |
| `data/` | `data/update-2024-counts` | Data updates |
| `docs/` | `docs/readme-update` | Documentation only |

---

## Quick Reference Card

| Goal | Command |
|---|---|
| Start fresh from remote | `git pull origin main` |
| See what changed | `git status` / `git diff` |
| Stage files | `git add <file>` |
| Save a snapshot | `git commit -m "message"` |
| Share with team | `git push origin <branch>` |
| New branch | `git checkout -b <name>` |
| Switch branches | `git checkout <name>` |
| View history | `git log --oneline` |
