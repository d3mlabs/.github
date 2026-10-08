# d3mlabs/.github

Org-wide GitHub defaults and reusable workflows for d3mlabs. GitHub reads
this repo for defaults that apply to every repo in the org that does not
define its own; reusable workflows here are callable from any d3mlabs repo.

## Reusable workflows

### `add-to-project`

Feeds a repo's issues to the
[D3M Labs Multi-Project View](https://github.com/orgs/d3mlabs/projects/5),
the planning board spanning every d3mlabs repo. See the header of
[`.github/workflows/add-to-project.yml`](.github/workflows/add-to-project.yml)
for why this is a workflow rather than the project's built-in automation.

A participating repo needs the org secrets `D3MLABS_BOT_APP_ID` /
`D3MLABS_BOT_PRIVATE_KEY` granted to it, plus this caller at
`.github/workflows/add-to-project.yml`:

```yaml
# Feed new issues to the D3M Labs Multi-Project View — see d3mlabs/.github.
name: add-to-project

on:
  issues:
    types: [opened]
  # Dispatching sweeps every open issue onto the board (one-time, re-runnable).
  workflow_dispatch:

jobs:
  add-to-project:
    uses: d3mlabs/.github/.github/workflows/add-to-project.yml@main
    with:
      backfill: ${{ github.event_name == 'workflow_dispatch' }}
    secrets:
      D3MLABS_BOT_APP_ID: ${{ secrets.D3MLABS_BOT_APP_ID }}
      D3MLABS_BOT_PRIVATE_KEY: ${{ secrets.D3MLABS_BOT_PRIVATE_KEY }}
```

After merging the caller, run it once from the Actions tab (`Run workflow`)
to backfill the repo's existing open issues.
