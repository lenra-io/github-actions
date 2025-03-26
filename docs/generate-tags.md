# generate-tags.yml

Generate comma separated list of tags for a version given as input, ready to use with docker.

## How to use ?

```yml
name: Build

on:
  push:

permissions: 
  packages: write

jobs:
  generate-tags:
    name: Generate tags from version
    uses: lenra-io/github-actions/.github/workflows/generate-tags.yml@main
    with:
      version: 1.1.0
      template: ghcr.io/${{ github.repository }}/server:[:version:]

  build-docker:
    name: Build docker image
    needs: generate-tags
    uses: lenra-io/github-actions/.github/workflows/build-docker.yml@main
    with:
      tags: ${{ needs.generate-tags.outputs.tags }}
```
