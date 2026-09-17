# Bamboo Deploy Sign Action

Sign Windows binaries (.exe / .msi) from a GitHub Actions workflow via the [Bamboo Deploy](https://www.bamboodeploy.com) API.

Wraps the upload → sign → poll → download flow into a single step.

## Usage

```yaml
- name: Sign Windows binary
  uses: bamboodeploy/sign-action@v2
  with:
    api-key: ${{ secrets.BAMBOO_API_KEY }}
    file:    ./dist/myapp.exe
```

The signed file replaces the input in place. Pass `output` to write it elsewhere.

Add `BAMBOO_API_KEY` as a repository secret (Settings → Secrets and variables → Actions). Generate the key in the Bamboo Deploy dashboard under **API Keys**.

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `api-key` | yes | n/a | API key, starts with `bd_live_` |
| `file` | yes | n/a | Path to the .exe or .msi to sign |
| `output` | no | input path | Where to write the signed binary |
| `poll-interval` | no | `10` | Seconds between sign-status polls |
| `poll-timeout` | no | `900` | Maximum seconds to wait |
| `cli-version` | no | `v1.0.0` | Tag or branch of `bamboodeploy/cli` to run |
| `api-base` | no | `https://api.bamboodeploy.com` | API base URL |

## Outputs

| Name | Description |
|---|---|
| `app-id` | The app id assigned by Bamboo Deploy |
| `job-id` | The sign job id |

## Requirements

- A Bamboo Deploy account with an active Premium subscription (signing requires Premium; uploads work on Free)
- Node 18+ on the runner (preinstalled on all GitHub-hosted runners, Windows included). The action runs [bamboodeploy/cli](https://github.com/bamboodeploy/cli) via `npx`.
- After your first reviewed build, ask Bamboo to enable **Auto-sign** on your account so clean builds sign in minutes instead of waiting for manual review.

## v1 to v2

v2 runs the CLI instead of curl + jq, so it works on `windows-*` runners and `output` is optional. Inputs are otherwise unchanged.

## Other CI systems

GitLab, Azure Pipelines, CircleCI, Jenkins, AppVeyor: one line, `npx github:bamboodeploy/cli sign dist/myapp.exe`. Snippets at [bamboodeploy.com/docs](https://www.bamboodeploy.com/docs/#ci).

## License

MIT
