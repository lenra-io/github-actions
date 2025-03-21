# test-flutter.yml

## How to use 

```yml
name: Build

on:
  push:

jobs:
  test:
    name: Test Flutter App
    uses: lenra-io/github-actions/.github/workflows/test-flutter.yml@main
```

This export `artifact-name` output with the name of an artifact that will contain an `lcov.info` file.
