# d3mlabs/.github

Org-wide GitHub defaults and reusable workflows for d3mlabs. GitHub reads
this repo for defaults that apply to every repo in the org that does not
define its own; reusable workflows here are callable from any d3mlabs repo.

## Reusable workflows

### `add-to-project`

Feeds a repo's newly opened issues to the
[D3M Labs Multi-Project View](https://github.com/orgs/d3mlabs/projects/5),
the planning board spanning every d3mlabs repo. Only issues opened by org
members, outside collaborators, or the ai-flow App are added; drive-by
issues on public repos stay in the repo for triage. See the header of
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

jobs:
  add-to-project:
    uses: d3mlabs/.github/.github/workflows/add-to-project.yml@main
    secrets:
      D3MLABS_BOT_APP_ID: ${{ secrets.D3MLABS_BOT_APP_ID }}
      D3MLABS_BOT_PRIVATE_KEY: ${{ secrets.D3MLABS_BOT_PRIVATE_KEY }}
```

The workflow only sees issues opened after it lands. A repo joining the
board with existing open issues gets them on once, by hand:

```sh
gh issue list --repo d3mlabs/<repo> --state open --limit 1000 --json url --jq '.[].url' |
  xargs -n1 gh project item-add 5 --owner d3mlabs --url
```
