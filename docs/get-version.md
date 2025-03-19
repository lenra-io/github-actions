# get-version.yml

Determine the version of the project based on the latest commit and generate a changelog using the Rust Convco cli tool.

## Example of usage

Create a `.github/workflows/build.yml`:

```yml
name: Build

on:
  push:

jobs:
  get-version:
    name: Get Version
    uses: lenra-io/github-actions/.github/workflows/get-version.yml@main
```
