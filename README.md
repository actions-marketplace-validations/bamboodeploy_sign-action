# Bamboo Deploy Sign Action

Sign Windows binaries (.exe / .msi) from a GitHub Actions workflow via the [Bamboo Deploy](https://www.bamboodeploy.com) API.

Wraps the upload → sign → poll → download flow into a single step.

## Usage

```yaml
- name: Sign Windows binary
  uses: bamboodeploy/sign-action@v1
  with:
    api-key: ${{ secrets.BAMBOO_API_KEY }}
    file:    ./dist/myapp.exe
    output:  ./dist/myapp-signed.exe
```

Add `BAMBOO_API_KEY` as a repository secret (Settings → Secrets and variables → Actions). Generate the key in the Bamboo Deploy dashboard under **API Keys**.

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `api-key` | yes | — | API key, starts with `bd_live_` |
| `file` | yes | — | Path to the .exe or .msi to sign |
| `output` | yes | — | Where to write the signed binary |
| `poll-interval` | no | `10` | Seconds between sign-status polls |
| `poll-timeout` | no | `900` | Maximum seconds to wait |
| `api-base` | no | `https://api.bamboodeploy.com` | API base URL |

## Outputs

| Name | Description |
|---|---|
| `app-id` | The app id assigned by Bamboo Deploy |
| `job-id` | The sign job id |

## Requirements

- A Bamboo Deploy account with an active Premium subscription (signing requires Premium; uploads work on Free)
- The runner must have `bash`, `curl`, and `jq` available — preinstalled on all `ubuntu-*` and `macos-*` runners. On `windows-*` runners use Git Bash (`shell: bash`) or switch to a Linux/macOS runner.

## License

MIT
