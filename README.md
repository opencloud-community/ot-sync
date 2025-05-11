# OpenTalk Repository Synchronization

This repository contains a GitHub Action that synchronizes repositories from GitLab to GitHub every 6 hours. It can also be triggered manually.

## Repositories Synchronized

- `https://gitlab.opencode.de/opentalk/ot-setup.git` → `git@github.com:opencloud-community/ot-setup.git`
- `https://gitlab.opencode.de/opentalk/docs.git` → `git@github.com:opencloud-community/ot-docs.git`

## Setup Instructions

For the synchronization to work, you need to set up an SSH deploy key with write access:

1. Generate an SSH key pair:
   ```bash
   ssh-keygen -t ed25519 -C "github-actions@github.com" -f id_sync
   ```

2. Add the **private key** as a repository secret in this repository:
   - Go to Settings > Secrets and variables > Actions
   - Create a new repository secret named `SYNC_SSH_PRIVATE_KEY`
   - Paste the contents of the `id_sync` file (private key)

3. Add the **public key** as a deploy key to both target repositories:
   - Go to Settings > Deploy keys in each target repository
   - Add a new deploy key with the contents of `id_sync.pub` (public key)
   - Make sure to check "Allow write access"
   - Name it something like "Repository Sync Action"

4. Update the GitHub Action workflow file (`.github/workflows/sync-repos.yml`) to use the SSH key

## Manual Trigger

To manually trigger the synchronization:
1. Go to the "Actions" tab in this repository
2. Select the "Sync repositories" workflow
3. Click "Run workflow"