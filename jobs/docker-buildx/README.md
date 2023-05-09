# Docker Buildx Action

## Usage

Here a full usable example to use it : 

```yaml
name: Build docker

on: push

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@3

      - name: Build docker images
        uses: https://github.com/lenra-io/github-actions/jobs/docker-buildx@main
        with:
          tags: user/app:latest
```