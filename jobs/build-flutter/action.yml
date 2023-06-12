# flutter build Action

## Usage

Here a full usable example with a simple Android build CI:

```yaml
name: Release Android App

on:
  push:

jobs:

  release:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@3


      - name: Build
        if: ${{ steps.get-version.outputs.release-published }}
        uses: lenra-io/github-actions/jobs/flutter-build@main
        with:
          target: android
```
