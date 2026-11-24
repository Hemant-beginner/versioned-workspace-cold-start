# Versioned Workspace Cold Start

A reusable, host-agnostic skill for orienting a coding agent inside a workspace that contains multiple project snapshots, one live repository, and supporting release, media, configuration, or draft folders.

## What It Does

The skill provides a read-only, evidence-backed workflow to:

- Inventory the workspace root safely in Git Bash.
- Identify the live source repository without modifying frozen snapshots.
- Read README, progress, release, and version metadata.
- Reconcile source, ship, package, and local release versions.
- Detect release lag and stale documentation.
- Produce a concise orientation summary with observed, documented, and inferred claims separated.
- Persist only minimal, non-secret project orientation when memory updates are allowed.

## Repository Contents

- [SKILL.md](SKILL.md) - The installable, host-agnostic agent skill.
- [Usage guide](docs/usage.md) - Trigger examples, workflow details, and output format.
- [Validation report](docs/validation.md) - Structural, privacy, English-language, and runtime checks.

## Requirements

- An agent host that supports Markdown instructions or `SKILL.md`, such as Qoder, Claude Code, Codex, OpenCode, Cursor, or Gemini CLI.
- Git Bash for the Windows shell examples; equivalent commands can be used in other POSIX shells.
- Git for local repository inspection.

## Install

1. Place the `versioned-workspace-cold-start` folder in the skill directory supported by your agent host.
2. Use the host's skill reload or registration command, or restart the session.
3. Invoke the skill through the host's skill menu or slash command where supported.
4. If the host has no skill loader, load `SKILL.md` as agent instructions or use a natural-language trigger.

The skill does not require bundled scripts, references, assets, dependencies, or network access.

## Quick Start

Use a prompt such as:

```text
Check this workspace and identify the live repository, frozen snapshots,
latest ship version, local release lag, and any stale documentation.
```

Or:

```text
Resume this project safely. Orient me first, but do not modify files,
run builds, fetch remotes, or publish anything.
```

## Safety and Privacy

- The workflow is read-only by default.
- It does not fetch remotes, run builds, execute artifacts, or publish releases.
- It never asks the agent to copy credentials, PINs, tokens, API keys, or secret values into notes or memory.
- Examples use placeholder paths only.
- This documentation package contains no personal names, account handles, email addresses, local user paths, or credentials.

## Validation Status

The package passed structural validation, Markdown link checks, English-only and privacy scans, Bash syntax checks for all embedded blocks, and a read-only Git Bash runtime smoke test.

See [Validation report](docs/validation.md) for details.

## License

MIT License. See [LICENSE](LICENSE).

## Contributing

- Keep all documentation in English.
- Do not add personal identifiers, local paths, account names, credentials, or secret values.
- Preserve the `SKILL.md` frontmatter and hyphen-case skill name.
- Keep the skill body under 500 lines.
- Keep host-specific paths and commands out of `SKILL.md`; document them per host instead.
- Validate links, code fences, and embedded shell syntax before proposing changes.
