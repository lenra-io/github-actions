# build-springboot.yml

Build a Spring Boot application and generate a JAR artifact.

## How to use ?

```yml
name: Build

on:
  push:

jobs:
  build:
    name: Build Spring Boot App
    uses: lenra-io/github-actions/.github/workflows/build-springboot.yml@main
    with:
      working-directory: server
      artifact-name: server
      envs: |
        VERSION='1.0.0'
    secrets: 
      envs: | 
        SENTRY_AUTH_TOKEN=${{ secrets.SENTRY_AUTH_TOKEN }}
```
