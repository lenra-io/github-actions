# merge-artifacts.yml

Merge multiple artifacts into a single one.

## How to use

```yml
name: Release

on:
  push:

jobs:

  build-spring:
    runs-on: ubuntu-latest
    steps:
      [...]
  build-web:
    runs-on: ubuntu-latest
    steps:
      [...]

  merge-artifacts:
    name: Merge Github artifacts
    needs: [build-spring, build-web]
    uses: ./.github/workflows/merge-artifacts.yml
    with:
      with-artifacts-names: web,api
      zip-artifacts: true,false
      artifact-name: release
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `with-artifacts-names` | Comma-separated list of artifact names to merge | Yes | `.` |
| `zip-artifacts` | Comma-separated list of bool, set whether to zip the merged artifacts or not | No | `false` |
| `artifact-name` | Name of the output merged artifact | Yes |  |

