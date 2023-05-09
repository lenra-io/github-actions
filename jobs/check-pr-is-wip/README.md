# Check if PR is in WIP state

## Usage

Here a full usable example to use it : 

```yaml
name: Build docker

on:
  push:
  pull-request:
    types:
      - opened
      - edited
      - synchronize
      - reopened

jobs:
  pr-is-wip:
    name: PR is WIP
    runs-on: ubuntu-latest
    steps:
      - id: pr-is-wip
        name: PR is WIP
        uses: https://github.com/lenra-io/github-actions/jobs/check-pr-is-wip@main
        secrets:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  build:
    name: Build
    if: needs.pr-is-wip.outputs.must-run == 'true'
    runs-on: ubuntu-latest
    needs: [ pr-is-wip ]
    steps:
      - name: Checkout Code
        uses: actions/checkout@3
      - [...]
```