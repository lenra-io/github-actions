# check-bun.yml

Run code quality checks on a Bun application.

## How to use

```yml
name: Check

on:
  pull_request:

jobs:
  check:
    name: Check Bun Code
    uses: lenra-io/github-actions/.github/workflows/check-bun.yml@main
    with:
      working-directory: app
      bun-version: latest
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Directory containing the Bun app | No | `.` |
| `bun-version` | Version of Bun to use | No | `latest` |
