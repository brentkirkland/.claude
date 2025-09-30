# Claude Commands Repository

A collection of useful Claude Code commands for software development workflows.

## Overview

This repository contains custom Claude commands that extend Claude Code's functionality. These commands provide helpful automation for common development tasks like committing changes, creating pull requests, and performing code quality checks.

## Installation

This repository IS your `.claude` directory. Clone it directly to your home directory:

```bash
# Clone this repo as your .claude directory
git clone <repo-url> ~/.claude
```

**Important**: These commands are designed to work from your root `.claude` directory and will **not work inside other git repositories**. Claude Code automatically discovers and loads commands from this central location.

## Available Commands

### `/commit`
Stages all files and creates a commit with an AI-generated message based on the changes.

### `/pr`
Analyzes commits on the current branch and creates a pull request with an AI-generated description. Supports GitHub PR templates.

### `/sanity-check`
Analyzes uncommitted changes to identify potential security issues, logic errors, code quality problems, and best practice violations.

### `/push`
Pushes the current branch to origin using `git push origin HEAD`.

## Usage

Once installed, these commands are available in any project when using Claude Code:

```bash
# In any git repository
claude commit
claude pr
claude sanity-check
claude push
```

## Requirements

- Claude Code CLI installed and configured
- Git installed and configured
- For `/pr` command: GitHub CLI (`gh`) installed and authenticated

## Command Documentation

Each command includes comprehensive documentation with usage examples, requirements, and detailed descriptions of functionality. See the individual command files in the `commands/` directory for full details.
