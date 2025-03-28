# test-bun.yml

Run automated tests for a Bun application.

It will run `bun test` command but with cache and automatic bun setup.

## How to use

```yml
name: Test

on:
  pull_request:

jobs:
  test:
    name: Test Bun App
    uses: lenra-io/github-actions/.github/workflows/test-bun.yml@main
    with:
      working-directory: app
      bun-version: latest
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Directory containing the Bun app | No | `.` |
| `bun-version` | Version of Bun to use | No | `latest` |
