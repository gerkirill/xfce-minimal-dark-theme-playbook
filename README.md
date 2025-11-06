# XFCE Minimal Dark Theme Recipe

This Ansible playbook recipe applies a minimal dark theme configuration to the XFCE desktop environment.

## Overview

The recipe automatically detects the regular user on the system and copies pre-configured XFCE theme files from the `HOME` directory to the user's home directory, applying a dark theme to the XFCE desktop environment.

## What It Does

1. **Detects Regular User**: Scans `/etc/passwd` to identify the regular user account (one with a valid shell like `/bin/bash`, `/bin/zsh`, etc.)
2. **Copies Configuration Files**: Transfers all XFCE configuration files from `HOME/.config/` to the user's home directory
3. **Sets Proper Ownership**: Ensures all copied files are owned by the detected regular user
4. **Preserves File Permissions**: Maintains the original file permissions during the copy process

## Configuration Files Included

The recipe includes dark theme configurations for:

- **XFCE Terminal** (`xfce4/terminal/terminalrc`)
- **Thunar File Manager** (`xfce4/xfconf/xfce-perchannel-xml/thunar.xml`)
- **Desktop Settings** (`xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml`)
- **Panel Settings** (`xfce4/xfconf/xfce-perchannel-xml/xfce4-panel.xml`)
- **Window Manager** (`xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml`)
- **General Settings** (`xfce4/xfconf/xfce-perchannel-xml/xsettings.xml`)

## Requirements

- XFCE desktop environment installed
- Ansible 2.9 or higher
- Root or sudo privileges (the playbook is designed to run as root)
- At least one regular user account with a valid shell

## Usage

### Running the Playbook

```bash
cd xfce-minimal-dark-theme-playbook/playbooks
ansible-playbook optional/xfce_dark_theme-v1.0.yaml
```

### Recipe Metadata

The recipe is defined in `recipes.yaml` with the following metadata:

- **Name**: XFCE Minimal Dark Theme
- **Version**: v1.0
- **Supported Architectures**: x86_64, arm64
- **Minimum RAM**: 1 GB
- **Options**: None required

## How User Detection Works

The playbook uses a smart detection mechanism to find the regular user:

1. Searches `/etc/passwd` for users with valid shells (`/bin/bash`, `/bin/zsh`, `/bin/sh`, `/bin/fish`)
2. Excludes the root user
3. Takes the first matching user
4. Extracts the user's home directory path

This approach avoids system users (which typically have shells like `/usr/sbin/nologin` or `/bin/false`) and correctly identifies the actual user account.

## Notes

- The playbook is idempotent and can be run multiple times safely
- Existing XFCE configuration files will be overwritten
- Users may need to log out and log back in for all changes to take effect
- The theme files are located in the `HOME` directory of this recipe

## Troubleshooting

If the playbook fails to detect a user:
- Ensure at least one regular user exists with a valid shell
- Check that the user's entry in `/etc/passwd` is properly formatted
- Verify the user has a home directory

If theme changes don't appear:
- Log out and log back in to XFCE
- Restart the XFCE session
- Check file permissions in `~/.config/xfce4/`