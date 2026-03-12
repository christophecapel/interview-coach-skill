# interview-coach-skill -- Claude Code Session Context

This file helps Claude Code understand the repo at the start of any session.

---

## What This Repo Is

A Claude skill prompt — not software. The main product is `SKILL.md` (~47K), a comprehensive interview coaching system with 23 commands covering the full interview lifecycle. The skill is used by renaming `SKILL.md` to `CLAUDE.md` in the user's local clone, then running Claude Code.

This repo contains the skill prompt, version history, reference materials, and release snapshots. No code, no dependencies, no build tooling.

---

## Key Files

| File/Folder | Purpose |
|---|---|
| `SKILL.md` | The skill prompt (~47K). Main product. Do NOT restructure without explicit direction. |
| `VERSIONS.md` | Version roadmap: shipped versions (v1-v3), planned versions (v4+), and feature details |
| `references/` | Supporting reference files loaded by SKILL.md commands (transcript formats, rubrics, examples, etc.) |
| `releases/` | Tagged release snapshots |
| `README.md` | Public-facing overview, setup instructions, command reference |
| `.claude/CLAUDE.md` | This file (session context for repo maintenance — separate from root CLAUDE.md which is the active skill copy) |

---

## Standards

### Commit Format

Every commit must follow: `<type>: <what changed> — <why>`

Types: `skill`, `refs`, `docs`, `chore`, `fix`

Examples:
```
skill: add salary command — comp strategy coaching for offer negotiation
refs: update transcript-formats.md — add Granola format support
docs: update README command table — reflect v3 additions
fix: schema migration gap — older coaching_state files now fully upgrade
chore: update VERSIONS.md — mark v3 as shipped
```

### PR Creation (mandatory)

After every `git push`, immediately create a PR. Do not stop after push.

**If the session ends without a PR after a push: this is a failure.** If the user says "we're done" or tries to close the session after a push without a PR having been created, flag it immediately before stopping: *"PR not created — do it now or the session is incomplete."*

```bash
gh pr create \
  --title "<type>: <concise description>" \
  --body "$(cat <<'EOF'
## What
- <bullet: files changed and what changed in each>

## Why
<1-3 sentences of context/goal>

## How to verify
- <concrete confirmation method>
EOF
)"
```

Rules:
- Title must follow the commit convention above
- Body must use the What / Why / How to verify format
- The PR URL printed by `gh pr create` is the only action required from the user — they click Merge, nothing else
- Do NOT skip this step under any circumstance

### Changelog Rule

Before creating any PR, check if it contains qualifying commits:

1. Run `git log --format="%ad %s" --date=short origin/main..HEAD`
2. If ANY commit has types `skill:`, `refs:`, `fix:` — draft a changelog entry in `changelog.md`
3. Include the changelog update in the same PR

Skip only if every commit is: `docs:` / `chore:` / merge

---

## How We Work

1. **Read before creating** — check if a file already exists before writing a new one. Avoid duplication.
2. **`git mv`, not `cp` + `rm`** — always use `git mv` when moving files. Preserves history.
3. **No orphaned files** — if content migrates, delete the source. No redirects nobody reads.
4. **One focused thing per session** — mixing concerns inflates blast radius and muddies git history. Resist scope creep.
5. **Auto-derive, don't ask** — if data is inferable from context, use it. Ask only when genuinely ambiguous. One question, not several.
6. **Deferred work needs persistent markers** — verbal instructions to "do it next time" vanish between sessions. Write a flag (TODO, `[deferred]` tag); detect the flag at session start.
7. **Fix conflicts autonomously** — when merge conflicts, workflow failures, or broken states occur, fix them directly without surfacing the problem to the user. Exception: when resolution requires credentials or out-of-band action the user controls.
8. **Build mechanisms, not habits** — every recurring action must have: (a) a clear trigger (not manual memory), (b) consistent execution when the trigger fires, (c) a defined expected outcome, (d) retry logic (up to N attempts) when the outcome isn't met, (e) a failure notification if retries are exhausted — never fail silently.

---

## Session Kickoff Protocol

At the start of every session, before doing anything else:

1. Read this file (`CLAUDE.md`)
2. Read `VERSIONS.md` — check current shipped version and any in-progress work
3. Run `git log --oneline -10` to see recent changes
4. State the session goal before touching anything

---

## End of Session

Before closing any session:

1. Summarize what was done
2. Update `VERSIONS.md` if `SKILL.md` was modified (version bump or roadmap update)
3. Update `changelog.md` if qualifying changes were made
4. Verify a PR was created for every push this session
5. Flag any deferred work with persistent markers (TODO, `[deferred]` tag)

---

## What NOT to Do

- Do not restructure `SKILL.md` without explicit direction — it is 47K and carefully structured across 23 commands with cross-references
- Do not create software files (no package.json, no .py, no databases)
- Do not add dependencies or build tooling
- Do not modify `references/` files without checking which SKILL.md commands depend on them
- Do not push directly to main without the user's direction

---

## Related Repos

- `christophecapel/myOS` — daily OS, STAR library (`careerOS/star-library/`)
- `christophecapel/job-hunting` — opportunities, coaching_state.md (symlinked from here), interview prep
- `christophecapel/portfolio` — PM portfolio case studies
