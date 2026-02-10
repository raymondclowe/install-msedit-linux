# install-msedit-linux

## Why use this script?

The `edit` tool is not currently available in common Linux package managers (such as `apt`, `dnf`, or `yum`). This script provides a convenient and reliable way to always install the latest official release directly from Microsoft's GitHub repository, without waiting for distribution maintainers to package it.

## Overview

This project provides a simple shell script to install the latest release of Microsoft's [`edit`](https://github.com/microsoft/edit) command-line editor on Linux.

## Features
- Automatically fetches the latest release from GitHub
- Downloads and installs the correct Linux binary for your architecture
- Adds the binary to `~/.local/bin` (creating the directory if needed)
- Optionally updates your `PATH` in `~/.profile` if required
- Checks if `edit` is already installed and up-to-date, and only updates if a newer version is available
- **Optional:** Can set Microsoft `edit` as your default `edit` command (overriding `/usr/bin/edit`) with a flag
- Provides clear instructions to undo this change

## Installation Methods

### Method 1: Automated Installation (Recommended)

#### 1. Install with a single command:
```sh
curl -fsSL https://raw.githubusercontent.com/raymondlowe/install-msedit-linux/main/install_msedit.sh | sh
```
- The script will:
  - Detect the latest version
  - Download and extract the binary
  - Place it in `~/.local/bin/edit`
  - Make it executable
  - Optionally update your `PATH` if needed
- After install, the script will tell you how to make Microsoft `edit` the default (see below).

#### 2. (Optional) Make Microsoft `edit` the default `edit` command
If you want the Microsoft version to take precedence over `/usr/bin/edit`, run:
```sh
curl -fsSL https://raw.githubusercontent.com/raymondlowe/install-msedit-linux/main/install_msedit.sh | sh -s -- --set-default
```
- This creates a symlink at `~/bin/edit` pointing to the Microsoft binary.
- Make sure `~/bin` is in your `PATH` (the script will tell you if it isn't).

#### 3. Undo (restore your previous default `edit`)
To remove the Microsoft `edit` as the default and restore your previous `edit` (e.g., `/usr/bin/edit`):
```sh
rm ~/bin/edit
```

#### 4. Reload your shell (if the script updated your `PATH`):
```sh
source ~/.profile
```

#### 5. Verify installation:
```sh
edit --version
```

### Method 2: Manual Installation (Using the Official Binary)

This method involves downloading the pre-compiled executable directly from the official GitHub repository, which provides faster launch times and more control over the installation process.

#### 1. Install zstd (if not already present)

The binary archive uses zstd compression, so you need to install it first:

**For Debian/Ubuntu:**
```sh
sudo apt install zstd
```

**For Fedora/CentOS/RHEL:**
```sh
sudo dnf install zstd
# or
sudo yum install zstd
```

#### 2. Download the latest binary

Navigate to the [Microsoft Edit releases page](https://github.com/microsoft/edit/releases/latest) on GitHub and download the file for your architecture (e.g., `edit-1.2.0-x86_64-linux-gnu.tar.zst` for most desktop PCs).

Alternatively, use `curl` in the terminal to download directly:

```sh
curl -L --no-progress-meter --output edit.tar.zst https://github.com/microsoft/edit/releases/latest/download/edit-1.2.0-x86_64-linux-gnu.tar.zst
```

> **Note:** Replace `x86_64` with `aarch64` for ARM64 systems. The version number in the filename (e.g., `1.2.0`) may change with new releases. Check the [releases page](https://github.com/microsoft/edit/releases/latest) for the current filename.

#### 3. Extract the archive

```sh
tar --zstd -xvf edit.tar.zst
```

This will produce an executable file named `edit`.

#### 4. Move the executable to your PATH

Place the binary in a directory that is in your system's PATH, for example, `/usr/local/bin`:

```sh
sudo mv edit /usr/local/bin/edit
```

Alternatively, you can move it to `~/.local/bin` for a user-specific installation:

```sh
mkdir -p ~/.local/bin
mv edit ~/.local/bin/edit
chmod +x ~/.local/bin/edit
```

Make sure `~/.local/bin` is in your PATH by adding this to your `~/.profile` or `~/.bashrc`:

```sh
export PATH="$HOME/.local/bin:$PATH"
```

Then reload your shell configuration for the PATH change to take effect:

```sh
source ~/.profile
# or, if you added it to .bashrc:
source ~/.bashrc
```

#### 5. Launch the editor

You can now run the editor simply by typing `edit` in your terminal:

```sh
edit [filename]
```

Verify the installation:

```sh
edit --version
```

## Requirements
- `curl`
- `awk`
- `tar`
- `zstd` or `unzstd` (for .tar.zst archives)
  - Install with: `sudo apt install zstd` (Debian/Ubuntu) or `sudo dnf install zstd` (Fedora/RHEL)
- `unxz` (for .xz archives)
- Standard POSIX `sh` (no bashisms required)
- **glibc 2.34+** (or whatever version the Microsoft binary requires)

> **Note:**
> The Microsoft `edit` binary may require a recent version of glibc (e.g., 2.34+). If you see errors about missing GLIBC versions when running `edit`, you may need to upgrade your Linux distribution to a newer release that includes a compatible glibc version.

## Notes
- The script is designed for x86_64 and aarch64 Linux systems.
- If you encounter issues, ensure you have the required tools installed.
- For more information about `edit`, see the [official repository](https://github.com/microsoft/edit).

---

*This project is not affiliated with Microsoft. It simply automates installation of their open source tool.*
