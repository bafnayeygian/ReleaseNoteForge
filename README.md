# ReleaseNoteForge

**ReleaseNoteForge** is a dependency-free Python CLI for generating structured release notes and changelogs from local Git history.

Version: **1.0.0**

It parses commit messages, groups changes into useful sections, detects Conventional Commits and breaking changes, summarizes contributors, and can export Markdown or JSON.

## Features

- Generate release notes from Git commits
- Compare tags, refs, or branches
- Auto-detect the latest reachable tag
- Conventional Commit detection
- Breaking-change detection
- Categories for Features, Fixes, Performance, Refactors, Docs, Tests, CI, Build, Chores, and Other
- Optional merge-commit filtering
- Contributor summary
- GitHub commit links when `origin` points to GitHub
- Markdown and JSON output
- No third-party runtime dependencies
- Automated tests and GitHub Actions

## Installation

```bash
git clone https://github.com/bafnayeygian/ReleaseNoteForge.git
cd ReleaseNoteForge
python -m pip install .
```

## Quick start

Generate notes since the latest tag:

```bash
release-note-forge .
```

Compare two refs:

```bash
release-note-forge . --from v1.0.0 --to HEAD
```

Generate Markdown:

```bash
release-note-forge . --from v1.0.0 --markdown RELEASE_NOTES.md
```

Generate JSON:

```bash
release-note-forge . --from v1.0.0 --json release.json
```

Include contributors:

```bash
release-note-forge . --from v1.0.0 --contributors
```

## Conventional commits

Recognized types:

```text
feat
fix
perf
refactor
docs
test
ci
build
chore
style
revert
```

Scopes are supported:

```text
feat(api): add pagination
fix(parser): handle blank lines
```

## Breaking changes

Detected from:

```text
feat!: remove old API
feat(config)!: rename timeout option
```

and commit bodies containing:

```text
BREAKING CHANGE:
```

## Example

Input history:

```text
feat: add token cache
fix: handle timeout correctly
docs: update installation
refactor: simplify parser
feat!: remove legacy config format
```

Generated output:

```markdown
# Release v2.0.0

## Breaking changes

- Remove legacy config format

## Features

- Add token cache

## Fixes

- Handle timeout correctly

## Documentation

- Update installation

## Refactors

- Simplify parser
```

## GitHub Actions example

```yaml
name: release-notes-preview

on:
  workflow_dispatch:

jobs:
  notes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install ReleaseNoteForge
        run: pip install git+https://github.com/bafnayeygian/ReleaseNoteForge.git

      - name: Generate notes
        run: release-note-forge . --contributors --markdown RELEASE_NOTES.md
```

## Security

ReleaseNoteForge reads Git metadata and invokes local `git` commands. It does not execute repository source code and does not intentionally make network requests.

See [SECURITY.md](SECURITY.md).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT License. See [LICENSE](LICENSE).
