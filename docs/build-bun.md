# build-bun.yml

Build a Bun application and package it into an artifact.

It will run `bun run build` command but with cache and automatic bun setup.

## How to use

```yml
name: Build

on:
  push:

jobs:
  build:
    name: Build Bun App
    uses: lenra-io/github-actions/.github/workflows/build-bun.yml@main
    with:
      working-directory: app
      bun-version: latest
      artifact-name: app
      artifact-path: dist/
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `working-directory` | Directory containing the Bun app | No | `.` |
| `bun-version` | Version of Bun to use | No | `latest` |
| `artifact-name` | Name for the build artifact | No | - |
| `artifact-path` | Path to files to include in artifact | No | `build/libs/*.jar` |
