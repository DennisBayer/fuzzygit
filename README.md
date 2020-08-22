# fuzzygit

`fuzzygit` aims to ease the usage of common Git commands by applying a simple
interactive interface based on [`fzf`][100].

Handling with multiple files or input arguments on the command line can
sometimes become cumbersome. Therefore `fuzzygit` marries commands such as
`git add`, `git restore`, `git switch` with `fzf` in order select items of
interesst rather than typing.

## Table of Contents

* [Table of Contents](#table-of-contents)
* [Background](#background)
* [Usage](#usage)
  * [Configuration](#configuration)
  * [Useful Extensions](#useful-extensions)
* [Install](#install)
  * [Dependencies](#dependencies)
* [Version Overview](#version-overview)
* [Maintainers](#maintainers)
* [Credits](#credits)
* [Contributing](#contributing)
* [License](#license)

## Background

This project started as a personal helper. Additionally I wanted to improve my
basic bash & linux skills. There might be projects which provide the same
(or even better) functionality. Some of them - which I'm aware of - are listed
in the [Credits](#credits) section.

## Usage

The following features are available:

* `fg_switch` - Shows a list of branches and switches to the selected one.
  * By default only local branches are shown. To get a list of remote branches,
  pass `-r` as an argument.
* `fg_add` - Shows a list of files which can be added to the staging area.
* `fg_unstage` - Shows the list of files which are in the staging area (aka
  added files) and puts them back into the working area.
* `fg_restore` - Show the list of files in the working area (aka modified files)
  and restores the version from the local repository.
* `fg_log` - Shows the commit history and shows the log between the selected one
  and the HEAD.
* `fg_hash` - Shows the commit history and passes the hashes of the selected
  entries to the command which was given as an argument.
  * E.g. to get detailed information about a commit use `fg_hash git show`.

Once `fzf` lists the available items, choose one or more (depending on the
current operation) and confirm by pressing <kbd>ENTER</kbd>.

> _Note:_ If possible, a preview is available. Whether the preview is shown by
default may vary depending on your `fzf` configuration.
See [Configuration](#configuration) for further information.

In short: usage does not differ from `fzf`: <kbd>TAB</kbd> to select,
<kbd>Shift-TAB</kbd> to unselect, <kbd>Shift-PgUp/PgDown</kbd> to scroll in
preview, and so on.

### Configuration

`fuzzygit` itself does not provide any configuration at the moment. It also aims
as litte as possible to alter your `fzf` configuration.

For some features, like the preview, you might consider to set an option, like
the preview-binding.

Here's my current configuration as a starting point:

```bash
export FZF_DEFAULT_OPTS=" \
  --layout=reverse \
  --info=inline \
  --multi \
  --cycle \
  --preview-window=right:60%:hidden \
  --preview '([[ -f {} ]] && (bat {} || less {})) || ([[ -d {} ]] && (lt {} -L2 | less)) || echo {} 2> /dev/null | head -200' \
  --color=dark \
  --bind '?:toggle-preview' \
  --bind 'ctrl-a:select-all' \
  --bind 'ctrl-alt-a:deselect-all' \
```

### Useful Extensions

If not already done check out the following projects which offer nice feature
to beautify fzf's preview and Git's diff output.

* [bat][101] - A cat(1) clone with syntax highlighting and Git integration.
* [delta][102] - A viewer for git and diff output.

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

If you'd like to use `fuzzygit` permanently consider to add `fuzzygit` to your
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

## Version Overview

The current version is `prototype`. As soon as a second person has successfully
tested this version, a release of `0.1.0` is planned.

Whether there will be a roadmap or issue list is a pending matter.

## Maintainers

* [Dennis Bayer][0]

## Credits

* [fzf][100] - For providing the base functionality `fuzzygit` is built upon.

## Contributing

As of now the status of this project could be labeled as "hobby" and
"provided as is". The future of this projects is depending on my agenda the next
time.

But if you would like to contribute either by submitting a bugfix or propose a
new feature, feel free to do so. To get in touch and not to lose track of the
issue please file a [new issue][1].

## License

[MIT](LICENSE)

[0]: https://github.com/DennisBayer
[1]: https://github.com/DennisBayer/fuzzygit/issues/new/choose
[100]: https://github.com/junegunn/fzf
[101]: https://github.com/sharkdp/bat
[102]: https://github.com/dandavison/delta
