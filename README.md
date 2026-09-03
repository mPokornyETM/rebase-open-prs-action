# Rebase Open PRs Action

[![GitHub release](https://img.shields.io/github/v/release/mPokornyETM/rebase-open-prs-action)](https://github.com/mPokornyETM/rebase-open-prs-action/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A GitHub Action that automatically rebases all open pull requests when the default branch is updated. This keeps PRs up-to-date and reduces merge conflicts.

## Features

- 🔄 Automatically rebases all open PRs when the default branch is pushed
- 📦 Special handling for Dependabot PRs (uses `@dependabot rebase` command)
- 📝 Option to skip draft PRs
- ⚡ Simple one-line configuration

## Usage

### Basic Usage

Create a workflow file (e.g., `.github/workflows/rebase-open-prs.yml`):

```yaml
name: Rebase Open PRs

on:
  push:
    branches:
      - master  # or main

permissions:
  contents: write
  pull-requests: write

jobs:
  rebase:
    runs-on: ubuntu-latest
    steps:
      - uses: mPokornyETM/rebase-open-prs-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Advanced Usage

```yaml
name: Rebase Open PRs

on:
  push:
    branches:
      - master

permissions:
  contents: write
  pull-requests: write

jobs:
  rebase:
    runs-on: ubuntu-latest
    steps:
      - uses: mPokornyETM/rebase-open-prs-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          skip-drafts: 'true'                    # Skip draft PRs (default: true)
          skip-dependabot-rebase: 'true'         # Use @dependabot rebase comment (default: true)
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token with permissions to update PRs | Yes | `${{ github.token }}` |
| `skip-drafts` | Skip draft PRs | No | `true` |
| `skip-dependabot-rebase` | Use `@dependabot rebase` comment instead of direct rebase for Dependabot PRs | No | `true` |

## Permissions

This action requires the following permissions:

```yaml
permissions:
  contents: write       # To update PR branches
  pull-requests: write  # To comment on PRs (for Dependabot)
```

## How It Works

1. When a push is made to the default branch (e.g., `master` or `main`)
2. The action lists all open (non-draft) PRs
3. For each PR:
   - If it's a Dependabot PR: Posts `@dependabot rebase` comment
   - Otherwise: Uses `gh pr update-branch --rebase` to rebase the PR branch

## Why Use This?

- **Reduce merge conflicts**: Keep PR branches up-to-date with the latest changes
- **Faster CI feedback**: PRs are always tested against the latest code
- **Less manual work**: No need to manually rebase PRs

## Publishing a release

To publish a new release the recommended workflow is to create a version tag locally, push it, and make a GitHub release. Example commands:

```bash
git checkout main
git pull

# create an annotated tag for the latest commit
git tag -a v1.2.3 -m "Release v1.2.3"

# push a single tag to origin
git push origin v1.2.3
```

The release pipeline will do the realse for you.


## License

MIT License - see [LICENSE](LICENSE) for details.
