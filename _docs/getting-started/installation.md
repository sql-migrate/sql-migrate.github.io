---
title: Installation
description: How to install PostgresCompare on Windows, macOS, and Linux
category: getting-started
order: 1
permalink: /docs/getting-started/installation/
---

PostgresCompare is available for Windows, macOS, and Linux. Choose your platform below for installation instructions.

## System Requirements

- **Windows**: Windows 10 or later (64-bit)
- **macOS**: macOS 10.15 (Catalina) or later
- **Linux**: Ubuntu 18.04+, Debian 10+, or other modern distributions

PostgresCompare connects directly to your PostgreSQL databases, so you'll need:
- Network access to your PostgreSQL servers
- Valid database credentials with read permissions

## Windows Installation

1. Download the Windows installer from the [downloads page](/downloads/)
2. Run the `.exe` installer
3. Follow the installation wizard
4. Launch PostgresCompare from the Start menu

The installer will create a Start menu shortcut and optionally a desktop shortcut.

## macOS Installation

1. Download the macOS disk image from the [downloads page](/downloads/)
2. Open the `.dmg` file
3. Drag PostgresCompare to your Applications folder
4. Launch PostgresCompare from Applications

On first launch, macOS may show a security warning. Right-click the app and select "Open" to bypass this.

## Linux Installation

### Debian/Ubuntu (.deb)

```bash
# Download the .deb package from the downloads page
sudo dpkg -i postgrescompare_*.deb
sudo apt-get install -f  # Install dependencies if needed
```

### Other Distributions (.tar.gz)

```bash
# Download and extract the archive
tar -xzf postgrescompare_*.tar.gz
cd postgrescompare

# Run the application
./PostgresCompare
```

## First Launch

When you first launch PostgresCompare, you'll see the welcome screen where you can:

1. Create a new project
2. Open an existing project
3. Enter your license key (or continue with trial)

If you have a license key, enter it now to unlock all features. Otherwise, you can use the 14-day trial to evaluate PostgresCompare.

## Next Steps

Once installed, you're ready to:

- [Create your first comparison](/docs/getting-started/quick-start/) - Connect to databases and compare schemas
- [Understanding results](/docs/getting-started/first-comparison/) - Learn what the comparison results mean
