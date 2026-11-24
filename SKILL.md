---
name: versioned-workspace-cold-start
description: Orient an agent in a multi-snapshot workspace with frozen version folders, a live Git repo, progress logs, and release/media/draft satellites. Use when resuming work, identifying the active repo and shipped version, detecting release lag, or safely inventorying Windows paths in Git Bash without overwriting snapshots.
---

# Versioned Workspace Cold Start

## Overview

Build an evidence-backed map of a workspace that contains multiple project snapshots, one live repository, and supporting release or media folders. Use this skill in Qoder, Claude Code, Codex, OpenCode, Cursor, Gemini CLI, or any coding-agent host that can load Markdown instructions. Finish with a concise orientation and a safe next step; do not begin implementation unless the user asks for it.

## Operating Rules

- Work read-only by default. Do not fetch, build, test, publish, run artifacts, or modify frozen snapshots.
- Use the host's available read-only tools. Treat shell examples as optional patterns, not mandatory commands.
- Treat folder names, timestamps, README claims, and progress-log summaries as clues, not proof.
- Separate `[observed]`, `[documented]`, and `[inferred]` claims.
- Report conflicting evidence instead of silently choosing one source.
- Never copy credentials, PINs, tokens, API keys, or other secret values into notes or memory.
- Recheck current files before relying on saved memory; memory is point-in-time context.

## 1. Inventory the Workspace Root

Derive the workspace root from the user request or current workspace. List root files and immediate children without recursively scanning dependency or build directories.

```bash
cd "C:/path/to/workspace"
ls -la
ls -lat
for d in */; do
  printf '\n=== %s ===\n' "$d"
  if [ -e "$d/.git" ]; then
    printf '.git present\n'
  fi
  ls -la "$d" | head -35
done
```

Record each child's apparent role: source repository, frozen snapshot, releases, packed products, demo media, sample configuration, documentation, draft, or unknown.

### Windows Git Bash Rule

Inside double quotes, a backslash immediately before `$` makes the dollar sign literal. A path such as `"...\$d"` therefore does not expand `d`.

Change directory once using a quoted forward-slash path, then iterate over quoted relative names:

```bash
cd "C:/path/to/workspace"
for d in */; do
  printf '\n=== %s ===\n' "$d"
  ls -la "$d" | head -35
done
```

Do not concatenate a Windows root, a backslash, and a shell variable inside one quoted path.

## 2. Identify the Live Repository

Evaluate every plausible source candidate. A `.git` entry is strong evidence, but it is not sufficient: a releases repository may also have `.git`.

For each candidate, check:

- Presence of `.git` as a directory or file.
- Source manifests and recent source/document modification times.
- README statements about active, archived, or frozen versions.
- Commit dates, tags, branch status, and remote metadata.
- Whether the folder contains product source or only packaged artifacts.

```bash
cd "C:/path/to/candidate"
git status --short --branch
git log --oneline -15
git tag --sort=-version:refname
git log -1 --format='%ci %s'
git remote -v
```

Do not fetch or contact remotes unless separately requested. If the candidate is not a Git repository, record that fact and continue with local documentation.

Classify folders only after combining evidence. If two candidates remain plausible, report the ambiguity and ask which one is authoritative.

## 3. Read Current State

Read the candidate repository's README and relevant current documents. Look for:

- Product purpose and stack.
- Version naming and version-to-ship mapping.
- Development, verification, and release instructions.
- Frozen-snapshot boundaries.
- Known constraints and unresolved work.

If a progress or resume log exists, inspect both ends and its dated headings. Determine ordering from the content; do not assume newest-first or oldest-first.

```bash
head -100 PROGRESS.md
tail -120 PROGRESS.md
```

Also inspect available release notes and version manifests, such as `latest.json`, `package.json`, `Cargo.toml`, or platform equivalents. Reconcile:

- Source version.
- Git tag and latest commit.
- Product ship label.
- Packaged or published version.

Treat test counts and statements such as "all gates green" as `[documented]` unless the checks were actually run in the current task. Call out stale cold-start sections that contradict direct evidence. If `PROGRESS.md` is absent, use Git history and current release documents and report the missing log.

## 4. Cross-Check Satellite Folders

Inspect release, packed-product, demo-media, draft, and sample-configuration folders identified during inventory. Adapt the following names to the actual workspace:

```bash
for d in releases packed-product demo-media drafts; do
  if [ -d "$d" ]; then
    printf '\n=== %s ===\n' "$d"
    ls -lat "$d" | head -35
  fi
done
```

Compare source state with:

- Newest local installer or portable artifact.
- Release README and release notes.
- Packaged ship folders.
- Version-specific verification screenshots or clips.
- Draft markers such as placeholders, legal-review flags, or explicit do-not-publish notices.

Describe release lag as a local observation unless an authorized remote check was performed. Do not execute downloaded or packed binaries during orientation.

## 5. Persist or Hand Off Orientation

If the host provides project memory and memory updates are allowed, update its existing project note using that host's native format. Otherwise, return the orientation without writing a memory file. Do not assume a specific memory filename, frontmatter schema, or index.

Persist only non-obvious orientation, such as:

- Which repository is active and which snapshots are frozen.
- Current source-to-ship version mapping.
- Confirmed local release lag.
- Owner constraints that affect where work may happen.
- Unresolved discrepancies that future sessions must recheck.

Include the observation date and concise evidence. Follow the host's existing memory conventions when present.

Do not persist secrets, architecture or file-map dumps, unverified test counts, temporary state, or claims already obvious from the current repository. Remove or revise stale memory that conflicts with current observations.

## Orientation Output

Return a compact summary with these fields:

- **Product and workspace purpose**
- **Live repository and supporting evidence**
- **Frozen snapshots and boundaries**
- **Current source, ship, and local release versions**
- **Satellite folder roles and release lag**
- **Discrepancies, missing files, and unknowns**
- **Safe next action**

Label material claims as `[observed]`, `[documented]`, or `[inferred]`. Stop after orientation unless implementation or another write action was explicitly requested.

## Resources

No bundled scripts, references, or assets are required.
