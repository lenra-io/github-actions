# generate-version-names.yml

Generate comma separated list of tags for a version given as input, ready to use with docker.

## How to use ?

```yml
name: Build

on:
  push:

permissions: 
  packages: write

jobs:
  generate-version-names:
    name: Generate version names from version
    uses: lenra-io/github-actions/.github/workflows/generate-version-names.yml@main
    with:
      version: 1.1.0
      template: ghcr.io/${{ github.repository }}/server:[:version:]

  build-docker:
    name: Build docker image
    needs: generate-version-names
    uses: lenra-io/github-actions/.github/workflows/build-docker.yml@main
    with:
      tags: ${{ needs.generate-version-names.outputs.tags }}
```
