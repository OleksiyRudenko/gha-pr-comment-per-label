# Autoadd a comment to a PR - 

**a GitHub action**

This [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
helps to add comments from predefined files based on labels assigned to a PR.

Define labels that trigger the action, comment source files and add this action to a workflow.

Commenting on a PR requires the always-available GitHub Token `{{ secrets.GITHUB_TOKEN }}`.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Inputs](#inputs)
- [Outputs](#outputs)
- [Usage Example](#usage-example)
- [Versions](#versions)
  - [What version to use?](#what-version-to-use)
  - [v1](#v1)
- [Action Users](#action-users)
- [License](#license)
- [No affiliation with GitHub Inc.](#no-affiliation-with-github-inc)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- generated with [DocToc](https://github.com/thlorenz/doctoc) -->

## Inputs

| property name  | value type | default value            | required | description |
| ---            | ---        | ---                      | ---      | ---         |
| `labels`       | string     | **n/a**                  | true     | an array of triggering labels as a stringified JSON    |
| `source_repo`  | string     | `'target'`               | false    | repository holding source files; defaults to PR origin |
| `source_token` | string     | `''`                     | false    | a token from an unrelated repo to pick files from it   |
| `source_path`  | string     | `'.github/pr-messages/'` | false    | path to messages source files   |
| `file_ext`     | string     | `'.md'`                  | false    | message file extension          |
| `token`        | string     | **n/a**                  | true     | required to post comments       |

**Notes:**
- `labels`: GitHub actions API doesn't allow to pass arrays or lists
  down to the actions, therefore a list must be represented by a stringified JSON.
  The parameter must be put in single quotes. Use double quotes for strings inside JSON string.
- `source_repo`: a repository that holds source files.
  - If value is `'target'` then replaced with target PR repo qualified name.
  - If value is an empty string (`''`) then PR sourcing repo is used (can be a forked repo).
  - Unrelated repo should be referred to as `'orgname/reponame'`.
- `source_path`: path to message source files on a source repo.
- `source_token`: pulling files from an unrelated repo requires token from that repo.
  The token should be registered on PR target repository.
- The minimally required action configuration requires a token being explicitly specified.
  Example below uses secrets.GITHUB_TOKEN available to the workflow as a token source.

```yml
- uses: oleksiyrudenko/gha-pr-comment-per-label@v1-latest
  with:
    labels: '["bug","release"]'
    token: '${{ secrets.GITHUB_TOKEN }}'
```

## Outputs

No outputs produced.

## Usage Example

This example 

```yaml
name: Comment on PR based on labels
on:
  pull_request_target:
    types: [ labeled ]
    branches:
      - master
      - main
jobs:
  add-pr-checklist:
  runs-on: ubuntu-latest
    steps:
      - name: Add PR Comment
        uses: oleksiyrudenko/gha-pr-comment-per-label@v1-latest
        with:
          token: '${{ secrets.GITHUB_TOKEN }}'
          labels: '["bug","release"]'
          source_repo: 'myorg/templates'
          source_token: ${{secrets.GH_PAT_MYORG_TEMPLATES}}
```

## Versions

### What version to use?

In workflow `.yml` file you are required to specify git ref (tag, branch, commit sha) to pick a specific version of action.
E.g. `uses: oleksiyrudenko/gha-pr-comment-per-label@v1`.
`@v1` here refers to a very specific version of the action script (a git tag in this case).

So, in this action the options are:
- `vX`, `vX.Y` - specific version
- `vX-latest` - latest release within the major version
  (no breaking changes, backward compatibility)
- `latest` - latest release, major version change may occur
- `<branch-name>` - version as per given branch name, useful for testing 

Whenever non-breaking changes introduced, having backward compatibility secured,
a patch version is released and the relevant `vX-latest` tag is moved to point
at the latest release within current major version.

Using `vX-latest` is a recommended choice.

Below are key features of the releases.

### v1

Features:
- Map labels against comment source file names, adding customizable file extension
- Pick a source file to comment from a PR target or origin repo or from an arbitrary repo
- Post a comment on a PR

## Action Users

As of July 27, 2022 (before publishing the action on Marketplace)
there are no registered users of the action yet.

## License

Scripts and documentation in this project are released under the [MIT license](LICENSE).

## No affiliation with GitHub Inc.

GitHub are registered trademarks of GitHub, Inc.
GitHub name used in this project are for identification purposes only.
The project is not associated in any way with GitHub Inc.
and is not an official solution of GitHub Inc.
It was made available in order to facilitate the use of the site GitHub.
