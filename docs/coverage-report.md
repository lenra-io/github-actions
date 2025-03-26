# coverage-report.yml

## Example of usage

```yml
name: Build

on:
  push:

jobs:
  test:
    name: Test Flutter App
    uses: lenra-io/github-actions/.github/workflows/test-flutter.yml@main
  coverage:
    name: Read Coverage Report
    needs: test
    uses: lenra-io/github-actions/.github/workflows/coverage-report.yml@main
    with:
      with-artifact-name: ${{ needs.test.outputs.artifact-name }}
      with-artifact-path: lcov.info
      min-lines-coverage: 20
```
