# GitTools CI/CD Actions

Reusable GitHub composite actions for authentication, secret loading, and git automation.

## Available Actions

### checkout

Path: `checkout`

Authenticates via GitHub App credentials (loaded from 1Password), creates an installation token, and checks out the repository.

Inputs:

- `op_service_account_token` (required): 1Password service account token.
- `fetch-depth` (optional, default `0`): Depth for `actions/checkout`.

### gh-app-creds

Path: `gh-app-creds`

Loads GitHub App credentials from 1Password.

Inputs:

- `op_service_account_token` (required): 1Password service account token.

Outputs:

- `gh_app_id`
- `gh_app_private_key`

### tfx-creds

Path: `tfx-creds`

Loads TFX token from 1Password.

Inputs:

- `op_service_account_token` (required): 1Password service account token.

Outputs:

- `tfx_token`

### nuget-creds

Path: `nuget-creds`

Loads NuGet API key from 1Password.

Inputs:

- `op_service_account_token` (required): 1Password service account token.

Outputs:

- `nuget_api_key`

### choco-creds

Path: `choco-creds`

Loads Chocolatey API key from 1Password.

Inputs:

- `op_service_account_token` (required): 1Password service account token.

Outputs:

- `choco_api_key`

### dockerhub-creds

Path: `dockerhub-creds`

Loads DockerHub credentials from 1Password.

Inputs:

- `op_service_account_token` (required): 1Password service account token.

Outputs:

- `docker_username`
- `docker_password`

### git-commit-push

Path: `git-commit-push`

Adds all changes, commits with the provided message, and force-pushes.

Inputs:

- `message` (required): Commit message.

## Example Usage

```yaml
name: CI
on:
  workflow_dispatch:

jobs:
  demo:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout with GitHub App auth
        uses: gittools/cicd/checkout@v1
        with:
          op_service_account_token: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}

      - name: Load TFX token
        id: tfx
        uses: gittools/cicd/tfx-creds@v1
        with:
          op_service_account_token: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}

      - name: Load DockerHub credentials
        id: dockerhub
        uses: gittools/cicd/dockerhub-creds@v1
        with:
          op_service_account_token: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}
```

## Notes

- These actions depend on `1password/load-secrets-action@v3` where applicable.
- Prefer version tags (for example `@v1`) in production workflows once releases are cut.
