# start-action

GitHub Actions for [gha-trigger](https://gha-trigger.github.io)

## What does this action do?

- Show GitHub Actions Step Summary by [gha-trigger/step-summary-action](https://github.com/gha-trigger/step-summary-action)
- Set Environment Variables by [gha-trigger/set-env-action](https://github.com/gha-trigger/set-env-action)
- Generate GitHub App Token by [tibdex/github-app-token](https://github.com/tibdex/github-app-token)
- Set the commit status to `pending`
- Checkout Main Repository and CI Repository by [actions/checkout](https://github.com/actions/checkout)

## Example

```yaml
on:
  workflow_dispatch:
    inputs:
      data:
        required: true
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: gha-trigger/start-action@main
        with:
          data: ${{inputs.data}}
          app_id: ${{secrets.APP_ID}}
          app_private_key: ${{secrets.APP_PRIVATE_KEY}}
```

## Inputs

### Required

- `data`: gha-trigger's workflow dispatch input
- `app_id`: GitHub App ID
- `app_private_key`: GitHub App Private Key

### Optional

- `main_repo_checkout_fetch_depth`
- `main_repo_checkout_lfs`
- `main_repo_checkout_submodules`
- `enable_ci_repo_checkout`
- `ci_repo_checkout_path`
- `ci_repo_checkout_ref`
- `ci_repo_checkout_fetch_depth`
- `ci_repo_checkout_lfs`
- `ci_repo_checkout_submodules`

## Outputs

- `github_app_token`: GitHub App's Access Token

## Environment Variables

Environment Variables are set by [gha-trigger/set-env-action](https://github.com/gha-trigger/set-env-action).

## LICENSE

[MIT](LICENSE)
