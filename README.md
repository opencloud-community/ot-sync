# OpenTalk Repository Synchronization

This repository contains a GitHub Action that synchronizes repositories from GitLab to GitHub every 6 hours. It can also be triggered manually.

## Repositories Synchronized

- `https://gitlab.opencode.de/opentalk/ot-setup.git` → `github.com:opencloud-community/ot-setup.git`
- `https://gitlab.opencode.de/opentalk/docs.git` → `github.com:opencloud-community/ot-docs.git`

## About the Workflow

The synchronization is handled by a GitHub Actions workflow that:

1. Runs automatically every 6 hours (at 0:00, 6:00, 12:00, and 18:00 UTC)
2. Can be triggered manually through the GitHub UI
3. Uses the GitHub CLI (gh) for authentication
4. Clones the source repositories from GitLab
5. Pushes the content to the target GitHub repositories

The workflow uses the default `GITHUB_TOKEN` provided by GitHub Actions, so no additional secrets need to be configured.

## Manual Trigger

To manually trigger the synchronization:
1. Go to the "Actions" tab in this repository
2. Select the "Sync repositories" workflow
3. Click "Run workflow"