# bootstrap-dotnet
This is a GitHub Action meant to be used as a [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) within an existing workflow. This action encapsulates setting up .NET SDK, MSBuild path (on Windows only), and NuGet CLI in one step.  Each is optional, but I'd argue if you are only using one of these then just use the actual action.

The action encapsulates the following other actions:

- [actions/setup-dotnet](https://github.com/actions/setup-dotnet)
- [microsoft/setup-msbuild](https://github.com/microsoft/setup-msbuild)
- [NuGet/setup-nuget](https://github.com/NuGet/setup-nuget)
- [darenm/Setup-VSTest](https://github.com/darenm/Setup-VSTest)

## Usage
You can use this composite Action in your own workflow by adding:

```yml
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - uses: actions/checkout@v2
      - id: foo
        uses: timheuer/bootstrap-dotnet@v1
        with:
          dotnet-version: 6.0.x
      - run: echo random-number ${{ steps.foo.outputs.random-number }}
        shell: bash
```

### Defaults
This action uses the following defaults:

- dotnet-version: 6.0.x
- MSBuild: latest (meaning the latest version installed on an agent)
- NuGet: latest (meaning the latest CLI version)
- VSTest: false (meaning VSTest.Console is not setup for you)

### Options
You can disable any step of these by opting out in your usage.

Example, to skip NuGet setup

```yml
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - uses: actions/checkout@v2
      - id: foo
        uses: timheuer/bootstrap-dotnet@v1
        with:
          dotnet-version: 6.0.x
          nuget: 'false'
      - run: echo random-number ${{ steps.foo.outputs.random-number }}
        shell: bash
```