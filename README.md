# Mac Development Ansible Playbook

![Mac Dev Playbook Logo](/files/Mac-Dev-Playbook-Logo.png)

[![CI][badge-gh-actions]][link-gh-actions]

This work is inspired by [geerlingguy](https://github.com/geerlingguy/mac-dev-playbook)'s work on automating the setup for my MacBook.

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.

This is a work in progress, and is mostly a means for me to document my current Mac's setup. I'll be evolving this playbook over time.

*See also*:

- [Boxen](https://github.com/boxen)
- [Battleschool](http://spencer.gibb.us/blog/2014/02/03/introducing-battleschool)
- [osxc](https://github.com/osxc)
- [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks) (the original inspiration for geerlingguy's project)
- [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) (the original inspiration for this project)
- [NileshGule/Mac-dev-playbook](https://github.com/NileshGule/Mac-dev-playbook)

## Installation

1. Clone this repository to your local drive.
2. Source [`bootstrap.sh`](bootstrap.sh)
3. Run `$ mac::setup`

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Use with a remote Mac

You can use this playbook to manage other Macs as well; the playbook doesn't even need to be run from a Mac at all! If you want to manage a remote Mac, either another Mac on your network, or a hosted Mac like the ones from [MacStadium](https://www.macstadium.com), you just need to make sure you can connect to it with SSH:

1. (On the Mac you want to connect to:) Go to System Preferences > Sharing.
2. Enable 'Remote Login'.

> You can also enable remote login on the command line:
>
> ```sh
> sudo systemsetup -setremotelogin on
> ```

Then edit the `inventory` file in this repository and change the line that starts with `127.0.0.1` to:

```ini
[ip address or hostname of mac]  ansible_user=[mac ssh username]
```

If you need to supply an SSH password (if you don't use SSH keys), make sure to pass the `--ask-pass` parameter to the `ansible-playbook` command.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages`, `vscode`, `dock`, and `osx`.

```sh
ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"
```

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
 - cowsay
 - git
 - go

mas_installed_apps:
 - { id: 443987910, name: "1Password" }
 - { id: 498486288, name: "Quick Resizer" }
 - { id: 557168941, name: "Tweetbot" }
 - { id: 497799835, name: "Xcode" }

composer_packages:
 - name: hirak/prestissimo
 - name: drush/drush
   version: '^8.1'

gem_packages:
 - name: bundler
   state: latest

npm_packages:
 - name: webpack

pip_packages:
 - name: mkdocs

configure_dock: true
dockitems_remove: []
dockitems_persist: []
```

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

- android-messages # Desktop client for Android Messages
- android-studio # Tools for building Android applications
- appgate-sdp-client # Software-defined perimeter for secure network access
- balance-lock # Keep your Mac audio centred
- balenaetcher # Tool to flash OS images to SD cards & USB drives
- cheatsheet # Tool to list all active shortcuts of the current application
- day-o # A simple menu bar clock replacement
- dbeaver-community # Free universal database tool and SQL client
- discord # Voice and text chat software
- disk-inventory-x # Disk usage utility
- docker # App to build and share containerized applications and microservices
- dozer # Tool to hide status bar icons
- dupeguru # Finds duplicate files in a computer system
- firefox-developer-edition # Web browser
- font-fantasque-sans-mono # Fantasque Sans Mono
- font-firacode-nerd-font # FiraCode Nerd Font (Fira Code)
- font-hack-nerd-font # Hack Nerd Font (Hack)
- font-ia-writer-mono # iA Writer Mono
- freemind # Mind-mapping software written in Java
- gitkraken # Git client focusing on productivity
- google-backup-and-sync # Access and sync your Google Drive content
- google-chrome # Web browser
- gramps # Genealogy software
- iina # Free and open-source media player
- insomnia # HTTP and GraphQL Client
- iterm2-beta # Terminal emulator as alternative to Apple's Terminal app
- licecap # Animated screen capture application
- meld # Visual diff and merge tool
- muzzle # Silence embarrassing notifications while screensharing
- rectangle # Move and resize windows using keyboard shortcuts or snap areas
- robo-3t # MongoDB management tool
- session-manager-plugin # Plugin for AWS CLI to start and end sessions that connect to managed instances
- signal-beta # Instant messaging application focusing on security
- slack # Team communication and collaboration software
- the-unarchiver # Unpacks archive files
- tunnelblick # Free and open-source OpenVPN client
- vagrant # Development environment
- virtualbox # Free and open-source hosted hypervisor for x86 virtualization
- visual-studio-code # Open-source code editor
- wkhtmltopdf # open source (LGPLv3) command line tool to render HTML into PDF
- wombat # Cross platform gRPC client
- yubico-yubikey-manager # Cross-platform application for configuring any YubiKey over all USB interfaces
- zenmap # Multi-platform graphical interface for official Nmap Security Scanner
- zoom # Video communication and virtual meeting platform
- zoom-outlook-plugin # Outlook Plugin for Zoom.us

Packages (installed with Homebrew):

- ansible # Automate deployment, configuration, and upgrading
- ansible-lint # Checks ansible playbooks for practices and behaviour
- bat # Clone of cat(1) with syntax highlighting and Git integration
- bitwarden-cli # Secure and free password manager for all of your devices
- broot # New way to see and navigate directory trees
- caddy # Powerful, enterprise-ready, open source web server with automatic HTTPS
- cowsay # Configurable talking characters in ASCII art
- curl # Get a file from an HTTP, HTTPS or FTP server
- dive # A tool for exploring each layer in a docker image
- docker-compose # Isolated development environments using Docker
- ericm/stonks/stonks # A terminal based stock visualizer and tracker.
- exa # Modern replacement for 'ls'
- fd # Simple, fast and user-friendly alternative to find
- gh # GitHub command-line tool
- git # Distributed revision control system
- git-extras # Small git utilities
- go # Open source programming language to build simple/reliable/efficient software
- gobuffalo/tap/buffalo # Rapid Web Development w/ Go
- gofumpt # Stricter gofmt
- graphviz # Graph visualization software from AT&T and Bell Labs
- helm # Kubernetes package manager
- homeassistant-cli # Command-line utility for Home Assistant
- httpie # User-friendly cURL replacement (command-line HTTP client)
- hugo # Configurable static site generator
- jawshooah/pyenv/pyenv-default-packages # Automatically install packages in python environments
- jq # Lightweight and flexible command-line JSON processor
- kubectx # Tool that can switch between kubectl contexts easily and create aliases
- lazydocker # Lazier way to manage everything docker
- molecule # Automated testing for Ansible roles
- moreutils # Collection of tools that nobody wrote when UNIX was young
- mosh # Remote terminal application
- muffet # Fast website link checker in Go
- multitail # Tail multiple files in one terminal simultaneously
- node # Platform built on V8 to build network applications
- nvm # Manage multiple Node.js versions
- openssl # Cryptography and SSL/TLS Toolkit
- pandoc # Swiss-army knife of markup format conversion
- pgcli # CLI for Postgres with auto-completion and syntax highlighting
- pipx # Execute binaries from Python packages in isolated environments
- poetry # Python package management tool
- pre-commit # Framework for managing multi-language pre-commit hooks
- pv # Monitor data's progress through a pipe
- pyenv-ccache # Make Python build faster, using the leverage of `ccache`
- pyenv-pip-migrate # Migrate pip packages from one Python version to another
- qrencode # QR Code generation
- rclone # Rsync for cloud storage
- ripgrep # Search tool like grep and The Silver Searcher
- rust # Safe, concurrent, practical language
- shellcheck # Static analysis and lint tool, for (ba)sh scripts
- shihanng/gig/gig # gitignore file generator
- sl # Prints a steam locomotive if you type sl instead of ls
- sqlite # Command-line interface for SQLite
- ssh-copy-id # Add a public key to a remote machine's authorized_keys file
- starship # Cross-shell prompt for astronauts
- stoken # Tokencode generator compatible with RSA SecurID 128-bit (AES)
- tealdeer # Very fast implementation of tldr in Rust
- tlk/imagemagick-x11/imagemagick # Tools and libraries to manipulate images in many formats (X11 support)
- tokei # Program that allows you to count code, quickly
- tox # Generic Python virtualenv management and test command-line tool
- tree # Display directories as trees (with optional color/HTML output)
- watch # Executes a program periodically, showing output fullscreen
- wget # Internet file retriever
- wireguard-tools # Tools for the WireGuard secure network tunnel
- wrk # HTTP benchmarking tool
- yadm # Yet Another Dotfiles Manager
- yarn # JavaScript package manager
- youtube-dl # Download YouTube videos from the command-line
- yq # Process YAML documents from the CLI
- zsh # UNIX shell (command interpreter)
- zsh-history-substring-search # Zsh port of Fish shell's history search

My [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions

### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).

1. Remap Caps Lock to Escape (requires macOS Sierra 10.12.1+).
2. Set trackpad tracking rate.
3. Set mouse tracking rate.
4. Configure extra Mail and/or Calendar accounts (e.g. Google, Exchange, etc.).

### Configuration to be added

- I have vim configuration in the repo, but I still need to add the actual installation:

    ```sh
    mkdir -p ~/.vim/autoload
    mkdir -p ~/.vim/bundle
    cd ~/.vim/autoload
    curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
    cd ~/.vim/bundle
    git clone git://github.com/scrooloose/nerdtree.git
    ```

## Testing the Playbook

Many people have asked me if I often wipe my entire workstation and start from scratch just to test changes to the playbook. Nope! Instead, I posted instructions for how I build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which I can continually run and re-run this playbook to test changes and make sure things work correctly.

Additionally, this project is [continuously tested on GitHub Actions' macOS infrastructure](https://github.com/geerlingguy/mac-dev-playbook/actions?query=workflow%3ACI).

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

[Jeff Geerling](https://www.jeffgeerling.com/), 2014 (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).

[badge-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/workflows/CI/badge.svg?event=push
[link-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/actions?query=workflow%3ACI
