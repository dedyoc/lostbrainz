Overview

- Branches:
    
    - main: Stable, production-ready code. Only updated via merges from develop or hotfixes.
        
    - develop: Integration branch for features. Reflects the latest development state.
        
    - feature/*: Short-lived branches for new features or tasks.
        
    - hotfix/*: Urgent fixes for main.
        
- Process: Team members work on isolated feature branches, integrate into develop, and periodically release to main.
    
- Tools: Pull Requests (PRs) or Merge Requests (MRs) for review and integration.
    

---

Detailed Workflow

1. Setup

- Initialize the Repo:
    
    bash
    
    ```bash
    git init my_project
    cd my_project
    git checkout -b main
    git commit --allow-empty -m "Initial commit"
    git checkout -b develop
    git push origin main
    git push origin develop
    ```
    
- Protect Branches: On the remote (e.g., GitHub), set main and develop as protected branches, requiring PRs and approvals.

2. Starting Work (Feature Development)

- Create a Feature Branch:
    
    - Base it on develop, not main.
        
    
    bash
    
    ```bash
    git checkout develop
    git pull origin develop  # Ensure it’s up-to-date
    git checkout -b feature/user-login
    ```
    
- Work and Commit:
    
    - Keep commits small, logical, and descriptive.
        
    
    bash
    
    ```bash
    echo "Login form" > login.html
    git add login.html
    git commit -m "Add login form UI"
    echo "Login logic" > login.js
    git add login.js
    git commit -m "Implement login functionality"
    ```
    
- Push to Remote:
    
    bash
    
    ```bash
    git push origin feature/user-login
    ```
    

3. Collaboration and Review

- Open a Pull Request:
    
    - Create a PR from feature/user-login to develop on the remote platform.
        
    - Include a description (e.g., “Adds user login with form and validation”).
        
- Code Review:
    
    - Team reviews the PR. Request changes if needed.
        
    - Update the branch locally if revisions are required:
        
    
    bash
    
    ```bash
    git checkout feature/user-login
    echo "Fix validation" >> login.js
    git add login.js
    git commit -m "Address review feedback: fix validation"
    git push origin feature/user-login
    ```
    
- Merge:
    
    - Once approved, merge the PR into develop (preferably with a merge commit for history).
        
    
    bash
    
    ```bash
    # Done via UI, or locally:
    git checkout develop
    git merge --no-ff feature/user-login
    git push origin develop
    ```
    
- Delete Branch:
    
    bash
    
    ```bash
    git push origin --delete feature/user-login
    ```
    

4. Releasing to Production

- Prepare a Release:
    
    - When develop is stable, merge it into main.
        
    
    bash
    
    ```bash
    git checkout main
    git pull origin main
    git merge --no-ff develop
    git tag -a v1.0.0 -m "Release v1.0.0"
    git push origin main
    git push origin v1.0.0
    ```
    
- Sync develop:
    
    bash
    
    ```bash
    git checkout develop
    git merge main  # Ensure develop stays in sync
    git push origin develop
    ```
    

5. Hotfixes

- Fix a Production Bug:
    
    - Branch from main for urgent fixes.
        
    
    bash
    
    ```bash
    git checkout main
    git checkout -b hotfix/login-bug
    echo "Fix login crash" >> login.js
    git add login.js
    git commit -m "Fix login crash in production"
    git push origin hotfix/login-bug
    ```
    
- PR and Merge:
    
    - PR to main, merge, and tag:
        
    
    bash
    
    ```bash
    git checkout main
    git merge --no-ff hotfix/login-bug
    git tag -a v1.0.1 -m "Hotfix v1.0.1"
    git push origin main
    git push origin v1.0.1
    ```
    
    - Merge back to develop:
        
    
    bash
    
    ```bash
    git checkout develop
    git merge main
    git push origin develop
    ```
    

---

Specific Cases and Guidelines

Case 1: Multiple Developers on the Same Feature

- Approach:
    
    - One developer creates feature/user-login and pushes it.
        
    - Others branch off it: git checkout -b feature/user-login-subtask origin/feature/user-login.
        
    - Push subtask branches and open PRs to feature/user-login, then merge into develop from there.
        
- Avoid: Committing directly to feature/user-login without coordination—use sub-branches.
    

Case 2: Conflicts During Merge

- Approach:
    
    - Pull latest changes before merging:
        
    
    bash
    
    ```bash
    git checkout develop
    git pull origin develop
    git checkout feature/user-login
    git merge develop
    ```
    
    - Resolve conflicts manually, commit, and push.
        
- Avoid: Force-pushing (git push --force) unless absolutely necessary and agreed upon.
    

Case 3: Long-Running Feature

- Approach:
    
    - Periodically rebase or merge develop into the feature branch to stay updated:
        
    
    bash
    
    ```bash
    git checkout feature/big-task
    git rebase develop
    git push --force  # Only if safe and team agrees
    ```
    
- Avoid: Letting the branch drift too far from develop, causing massive conflicts later.
    

Case 4: Emergency Hotfix

- Approach: See hotfix steps above. Keep changes minimal and focused.
    
- Avoid: Mixing feature work into hotfix branches—keep them separate.
    

---

What to Avoid (Pitfalls and Solutions)

1. Committing Directly to main or develop:
    
    - Problem: Breaks stability and collaboration.
        
    - Solution: Always use feature branches. Protect main and develop on the remote.
        
2. Force Pushing Shared Branches:
    
    - Problem: Overwrites others’ work.
        
    - Solution: Use --force-with-lease if needed, and communicate with the team.
        
3. Large, Unreviewed PRs:
    
    - Problem: Hard to review, increases bugs.
        
    - Solution: Break work into smaller PRs (e.g., UI first, logic later).
        
4. Ignoring Conflicts:
    
    - Problem: Delays integration and creates messy history.
        
    - Solution: Resolve conflicts early by syncing with develop regularly.
        
5. Vague Commit Messages:
    
    - Problem: Obscures history and intent.
        
    - Solution: Use clear messages (e.g., “Fix login validation error” vs. “Update”).
        
6. Not Testing Before Merging:
    
    - Problem: Breaks develop or main.
        
    - Solution: Run tests locally and use CI/CD pipelines on PRs.
        

---

Team Practices

- Daily Sync: Pull develop at the start of the day.
    
- PR Reviews: Assign at least one reviewer; aim for quick turnaround.
    
- Naming Convention: Use prefixes like feature/, hotfix/, bugfix/ for clarity (e.g., feature/user-login).
    
- Commit Hygiene: Squash trivial commits before merging if needed (e.g., git rebase -i).
    

---

Example Commit Count

- Start: 2 commits on main, 3 on develop.
    
- Feature branch: 2 commits, merged to develop (merge commit) → develop now has 6 commits.
    
- Hotfix: 1 commit, merged to main (merge commit) → main has 4 commits.