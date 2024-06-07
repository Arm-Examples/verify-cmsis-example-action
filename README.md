# verify-cmsis-example-action

This repository contains the GitHub action used to verify if a CMSIS project is compatible with CMSIS tools.

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/verify-cmsis-example-action@latest
  with:
    branch:       # Branch to get the project from if not `main`
    project-file: # Path to the project to verify (file with `.csolution.yml` extension etc.) (required)
    API_TOKEN:    # API token for publishing projects (required)
```

All inputs marked with `(required)` are required.

### Example usage

This action will attempt to extract some information from a project using CMSIS tools in order to validate whether a project is valid. If it passes then it should be uploadable to [keil.arm.com](https://keil.arm.com).

Any project verified with this action will be compatible with [Keil Studio Cloud](https://studio.keil.arm.com/).

```yaml
# ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  verify-project-action:
    name: Verify
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Arm-Examples/verify-cmsis-example-action@latest
        name: Verify
        with:
          branch: ${{ github.ref }}
          project-file: ./hello.csolution.yml
          API_TOKEN: ${{ secrets.KEIL_API_TOKEN }}
```

## API Token Access

Use of this GitHub Action requires an API token. To request a token for use in your own organization please contact any of the repository contributors.
