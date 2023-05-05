# Semantic release Action

## Usage

Here a full usable example with a simple Docker CI : 

```yaml
name: Release Docker

on:
  push:

jobs:

  release:
    name: Release
    runs-on: ubuntu-latest

    outputs:
      release-published: ${{ steps.get-version.outputs.release-published }}
      release-version: ${{ steps.get-version.outputs.release-version }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@3

      - id: get-version
        name: release
        uses: https://github.com/lenra-io/github-actions/jobs/semantic-release@main
        with:
          dry-run: 'true'
          release-branches: 'stable'
          prerelease-branches: 'beta,alpha'

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          push: ${{ steps.get-version.outputs.release-published }}
          tags: user/app:v${{ steps.get-version.outputs.release-version }}
      
      - name: Release
        if: ${{ steps.get-version.outputs.release-published }}
        uses: https://github.com/lenra-io/github-actions/jobs/semantic-release@main
        with: 
          release-branches: 'stable'
          prerelease-branches: 'beta,alpha'
```