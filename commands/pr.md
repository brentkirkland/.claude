# Create Pull Request

Analyzes commits on the current branch and creates a pull request with an AI-generated description based on the commit history.

## Usage

```bash
claude pr
```

## Description

This command will:

1. Analyze all commits on the current branch since it diverged from the main branch
2. Extract commit messages, changes, and patterns from the commit history
3. Generate a comprehensive PR title and description based on the changes
4. Check for existing GitHub PR templates in the repository (case-insensitive search for `pull_request_template.md`)
5. Create a pull request using the `gh` CLI with the generated content
6. Push the current branch to remote if needed

The AI will examine the commit history to understand the scope of changes and create a detailed PR description that includes:
- Summary of changes made
- List of commits included
- Test plan based on the changes
- Any breaking changes or migration notes

## Requirements

- Must be run in a git repository with a remote origin
- GitHub CLI (`gh`) must be installed and authenticated
- Current branch must have commits that differ from the main branch
- Write access to the repository for creating pull requests

## Examples

```bash
$ claude pr
Analyzing commits on feature/user-auth...
Found 3 commits since main branch:
- feat: add JWT token authentication
- fix: handle expired tokens gracefully
- docs: update API documentation

Generating PR description...
Creating pull request...
https://github.com/user/repo/pull/123
```

## Options

The command automatically detects:
- Base branch (usually `main` or `master`)
- Current branch name for the PR head
- Commit range to analyze
- Whether the branch needs to be pushed to remote

## GitHub Templates

The command searches for pull request templates in the following locations (case-insensitive):
- `.github/pull_request_template.md`
- `.github/PULL_REQUEST_TEMPLATE.md`
- `pull_request_template.md`
- `PULL_REQUEST_TEMPLATE.md`

If found, the template will be used as a base structure and the AI will populate the relevant sections with generated content based on the commit analysis.