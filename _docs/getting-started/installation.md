---
title: Installation
description: How to install PostgresCompare on Windows, macOS, and Linux
category: getting-started
order: 1
permalink: /docs/getting-started/installation/
---

PostgresCompare is available for Windows, macOS, and Linux.

## System Requirements

| Platform | Minimum version |
|----------|-----------------|
| Windows | Windows 10 or later |
| macOS | macOS 11 or later (Apple Silicon and Intel) |
| Linux | Ubuntu 20.04+ / Debian 11+ |

You'll also need:
- 250 MB of free disk space, and 4 GB of RAM (8 GB recommended)
- Network access to your PostgreSQL servers (PostgreSQL 9.2 through 18)
- Database credentials with read permissions on the schemas you want to compare

## Installation

Get PostgresCompare from the [Downloads page](/downloads). Enter your email and we'll send you the download links for every platform.

### Windows

1. Run the `PostgresCompare-<version>-windows-x64-setup.exe` installer
2. Follow the prompts — this also puts the `pgc` CLI on your PATH
3. Launch PostgresCompare from the Start menu

### macOS

Both formats are signed and notarized, with native builds for Apple Silicon
(`arm64`) and Intel (`x64`):

1. Open the `.dmg` and drag PostgresCompare to your Applications folder, or run
   the `.pkg` if you would rather use an installer
2. Launch PostgresCompare from Applications

### Linux

Two formats, both 64-bit. The AppImage runs anywhere without installing:

```bash
chmod +x PostgresCompare-*-linux-x64.AppImage
./PostgresCompare-*-linux-x64.AppImage
```

On Debian and Ubuntu you can install the package instead, which adds a desktop
entry:

```bash
sudo apt install ./PostgresCompare-*-linux-x64.deb
```

Use `apt install ./…` rather than `dpkg -i`. `dpkg` does not resolve
dependencies, so if anything is missing it leaves the package half-configured.

#### Enabling universe on Ubuntu

The package depends on `libwebkit2gtk-4.1-0` and `libgtk-3-0`. On Ubuntu,
`libwebkit2gtk-4.1-0` comes from the **universe** component, which the minimal,
server and cloud images ship with disabled — apt can see the dependency but has
no repository that provides it. The symptom is a complaint that
`libwebkit2gtk-4.1-0` is not installable, followed by *unable to correct
problems, you have held broken packages*.

Enable universe, then install:

```bash
sudo add-apt-repository universe
sudo apt update
sudo apt install ./PostgresCompare-*-linux-x64.deb
```

## First Launch

When you first launch PostgresCompare, the welcome screen offers two ways to start:

- **Try the demo** — Opens a real comparison built from sample data, so you can explore the results and a deployment script without connecting to a database. This is the quickest way to see what PostgresCompare does.
- **Connect my databases** — Add a connection to your PostgreSQL server and compare your own schemas.

If PostgresCompare detects PostgreSQL connections already set up on your machine, it also offers **Import from environment** to add them in one click.

Your first 14 days include everything in Pro. After that, PostgresCompare
becomes the free tier: you can still use Quick compare to compare two
databases, generate a deployment script and deploy it. Saved projects,
comparison history, data comparison, pipelines, the `pgc` CLI and the advanced
comparison options need a subscription — enter a license key in the
license dialog to unlock them.

## Next Steps

Once installed, you're ready to:

- [Create your first comparison](/docs/getting-started/quick-start/) - Connect to databases and compare schemas
- [Understanding results](/docs/getting-started/first-comparison/) - Learn what the comparison results mean
