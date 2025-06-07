# Workflows

Here are some generic reusable workflows you can use in your projects.

Table of contents:

- [Workflows](#workflows)
  - [Generic](#generic)
    - [Define the next version for your apps based on commits](#define-the-next-version-for-your-apps-based-on-commits)
  - [Golang](#golang)
    - [Testing your code](#testing-your-code)
    - [Building and releasing Go applications](#building-and-releasing-go-applications)


## Generic

### Define the next version for your apps based on commits

This workflow grabs the latest tag, defines the next version based on the commits after it.

`LarsNieuwenhuizen/workflows/.github/workflows/define-version@main`

The outputs:

| Name             |  Type   | Description | Example |
|------------------|---------|-------------|---------|
| release-version  | string  | The next application version based on commits | v1.1.0 |

Example usage:

---

.github/workflows/version.yml

```yaml
name: My awesome workflow in which I need to define the next version of my app

on:
  workflow_dispatch:

jobs:
  version:
    uses: LarsNieuwenhuizen/workflows/.github/workflows/define-version.yml@main

  use-version:
    needs: version
    steps:
    - name: Echo the version
      run: |
        echo "${{ needs.version.outputs.release-version }}"
```

## Golang

### Testing your code

Test you go code with the test-go.yml workflow.

`LarsNieuwenhuizen/workflows/.github/workflows/test-go.yml@main`

The inputs

| Name         | Required | Type   | Default    | Description                              |
|--------------|----------|--------|------------|------------------------------------------|
| name         | no       | string | go-project | A name just for reference in the summary |
| go-version   | no       | string | 1.23       | The Golang version to use                |
| path         | no       | string | .          | The path to test, for module pkg use full path |

Example usage:

.github/workflows/test.yml

---

```yaml
name: Testing Go code

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
        path: "github.com/username/my-application/pkg/example"
        name: "Example"
```

### Building and releasing Go applications

Build and create a release your Go app.

This workflow defines the next version, builds the app for Linux and Mac and creates a release in your github repo!

`LarsNieuwenhuizen/workflows/.github/workflows/build-and-release-go.yml@main`

> [!IMPORTANT]
> This workflow creates a release in your Github repository, so it needs write permissions.
> Do not forget to add this in your workflow!
> Check the example -> permissions.contents = write

The inputs

| Name         | Required | Type   | Default    | Description                              |
|--------------|----------|--------|------------|------------------------------------------|
| app-name     | no       | string | app        | The name of your application binary that will be created |
| go-version   | no       | string | 1.23       | The Golang version to use                |
| build-file   | no       | string | main.go    | The default file which will be used in the build command |

Example usage:

---

.github/workflows/build-release.yml

```yaml
name: Build and Release

on:
  pull_request:
    types:
      - closed

permissions:
  contents: write

jobs:
  build-release:
    if: github.event.pull_request.merged == true
    uses: LarsNieuwenhuizen/workflows/.github/workflows/build-and-release-go.yml@main
```
