# Pie Code Changes Tracker GitHub Action to increase Coverage

This GitHub Action automatically tracks and uploads pull request code changes to Pie's platform. It captures PR metadata including author, commit info, PR number, title, and URL.

## Features

- Automatic extraction of PR information from GitHub context
- Secure API key authentication
- Simple integration with existing GitHub workflows
- Built-in validation and error handling
- Works with merged PRs, opened PRs, and other PR events

## Prerequisites

Before using this action, you'll need:

- A Pie account - Sign up at https://app.pie.inc
- A Pie API key - After signing up, you can generate your API key for your app

## Usage

To use this action in your GitHub workflow, add the following step to your `.github/workflows/your-workflow.yml` file:

```yaml
- name: Track Code Changes in Pie
  uses: pielabsai/codechanges-action@v1.0  # Replace with actual repository/version
  with:
    pie_api_key: ${{ secrets.PIE_API_KEY }}
```

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `pie_api_key` | Your Pie API key for authentication. Should be stored as a GitHub secret. | Yes |

## Captured Information

The action automatically captures and sends the following information:

- **Author**: The GitHub username of the PR author
- **Merge Commit**: The SHA of the merge commit
- **PR Number**: The pull request number
- **PR Title**: The title of the pull request
- **PR URL**: The direct URL to the pull request
- **Repository**: The repository name in format `owner/repo`

## Example Workflows

### Track on PR Merge

```yaml
name: Track Code Changes
on:
  pull_request:
    types: [closed]

jobs:
  track-changes:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Track Code Changes
        uses: pielabsai/codechanges-action@v1.0
        with:
          pie_api_key: ${{ secrets.PIE_API_KEY }}
```

### Track on PR Open/Update

```yaml
name: Track Code Changes
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  track-changes:
    runs-on: ubuntu-latest
    steps:
      - name: Track Code Changes
        uses: pielabsai/codechanges-action@v1.0
        with:
          pie_api_key: ${{ secrets.PIE_API_KEY }}
```

### Track All PR Events

```yaml
name: Track Code Changes
on:
  pull_request:
    types: [opened, synchronize, reopened, closed]

jobs:
  track-changes:
    runs-on: ubuntu-latest
    steps:
      - name: Track Code Changes
        uses: pielabsai/codechanges-action@v1.0
        with:
          pie_api_key: ${{ secrets.PIE_API_KEY }}
```

## Trigger Strategie

### 1. On PR Merge (Recommended)

Best for tracking completed changes:

```yaml
on:
  pull_request:
    types: [closed]

jobs:
  track-changes:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
```

### 2. On PR Creation

Track changes as soon as PRs are opened:

```yaml
on:
  pull_request:
    types: [opened]
```

### 3. On PR Updates

Track every update to a PR:

```yaml
on:
  pull_request:
    types: [opened, synchronize]
```

### 4. Multiple Triggers

Combine with other workflows:

```yaml
on:
  pull_request:
    types: [opened, closed]
    branches: [main, develop]
  workflow_dispatch:
```

## Error Handling

The action includes built-in error handling for:

- Missing PR context
- API authentication failures
- Upload failures
- Network errors

Any errors will cause the action to fail with an appropriate error message in the workflow logs.

## Security Notes

- Always store your Pie API key as a GitHub secret
- Never commit API keys directly in your workflow files
- The action automatically handles secure transmission of data
- All communications with the API use HTTPS

## Troubleshooting

### Action not capturing PR information

Make sure the action is triggered on a pull request event:

```yaml
on:
  pull_request:
    types: [opened, closed]
```

### API authentication failures

Verify that:
1. Your `PIE_API_KEY` secret is set correctly in GitHub repository settings
2. The API key is valid and not expired
3. Your account has permission to access the API

## Support

For issues, feature requests, or questions about this GitHub Action, please open an issue in the repository.

## License

This GitHub Action is available under the MIT License. See the LICENSE file for more details.
