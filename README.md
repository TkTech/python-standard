# python-standard

Re-usable github workflows for a typical pure-python project flow. These
are only meant to work with a specific project layout and may not work for
your project.

## Usage

Example project layout:

```
.
├── .github
│ └── workflows
│     ├── release.yml
│     └── test.yml
├── docs
│ ├── conf.py
│ ├── index.rst
│ └── makefile
├── <project_name>
│ └── __init__.py
├── pyproject.toml
```

Example github action workflow for running tests:

```yaml
name: Tests

on: [push]

jobs:
  test:
    name: Running tests.
    uses: tktech/python-standard/.github/workflows/test.yml@v1
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.8", "3.9", "3.10", "3.11", "3.12", pypy3.9]
    with:
      use_poetry: true
      use_sphinx: true
      use_black: true
      python_version: ${{ matrix.python-version }}
```

You can also use the release workflow, which will build and publish your
package to pypi. Optionally, it'll also build and publish your documentation
to github pages. Make sure you have a ``PYPI_TOKEN`` secret set up in your
repository secrets:

```yaml
on:
  workflow_dispatch:
  release:
    types:
      - published
name: Release
jobs:
  build:
    permissions:
      id-token: write
      pages: write
    uses: tktech/python-standard/.github/workflows/release.yml@v1
    with:
      use_poetry: true
      use_sphinx: true
      python_version: '3.11'
      platform: 'ubuntu-latest'
    secrets:
      PYPI_TOKEN: ${{ secrets.PYPI_TOKEN }}
```

This example will build and publish your package to pypi when you create &
publish a release on github.

## Why?

Because I have far too many Python projects and github deprecates things far
too often.
