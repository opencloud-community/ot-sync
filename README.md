# OpenTalk Repository Synchronization

This repository contains a GitHub Action that synchronizes repositories from GitLab to GitHub every 6 hours. It can also be triggered manually.

## Repositories Synchronized

- `https://gitlab.opencode.de/opentalk/ot-setup.git` → `github.com:opencloud-community/ot-setup.git`
- `https://gitlab.opencode.de/opentalk/docs.git` → `github.com:opencloud-community/ot-docs.git`
- `https://gitlab.opencode.de/opentalk/controller.git` → `github.com:opencloud-community/ot-controller.git`

## About the Workflow

The synchronization is handled by a GitHub Actions workflow that:

1. Runs automatically every 6 hours (at 0:00, 6:00, 12:00, and 18:00 UTC)
2. Can be triggered manually through the GitHub UI
3. Verifies target repositories exist before attempting synchronization
4. Compares latest commits to check if synchronization is needed
5. Clones the source repositories from GitLab (only if changes detected)
6. Pushes the content to the target GitHub repositories

The workflow uses a Personal Access Token (PAT) stored as the `SYNC_PAT` secret for authentication with GitHub, which allows cross-repository access.

## Adding New Repositories

To add a new repository to be synchronized, copy an existing sync step in the workflow file and modify the repository details:

```yaml
- name: Sync new-repository-name
  run: |
    # Extract target owner/repo for API call
    TARGET_REPO_PATH=$(echo "github.com:opencloud-community/new-repo-name.git" | sed 's/.*github.com[:\/]\(.*\)\.git/\1/')

    # Check if target repository exists
    echo "=== Processing repository: new-repo-name ==="
    echo "Checking if target repository exists..."
    if ! curl -s -o /dev/null -w "%{http_code}" -H "Authorization: token ${{ secrets.SYNC_PAT }}" "https://api.github.com/repos/$TARGET_REPO_PATH" | grep -q "200"; then
      echo "🛑 Target repository $TARGET_REPO_PATH doesn't exist or not accessible. Please create it first."
      exit 1
    fi

    # ... rest of the sync logic ...
```

The workflow follows a structured approach for each repository to ensure reliability across different GitHub Actions environments.

## Manual Trigger

To manually trigger the synchronization:
1. Go to the "Actions" tab in this repository
2. Select the "Sync repositories" workflow
3. Click "Run workflow"