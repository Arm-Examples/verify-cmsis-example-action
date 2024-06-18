# Verify CMSIS example action

This repository contains the GitHub action used to verify if a CMSIS project is compatible with [CMSIS ecosystem](https://www.keil.arm.com/cmsis).

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/verify-cmsis-example-action@latest
  with:
    branch:            # Branch to get the project from if not `main`
    project-file:      # Path to the project (relative to the project-directory) to verify (e.g. file in the repository with `.csolution.yml` extension, etc.) (required)
    project-directory: # Path to the project directory (defaults to the root of the GitHub repository). This should be used if the repository contains several independent projects
    output-artifact:   # Optional name override for the generated artifacts. Needed if running the action multiple times in one run so that artifacts don't conflict with eachother
    API_TOKEN:         # API token to access Arm Online services (required)
```

All inputs marked with `(required)` are required.

### Example usage

This action will attempt to extract some information from a project using [CMSIS tools](https://github.com/Open-CMSIS-Pack/devtools) in order to validate whether a project is valid or not according to CMSIS specification. If it passes, then the project will be compatible with any CMSIS tools including [CMSIS toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox), KEIL MDK v6, [Keil Studio Cloud](https://studio.keil.arm.com/cmsis).

Hereafter is an example of a github action workflow which can be included in any CMSIS repository for Continuous Integration check before merge.
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
          API_TOKEN: ${{ secrets.CMSIS_API_KEY }}
```

## API Token Access

Use of this GitHub Action requires an API key. To request a token for use in your own organization, please contact any of the repository contributors or support@arm.com.
