# PR-Guides-and-Automation

A repository for managing pull request templates, workflows, and automation scripts to streamline the development process and ensure code quality.

## Overview

This repository implements best practices for pull requests. It includes tools and configurations to enforce good pull request practices on GitHub.
Can read more about [here](https://www.conventionalcommits.org/en/v1.0.0/#specification) 

## Features

1. **Pull Request Template**: Guides contributors to provide essential information about their changes.
2. **Automatic Labeling**: Labels PRs based on size and affected directories.
3. **Semantic Title Validation**: Ensures PR titles follow conventional commit semantics.
4. **Automatic Reviewer Assignment**: Uses CODEOWNERS to assign reviewers based on changed files.

## Setup

1. Place the `pull_request_template.md` file in the `.github/` directory.
2. Add the `pr-labeler.yml` workflow to `.github/workflows/`.
3. Create a `labeler.yml` file in the `.github/` directory for directory-based labeling.
4. Add a `semantic.yml` file to `.github/` and install the Semantic Pull Request app from GitHub Marketplace.
5. (Optional) Add a `CODEOWNERS` file to `.github/` for automatic reviewer assignment.

## Usage

When creating a pull request:

1. Fill out the template provided in the PR description.
2. Ensure your PR title follows the semantic convention (e.g., "feat:", "fix:", etc.).
3. Keep PRs small (ideally under 250 lines of code).
4. Check the automatically applied labels and assigned reviewers.

## Files and Their Purposes

### `.github/pull_request_template.md`
**Purpose**: Provides a standardized template for pull requests.
**What it does**: Automatically populates the PR description with prompts for:
- What changes were made
- How the changes were implemented
- Link to related Jira ticket
- Code checklist (e.g., tested, documented)

### `.github/workflows/pr-labeler.yml`
**Purpose**: Automates the labeling of pull requests.
**What it does**: 
- Assigns labels based on the number of lines changed (XS, S, M, Too Large)
- Triggers the labeler action to apply directory-based labels

### `.github/workflows/pr-labeler.yml`
**Purpose**: Automates the labeling of pull requests.
**What it does**: 
- Assigns labels based on the number of lines changed (XS, S, M, Too Large)
- Triggers the labeler action to apply directory-based labels

Example file:
```instructions:
  - name: Create a new workflow file
    path: .github/workflows/pr-labeler.yml
    content: |
      name: PR Labeler

      on:
        pull_request:
          types: [opened, synchronize, reopened]

      jobs:
        label:
          runs-on: ubuntu-latest
          steps:
            - name: Checkout code
              uses: actions/checkout@v2

            - name: Run PR Labeler
              uses: actions/labeler@v2
              with:
                repo-token: ${{ secrets.GITHUB_TOKEN }}
                configuration-path: .github/labeler.yml
```

### `.github/labeler.yml`
**Purpose**: Configures directory-based labeling for pull requests.
**What it does**: Defines rules to apply specific labels based on which directories or files are changed in the PR.

### `.github/semantic.yml`
**Purpose**: Enforces semantic versioning in PR titles.
**What it does**: 
- Validates that PR titles follow the conventional commit format
- Configures which types of commits are allowed (e.g., feat, fix, docs)

### `.github/CODEOWNERS`
**Purpose**: Automatically assigns reviewers to pull requests.
**What it does**: Defines individuals or teams responsible for reviewing changes in specific parts of the codebase.
