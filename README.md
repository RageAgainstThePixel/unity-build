# Unity Build Pipeline

A GitHub Action that builds Unity based projects.

> **Warning**
>
> This action requires that your Unity project be setup and using the [com.utilities.buildpipeine](https://github.com/RageAgainstThePixel/com.utilities.buildpipeine) package from OpenUPM.

## How to use

```yaml
name: Unity Build

on:
  push:
    branches:
      - 'main'
  pull_request:
    branches:
      - '*'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

concurrency:
  group: ${{ github.ref }}
  cancel-in-progress: true

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        include:
          - os: windows-latest
            build-target: StandaloneWindows64
          - os: macos-latest
            build-target: StandaloneOSX
          - os: linux-latest
            build-target: StandaloneLinux64

    steps:
      - uses: actions/checkout@v3
        with:
          clean: true

      - name: validate editor installation
        uses: xrtk/unity-validate@v2

      - name: Unity Build (${{ matrix.build-target }})
        uses: RageAgainstThePixel/unity-build@v2
        with:
          build-target: ${{ matrix.build-target }}
```
