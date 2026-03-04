---
name: feature-release
description: Implement a feature/bug fix and release it directly without PR.
type: flow
---

```d2
implement: |md
  Implement the feature or bug fix:
  1. Understand the requirements
  2. Make code changes
  3. Follow existing code patterns
  4. Keep changes minimal
|

check: |md
  Run quality checks:
  - `uv run pyright <changed_files> --pythonversion 3.12`
  - `uv run ruff check <changed_files>`
  - `uv run ruff format <changed_files> --check`
  - `uv run pytest tests/ -x` (if tests exist)
|

commit_feature: |md
  Commit the feature changes with conventional commit message:
  - `git add <files>`
  - `git commit -m "<type>(<scope>): <subject>"`
  - Types: feat, fix, refactor, docs, perf, test, chore
|

update_changelog: |md
  Update CHANGELOG.md:
  - Add new version section under Unreleased
  - Format: `## 1.X.0 (YYYY-MM-DD)`
  - List the changes as bullet points
  - Keep Unreleased header above
|

bump_version: |md
  Update pyproject.toml version:
  - Keep MAJOR.MINOR fixed (e.g., 1.14)
  - Bump PATCH for any change (features, fixes, etc.)
  - Example: 1.14.10 → 1.14.11
|

uv_sync: "Run `uv sync` to update uv.lock."

commit_release: |md
  Commit version bump:
  - `git add CHANGELOG.md pyproject.toml uv.lock`
  - `git commit -m "chore(release): bump version to X.Y.0"`
|

tag_push: |md
  Tag and push the release:
  - `git tag -a X.Y.0 -m "Release X.Y.0"`
  - `git push origin main`
  - `git push origin X.Y.0`
|

BEGIN -> implement -> check -> commit_feature -> update_changelog -> bump_version -> uv_sync -> commit_release -> tag_push -> END
```

## Guidelines

### Commit Message Format
```
<type>(<scope>): <subject>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring
- `docs`: Documentation changes
- `perf`: Performance improvements
- `test`: Test changes
- `chore`: Build/tooling changes

### Versioning Policy (Patch-bump-only)
- Format: `MAJOR.MINOR.PATCH`
- **MAJOR.MINOR stays fixed** (e.g., `1.14`)
- **PATCH is bumped for any change**: features, bug fixes, refactors, docs, etc.
- **MAJOR/MINOR only changes by explicit manual decision**
- Examples:
  - `1.14.10` → `1.14.11` (normal release)
  - `1.14.10` → `1.15.0` (only if explicitly requested)

### Files to Update
1. Source files (implementation)
2. `CHANGELOG.md` (add new version section)
3. `pyproject.toml` (bump version)
4. `uv.lock` (auto-generated via `uv sync`)

### Workflow Notes
- This is for **direct releases without PR** (fork-only workflow)
- All development is done on the fork (`yumesha/kimi-cli`)
- Never create PRs on upstream (`MoonshotAI/kimi-cli`)
- Push tags to `origin` (the fork), not `upstream`
