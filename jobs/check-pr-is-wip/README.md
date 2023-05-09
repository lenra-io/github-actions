# Docker Buildx Action

## Usage

Here a full usable example to build and push a docker image : 

```yaml
name: Build docker

on:
  workflow_call:
    inputs:
      # pass in environment through manual trigger, if not passed in, default to 'dev'
      env:
        required: true
        type: string
        default: 'dev'
      # working-directory is added to accommodate monorepo.  For multi repo, defaults to '.', current directory
      working-directory:
        required: false
        type: string
        default: '.'

jobs:

  build:
    name: Build
    runs-on: ubuntu-latest

    defaults:
      run:
        working-directory: ${{ inputs.working-directory }}

    # important to specify the environment here so workflow knows where to deploy your artifact to.
    # default environment to "dev" if it is not passed in through workflow_dispatch manual trigger
    environment: ${{ inputs.env || 'dev' }}

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

      - id: get-version
        name: release
        uses: https://github.com/lenra-io/github-actions/jobs/docker-buildx@main
        with:
          tags: user/app:${{ steps.get-version.outputs.release-version }}

```