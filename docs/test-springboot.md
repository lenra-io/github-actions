# test-springboot.yml

Run tests and generate coverage report for a Spring Boot application in an artifact.

## How to use ? 

```yml
name: Build

on:
  push:

jobs:
  test:      
    name: Test Spring Boot App
    uses: lenra-io/github-actions/.github/workflows/test-springboot.yml@main
    with:
      artifact-name: coverage-report
```
