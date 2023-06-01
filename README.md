# github-actions
GitHub Repository to reduce workflows for all our repository

## Table of contents

???


## Workflows

### Release workflows

Theses workflows will help you to releases your application's new tag based on commit and PR that was included in it.

#### Semantic-Release

It'll use Semantic-Release node tool to parse your commits messages and generate the new tags without any artifacts to upload.

Simple but useful for libs you can include using the git tag as versionning info.

To use it just past the content of this into your `.github/workflows/release.yml` file in your repository :

```yaml
name: Release
on:
  push:
    branches:
      - main
      - rc
      - beta
      - alpha
      - "*.x"
jobs:
  release:
    uses: lenra-io/github-actions/.github/workflows/release.yml@2-task-add-semantic-release
    secrets:
      token: ${{ secrets.WORKFLOW_GITHUB_TOKEN }}
```

## Jobs

Links to the jobs documentation. Clic on the link on the tools you need to have more info.

### Release Jobs

- [semantic-release](jobs/semantic-release/README.md)
