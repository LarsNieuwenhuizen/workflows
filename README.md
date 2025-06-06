# Workflows

Here are some generic reusable workflows you can use in your projects.

## Golang

### Testing your code

Test you go code with the test-go.yml workflow.

The inputs

| Name         | Required | Type   | Default    | Description                              |
|--------------|----------|--------|------------|------------------------------------------|
| project-name | no       | string | go-project | A name just for reference in the summary |
| go-version   | no       | string | 1.23       | The Golang version to use                |
| path         | no       | string | .          | The path to test, for module pkg use full path |

Example:

```yaml
name: Testing code

on:
  workflow_dispatch:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened
    branches:
      - main
  push:
    branches:
      - main

jobs:
    test:
      uses: LarsNieuwenhuizen/workflows/.github/workflows/test-go.yml@main
      with:
        path: "."
```
