# Build, Deploy to GitHub Pages and Deploy PR Preview Action

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![Release version badge](https://img.shields.io/github/v/release/chvmvd/build-deploy-and-preview-action.svg?logo=github)](https://github.com/chvmvd/build-deploy-and-preview-action/releases)
[![license](https://img.shields.io/badge/license-MIT-informational.svg)](LICENSE)
![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)

GitHub Action that automatically builds your project, deploys to GitHub Pages and creates a PR preview.

## Table of Contents

- [Build, Deploy to GitHub Pages and Deploy PR Preview Action](#build-deploy-to-github-pages-and-deploy-pr-preview-action)
  - [Table of Contents](#table-of-contents)
  - [About](#about)
  - [Usage](#usage)
  - [Configuration](#configuration)
  - [License](#license)
  - [Contributing](#contributing)

## About

This action will automatically build your project, deploy to GitHub Pages and create a PR preview.

## Usage

Usage is very simple and can be done with the following two steps.

First, you need to create a workflow file in your repository. For example, if you want to use this action with a Vite project, you can create a file called `build-deploy-and-preview.yml` in the `.github/workflows` directory of your repository with the following contents:

```yaml
name: Build, Deploy to GitHub Pages and Deploy PR Preview

on:
  push:
    branches: [main, master]
  pull_request:
    types:
      - opened
      - reopened
      - synchronize
      - closed

permissions:
  contents: write
  pull-requests: write

concurrency: ci-${{ github.ref }}

jobs:
  build-deploy-and-preview:
    name: Build, Deploy to GitHub Pages and Deploy PR Preview
    runs-on: ubuntu-latest
    steps:
      - name: Build, Deploy to GitHub Pages and Deploy PR Preview
        uses: chvmvd/build-deploy-and-preview-action@v1.0.0
        with:
          type: vite
```

Second, you need to ensure that your GitHub repository is configured to use GitHub Pages. You can do this by going to the `Settings` tab of your repository and selecting the `Pages` section. You can then select the branch that you want to use for GitHub Pages(The `gh-pages` is used by convention and is used by this action by default).

## Configuration

The following configuration options are available:

| Name                | Description                                                                                                              | Required | Default      |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------ | -------- | ------------ |
| `type`              | The type of project you want to build(currently only `vite` is supported).                                               | `true`   |              |
| `rootDir`           | The root directory of your project.                                                                                      | `false`  | `.`          |
| `folder`            | The folder that contains the built files(If set to `auto`, it will use the default folder for the type of the project.). | `false`  | `auto`       |
| `pr-preview`        | Whether to create a PR preview or not.                                                                                   | `false`  | `true`       |
| `deployment-branch` | The branch to use for GitHub Pages.                                                                                      | `false`  | `gh-pages`   |
| `umbrella-dir`      | The directory that will contain all PR previews.                                                                         | `false`  | `pr-preview` |
| `custom-url`        | The custom URL to use.                                                                                                   | `false`  | `""`         |

## License

This project is licensed under the terms of the [MIT license](LICENSE).

## Contributing

Issues and pull requests are always welcome.
