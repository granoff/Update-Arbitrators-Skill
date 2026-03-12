---
name: update-arbitrators-skill
description: For each skill that arbitrates other skills, runs the arbitrator's SHA-based version check. If any reference skill differs from the arbitrator's recorded SHA, updates the arbitrator per its updating-arbitrator workflow. Reports which arbitrators were updated, offers to review changes, and to commit and push each with a GitHub-appropriate message. Use when the user asks to update arbitrators, check arbitrator versions, or keep arbitrator skills in sync with reference skills.
---

# Update Arbitrators Skill

This skill finds all arbitrator skills, runs each one's version-check workflow, and updates any that are out of date. It then reports results and offers review, commit, and push options.

## Discover Arbitrators

An arbitrator skill is one that:
- Contains `references/updating-arbitrator.md`
- Contains `references/skill-versions.md` with a Reference SHAs table

Search under `~/.cursor/skills/` for directories matching `**/references/updating-arbitrator.md`. The parent directory of that file is the arbitrator skill root.

**Known arbitrators:** SwiftUI-Skill-Arbitrator, iOS-Accessibility-Skill-Arbitrator.

## Workflow

### Phase 1: Check Each Arbitrator

For each arbitrator:

1. **Read** `references/skill-versions.md` to get the Reference SHAs table (Skill, Repo, Reference SHA).
2. **Resolve repo paths** — For each unique Repo, find the local git root. See [references/repo-paths.md](references/repo-paths.md). Run `git rev-parse HEAD` from that root to get the current local HEAD SHA.
3. **Compare** — For each skill in the table, compare local HEAD (from its repo) with the Reference SHA. If any differ, the arbitrator is **out of date**.
4. **Record** — Note which arbitrators are out of date and which reference skills have SHA mismatches.

### Phase 2: Update Out-of-Date Arbitrators

For each arbitrator marked out of date:

1. **Load** that arbitrator's `references/updating-arbitrator.md`.
2. **Follow** the workflow there:
   - Identify what changed (git log, git diff from reference SHA to HEAD for each changed reference skill)
   - Assess impact on arbitrator content (routing, conflicts, Primary Strengths)
   - Update SKILL.md, arbitration-rules.md, skill-versions.md as needed
   - Verify consistency
3. **Track** — Record that this arbitrator was updated.

### Phase 3: Report and Offer Options

1. **Report** — Summarize which arbitrators were checked, which were out of date, and which were updated. If none were out of date, state that all arbitrators are up to date.
2. **For each updated arbitrator**, offer:
   - **Review changes** — Run `git diff` (or `git status` + `git diff`) from the arbitrator directory to show what changed. If the arbitrator is not in a git repo, show the modified files and a summary of edits.
   - **Commit and push** — If the user accepts, from the arbitrator directory run:
     - `git add . && git commit -m "<short message>" && git push`
     - If the arbitrator is not in a git repo, inform the user and skip this step.
     - Use a short, GitHub-appropriate message, e.g.:
       - `chore(arbitrator): sync with updated reference skills`
       - `chore(swiftui-arbitrator): update SHAs for swiftui-expert-skill, swiftui-pro`
       - `chore(ios-a11y-arbitrator): update SHAs for apple-accessibility-skills, ios-accessibility`

## SHA Comparison Method

Use the method defined in each arbitrator's `references/skill-versions.md`:

1. Get local HEAD: `git rev-parse HEAD` from the reference skill's **repo root** (not the skill subdirectory).
2. Compare with the Reference SHA from the table (full 40-char or first 8 chars).
3. Mismatch = arbitrator is out of date for that reference skill.

**Multi-skill repos:** Some repos contain multiple reference skills (e.g. apple-accessibility-skills has appkit, swiftui, uikit auditors). One `git rev-parse HEAD` at the repo root applies to all skills in that repo.

## Commit Message Guidelines

- Prefix with `chore(arbitrator)` or `chore(<arbitrator-name>)`.
- Keep under ~72 chars.
- Examples: `chore(swiftui-arbitrator): sync with reference skills`, `chore(ios-a11y-arbitrator): update SHAs for apple-accessibility-skills`

## Checklist

- [ ] Discovered all arbitrators
- [ ] Ran version check for each (SHA comparison)
- [ ] Updated each out-of-date arbitrator per its updating-arbitrator.md
- [ ] Reported which were updated
- [ ] Offered review for each updated arbitrator
- [ ] Offered commit and push with appropriate message for each
