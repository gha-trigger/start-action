# start-action

GitHub Actions for [gha-trigger](https://gha-trigger.github.io)

## What does this action do?

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

## Update a commit status per workflow

start-action is run per job, so commit statuses are updated per job by default.
To update commit statuses, start-action calls GitHub API so it may cause GitHub API rate limiting.
If you'd like to decrease API call, you can update commit statuses per not job but workflow.
To do so, please do the following things.

- Set the environment variable `GHA_WORKFLOW_COMMIT_STATUS` to `true` in workflow scope
- Set the parameter `start_workflow` to `true` at only one `start-action`
- Remove `end-action` from all jobs except for the last job
- Add a job to update a commit status at the end of workflow

e.g.

```yaml
env:
  GHA_WORKFLOW_COMMIT_STATUS: "true"
jobs:
  foo:
    steps:
      - uses: gha-trigger/start-action@main
        id: start
        with:
          # ...
          # commit status is changed to "pending"
          update_commit_status: true # set this parameter at only this step
      # ...
  bar:
    steps:
      - uses: gha-trigger/start-action@main
        id: start
        with:
          # ...
          # Don't set the parameter `update_commit_status`
          # commit status isn't changed
      # ...
  status-check:
    runs-on: ubuntu-latest
    needs: [foo, bar] # Run this job lastly
    if: always()
    steps:
      - uses: gha-trigger/start-action@main
        id: start
        with:
          # ...
          # Don't set the parameter `update_commit_status`
          # commit status isn't changed
      - uses: gha-trigger/end-action@main
        if: always()
        with:
          github_token: ${{steps.start.outputs.github_app_token}}
          state: ${{job.status}}
```


## LICENSE

[MIT](LICENSE)
