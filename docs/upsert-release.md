# upsert-release.yml

## Example of usage

```yml
name: Build

on:
  push:

jobs:
  prepare:
    name: Prepare release
    uses: actions/upload-artifact@v6
    with:
      artifact-name: 'release'
      artifact-path: 'README.md'

  release-github:
    name: Release GitHub
    uses: lenra-io/github-actions/.github/workflows/upsert-release.yml@main
    with:
      version: "v1.0.0"
      changelog: "This is changelogs"
      with-artifact-name: 'release'
      with-artifact-path: 'README.md'
```
