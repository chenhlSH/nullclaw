# Installation

This guide covers the main installation paths for macOS, Linux, and Windows.

## Prerequisites

- If building from source, use **Zig 0.15.2**.
- Git (required for source install).

Check Zig version:

```bash
zig version
```

The output must be `0.15.2`.

## Option 1: Homebrew (recommended for macOS/Linux)

```bash
brew install nullclaw
nullclaw --help
```

If the command works, installation is complete.

## Option 2: Build from Source (cross-platform)

```bash
git clone https://github.com/nullclaw/nullclaw.git
cd nullclaw
zig build -Doptimize=ReleaseSmall
zig build test --summary all
```

Build output:

- `zig-out/bin/nullclaw`

## Add Binary to PATH

### macOS/Linux (zsh/bash)

```bash
zig build -Doptimize=ReleaseSmall -p "$HOME/.local"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
# bash users: use ~/.bashrc
source ~/.zshrc
```

### Windows (PowerShell)

```powershell
zig build -Doptimize=ReleaseSmall -p "$HOME\.local"

$bin = "$HOME\.local\bin"
$user_path = [Environment]::GetEnvironmentVariable("Path", "User")
if (-not ($user_path -split ";" | Where-Object { $_ -eq $bin })) {
  [Environment]::SetEnvironmentVariable("Path", "$user_path;$bin", "User")
}
$env:Path = "$env:Path;$bin"
```

## Verify Installation

```bash
nullclaw --help
nullclaw --version
nullclaw status
```

If `status` returns component state successfully, runtime basics are ready.

## Upgrade and Uninstall

### Homebrew

```bash
brew update
brew upgrade nullclaw
brew uninstall nullclaw
```

### Source install

- Upgrade: `git pull`, then rebuild with `zig build -Doptimize=ReleaseSmall`.
- Uninstall: delete the installed `nullclaw` binary and remove the PATH entry.
