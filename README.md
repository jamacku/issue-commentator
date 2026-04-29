<!-- markdownlint-disable MD033 MD041 -->
<p align="center">
  <img src="https://github.com/redhat-plumbers-in-action/team/blob/70f67465cc46e02febb16aaa1cace2ceb82e6e5c/members/black-plumber.png" width="100" />
  <h1 align="center">Issue Commentator</h1>
</p>

[![GitHub Marketplace][market-status]][market] [![Lint Code Base][linter-status]][linter] [![Unit Tests][test-status]][test] [![CodeQL][codeql-status]][codeql] [![Check dist/][check-dist-status]][check-dist]

[![codecov][codecov-status]][codecov]

<!-- Status links -->

[market]: https://github.com/marketplace/actions/issue-commentator
[market-status]: https://img.shields.io/badge/Marketplace-Issue%20Commentator-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=

[linter]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/lint.yml
[linter-status]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/lint.yml/badge.svg

[test]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/unit-tests.yml
[test-status]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/unit-tests.yml/badge.svg

[codeql]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/codeql-analysis.yml
[codeql-status]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/codeql-analysis.yml/badge.svg

[check-dist]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/check-dist.yml
[check-dist-status]: https://github.com/redhat-plumbers-in-action/issue-commentator/actions/workflows/check-dist.yml/badge.svg

[codecov]: https://codecov.io/gh/redhat-plumbers-in-action/issue-commentator
[codecov-status]: https://codecov.io/gh/redhat-plumbers-in-action/issue-commentator/branch/main/graph/badge.svg

<!-- -->

Issue Commentator is a GitHub Action that creates and updates status comments on issues and pull requests. It uses hidden metadata to track previously posted comments, ensuring that repeated runs update the existing comment instead of creating duplicates.

## Features

* Automatically creates comments on GitHub issues and pull requests
* Tracks previously posted comments using hidden metadata in the issue body -- repeated runs update the existing comment instead of posting duplicates
* Skips unnecessary API calls when the comment content has not changed
* Supports multi-section messages -- each line of the `message` input becomes a separate section divided by horizontal rules (`---`)

## Usage

```yml
name: Issue Comment
on:
  pull_request:
    types: [ opened, reopened, synchronize ]

permissions:
  contents: read

jobs:
  comment:
    runs-on: ubuntu-latest

    permissions:
      issues: write
      pull-requests: write

    steps:
      - name: Comment on Pull Request
        uses: redhat-plumbers-in-action/issue-commentator@v1
        with:
          issue: ${{ github.event.pull_request.number }}
          message: |
            Hello from Issue Commentator!
          token: ${{ secrets.GITHUB_TOKEN }}
```

> [!NOTE]
>
> The `token` must have permission to create and update comments on the target issue or pull request. When using `secrets.GITHUB_TOKEN`, ensure the job has `issues: write` and/or `pull-requests: write` permissions.

## Configuration options

Action currently accepts the following options:

```yml
# ...

- uses: redhat-plumbers-in-action/issue-commentator@v1
  with:
    issue:    <number>
    message:  <string>
    token:    <GitHub token or PAT>

# ...
```

### issue

Number of the issue or pull request where the comment should be posted.

* default value: `undefined`
* requirements: `required`

### message

Content of the comment. Supports multi-line input where each non-empty line becomes a separate section in the final comment, divided by horizontal rules (`---`). Individual lines can optionally be JSON strings which will be parsed before composing the final message.

* default value: `undefined`
* requirements: `required`

### token

GitHub token or PAT used for creating and updating comments.

```yml
# required permissions
permissions:
  issues: write
  pull-requests: write
```

* default value: `undefined`
* requirements: `required`
* recomended value: `secrets.GITHUB_TOKEN`
