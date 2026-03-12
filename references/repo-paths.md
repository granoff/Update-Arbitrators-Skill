# Resolving Reference Skill Repo Paths

When running the version check, you need the **git repo root** for each reference skill. Skills are often installed under `~/.cursor/skills/`. Repo names in skill-versions.md (e.g. `AvdLee/SwiftUI-Agent-Skill`) map to directory names that may drop the org prefix or use different separators.

## Resolution Strategy

1. **List candidates** — Under `~/.cursor/skills/`, look for directories whose names match the repo (with or without org):
   - `AvdLee/SwiftUI-Agent-Skill` → `SwiftUI-Agent-Skill`, `AvdLee-SwiftUI-Agent-Skill`, or similar
   - `twostraws/SwiftUI-Agent-Skill` → `SwiftUI-Agent-Skill-PaulHudson`, `twostraws-SwiftUI-Agent-Skill`, or similar
   - `rgmez/apple-accessibility-skills` → `apple-accessibility-skills`
   - `dadederk/iOS-Accessibility-Agent-Skill` → `iOS-Accessibility-Agent-Skill`
   - `PasqualeVittoriosi/swift-accessibility-skill` → `swift-accessibility-skill`

2. **Verify git root** — From a candidate directory, run `git rev-parse --show-toplevel`. If it succeeds, that is the repo root. The skill may live in a subdirectory (e.g. `apple-accessibility-skills/skills/appkit-accessibility-auditor`); the repo root is the parent of `.git`.

3. **Match by skill path** — If multiple candidates exist (e.g. two SwiftUI-Agent-Skill clones), confirm by checking that the expected skill subpath exists (e.g. `swiftui-expert-skill/SKILL.md` or `swiftui-pro/SKILL.md`).

## Repo → Root Mapping (Reference)

| Repo (from skill-versions) | Typical local root |
|---------------------------|---------------------|
| AvdLee/SwiftUI-Agent-Skill | ~/.cursor/skills/SwiftUI-Agent-Skill |
| twostraws/SwiftUI-Agent-Skill | ~/.cursor/skills/SwiftUI-Agent-Skill-PaulHudson |
| rgmez/apple-accessibility-skills | ~/.cursor/skills/apple-accessibility-skills |
| dadederk/iOS-Accessibility-Agent-Skill | ~/.cursor/skills/iOS-Accessibility-Agent-Skill |
| PasqualeVittoriosi/swift-accessibility-skill | ~/.cursor/skills/swift-accessibility-skill |

If a path is unknown or `git rev-parse` fails, notify the user that version verification was skipped for that skill and proceed with the rest.
