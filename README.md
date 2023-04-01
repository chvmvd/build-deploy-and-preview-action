# Build, Deploy to GitHub Pages and Deploy PR Preview Action

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![Release version badge](https://img.shields.io/github/v/release/chvmvd/build-deploy-and-preview-action.svg?logo=github)](https://github.com/chvmvd/build-deploy-and-preview-action/releases)
[![license](https://img.shields.io/badge/license-MIT-informational.svg)](LICENSE)
![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)
[![GitHub Marketplace badge](https://img.shields.io/badge/action-marketplace-blue.svg?logo=github&color=orange)](https://github.com/marketplace/actions/build-deploy-to-github-pages-and-deploy-pr-preview)

GitHub Action that automatically builds your project, deploys it to GitHub Pages and creates a PR preview.

## Table of Contents

- [Build, Deploy to GitHub Pages and Deploy PR Preview Action](#build-deploy-to-github-pages-and-deploy-pr-preview-action)
  - [Table of Contents](#table-of-contents)
  - [About](#about)
  - [Usage](#usage)
    - [Vite](#vite)
    - [Docusaurus](#docusaurus)
  - [Configuration](#configuration)
  - [License](#license)
  - [Contributing](#contributing)

## About

This action will automatically build your project, deploy it to GitHub Pages and also create a PR preview.

## Usage

Usage is very simple and can be done with just a few steps.

### Vite

If you are using Vite, it is very easy to use this action. This action will automatically configure the base URL for you.

First, you need to create a workflow file in your repository. For example, you can create a file called `build-deploy-and-preview.yml` in the `.github/workflows` directory of your repository with the following contents:

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
        uses: chvmvd/build-deploy-and-preview-action@v1.2.0
        with:
          type: vite
```

Second, you need to ensure that your GitHub repository is configured to use GitHub Pages. You can do this by going to the `Settings` tab of your repository and selecting the `Pages` section. You can then select the branch that you want to use for GitHub Pages(The `gh-pages` is used by convention and is used by this action by default).

<img width="759" alt="capture of the `Pages` section" src="https://user-images.githubusercontent.com/104971044/229016748-96f21ec8-242e-4a74-a8a5-ed5576f00347.png">

### Docusaurus

First, since GitHub Pages deploys your project to `https://<owner>.github.io/<repo>`, you have to configure the base URL for your project.

You can do this by editing the `docusaurus.config.js` file in the root directory of your project.

Inside `docusaurus.config.js`, add the following line to the `baseUrl` property of the `config` object(This action provides the `BASE_URL` environment variable that contains the base URL of your project):

```js
baseUrl: process.env.GITHUB_ACTIONS ? `${process.env.BASE_URL}/` : "/",
```

Second, you need to create a workflow file in your repository. For example, you can create a file called `build-deploy-and-preview.yml` in the `.github/workflows` directory of your repository with the following contents:

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
        uses: chvmvd/build-deploy-and-preview-action@v1.2.0
        with:
          type: docusaurus
```

Third, you need to ensure that your GitHub repository is configured to use GitHub Pages. Please refer to the [Vite](#vite) section for more information.

## Configuration

The following configuration options are available:

| Name                 | Description                                                                                                                                                                                                                 | Required | Default            |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------ |
| `type`               | The type of your project. (Other settings will be automatically set for the type of the project. Currently only `vite` and `docusaurus` are supported.)                                                                     | `true`   |                    |
| `package-manager`    | The package manager that will be used to install dependencies. (`npm`, `yarn` or `pnpm`)                                                                                                                                    | `false`  | `npm`              |
| `rootDir`            | The root directory of your project. (Default is the current directory.)                                                                                                                                                     | `false`  | `.`                |
| `folder`             | The folder that contains the built files. (If set to `auto`, it will use the default folder for the type of the project.)                                                                                                   | `false`  | `auto`             |
| `pr-preview`         | Whether to create a PR preview or not.                                                                                                                                                                                      | `false`  | `true`             |
| `production-branch`  | The branch that contains the production code. (Default is `main` or `master`)                                                                                                                                               | `false`  | `main` or `master` |
| `development-branch` | The branch that contains the development code. It will be deployed to `https://<owner>.github.io/<repo>/<branch>`. (This can be multiple branches. If you want to specify multiple branches, separate them with a newline.) | `false`  |                    |
| `deployment-branch`  | The branch that the project will be deployed to.                                                                                                                                                                            | `false`  | `gh-pages`         |
| `umbrella-dir`       | The directory that will contain all PR previews.                                                                                                                                                                            | `false`  | `pr-preview`       |
| `custom-url`         | The custom URL to deploy.                                                                                                                                                                                                   | `false`  |                    |

## Roadmap

Currently, only Vite and Docusaurus are supported. I will add support for other frameworks in the future.

## License

This project is licensed under the terms of the [MIT license](LICENSE).

## Contributing

Issues and pull requests are always welcome.
