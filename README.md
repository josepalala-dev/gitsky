# Gitsky v0.1

### What

Explain and help with the conventional wisdom of git  using -no-ff

# Graphify-Safe Git Workflow

## Core Idea

When using **Graphify with a Git-based codebase, treat Git history as part of the knowledge context**.

Graphify builds a knowledge graph from the source code and its relationships. Git operations that rewrite or unexpectedly reshape history can leave Graphify's cached knowledge out of sync with the actual repository.

To avoid this, this repository follows a **no-rebase, no-fast-forward workflow**.

## Git Rules

### 1. Never Rebase

Do not use:

```bash
git rebase
```

Rebasing rewrites commit history. Even when the resulting source code is correct, the rewritten commit structure can cause Graphify's existing knowledge or cached nodes to become stale or inconsistent.

**Prefer merge commits instead.**

---

### 2. Never Fast-Forward Feature Branches

When updating or integrating a feature branch, do not use fast-forward-only behavior:

```bash
git merge --ff-only
```

A fast-forward can move the branch pointer without creating an explicit merge point.

For Graphify workflows, we prefer an explicit and traceable history:

```text
main
  │
  ├── A
  │
  ├── B
  │      \
  │       C ── D   feature
  │              \
  └─────────────── M
```

The merge commit `M` clearly records where the feature branch was integrated.

---

### 3. Do Not Merge Main/Develop by Rebasing

When bringing `main` or `develop` into a feature branch, do not rebase the feature branch onto the latest base:

```bash
# Avoid
git rebase develop
```

Instead, merge the base branch:

```bash
git merge develop
```

This preserves the existing history instead of rewriting the feature branch.

---

### 4. Merge Features Into Main/Develop

When integrating a feature into `main` or `develop`, use a normal merge commit rather than a rebase-based integration.

Prefer:

```bash
git merge --no-ff feature/my-feature
```

This creates an explicit integration point:

```text
A──B──C──────────M   develop
     \          /
      D──E──────    feature
```

instead of rewriting the feature history.

## Why?

The goal is simple:

> **Don't make Git history move underneath Graphify.**

Think of Graphify as a map of the repository.

If Git rewrites the roads after Graphify has mapped them, the map can contain references to roads that no longer exist.

A merge-based workflow keeps the existing commits intact and adds new history rather than rewriting old history.

## The Rule of Thumb

```text
REBASE        ❌
FF-ONLY       ❌
REBASE MERGE  ❌

NORMAL MERGE  ✅
--NO-FF MERGE ✅
PRESERVE HISTORY ✅
```

The objective is not to make Git history perfect.

The objective is to make Git history **stable, explicit, and predictable for Graphify**.

## Core Principle

**Preserve history. Don't rewrite it.**

When Graphify is part of the development workflow:

> **Git history should be treated as an input to the knowledge system, not merely as a developer convenience.**

If history changes, Graphify should be considered potentially stale and should be revalidated before its knowledge is trusted.

---

Powered by gitskylabs
