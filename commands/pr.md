# Create Pull Request

Analyzes commits on the current branch and creates a pull request with an AI-generated description (using the the repo's pull request template) based on the commit history.

## Usage

```bash
claude pr
```

## Description

This command will:

1. Search for and validate a GitHub PR template exists in the repository
2. Analyze all commits on the current branch since it diverged from the main branch
3. Extract commit messages, changes, and patterns from the commit history
4. Generate a comprehensive PR title and description based on the changes using the repo's pull request template
5. Create a pull request using the `gh` CLI with the populated template
6. Push the current branch to remote if needed

**IMPORTANT**: The command will fail if no PR template is found in the repository. A template is required for consistent PR formatting.

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
- **A PR template must exist in one of the supported locations (see GitHub Templates section)**

## Examples

### Successful PR Creation

```bash
$ claude pr
Searching for PR template...
Found template: .github/pull_request_template.md
Analyzing commits on feature/user-auth...
Found 3 commits since main branch:
- feat: add JWT token authentication
- fix: handle expired tokens gracefully
- docs: update API documentation

Populating template with generated content...
Creating pull request...
https://github.com/user/repo/pull/123
```

### No Template Found (Command Fails)

```bash
$ claude pr
Searching for PR template...
Error: No PR template found in repository.
Please add a pull request template in one of these locations:
- .github/pull_request_template.md
- .github/PULL_REQUEST_TEMPLATE.md
- pull_request_template.md
- PULL_REQUEST_TEMPLATE.md

Command aborted.
```

## Options

The command automatically detects:

- Base branch (usually `main` or `master`)
- Current branch name for the PR head
- Commit range to analyze
- Whether the branch needs to be pushed to remote

## GitHub Templates

**REQUIRED**: The command requires a pull request template to be present in the repository. It searches for templates in the following locations (case-insensitive):

- `.github/pull_request_template.md`
- `.github/PULL_REQUEST_TEMPLATE.md`
- `pull_request_template.md`
- `PULL_REQUEST_TEMPLATE.md`

The template will be used as the base structure and the AI will intelligently populate the relevant sections with generated content based on the commit analysis. If no template is found in any of these locations, the command will fail with an error message listing the expected template locations.
