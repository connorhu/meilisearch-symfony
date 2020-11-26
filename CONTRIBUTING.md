# Contributing

First of all, thank you for contributing to MeiliSearch! The goal of this document is to provide everything you need to know in order to contribute to MeiliSearch and its different integrations.

<!-- MarkdownTOC autolink="true" style="ordered" indent="   " -->

- [Assumptions](#assumptions)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Git Guidelines](#git-guidelines)

<!-- /MarkdownTOC -->

## Assumptions

1. **You're familiar with [GitHub](https://github.com) and the [Pull Request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) (PR) workflow.**
2. **You've read the MeiliSearch [documentation](https://docs.meilisearch.com) and the [README](/README.md).**
3. **You know about the [MeiliSearch community](https://docs.meilisearch.com/resources/contact.html). Please use this for help.**

## How to Contribute

1. Make sure that the contribution you want to make is explained or detailed in a GitHub issue! Find an [existing issue](https://github.com/meilisearch/meilisearch-symfony/issues/) or [open a new one](https://github.com/meilisearch/meilisearch-symfony/issues/new).
2. Once done, [fork the meilisearch-symfony repository](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) in your own GitHub account. Ask a maintainer if you want your issue to be checked before making a PR.
3. [Create a new Git branch](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-and-deleting-branches-within-your-repository).
4. Review the [Development Workflow](#workflow) section that describes the steps to maintain the repository.
5. Make your changes.
6. [Submit the branch as a PR](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork) pointing to the `master` branch of the main meilisearch-symfony repository. A maintainer should comment and/or review your Pull Request within a few days. Although depending on the circumstances, it may take longer.<br>
 We do not enforce a naming convention for the PRs, but **please use something descriptive of your changes**, having in mind that the title of your PR will be automatically added to the next [release changelog](https://github.com/meilisearch/meilisearch-symfony/releases/).

## Development Workflow

### Using Composer

#### Setup

Install the dependencies:

```sh
$ composer install
```

#### Tests and Linter

Each Pull Request should pass the tests, and the linter to be accepted.

```sh
# Tests
$ docker run -p 7700:7700 getmeili/meilisearch:latest ./meilisearch --master-key=masterKey --no-analytics
$ composer test:unit
# Linter
$ composer lint:check
# Linter (with auto-fix)
$ composer lint:fix
```

### Using the Docker Environment

These commands do not work on MacOS, see [this issue](https://github.com/meilisearch/meilisearch-symfony/issues/6).

#### Setup

To start and build your Docker environment, just execute the next command in a terminal:

```sh
$ docker-compose up -d
```

Be sure no other MeiliSearch instance is currently running on your machine.

#### Tests and Linter

Each Pull Request should pass the tests, and the linter to be accepted.

```sh
# Tests
$ docker-compose exec -e MEILISEARCH_URL=http://meilisearch:7700 php composer test:unit
# Linter
$ docker-compose exec php composer lint:check
# Linter (with auto-fix)
$ docker-compose exec php composer lint:fix
```

### Release Process

MeiliSearch tools follow the [Semantic Versioning Convention](https://semver.org/).

#### Automated Changelogs

For each PR merged on `master`, a GitHub Action is running and updates the next release description as a draft release in the [GitHub interface](https://github.com/meilisearch/meilisearch-symfony/releases). If you don't have the right access to this repository, you will not be able to see the draft release until the release is published.

The draft release description is therefore generated and corresponds to all the PRs titles since the previous release. This means each PR should only do one change and the title should be descriptive of this change.

About this automation:
- As the draft release description is generated on every push on `master`, don't change it manually until the final release publishment.
- If you don't want a PR to appear in the release changelogs: add the label `skip-changelog`. We suggest removing PRs updating the README or the CI (except for big changes).
- If the changes you are doing in the PR are breaking: add the label `breaking-change`. In the release tag, the minor will be increased instead of the patch. The major will never be changed until [MeiliSearch](https://github.com/meilisearch/MeiliSearch) is stable.
- If you did any mistake, for example the PR is already closed but you forgot to add a label or you misnamed your PR, don't panic: change what you want in the closed PR and run the job again.

*More information about the [Release Drafter](https://github.com/release-drafter/release-drafter), used to automate these steps.*

#### How to Publish the Release

Make a PR modifying the file [`src/MeiliSearchBundle.php`](/src/MeiliSearchBundle.php) with the right version.

```ruby
VERSION = 'X.X.X'
```

Once the changes are merged on `master`, you can publish the current draft release via the [GitHub interface](https://github.com/meilisearch/meilisearch-symfony/releases).

A WebHook will be triggered and push the new package to [Packagist](https://packagist.org/packages/meilisearch/search-bundle).

## Git Guidelines

### Git Branches

All changes must be made in a branch and submitted as PR.
We do not enforce any branch naming style, but please use something descriptive of your changes.

### Git Commits

As minimal requirements, your commit message should:
- be capitalized
- not finish by a dot or any other punctuation character (!,?)
- start with a verb so that we can read your commit message this way: "This commit will ...", where "..." is the commit message.
  e.g.: "Fix the home page button" or "Add more tests for create_index method"

We don't follow any other convention, but if you want to use one, we recommend [this one](https://chris.beams.io/posts/git-commit/).

### GitHub Pull Requests

Some notes on GitHub PRs:
- [Convert your PR as a draft](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/changing-the-stage-of-a-pull-request) if your changes are a work in progress: no one will review it until you pass your PR as ready for review.<br>
  The draft PR can be very useful if you want to show that you are working on something and make your work visible.
- The branch related to the PR must be **up-to-date with `master`** before merging. You need to [rebase your branch](https://gist.github.com/curquiza/5f7ce615f85331f083cd467fc4e19398) if it is not.
- All PRs must be reviewed and approved by at least one maintainer.
- All PRs have to be **squashed and merged**.
- The PR title should be accurate and descriptive of the changes. The title of the PR will be indeed automatically added to the next [release changelogs](https://github.com/meilisearch/meilisearch-symfony/releases/).

Thank you again for reading this through, we can not wait to begin to work with you if you made your way through this contributing guide ❤️