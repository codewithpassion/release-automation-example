# GitHub Workflow Files

This repository contains a set of GitHub workflow files that automate various processes related to versioning, pre-releases, and pull request management. Below is an explanation of each workflow file and its purpose.

These workflows work together to automate versioning, create pre-releases, handle pull requests, and perform actions based on specific comments on pull requests.

Developers can customize and extend these workflows based on their specific requirements. The workflows can be triggered manually or automatically based on specific events such as creating a pre-release or commenting on a pull request.

## TL;DR

### Manual execution
- `manual-pre-release.yml`: Creates a pre-release.

### Automatic actions based on events
- `on-pr-comment.yml`: Triggers when a comment is created on a pull request.

### Helper workflows
- `wc-build-and-release.yml`: Builds the code on a pre-release.
- `wc-check-for-pull-request.yml`: Checks for a pull request.
- `wc-version.yml`: Handles git versioning.


## wc-version.yml

The `wc-version.yml` workflow is responsible for handling git versioning. It can be called by other workflows and accepts the following inputs:

- `release-branch`: The branch to compare against (default: "main").
- `prefix`: The prefix to use for the version (default: "v").
- `log-paths`: The path to the sub-module (default: "./").

The workflow outputs the new version and the previous version.

## manual-pre-release.yml

The `manual-pre-release.yml` workflow is triggered manually and creates a pre-release. It performs the following steps:

1. Gets the version using the `wc-version.yml` workflow.
2. Checks for the existence of a pull request using the `check-for-pull-request.yml` workflow.
3. Creates a pre-release tag and updates the release body based on whether it's a pull request or a branch.
4. Executes the `wc-build-and-release.yml` workflow to perform additional actions on the pre-release.

## wc-check-for-pull-request.yml

The `wc-check-for-pull-request.yml` workflow checks if a pull request exists for a given branch. It accepts the branch name as an input and outputs whether a pull request exists and the pull request number.

## build-on-pre-release.yml

The `build-on-pre-release.yml` workflow is triggered when a pre-release is created. It performs the following steps:

1. Checks out the code using the pre-release tag.
2. Runs a build script (`./ops/scripts/build.sh`).
3. Updates the release body with additional information.
4. Adds a reaction to the release.

## on-pr-comment.yml

The `on-pr-comment.yml` workflow is triggered when a comment is created on a pull request. It checks if the comment contains the word "release" and the pull request is open. If both conditions are met, it adds a reaction to the comment.

