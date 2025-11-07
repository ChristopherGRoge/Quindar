# Quindar Tone API - Dev Container

This development container provides a complete, isolated environment for developing the Quindar Tone API with all dependencies pre-installed.

## Features

✅ **Lightweight Debian Linux** - Based on Debian Bookworm Slim
✅ **Latest Rust Toolchain** - Pre-configured with rustup, clippy, rustfmt
✅ **All System Dependencies** - pkg-config, OpenSSL, ALSA libraries
✅ **Sudo Access** - Full sudo privileges inside the container
✅ **VS Code Integration** - Rust extensions and settings pre-configured
✅ **Port Forwarding** - API port 42069 automatically forwarded
✅ **Development Tools** - Git, GH CLI, debugging tools

## Prerequisites

1. **Docker** - Must be installed and running
2. **VS Code** - With "Dev Containers" extension installed
3. **Docker Group Membership** - Your user must be in the docker group

## Quick Start

### Option 1: VS Code (Recommended)

1. Open this project in VS Code
2. Press `F1` and select: **Dev Containers: Reopen in Container**
3. Wait for the container to build (first time takes ~5 minutes)
4. Once ready, you'll have a full dev environment!

### Option 2: Command Line

```bash
# Build the container
docker build -t quindar-dev .devcontainer

# Run the container
docker run -it --rm \
  -v $(pwd):/workspace \
  -w /workspace \
  -p 42069:42069 \
  quindar-dev
```

## Using the Dev Container

### Build the Project

```bash
# Full build with audio support
cargo build --release

# Build without audio (no ALSA needed)
cargo build --release --no-default-features

# Run the server
cargo run --release
```

### Run Tests

```bash
cargo test
```

### Code Formatting and Linting

```bash
# Format code
cargo fmt

# Run clippy lints
cargo clippy
```

### Install Additional Packages

You have full sudo access inside the container:

```bash
sudo apt-get update
sudo apt-get install <package-name>
```

## Container Specifications

**Base Image:** `debian:bookworm-slim`
**User:** `vscode` (non-root with sudo)
**Default Shell:** `/bin/bash`
**Working Directory:** `/workspaces/Quindar-Break-In-Communicator`

## Pre-installed System Packages

- `build-essential` - GCC, make, etc.
- `pkg-config` - Library configuration tool
- `libssl-dev` - OpenSSL development headers
- `libasound2-dev` - ALSA audio development libraries
- `git` - Version control
- `curl`, `wget` - Download tools
- `vim`, `nano` - Text editors
- `gdb`, `lldb` - Debuggers

## Pre-installed Rust Tools

- `rustc` - Rust compiler
- `cargo` - Rust package manager
- `clippy` - Rust linter
- `rustfmt` - Code formatter
- `rust-src` - Rust source code (for rust-analyzer)

## VS Code Extensions

The following extensions are automatically installed:

- **rust-analyzer** - Rust language server
- **CodeLLDB** - Native debugger
- **Even Better TOML** - TOML syntax support
- **crates** - Cargo.toml helper
- **Error Lens** - Inline error display

## Troubleshooting

### Container Won't Build

```bash
# Clean up and rebuild
docker system prune -a
# Then reopen in VS Code
```

### Permission Issues

```bash
# Inside container, fix ownership
sudo chown -R vscode:vscode /workspaces
```

### Port Already in Use

```bash
# Check what's using port 42069
sudo lsof -i :42069
# Kill the process or change the port in .env
```

## Customization

### Add More Packages

Edit `.devcontainer/Dockerfile` and add packages to the `RUN apt-get install` command.

### Change Rust Version

```bash
# Inside container
rustup default stable  # or nightly, or specific version
rustup update
```

### Add VS Code Extensions

Edit `.devcontainer/devcontainer.json` and add extension IDs to the `extensions` array.

## Benefits Over Local Development

1. **No System Pollution** - All dependencies isolated in container
2. **Reproducible Builds** - Same environment every time
3. **Easy Cleanup** - Just delete the container
4. **Cross-Platform** - Works on Windows, Mac, Linux
5. **Full Control** - Sudo access for any system-level changes
6. **Fast Setup** - New developers can start in minutes

## CI/CD Integration

The dev container matches the CI environment, ensuring "works on my machine" issues are minimized.

## Resetting the Environment

```bash
# From VS Code: F1 -> Dev Containers: Rebuild Container
# Or from terminal:
docker-compose down
docker-compose up --build
```

## Additional Resources

- [VS Code Dev Containers Docs](https://code.visualstudio.com/docs/devcontainers/containers)
- [Rust in Containers](https://docs.docker.com/language/rust/)
- [Project README](../README.md)
