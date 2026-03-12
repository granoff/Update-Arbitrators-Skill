# Update Arbitrators Skill

An [Agent Skills](https://agentskills.io/home) skill that keeps arbitrator skills in sync with their reference skills. It runs each arbitrator's SHA-based version check, updates any that are out of date, and offers to commit and push the changes.

## What This Skill Does

- **Version check** — Compares local HEAD of each reference skill repo against the arbitrator's recorded SHA
- **Update workflow** — For out-of-date arbitrators, follows the arbitrator's `updating-arbitrator.md` workflow (review changes, assess impact, update SHAs and content)
- **Report and options** — Summarizes which arbitrators were updated, offers to review diffs, and to commit and push with GitHub-appropriate messages

Use this skill when you want to check arbitrator versions, bring arbitrators up to date with updated reference skills, or keep your arbitrator skills maintained.

## Supported Arbitrators

This skill discovers arbitrators by searching for `references/updating-arbitrator.md` under `~/.cursor/skills/`. Known arbitrators:

| Arbitrator | Reference Skills |
|------------|------------------|
| **SwiftUI-Skill-Arbitrator** | swiftui-expert-skill, swiftui-pro |
| **iOS-Accessibility-Skill-Arbitrator** | appkit/swiftui/uikit-accessibility-auditor, ios-accessibility, swift-accessibility-skill |

## Requirements

- Reference skill repos must be installed locally (typically under `~/.cursor/skills/`)
- Each reference skill must be in a git repository
- Arbitrator skills must contain `references/skill-versions.md` with a Reference SHAs table

## Usage

Ask the agent to run the Update Arbitrators workflow, for example:

- "Update arbitrators"
- "Check arbitrator versions"
- "Run the Update-Arbitrators-Skill"

The agent will:

1. **Check** each arbitrator's reference SHAs against local HEAD
2. **Update** any out-of-date arbitrators per their update workflow
3. **Report** results and offer to review changes
4. **Offer** to commit and push each updated arbitrator with a short message

## Workflow Summary

1. **Phase 1** — Discover arbitrators, read `skill-versions.md`, run `git rev-parse HEAD` in each reference repo, compare SHAs
2. **Phase 2** — For mismatches, load `updating-arbitrator.md`, review diffs, update arbitrator files as needed
3. **Phase 3** — Report which were updated, show diffs, offer `git add . && git commit -m "..." && git push`

See [SKILL.md](SKILL.md) for full instructions.

## License

[MIT License](LICENSE)