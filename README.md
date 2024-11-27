# Auto Release Labeler and Note

[Japanese🇯🇵](./docs/README.jp.md)

## Overview
This workflow is a GitHub Actions setup that automates the release process. It automatically assigns labels to Pull Requests (PRs) targeting specific branches and generates release tags and notes upon merging. This automation aims to streamline release operations and ensure consistency.

Release tags are named based on the title of the labeled PR. For example, if the title of a PR with a label is `1.1.0`, a tag named `v1.1.0` will be created.

Release notes are generated as a bullet-point list, including all merged PRs and the names of contributors since the last release.

## Example
The following is a sample configuration designed for a GitHub Flow workflow. When you create a branch named `release/O.O.O` and open a PR targeting the `main` branch, a `release` label will be automatically assigned to the PR.

This makes it clear that the branch is intended for a release PR.

For the PR title, specify the version name, such as `3.1.1` or `4.1.3`.

When a PR with the release label is merged, a tag and release notes are automatically generated. If the PR title is `3.1.1`, the tag and release notes will be created as `v3.1.1`.

```yml
name: Release Management

on:
  pull_request:
    types:
      - opened
      - synchronize
      - closed
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  release-management:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
    
      - name: Auto Release Labeler and Note
        uses: KaitoMuraoka/Auto-Release-Labeler-and-Note@0.1.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          release_label: 'release'
          release_tag: ${{ github.event.pull_request.title }}
```