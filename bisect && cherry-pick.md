

# Git Bisect & Git Cherry-pick

---

## Table of Contents

- Git Bisect
  - Introduction
  - How It Works
  - Step-by-Step Usage
  - Automating with Tests
  - When to Use Git Bisect
- Git Cherry-pick
  - Introduction
  - Basic Usage
  - Cherry-picking Multiple Commits
  - Handling Conflicts
  - When to Use Git Cherry-pick
- Merge vs Cherry-pick


---

# Git Bisect

## Introduction

git bisect is a powerful debugging tool that helps identify the commit that introduced a bug.

If you know:
- The current version is broken
- A previous version was working

You can use git bisect to perform a binary search between those two points and quickly locate the problematic commit.

---

## How It Works

git bisect uses a binary search algorithm to reduce the number of commits that need to be tested.

Instead of checking commits one by one:

- Without bisect → You may need to test every commit
- With bisect → You only test log2(n) commits

Example:
- 100 commits → Only about 7 tests needed

---

## Step-by-Step Usage

### 1. Start the bisect process

```
git bisect start
```
---

### 2. Mark the current commit as bad

```
git bisect bad
```

Or specify a commit explicitly:

```
git bisect bad <commit-hash>
```
---

### 3. Mark a known good commit

```
git bisect good <commit-hash>
```
Git will automatically check out a commit halfway between the good and bad commits.

---

### 4. Test the project

If the current commit works:

```
git bisect good
```
If it is broken:
```
git bisect bad
```
Git continues narrowing the range until it identifies the first bad commit.

---

### 5. Finish the process

```
git bisect reset
```
This returns you to your original branch.

---

## Automating with Tests

If your project includes automated tests, you can automate the bisect process:

```
git bisect run npm test
```
or

```
git bisect run pytest
```
Git will execute the command and use the exit status to determine whether the commit is good or bad.

---

## When to Use Git Bisect

- A bug was introduced at an unknown point in history
- The repository has many commits
- Manual searching would be inefficient
- You want a systematic debugging approach

---

# Git Cherry-pick

## Introduction

git cherry-pick allows you to apply a specific commit from one branch onto another branch.

Unlike merge, it does not integrate the entire branch history. It applies only selected commits.

---

## Basic Usage

Apply a single commit:

```
git cherry-pick <commit-hash>
```
This creates a new commit on the current branch containing the same changes.

> Note: The new commit will have a different hash.

---

## Cherry-picking Multiple Commits

Apply multiple specific commits:

```
git cherry-pick <hash1> <hash2>
```
Apply a range of commits:

```
git cherry-pick A^..B
```
---

## Handling Conflicts

If a conflict occurs:

1. Resolve the conflict manually
2. Stage the resolved files:

```
git add .
```
3. Continue the cherry-pick process:

```
git cherry-pick --continue
```
To cancel the operation:

```
git cherry-pick --abort
```
---

## When to Use Git Cherry-pick

- Applying a hotfix to production
- Backporting a bug fix to a release branch
- Copying a specific feature without merging the entire branch
- Selectively moving commits between branches

---

# Merge vs Cherry-pick

| Merge | Cherry-pick |
|-------|------------|
| Combines entire branch history | Applies selected commits only |
| Preserves original commit structure | Creates new commit(s) |
| Best for full integration | Best for selective changes |

