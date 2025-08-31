# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains a collection of reusable GitHub Actions for automated development workflows. Each action is organized in its own directory with a standalone `action.yml` file.

## Architecture

### Action Structure
- **autofix/**: Automatically fixes formatting issues and commits changes
- **automerge/**: Handles automatic merging of Dependabot PRs for minor/patch updates
- **deploy/**: Deploys applications to CloudFlare Pages (supports both Next.js and Worker deployments)
- **notify/**: Sends Slack notifications with deployment status
- **prepare/**: Sets up environment with Node.js/pnpm based on `.tool-versions` file
- **todo/**: Converts TODO comments to GitHub issues using alstr/todo-to-issue-action

### Key Dependencies
All actions use pnpm as the package manager and expect a `.tool-versions` file for Node.js and pnpm version specification.

### Action Workflows
Actions are designed to be composable:
1. Most actions start with the `prepare` action to set up the environment
2. `autofix` uses `prepare` internally and can take a customizable `format-command` input
3. `deploy` is comprehensive and handles build detection (next-on-pages vs worker), deployment, GitHub deployment creation, cache purging, and notifications

## Development Commands

Since this is a collection of GitHub Actions without a traditional application structure, there are no standard build/test commands. Development involves:

- Modifying individual `action.yml` files
- Testing actions within GitHub workflows
- Using semantic-release for automated versioning (triggered on main branch pushes)

## Important Notes

### Git Configuration
Actions that commit changes use the following git configuration:
- Name: "Luca Steeb (bot)" or "Luca Steeb"  
- Email: "contact@luca-steeb.com"

### Deployment Specifics
The deploy action supports both Next.js (via next-on-pages) and Cloudflare Workers (via opennextjs-cloudflare), with automatic detection based on package.json dependencies.

### Security Requirements
Actions require various tokens and API keys as inputs:
- GitHub tokens for repository access
- CloudFlare API tokens for deployments
- Sentry auth tokens for sourcemaps
- Webhook URLs for notifications

## Testing Actions

Actions are tested through actual GitHub workflows. The `.github/workflows/` directory contains:
- `release.yml`: Handles semantic releases
- `auto-merge.yml`: Manages Dependabot PR auto-merging