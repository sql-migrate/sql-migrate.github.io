---
title: CLI Installation
description: How to install the PostgresCompare command-line interface
category: cli
order: 7
permalink: /docs/cli/installation/
---

The PostgresCompare command-line interface (`pgc`) is available for Windows, macOS, and Linux. The download is a single self-contained executable — no runtime or installer required.

## Supported Platforms

| Platform | Architecture | Binary |
|---|---|---|
| Linux | x64 | `pgc` |
| macOS | ARM64 (Apple Silicon) | `pgc` |
| macOS | x64 (Intel) | `pgc` |
| Windows | x64 | `pgc.exe` |

## Download

Download the `pgc` binary for your platform from the [Downloads page](/downloads/).

## Installation

### macOS and Linux

1.  **Make Executable:**
    After downloading, give the binary execute permissions.
    ```bash
    chmod +x pgc
    ```

2.  **Move to PATH:**
    Move the binary to a location in your system's `PATH`, such as `/usr/local/bin`.
    ```bash
    sudo mv pgc /usr/local/bin/pgc
    ```

3.  **Verify:**
    ```bash
    pgc --version
    ```

> **macOS Gatekeeper:** On first run, macOS may block the binary because it was downloaded from the internet. Open **System Settings → Privacy & Security** and click **Allow Anyway**, or run `xattr -d com.apple.quarantine pgc` before moving it to your PATH.

### Windows

1.  **Move to a Folder:**
    Move the downloaded `pgc.exe` to a folder of your choice, for example `C:\Program Files\PostgresCompare\`.

2.  **Add to PATH:**
    Add that folder to your system's or user's `PATH` environment variable so you can run `pgc` from any terminal.

    - Search for "Edit the system environment variables" in the Start Menu.
    - Click the **Environment Variables…** button.
    - In the **System variables** section, find and select the `Path` variable, then click **Edit…**.
    - Click **New** and add the path to the folder where you placed `pgc.exe`.
    - Click **OK** on all dialogs to save.

3.  **Verify:**
    Open a **new** Command Prompt or PowerShell window and run:
    ```powershell
    pgc --version
    ```

## First Steps

Initialize a configuration file in your project:

```bash
pgc config init
```

This creates a `pgc.yaml` file with examples of all available settings.

Run your first comparison:

```bash
pgc diff postgres://localhost/db1 postgres://localhost/db2
```

## Next Steps

- [CLI Commands](/docs/cli/commands/) — Full command reference
- [Data Sources](/docs/cli/data-sources/) — All supported data source types
- [Configuration](/docs/cli/configuration/) — Configure `pgc.yaml` for your project
- [Examples](/docs/cli/examples/) — CI/CD integration and workflow examples
