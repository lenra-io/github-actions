# Docker Buildx Action

## Usage

Here a full usable example to build and push a docker image : 

```yaml
name: Build docker

on:
  push:
    tags:
      - '*'

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@3
      - id: versions
        name: PR is WIP
        uses: https://github.com/lenra-io/github-actions/jobs/full-version-to-list-of-versions@main
        with:
          version: ${{ github.ref_name }}
          prefix: 'user/app:'
          separator: ','
          use-latest: 'true'
      - name: Build docker images
        uses: https://github.com/lenra-io/github-actions/jobs/docker-buildx@main
        with:
          tags: ${{ steps.versions.outputs.versions }}
```