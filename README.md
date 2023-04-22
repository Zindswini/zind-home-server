# zind-home-server

[![build-ublue](https://github.com/zindswini/zind-home-server/actions/workflows/build.yml/badge.svg)](https://github.com/zindswini/zind-home-server/actions/workflows/build.yml)

My personal (immutable) home server configuration

## A desktop environment?
Yep. Having a real DE around makes certain tasks so much faster. Think local copy organizing files on a NAS, using handbrake, accessing LAN webuis, etc. 

With a couple useful flatpak apps, container based services, and a strong immutable fedora base, I should have quite the nifty little home server.

## Features

- Start with a uBlue lxqt image
- Add the following "server" packages
  - cockpit
  - cockpit-* (cockpit addons)
- networking utilities
  - wireguard-tools
  - tailscale
- Various other small tools
- Distrobox (from upstream)

- Sets automatic update timer to every night at 3am
- Sets flatpaks to update twice a day


### [yafti](https://github.com/ublue-os/yafti/)

`yafti` is the uBlue firstboot installer, and it's configuration can be found in `/etc/yafti.yml`. It includes an optional selection of Flatpaks to install, with a new group added for the Flatpaks declared in `recipe.yml`. You can look at what's done in the config and modify it to your liking.

The files `/etc/profile.d/ublue-firstboot.sh` and `/etc/skel.d/.config/autostart/ublue-firstboot.desktop` set up `yafti` so that it starts on boot, so if you wish to retain that functionality those files shouldn't be touched.

## Installation

> **Warning**
> This is an experimental feature and should not be used in production, try it in a VM for a while!

To rebase an existing Silverblue/Kinoite installation to the latest build:

```
sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/zindswini/zind-home-server:latest
```

This repository builds date tags as well, so if you want to rebase to a particular day's build:

```
sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/zindswini/zind-home-server:20230403
```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `release.yml`, so you won't get accidentally updated to the next major version.

## Just

The `just` task runner is included in main for further customization after first boot.
You can copy the justfile from `/etc/justfile` to `~/.justfile` to get started. Once `just` supports [include directives](https://just.systems/man/en/chapter_52.html), you can just include the file in `/etc` into your own justfile, where you have the option of adding new tasks.
After that run the following commands:

- `just` - Show all tasks, more will be added in the future
- `just bios` - Reboot into the system bios (Useful for dualbooting)
- `just changelogs` - Show the changelogs of the pending update
- Set up distroboxes for the following images:
  - `just distrobox-boxkit`
  - `just distrobox-debian`
  - `just distrobox-opensuse`
  - `just distrobox-ubuntu`
- `just setup-flatpaks` - Install all of the flatpaks declared in recipe.yml
- `just setup-gaming` - Install Steam, Heroic Game Launcher, OBS Studio, Discord, Boatswain, Bottles, and ProtonUp-Qt. MangoHud is installed and enabled by default, hit right Shift-F12 to toggle
- `just update` - Update rpm-ostree, flatpaks, and distroboxes in one command

Check the [just website](https://just.systems) for tips on modifying and adding your own recipes.

## Verification

These images are signed with sisgstore's [cosign](https://docs.sigstore.dev/cosign/overview/). You can verify the signature by downloading the `cosign.pub` key from this repo and running the following command:

    cosign verify --key cosign.pub ghcr.io/zindswini/zind-home-server

If you're forking this repo, the uBlue website has [instructions](https://ublue.it/making-your-own/) for setting up signing properly.
