# flutter distributor Action

## Usage

Here a full usable example with a simple Android build CI:

```yaml
name: Release Apps

on:
  push:

jobs:

  build:
    name: Build ${{ matrix.platform }}
    strategy:
      matrix:
        platform: [ web, windows, linux, macos, android, ios ]
        include:
          - platform: web
            targets: zip
            runner: ubuntu-latest
            artifacts: dist/web/lenra*.zip
          - platform: linux
            targets: zip,deb,rpm,appimage
            runner: ubuntu-latest
            artifacts: dist/linux/lenra*.{zip,deb,rpm,AppImage}
          - platform: android
            targets: apk,aab
            runner: ubuntu-latest
            artifacts: dist/android/lenra*.{apk,aab}
          - platform: windows
            targets: zip,exe,msix
            runner: windows-latest
            artifacts: dist/windows/lenra*.{zip,exe,msix}
          - platform: macos
            targets: zip,dmg
            runner: macos-latest
            artifacts: dist/macos/lenra*.{zip,dmg}
          - platform: ios
            targets: ipa
            runner: macos-latest
            artifacts: dist/ios/lenra*.ipa
    runs-on: ${{ matrix.runner }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      
      - name: Build
        if: ${{ steps.get-version.outputs.release-published }}
        uses: lenra-io/github-actions/jobs/build-flutter@main
        with:
          target: android
      
      - name: Upload Artifacts
        if: ${{ steps.get-version.outputs.release-published }}
        uses: actions/upload-artifact@v2
        with:
          name: ${{ matrix.platform }}
          path: ${{ matrix.artifacts }}
  
  release:
    name: Release
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download Artifacts
        uses: actions/download-artifact@v2
        with:
          path: artifacts/
      
      - name: Create Release
        runs: |
          gh release create next-release \
            --title next-release \
            --notes next-release \
            --draft \
            --prerelease
            ./artifacts/*/*
```
