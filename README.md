# moodist-workspace

Private working repository for contributing to Moodist.

## What this repo is

This is a private companion repo to the moodist fork at
[ntl2105/moodist](https://github.com/ntl2105/moodist). It contains
planning documents, prompts, setup instructions, and working context
used during the contribution process. Nothing here goes upstream.

## Files

### TASKS.md
Master task list tracking all planned PRs and their current status.
Each PR maps to a specific contribution to moodist.

### PROMPTS.md
Log of all Claude Code prompts used to implement each PR, including
analysis performed before writing each prompt and verification steps
taken after. Used for reproducibility and audit trail.

### SETUP.md
Complete infrastructure setup guide — EC2 instance configuration,
SSH setup, installed packages, and troubleshooting notes. Use this
to rebuild the environment from scratch if needed.

### CLAUDE.md
Context file for Claude Code sessions. Copy this into the root of
the moodist repo before starting a Claude Code session:
  cp CLAUDE.md ~/moodist/CLAUDE.md
This file is gitignored in the moodist repo and never committed upstream.

### zshrc.txt
Mac-side shell functions for EC2 management (ec2-start, ec2-stop,
ec2-status). Added these to ~/.zshrc locally.

## Workflow

1. Plan the PR here in TASKS.md
2. Design the Claude Code prompt here in PROMPTS.md
3. Update CLAUDE.md with the current task
4. Copy CLAUDE.md to ~/moodist/CLAUDE.md on EC2
5. Run Claude Code on EC2: cd ~/moodist && claude
6. Review output, commit manually, push to ntl2105/moodist
7. Open draft PR on fork, share with supervisor
8. Update TASKS.md status after review

## EC2 Instance

- Region: ap-southeast-1 (Singapore)
- Type: c7i.flex.large
- OS: Ubuntu 22.04 LTS
