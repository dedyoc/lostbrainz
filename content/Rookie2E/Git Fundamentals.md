# Git Deep Dive: Understanding the Internals

## 1. Git Basics: The Foundation

Git is a distributed version control system that tracks changes via a directed acyclic graph (DAG) of commits. A repository is a `.git` directory storing all project data.

**Key Components:**

* **Objects:** Git’s core data (blobs, trees, commits, tags).
* **Refs:** Pointers to commits (e.g., branches, HEAD).
* **Index:** The staging area, a snapshot of what’s next to commit.

**Behind the Scenes:**

* **Blob:** Stores file content (one per unique version).
* **Tree:** Maps directory structure, linking blobs and subtrees.
* **Commit:** Ties a tree to metadata (author, message, parent commit).

**Example:**

```bash
git init deep_dive
cd deep_dive
echo "Hello" > file.txt
git add file.txt
git commit -m "Initial commit"  # Commit 1
```

**Under the Hood:**

* Git creates a blob for `file.txt`’s content ("Hello").
* A tree links the blob to the filename `file.txt`.
* A commit object references this tree, with metadata (message, timestamp, no parent since it’s the first commit).
* `.git/objects/` stores these as hashed files (e.g., `e69de29` for the blob).
* `HEAD` points to `main`, which points to this commit.

**Commit Count: 1**

## 2. Tracking Changes: The Index and Commits

The staging area (index) is a temporary snapshot. Committing moves it to the repo’s history.

**Example:**

```bash
echo "World" >> file.txt
git add file.txt
git commit -m "Add World"  # Commit 2
```

**Behind the Scenes:**

* New blob for updated `file.txt` content ("Hello\nWorld").
* New tree reflecting the updated file.
* New commit with parent pointing to Commit 1’s hash.
* Git compresses objects using zlib and stores them in `.git/objects/`.

**Commit Count: 2**

## 3. Branching: Pointers in Action

A branch is a movable pointer to a commit. Creating a branch doesn’t alter history—it’s just a ref.

**Example:**

```bash
git checkout -b feature
echo "Feature" >> file.txt
git add file.txt
git commit -m "Feature work"  # Commit 3
```

**Behind the Scenes:**

* `.git/refs/heads/feature` created, pointing to Commit 2 initially.
* After committing, `feature` updates to Commit 3’s hash.
* `HEAD` points to `feature`, while `main` stays at Commit 2.
* DAG: Commit 1 <- Commit 2 <- Commit 3.

**Commit Count: 3**

## 4. Merging: Combining Histories

Merging integrates changes from one branch into another. The commit count depends on the merge strategy.

**Scenario 1: Fast-Forward Merge**

If `main` hasn’t diverged, Git moves the pointer forward.

```bash
git checkout main
git merge feature
```

**Behind the Scenes:**

* `main` moves from Commit 2 to Commit 3.
* No new objects created—Git updates `.git/refs/heads/main`.
* DAG unchanged: Commit 1 <- Commit 2 <- Commit 3.

**Commit Count: 3 (no new commit).**

**Scenario 2: Three-Way Merge with Merge Commit**

If `main` diverges, Git creates a merge commit.

```bash
git checkout main
echo "Main update" >> file.txt
git add file.txt
git commit -m "Main work"  # Commit 4

git merge feature  # Commit 5 (merge commit)
```

**Behind the Scenes:**

* Commit 4 creates a new blob, tree, and commit object (parent: Commit 2).
* The merge operation identifies Commit 2 as the common ancestor and performs a three-way diff:
    * Base: Commit 2
    * Ours: Commit 4 (`main`)
    * Theirs: Commit 3 (`feature`)
* Git combines the changes ("Feature" from `feature` and "Main update" from `main`).
* A new tree object is created to reflect the merged content.
* A new merge commit (Commit 5) is created with two parents: Commit 4 and Commit 3.
* `.git/refs/heads/main` now points to Commit 5.
* DAG: Commit 1 <- Commit 2 <- Commit 3 <- Commit 5, Commit 2 <- Commit 4 <- Commit 5.


**Commit Count: 5**


## 5. Remote Repositories: Syncing the DAG

Remotes store a copy of your repo. Pushing/pulling syncs refs and objects.

**Example:**

```bash
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

**Behind the Scenes:**

* Git sends any missing objects (blobs, trees, commits) to the remote repository.
* Remote refs are updated (e.g., `refs/heads/main`).
* No new commits are created locally—the count remains at 5.


## 6. Rebasing: Rewriting History

Rebasing replays commits on a new base, avoiding merge commits.

**Example:**

```bash
git checkout feature
git rebase main
git checkout main
git merge feature  # Fast-forward
```

**Behind the Scenes:**

* Commit 3's changes are applied onto Commit 4, creating a new commit (Commit 5').
* The old Commit 3 is abandoned unless referenced elsewhere (e.g., by another branch).
* The `feature` branch pointer is updated to point to Commit 5'.
* A fast-forward merge moves the `main` branch pointer to Commit 5'.
* DAG: Commit 1 <- Commit 2 <- Commit 4 <- Commit 5'.

**Commit Count: 4 (no merge commit)**

## 7. Plumbing: Peek Under the Hood

Use low-level commands to see Git’s internals.

**Example:**

```bash
# View objects
git cat-file -p <hash>  # Show blob/tree/commit content

# Check HEAD
cat .git/HEAD  # e.g., ref: refs/heads/main

# List all objects
git rev-list --all | xargs git cat-file -t
```

## Deep Dive Summary

| Concept        | Mechanism                          | Commit Impact |
|----------------|-----------------------------------|----------------|
| Commit         | New tree + commit object           | +1            |
| Branch         | New ref, no object                | 0             |
| Fast-Forward  | Moves ref, no new objects         | 0             |
| Merge Commit   | New commit with 2+ parents        | +1            |
| Rebase         | New commits, rewrites history      | +0 (replaces) |

## Commit Count Recap

Starting at 2 commits on `main`, commit on `feature` (1), merge:

* Fast-forward: 3 commits.
* Merge commit (if `main` diverged): 5 commits.

## Final Insight

Git’s power lies in its object model and DAG. Every commit is immutable, stored as a hash-linked object, making history tamper-proof yet flexible. Merging and rebasing manipulate this graph differently, impacting commit counts based on divergence and strategy.