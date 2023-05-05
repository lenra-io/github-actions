# Semantic release Action

## Usage

Here a full usable example with a rust CI : 

```yaml
name: Build Rust

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
          release-branches: 'stable'
          prerelease-branches: 'beta,alpha'

      - name: Install rust
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          profile: minimal
          override: true
          target: x86_64-unknown-linux-musl

      - name: Install cargo-edit
        uses: actions-rs/cargo@v1
        with:
          command: install
          args: cargo-edit

      - name: Set version
        uses: actions-rs/cargo@v1
        with:
          command: set-version
          args: "${{ steps.get-version.outputs.release-version }}"

      - name: Build target
        uses: actions-rs/cargo@v1
        with:
          use-cross: true
          command: build
          args: --release --target x86_64-unknown-linux-musl

      - name: Zip
        shell: bash
        run: tar -C "target/x86_64-unknown-linux-musl/release" -czf "lenra-linux-x86_64.tar.gz" "lenra"

      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: lenra-linux-x86_64
          path: lenra-linux-x86_64.tar.gz

  release:
    name: Release
    runs-on: ubuntu-latest
    needs: [ build ]

    defaults:
      run:
        working-directory: ${{ inputs.working-directory }}

    environment: ${{ inputs.env || 'dev' }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@3

      - name: Download artifacts
        uses: actions/download-artifact@v3
        with:
          path: artifacts/

      - name: Get version
        run: |
          echo "Publishing version: ${{ needs.build.outputs.release-version }}"

      - name: Release
        uses: https://github.com/lenra-io/github-actions/jobs/semantic-release@main
        with: 
          release-branches: 'stable'
          prerelease-branches: 'beta,alpha'
          assets: '{"path":"artifacts/*/*"}'
```