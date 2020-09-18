# Contributing to [fuzzygit][0]

👍🎉 First off, thanks for considering contributing! 🎉👍

Despite fuzzygit is _only_ my spare time project at the moment, I'd appreciate
any feedback.

## Table of Contents <!-- omit in toc -->

* [Scope of this Project](#scope-of-this-project)
* [How to Contribute](#how-to-contribute)
  * [Filing a Bug](#filing-a-bug)
  * [Suggest a new Feature](#suggest-a-new-feature)
  * [Submitting a Pull Request](#submitting-a-pull-request)
    * [Forking](#forking)
* [Miscellaneous](#miscellaneous)

## Scope of this Project

`fuzzygit` does not aim to be the all-inclusive package regarding the cli usage
of Git. It rather tries to be a lean and clean helper for the everyday usage.

Codewise it focuses on principles like KISS (keep it simple, stupid) and YAGNI
(you ain't gonna need it). This means, that an 80% solution is absolutely fine,
as long as it adds some value for the user.

On the feature side, I'd like to apply the KISS principle as well. Let's be
honest: who pays attention to a cluttered GUI doing an operation several times
a day? 😉

## How to Contribute

As stated, any feedback is welcome. This includes among other things:

### Filing a Bug

Please use the [issue template][1] provided for this project as it helps to
understand the bug and avoids unnecessary round-trips.

### Suggest a new Feature

At the moment `fuzzygit` is tailored to my needs, but I'm open-minded for
suggestions which fit in the scope of this project (see above). A
[feature template][1] is also available.

### Submitting a Pull Request

In general, I recommend filing an issue in first place to discuss your request.
This increases the likelihood that the PR will be accepted later on. In the
worst case you are about to save your valuable time. 😉

Before submitting a pull request please check if the following criteria are met:

Your PR...

* ...is in accordance with the scope of this project.
* ...aligns with the existing code formatting.
* ...is well tested (e.g. can handle with various file name pitfalls).
* ...updates the documentation as well, if there are any API changes.
* ...is linked to an issue. See [GitHub documentation][100] for further
  information.
* ...follows the Git [commit message conventions][101].

#### Forking

The repository branches are structured as follows:

* `master` - The main branch, which is used to tag releases. It should always be
  in a production ready state.
* `develop` - The development branch, which serves as an integration branch.
* `feature/<description|issueId>` - A feature branch.
* `release/<version>` - A branch to prepare a release.

Please use the `develop` branch to start your own feature- or bugfix branch.
This might look a bit over the top for such a small project, but helps to keep
track.

## Miscellaneous

Any contribution are considered to be licensed under the same licence `fuzzgit`
uses. See [LICENSE](../LICENSE).

Last but not least: Be kind and prudent to each other (you know, language
barriers, level of experience and so on 😄). We all spend our free time and
keeping the fun in it is a major point for me. Everyone might have a bad day,
but consider double checking your phrasing.

[0]: https://github.com/DennisBayer/fuzzygit
[1]: https://github.com/DennisBayer/fuzzygit/issues/new/choose
[100]: https://docs.github.com/en/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue
[101]: https://chris.beams.io/posts/git-commit/
