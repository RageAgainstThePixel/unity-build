# Unity Build Pipeline

A GitHub Action that builds Unity based projects.

> **Warning**
>
> This action requires that your Unity project be setup and using the [com.utilities.buildpipeine](https://github.com/RageAgainstThePixel/com.utilities.buildpipeine) package from OpenUPM

[![openupm](https://img.shields.io/npm/v/com.utilities.buildpipeline?label=openupm&registry_uri=https://package.openupm.com)](https://openupm.com/packages/com.utilities.buildpipeline/)

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
          - os: ubuntu-latest
            build-target: StandaloneLinux64

    steps:
      - uses: actions/checkout@v3

        # Installs the Unity Editor based on your project version text file
        # sets -> env.UNITY_EDITOR_PATH
        # sets -> env.UNITY_PROJECT_PATH
        # https://github.com/XRTK/unity-setup
      - uses: xrtk/unity-setup@v6
        with:
          build-targets: ${{ matrix.build-target }}

      - name: Unity Build (${{ matrix.build-target }})
        uses: RageAgainstThePixel/unity-build@v5
        with:
          build-target: ${{ matrix.build-target }}
```
