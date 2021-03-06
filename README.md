<p align="center">
  <img src="/docs/assets/images/fuzzygit.png" alt="fuzzygit banner" /><br>
  <img alt="GitHub" src="https://img.shields.io/github/license/DennisBayer/fuzzygit">
  <img alt="GitHub release (latest SemVer)" src="https://img.shields.io/github/v/release/DennisBayer/fuzzygit">  
  <img alt="GitHub release (latest SemVer including pre-releases)" src="https://img.shields.io/github/v/release/DennisBayer/fuzzygit?include_prereleases&label=pre-release&sort=semver">
  <img alt="GitHub issues" src="https://img.shields.io/github/issues/DennisBayer/fuzzygit">
</p>

# fuzzygit

`fuzzygit` aims to ease the usage of common Git commands by applying a simple
interactive interface based on [`fzf`][100].

Handling with multiple files or input arguments on the command line can
sometimes become cumbersome. Therefore `fuzzygit` marries commands such as
`git add`, `git restore`, `git switch` with `fzf` in order select items of
interest rather than typing.

<p align="center">
  <img src="/docs/assets/images/fuzzygit-demo.gif" width="800" height="450" alt="fuzzygit demo" />
</p>

## Table of Contents <!-- omit in toc -->

* [Background](#background)
* [Usage](#usage)
  * [Finder](#finder)
  * [Interaction](#interaction)
  * [Configuration](#configuration)
    * [fzf](#fzf)
* [Install](#install)
  * [Dependencies](#dependencies)
* [Version Overview](#version-overview)
* [Maintainers](#maintainers)
* [Credits](#credits)
  * [Useful Third-Party Tools](#useful-third-party-tools)
* [Contributing](#contributing)
* [License](#license)

## Background

This project started as a personal helper. Additionally, I wanted to improve my
basic bash & Linux skills. There might be projects which provide the same
(or even better) functionality. Some of them - which I'm aware of - are listed
in the [Credits](#credits) section.

## Usage

The following features are available:

* `fuzzygit <functionName> [<args..>]`  - Invokes one of the functions, if an
  argument is passed.
  * E.g. `fuzzygit add` or `fuzzygit hash git show`.
  * `--help` - Shows the help.
* `add` - Shows a list of files which can be added to the staging area.
* `branch (contains | no-merged)`
  Lists branches which either contain another branch or which do not contain a
  certain commit. The desired action is passed as an argument.
* `cherry <branch name> [<git log option>...]` - Cherry picks the selected
  commits from the given branch and the HEAD. The first parameter _must_ be a
  branch name.
* `flog [<git log option>...]` - Lists all items of the git tree and shows
  the log of the selected item.
* `hash <command for hash>` - Shows the commit history and passes the hashes
  of the selected entries to the command which was given as an argument.
  * E.g. to get detailed information about a commit use `hash git show`.
* `log [<git log option>...]` - Shows the commit history and shows the log
  between the selected one and the HEAD.
* `restore` - Show the list of files in the working area (aka modified files)
  and restores the version from the local repository.
* `switch [-r]` - Shows a list of branches and switches to the selected one.
  * By default, only local branches are shown. To get a list of remote branches,
  pass `-r` as an argument.
* `unstage` - Shows the list of files which are in the staging area (aka
  added files) and puts them back into the working area.

> `<git log option>` - By default `--stat` is passed to `git log`. To get a
different preview, pass a custom flag, like `--oneline`.

### Finder

Once `fzf` lists the available items, choose one or more (depending on the
current operation) and confirm by pressing <kbd>Enter</kbd>.

> _Note:_ If possible, a preview is available. Whether the preview is shown by
default may vary depending on your `fzf` configuration.
See [Configuration](#configuration) for further information.

In short: usage does not differ from the [keybindings][104] of the `fzf` finder:

| Shortcut                     | Action            |
| ---------------------------- | ----------------- |
| <kbd>Tab</kbd>               | select            |
| <kbd>Shift-Tab</kbd>         | unselect          |
| <kbd>Enter</kbd>             | confirm selection |
| <kbd>Shift-PgUp/PgDown</kbd> | scroll in preview |
| <kbd>Ctrl-j/n</kbd>          | move cursor up    |
| <kbd>Ctrl-k/p</kbd>          | move cursor down  |
| <kbd>Ctrl-q/Esc</kbd>        | quit              |

### Interaction

There are several possibilities for you to integrate `fuzzygit` into your
workflow. Choose the one which suits you the most.

1. Calling the main function and pass an argument to indicate which function
   should be called: `fuzzygit add`, `fuzzygit switch -r`.
2. Calling the function directly: `fg_add`, `fg_unstage`.
3. Defining a git alias:

    ```bash
    fg = "!f() { bash -c -i 'fuzzygit "$@"' bash "$@"; }; f"
    ```

    and invoking via `git fg restore`, `git fg hash git show`.

### Configuration

The config file `${XDG_CONFIG_HOME}/fuzzygit/config` will be respected. Just
put your favorite values in there.

* `FUZZYGIT_IS_ECHO_STATUS=(true|false)` - [default: `true`] -
  Toggle fuzzygit status information.
* `FUZZYGIT_PREVIEW_GIT_LOG_OPTS` - [default: `--stat`] -
  Set the options which are passed to `git log` when it is used in a preview
  environment.
* `FUZZYGIT_GIT_LOG_PRETTY_FORMAT` - [default: `(%ar) %C(bold)%s%C(reset) - %aN%`] -
  Set the pretty log format for `git log`.<br>_Note:_ The pretty format always
  starts with `--pretty=format:%h - `. Your config will be appended.

#### fzf

`fuzzygit` aims to alter your `fzf` configuration as little as possible.
For some features, like the preview, you might consider setting an option,
e.g. a binding toggle.

Here's my current configuration as a starting point:

```bash
export FZF_DEFAULT_OPTS=" \
  --layout=reverse \
  --info=inline \
  --multi \
  --cycle \
  --color=dark \
  --preview-window=right:60%:hidden \
  --preview '([[ -f {} ]] && (bat {} || less {})) || ([[ -d {} ]] && (lt {} -L2 | less)) || echo {} 2> /dev/null | head -200' \
  --bind '?:toggle-preview' \
  --bind 'ctrl-a:select-all' \
  --bind 'ctrl-alt-a:deselect-all' \
```

## Install

Just clone the repository to a directory of your choice...

```bash
git clone https://github.com/DennisBayer/fuzzygit.git
  or
git clone git@github.com:DennisBayer/fuzzygit.git
```

...and source the main file.

```bash
source <path to repository>/fuzzygit
```

If you'd like to use `fuzzygit` permanently consider adding `fuzzygit` to your
`PATH`.

```bash
echo "export PATH=\"$(pwd)/fuzzygit:\$PATH\"" >> ~/.bashrc
```

### Dependencies

`fzf` is required to run:

```bash
$ fzf --version
0.21.1 (334a4fa)
```

Some basic tools are required as well:

* `awk`, `sed`, `xargs`, `printf`, `tac`

## Version Overview

| Version        | Date       | Notes |
| ---            | ---        | --- |
| `0.2.0`        | 2020-12-06 | Feature release ([Release Notes][302]) |
| `0.2.0-beta.1` | 2020-11-22 | Feature release ([Release Notes][301]) |
| `0.1.0`        | 2020-09-18 | Feature release ([Release Notes][300]) |
| `0.1.0-beta.1` | 2020-09-01 | Initial release with a basic feature set. |

Whether there will be a roadmap or issue list is a pending matter.

## Maintainers

* [Dennis Bayer][0]

## Credits

* [fzf][100] - For providing the base functionality `fuzzygit` is built upon.
* [forgit][103] - "A utility tool powered by fzf for using git interactively."
  Does basically the same as `fuzzygit`, but did not fit my workflow at the time
  I discovered it.
* [git-fuzzy][105] - "interactive `git` with the help of `fzf`". Another idea
  of combining Git and fzf. Have a look, if you're looking for a more visual
  approach.

### Useful Third-Party Tools

If not already done check out the following projects which offer nice features
to beautify fzf's preview and Git's diff output.

* [bat][101] - A cat(1) clone with syntax highlighting and Git integration.
* [delta][102] - A viewer for git and diff output.

## Contributing

As of now the status of this project could be labeled as "hobby" and
"provided as is". The future of this project depends on my agenda the next
time.

But if you would like to contribute either by submitting a bugfix or propose a
new feature, feel free to do so. To get in touch and not to lose track of the
issue please file a [new issue][1]. See [CONTRIBUTING](docs/CONTRIBUTING.md).

## License

[MIT](LICENSE)

[0]: https://github.com/DennisBayer
[1]: https://github.com/DennisBayer/fuzzygit/issues/new/choose
[100]: https://github.com/junegunn/fzf
[101]: https://github.com/sharkdp/bat
[102]: https://github.com/dandavison/delta
[103]: https://github.com/wfxr/forgit
[104]: https://github.com/junegunn/fzf#using-the-finder
[105]: https://github.com/bigH/git-fuzzy
[300]: https://github.com/DennisBayer/fuzzygit/releases/tag/0.1.0
[301]: https://github.com/DennisBayer/fuzzygit/releases/tag/0.2.0.beta.1
[302]: https://github.com/DennisBayer/fuzzygit/releases/tag/0.2.0
