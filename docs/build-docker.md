# build-docker.yml

Build a Docker image with a prebuild artifact included.

# How to use ?

```yml
build: [...]

generate-version-names:
    name: Generate version names
    uses: lenra-io/github-actions/.github/workflows/generate-version-names.yml@main
    with:
      version: 1.0.0
      template: ghcr.io/${{ github.repository }}/server:[:version:]

build-docker:
    name: Build docker image
    needs: [generate-version-names, build]
    uses: lenra-io/github-actions/.github/workflows/build-docker.yml@main
    with:
      working-directory: server
      tags: ${{ needs.generate-version-names.outputs.tags }}
      dockerfile: ci.Dockerfile
      with-artifact-name: server
      with-artifact-path: build/libs/
```
