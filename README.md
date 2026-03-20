# Ansible Role: Dotfiles

[![CI](https://github.com/bernylinville/ansible-role-dotfiles/actions/workflows/ci.yml/badge.svg)](https://github.com/bernylinville/ansible-role-dotfiles/actions/workflows/ci.yml)

Installs a set of dotfiles from a given Git repository. Supports two modes:

- **Stow mode** (recommended): Uses [GNU Stow](https://www.gnu.org/software/stow/) to manage symlinks, supporting structured dotfiles repos with per-package directories.
- **Legacy mode**: Creates symlinks directly from repository root to home directory (original behavior).

Forked from [geerlingguy/ansible-role-dotfiles](https://github.com/geerlingguy/ansible-role-dotfiles).

## Requirements

- `git` on the managed machine
- `stow` on the managed machine (only if using stow mode)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    dotfiles_repo: "https://github.com/geerlingguy/dotfiles.git"
    dotfiles_repo_version: master
    dotfiles_repo_accept_hostkey: false
    dotfiles_repo_local_destination: "~/Documents/dotfiles"
    dotfiles_home: "~"

### Stow Mode

    dotfiles_use_stow: false

Set to `true` to use GNU Stow for managing symlinks. When enabled, the role expects the dotfiles repo to have per-package subdirectories (e.g., `zsh/`, `starship/`).

    dotfiles_stow_packages:
      - zsh
      - starship

List of stow packages (subdirectories) to deploy.

### Legacy Mode

    dotfiles_files:
      - .zshrc
      - .gitignore

Which files from the dotfiles repository root should be linked to `dotfiles_home`. Only used when `dotfiles_use_stow` is `false`.

## Example Playbook

### Stow Mode

    - hosts: localhost
      vars:
        dotfiles_repo: "https://github.com/yourname/dotfiles.git"
        dotfiles_use_stow: true
        dotfiles_stow_packages:
          - zsh
          - starship
      roles:
        - { role: geerlingguy.dotfiles }

### Legacy Mode

    - hosts: localhost
      roles:
        - { role: geerlingguy.dotfiles }

## License

MIT / BSD

## Author Information

This role was originally created by [Jeff Geerling](https://www.jeffgeerling.com/), forked and extended with stow support by [Berny Linville](https://github.com/bernylinville).
