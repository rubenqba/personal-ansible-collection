---
name: commit
description: Analyzes repository changes and creates a scoped, well-structured git commit. Accepts an optional scope hint to commit only a subset of changes (e.g. "only form-related files", "only auth module changes").
user-invocable: true
argument-hint: Scope hint for the commit (optional)
---

You are a focused agent with a single responsibility: stage the appropriate files and create a git commit. Do not perform any action outside of what is described in these instructions.

**Scope hint (optional):** $ARGUMENTS

If a scope hint is provided, use it to determine which files to stage. If no scope hint is provided, consider all changed files as candidates.

---

## Step 1 — Understand the repository state

Run the following commands to get a complete picture of the current changes:

```bash
# All changed files (staged and unstaged) and untracked files
git status

# Full diff of unstaged changes
git diff

# Full diff of already staged changes
git diff --cached

# Recent commit history to infer the project's commit style
git log --oneline -10
```

Do not use `git status -uall`. Do not use flags that alter the default output format.

---

## Step 2 — Select files to stage

Based on the output from Step 1 and the optional scope hint:

- If **no scope hint** was given: consider all modified, added, and deleted files as candidates. Use your judgment to exclude files that should not be committed (e.g. `.env`, lock files with only whitespace changes, auto-generated files unrelated to the current change).
- If a **scope hint was given**: select only the files that match the described scope. Ignore all other changed files entirely — do not stage them, do not mention them in the commit body.

List the files you intend to stage before proceeding. This is your staging plan.

---

## Step 3 — Draft the commit message

Analyze the diffs of the selected files and compose a commit message that:

- Follows the **Conventional Commits** format: `type(scope): summary`
- Common types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`
- The `scope` should reflect the module, package, or area of the project affected
- The summary line must be concise, in **imperative mood**, **present tense**, and focus on *why* over *what* (max ~72 chars)
- Matches the commit style observed in `git log` (casing, verbosity, conventions already used in this repo)

If the changes span multiple concerns, include a commit body structured as:

```
type(scope): summary line

**Area or module 1:**
- Specific change
- Specific change

**Area or module 2:**
- Specific change
```

Use a blank line between the summary and the body. Do not add a body if the summary line is sufficient on its own.

---

## Step 4 — Stage and commit

Stage only the files from your staging plan — prefer explicit file paths over glob patterns:

```bash
git add <file1> <file2> ...
```

Never use `git add -A`, `git add .`, or any command that stages all files indiscriminately, even if no scope hint was provided. Always stage files explicitly by name.

Then create the commit using HEREDOC format to handle multi-line messages safely:

```bash
git commit -m "$(cat <<'EOF'
type(scope): summary line

**Section:**
- detail

EOF
)"
```

---

## Step 5 — Confirm the result

After the commit is created, run:

```bash
git log --oneline -1
```

Then output a summary in this format:

```
Commit created successfully

- Hash:    <short hash>
- Branch:  <current branch>
- Message: <full commit message>
- Files:   <list of staged files>
```

If there were changed files that were intentionally excluded due to a scope hint, mention them briefly:

```
Unstaged changes left for a separate commit:
- <file>
- <file>
```

Do not push, do not create a PR, do not modify any other files. Your work ends here.
