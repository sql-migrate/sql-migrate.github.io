---
title: CLI Installation
description: Install the PostgresCompare command-line interface
category: cli
order: 8
permalink: /docs/cli/installation/
---

The PostgresCompare CLI allows you to run comparisons and generate deployment scripts from the command line, enabling automation and CI/CD integration.

## Installation

### Windows

The CLI is included with the PostgresCompare desktop application. After installing PostgresCompare, add the installation directory to your PATH:

```powershell
# Add to PATH (adjust path if installed elsewhere)
$env:Path += ";C:\Program Files\PostgresCompare"
```

Or run directly:

```powershell
& "C:\Program Files\PostgresCompare\pgcompare.exe" --version
```

### macOS

The CLI is bundled inside the application package:

```bash
# Create an alias
alias pgcompare="/Applications/PostgresCompare.app/Contents/MacOS/pgcompare"

# Or add to PATH in ~/.zshrc or ~/.bash_profile
export PATH="$PATH:/Applications/PostgresCompare.app/Contents/MacOS"
```

### Linux

The CLI is included in the installation directory:

```bash
# If installed via .deb
pgcompare --version

# If using .tar.gz, add to PATH
export PATH="$PATH:/path/to/postgrescompare"
```

## Verifying Installation

Run the version command to verify the CLI is working:

```bash
pgcompare --version
# PostgresCompare CLI 4.0.1
```

## Authentication

The CLI uses the same license as the desktop application. If you've already activated PostgresCompare, the CLI will use that license automatically.

For headless environments (CI/CD servers), set the license key via environment variable:

```bash
export PGCOMPARE_LICENSE="your-license-key-here"
```

Or pass it as a command-line argument:

```bash
pgcompare --license "your-license-key-here" compare ...
```

## Next Steps

- [CLI Commands](/docs/cli/commands/) - Full command reference
- [CLI Examples](/docs/cli/examples/) - Common usage patterns
- [CI/CD Integration](/docs/guides/ci-cd-integration/) - Automate with CI/CD
