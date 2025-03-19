# build-flutter.yml

## Example of usage

```
name: Build

on:
  push:
jobs:
  build:
    name: Build
    uses: lenra-io/github-actions/.github/workflows/build-flutter.yml@main
    strategy:
      matrix:
        platform: [ android, ios ]
        include:
          - platform: web
            targets: zip
            runner: ubuntu-latest
            artifacts: dist/web/*.zip
            flutter-extra-args: release
          - platform: linux
            targets: zip,deb,rpm,appimage
            runner: ubuntu-latestv
            artifacts: dist/linux/*.{zip,deb,rpm,AppImage}
            flutter-extra-args: release
          - platform: android
            targets: aab
            runner: ubuntu-latest
            artifacts: dist/android/*.{aab}
            flutter-extra-args: release
          - platform: android
            targets: apk
            runner: ubuntu-latest
            artifacts: dist/android/*.{apk}
            flutter-extra-args: release,split-per-abi
          - platform: windows
            targets: zip,exe,msix
            runner: windows-latest
            artifacts: dist/windows/*.{zip,exe,msix}
            flutter-extra-args: release
          - platform: macos
            targets: zip,dmg
            runner: macos-latest
            artifacts: dist/macos/*.{zip,dmg}
            flutter-extra-args: release
          - platform: ios
            targets: ipa
            runner: macos-latest
            artifacts: dist/ios/*.ipa
            flutter-extra-args: release
    with:
      version: 1.0.0
      build: ${{ github.run_id }}
      platform: ${{ matrix.platform }}
      targets: ${{ matrix.targets }}
      runner: ${{ matrix.runner }}
```
