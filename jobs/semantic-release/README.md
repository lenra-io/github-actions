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

    steps:
      - name: Checkout Code
        uses: actions/checkout@3

      # Run the Semantic release action to get the version in read-only mode (dry-run) So this won't create a tag
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
        if: ${{ steps.get-version.outputs.release-published }}
        uses: docker/build-push-action@v4
        with: # We can use here the outputs of the previous semantic release job to define the tag to publish
          push: true # In that case, if it's `false` the job run for nothing
          tags: user/app:v${{ steps.get-version.outputs.release-version }}

      - name: Release
        if: ${{ steps.get-version.outputs.release-published }}
        uses: https://github.com/lenra-io/github-actions/jobs/semantic-release@main
        with:
          release-branches: 'stable'
          prerelease-branches: 'beta,alpha'
```
