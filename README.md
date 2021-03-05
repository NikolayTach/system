# Nix System Configuration

![system build](https://github.com/kclejeune/system/workflows/system%20build/badge.svg)

This repository manages system configurations for all of my
macOS, nixOS, and linux machines.

## Structure

### Configurations

This repository is a flake, so configurations are specified
in [flake.nix](./flake.nix). Platform specific base configurations are found
in [./machines](./machines); these configurations share as much functionality as
possible in [./modules/common.nix](./modules/common.nix).

Additionally, for systems that aren't running nixOS or nix-darwin,
[./modules/home.nix](./modules/home.nix) defines a usable configuration
for any linux system running nix.

### User Customization

User "profiles" are specified in [./modules/profiles](./modules/profiles); these modules configure
contextual, identity-specific settings such as SSL certificates or work vs. personal email addresses.

## Prerequisites
### Installing Nix Package Manager

Run the installer script to perform a multi-user installation
on darwin or any other type of linux. This script can optionally accept an argument with the URL to a nix installer, but will use the nixFlakes installer by default:

```bash
./install-nix.sh
```

Note that this step is naturally skipped on NixOS since `nix` is the package manager by default.

## System Bootstrapping

### NixOS

Follow the installation instructions, then run

```bash
sudo nixos-install --flake github:kclejeune/system#phil
```

### Darwin/Linux

Clone this repository into `~/.nixpkgs` with

```bash
git clone https://github.com/kclejeune/system ~/.nixpkgs
```

You can bootstrap a new nix-darwin system using

```bash
nix develop -c sysdo disksetup && sysdo build --darwin [host] && ./result/activate-user && ./result/activate
```

or a home-manager configuration using

```bash
nix develop -c sysdo build --home-manager [host] && ./result/activate
```
## CLI Documentation: `sysdo`

**Usage**:

```console
$ sysdo [OPTIONS] COMMAND [ARGS]...
```

**Options**:

* `--install-completion`: Install completion for the current shell.
* `--show-completion`: Show completion for the current shell, to copy it or customize the installation.
* `--help`: Show this message and exit.

**Commands**:

* `bootstrap`: builds an initial configuration
* `build`: builds the specified flake output; infers...
* `cache`: cache the output environment of flake.nix
* `clean`: remove previously built configurations and...
* `disksetup`: configure disk setup for nix-darwin
* `fmt`: run formatter on all nix files
* `gc`: run garbage collection on unused nix store...
* `switch`: builds and activates the specified flake...
* `update`: update all flake inputs or optionally...

## `sysdo bootstrap`

builds an initial configuration

**Usage**:

```console
$ sysdo bootstrap [OPTIONS] [HOST]
```

**Arguments**:

* `[HOST]`: the hostname of the configuration to build

**Options**:

* `--nixos / --no-nixos`: [default: False]
* `--darwin / --no-darwin`: [default: False]
* `--home-manager / --no-home-manager`: [default: False]
* `--help`: Show this message and exit.

## `sysdo build`

builds the specified flake output; infers correct platform to use if not specified

**Usage**:

```console
$ sysdo build [OPTIONS] [HOST]
```

**Arguments**:

* `[HOST]`: the hostname of the configuration to build

**Options**:

* `--nixos / --no-nixos`: [default: False]
* `--darwin / --no-darwin`: [default: False]
* `--home-manager / --no-home-manager`: [default: False]
* `--help`: Show this message and exit.

## `sysdo cache`

cache the output environment of flake.nix

**Usage**:

```console
$ sysdo cache [OPTIONS]
```

**Options**:

* `--cache-name TEXT`: [default: kclejeune]
* `--help`: Show this message and exit.

## `sysdo clean`

remove previously built configurations and symlinks from the current directory

**Usage**:

```console
$ sysdo clean [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `sysdo disksetup`

configure disk setup for nix-darwin

**Usage**:

```console
$ sysdo disksetup [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `sysdo fmt`

run formatter on all nix files

**Usage**:

```console
$ sysdo fmt [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `sysdo gc`

run garbage collection on unused nix store paths

**Usage**:

```console
$ sysdo gc [OPTIONS]
```

**Options**:

* `-d, --delete-older-than [AGE]`: specify minimum age for deleting store paths
* `--dry-run / --no-dry-run`: test the result of garbage collection  [default: False]
* `--help`: Show this message and exit.

## `sysdo switch`

builds and activates the specified flake output; infers correct platform to use if not specified

**Usage**:

```console
$ sysdo switch [OPTIONS] [HOST]
```

**Arguments**:

* `[HOST]`: the hostname of the configuration to build

**Options**:

* `--nixos / --no-nixos`: [default: False]
* `--darwin / --no-darwin`: [default: False]
* `--home-manager / --no-home-manager`: [default: False]
* `--help`: Show this message and exit.

## `sysdo update`

update all flake inputs or optionally specific flakes

**Usage**:

```console
$ sysdo update [OPTIONS]
```

**Options**:

* `-f, --flake [FLAKE]`: specify an individual flake to be updated
* `--commit / --no-commit`: commit the updated lockfile  [default: False]
* `--help`: Show this message and exit.

