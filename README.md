# Laptop

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

## Requirements

We support:

- macOS Mavericks (10.9)
- macOS Yosemite (10.10)
- macOS El Capitan (10.11)
- macOS Sierra (10.12)
- macOS High Sierra (10.13)
- macOS Mojave (10.14)
- macOS Catalina (10.15)

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/Vets-Who-Code/laptop/master/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

## Debugging

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/Vets-Who-Code/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

## What it sets up

macOS tools:

- [Homebrew] for managing operating system libraries.

[homebrew]: http://brew.sh/

Unix tools:

- [Universal Ctags] for indexing files for vim tab completion
- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [The Silver Searcher] for finding things in files
- [Tmux] for saving project state and switching between projects
- [Watchman] for watching for filesystem events
- [Zsh] as your shell

[universal ctags]: https://ctags.io/
[git]: https://git-scm.com/
[openssl]: https://www.openssl.org/
[the silver searcher]: https://github.com/ggreer/the_silver_searcher
[tmux]: http://tmux.github.io/
[watchman]: https://facebook.github.io/watchman/
[zsh]: http://www.zsh.org/

Heroku tools:

- [Heroku CLI] and for interacting with the Heroku API

[heroku cli]: https://devcenter.heroku.com/articles/heroku-cli

GitHub tools:

- [GitHub CLI] for interacting with the GitHub API

[github cli]: https://cli.github.com/

Gatsby:

- [Gatsby CLI] for generating gatsby boilerplate static app

[gatsby cli]: https://www.gatsbyjs.com/

Image tools:

- [ImageMagick] for cropping and resizing images

Programming languages, package managers, and configuration:

- [Node.js] and [npm], for running apps and installing JavaScript packages
- [NVM] stable for writing general-purpose code
- [Yarn] for managing JavaScript packages

[imagemagick]: http://www.imagemagick.org/
[node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[nvm]: https://github.com/nvm-sh/nvm
[yarn]: https://yarnpkg.com/en/

## Customize in `~/.laptop.local`

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.
