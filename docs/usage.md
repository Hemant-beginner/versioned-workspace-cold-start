# Usage Guide

## When to Use

Use `versioned-workspace-cold-start` when a workspace contains more than one plausible project directory and you need to determine:

- Which directory is the live source repository.
- Which directories are frozen snapshots or historical versions.
- Which version is current in source, Git tags, product labels, and local packages.
- Whether releases, demo media, or drafts lag behind source.
- Which documentation is stale or contradictory.
- What the safest next action is.

## Example Triggers

```text
Check the folder and explain what is going on.
```

```text
Which version is active, and are the local releases current?
```

```text
Orient me in this multi-version workspace without changing anything.
```

```text
Resume this project, but verify the live repository and latest ship first.
```

## Inputs

The skill can derive most inputs from the current workspace. Provide a workspace root when the target is not the current directory.

Useful optional context includes:

- The product or project name.
- Known frozen folder names.
- Known release or package locations.
- Whether memory updates are allowed.
- Whether remote access is authorized.

## Workflow

### 1. Inventory the Root

List root files and immediate child directories. Avoid recursive scans of dependencies, build output, caches, and vendor folders.

Classify each child by apparent role:

- Live source candidate.
- Frozen snapshot.
- Releases or packages.
- Demo media.
- Sample configuration.
- Documentation or draft.
- Unknown.

### 2. Identify the Live Repository

Check all plausible source candidates. Combine `.git` presence, source manifests, modification times, README statements, commit dates, tags, and branch status.

Do not assume that every Git repository is the live product source. A releases repository may also have `.git`.

### 3. Read Current State

Read the README, progress or resume log, release notes, and version manifests. Inspect both ends of long logs and determine ordering from dated headings.

Reconcile:

- Source version.
- Latest commit and tag.
- Product ship label.
- Packaged version.
- Local published version.

Treat test counts and green-gate statements as documented claims unless they were run during the current task.

### 4. Cross-Check Satellites

Inspect local release folders, packed products, demo media, drafts, and sample configurations. Compare their newest version labels and timestamps with source.

Describe release lag as a local observation unless an authorized remote check was performed.

### 5. Persist Minimal Orientation

When memory updates are allowed, update the existing project memory instead of creating a duplicate. Record only non-obvious orientation and unresolved discrepancies.

Never persist credentials, secret values, local user paths, architecture dumps, unverified test counts, or temporary state.

## Output Format

Use this compact structure:

```text
Product and workspace purpose
Live repository and supporting evidence
Frozen snapshots and boundaries
Current source, ship, and local release versions
Satellite folder roles and release lag
Discrepancies, missing files, and unknowns
Safe next action
```

Label material claims as:

- `[observed]` - Directly seen in files, Git metadata, or command output.
- `[documented]` - Stated by README, progress, release notes, or other documents.
- `[inferred]` - Derived from combined evidence but not directly confirmed.

## Windows Git Bash Notes

Inside double quotes, a backslash before `$` makes the dollar sign literal. Avoid paths such as `"...\$d"` when `d` is meant to expand.

Prefer this pattern:

```bash
cd "C:/path/to/workspace"
for d in */; do
  printf '\n=== %s ===\n' "$d"
  ls -la "$d" | head -35
done
```

Change directory once, use forward slashes for the Git Bash path, and iterate over quoted relative names.

## Non-Goals

The skill does not:

- Modify source code or frozen snapshots.
- Fetch or contact remotes without separate authorization.
- Run builds, tests, installers, or packaged binaries.
- Publish releases or upload documentation.
- Begin implementation after orientation unless explicitly requested.
