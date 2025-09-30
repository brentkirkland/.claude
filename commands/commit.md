# Commit Changes

Stages all files and creates a commit with an AI-generated message based on the changes.

## Usage

```bash
claude commit
```

## Description

This command will:

1. Stage all modified, new, and deleted files using `git add .`
2. Analyze the staged changes using `git diff --cached`
3. Generate an appropriate commit message based on the changes
4. Create the commit with the generated message

The AI will examine the diff to understand what was changed and create a concise, descriptive commit message following conventional commit format when appropriate.

## Requirements

- Must be run in a git repository
- Git must be installed and configured

## Example

```bash
$ claude commit
Staging all files...
Analyzing changes...
Generated commit message: "feat: add user authentication with JWT tokens"
Committing changes...
[main 1a2b3c4] feat: add user authentication with JWT tokens
 3 files changed, 45 insertions(+), 2 deletions(-)
```
