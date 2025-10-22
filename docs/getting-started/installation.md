# Installation Guide

This guide covers different ways to install and run Republic blockchain.

## System Requirements

### Minimum Requirements
- **CPU**: 2 cores
- **RAM**: 4 GB
- **Storage**: 20 GB available space
- **Network**: Broadband internet connection

### Recommended Requirements
- **CPU**: 4+ cores
- **RAM**: 8+ GB
- **Storage**: 100+ GB SSD
- **Network**: Stable broadband with low latency

### Supported Platforms
- Linux (Ubuntu 20.04+, CentOS 8+, Arch Linux)
- macOS (10.15+)
- Windows (10/11 with WSL2)

## Prerequisites

### Install Rust

Republic is written in Rust, so you'll need the Rust toolchain:

=== "Linux/macOS"
    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    source ~/.cargo/env
    ```

=== "Windows"
    Download and run [rustup-init.exe](https://rustup.rs/) from the official website.

Verify installation:
```bash
rustc --version
cargo --version
```

### Install Git

=== "Ubuntu/Debian"
    ```bash
    sudo apt update
    sudo apt install git
    ```

=== "CentOS/RHEL"
    ```bash
    sudo yum install git
    ```

=== "macOS"
    ```bash
    # Using Homebrew
    brew install git

    # Or install Xcode Command Line Tools
    xcode-select --install
    ```

=== "Windows"
    Download from [git-scm.com](https://git-scm.com/download/win)

## Installation Methods

### Method 1: Build from Source (Recommended)

#### 1. Clone the Repository
```bash
git clone https://github.com/republic-chain/republic.git
cd republic
```

#### 2. Build the Project
```bash
# Debug build (faster compilation, slower execution)
cargo build

# Release build (slower compilation, faster execution)
cargo build --release
```

#### 3. Install Binaries
```bash
# Install to ~/.cargo/bin
cargo install --path . --bins

# Or create symlinks
ln -s $(pwd)/target/release/republic-node ~/.local/bin/
ln -s $(pwd)/target/release/republic-cli ~/.local/bin/
```

#### 4. Verify Installation
```bash
republic-node --version
republic-cli --version
```

### Method 2: Pre-built Binaries

Download pre-built binaries from our [releases page](https://github.com/republic-chain/republic/releases):

=== "Linux"
    ```bash
    # Download latest release
    wget https://github.com/republic-chain/republic/releases/latest/download/republic-linux-x86_64.tar.gz

    # Extract and install
    tar -xzf republic-linux-x86_64.tar.gz
    sudo mv republic-* /usr/local/bin/
    ```

=== "macOS"
    ```bash
    # Download latest release
    curl -L https://github.com/republic-chain/republic/releases/latest/download/republic-darwin-x86_64.tar.gz -o republic-darwin.tar.gz

    # Extract and install
    tar -xzf republic-darwin.tar.gz
    sudo mv republic-* /usr/local/bin/
    ```

=== "Windows"
    1. Download `republic-windows-x86_64.zip` from releases
    2. Extract to a folder (e.g., `C:\Republic`)
    3. Add the folder to your PATH environment variable

### Method 3: Docker

Run Republic using Docker:

```bash
# Pull the latest image
docker pull republicchain/republic:latest

# Run a node
docker run -d \
  --name republic-node \
  -p 8545:8545 \
  -p 9000:9000 \
  -v republic-data:/data \
  republicchain/republic:latest

# Run CLI commands
docker exec republic-node republic-cli --help
```

### Method 4: Package Managers

#### Homebrew (macOS/Linux)
```bash
# Add the tap
brew tap republic-chain/republic

# Install
brew install republic
```

#### Arch Linux (AUR)
```bash
# Using yay
yay -S republic-blockchain

# Or using paru
paru -S republic-blockchain
```

## Configuration

### Create Configuration Directory
```bash
mkdir -p ~/.republic
cd ~/.republic
```

### Generate Configuration
```bash
# Generate default configuration
republic-node config generate --output config.toml

# Generate for specific network
republic-node config generate --network mainnet --output mainnet.toml
republic-node config generate --network testnet --output testnet.toml
```

### Sample Configuration
```toml
# ~/.republic/config.toml
[node]
data_dir = "~/.republic/data"
bind_address = "0.0.0.0:9000"
rpc_address = "127.0.0.1:8545"

[consensus]
block_time = 2000  # milliseconds
max_validators = 100

[network]
max_peers = 50
bootstrap_nodes = [
    "/ip4/54.123.45.67/tcp/9000/p2p/12D3KooWQ...",
    "/ip4/98.76.54.32/tcp/9000/p2p/12D3KooWR..."
]

[logging]
level = "info"
format = "json"

[metrics]
enabled = true
address = "127.0.0.1:9090"
```

## Environment Setup

### Environment Variables
Create a `.env` file or set in your shell:

```bash
# Configuration
export REPUBLIC_CONFIG=~/.republic/config.toml
export REPUBLIC_DATA_DIR=~/.republic/data

# Logging
export RUST_LOG=republic=info,consensus=debug
export RUST_BACKTRACE=1

# Network
export REPUBLIC_NETWORK=testnet
export REPUBLIC_BOOTSTRAP_NODES=...
```

### Shell Completion
Enable shell completion for better CLI experience:

=== "Bash"
    ```bash
    # Add to ~/.bashrc
    eval "$(republic-cli completion bash)"
    eval "$(republic-node completion bash)"
    ```

=== "Zsh"
    ```bash
    # Add to ~/.zshrc
    eval "$(republic-cli completion zsh)"
    eval "$(republic-node completion zsh)"
    ```

=== "Fish"
    ```bash
    # Add to ~/.config/fish/config.fish
    republic-cli completion fish | source
    republic-node completion fish | source
    ```

## Verification

### Test Installation
```bash
# Check versions
republic-node --version
republic-cli --version

# Test configuration
republic-node config validate ~/.republic/config.toml

# Run basic tests
republic-node test-config
```

### Health Check
```bash
# Start a test node
republic-node start --config ~/.republic/config.toml &

# Wait a moment, then check status
sleep 5
republic-cli node-status

# Stop the node
republic-cli node-stop
```

## Next Steps

After successful installation:

1. **[Quick Start Guide](quickstart.md)** - Run your first node and transaction
2. **[Configuration Guide](configuration.md)** - Detailed configuration options
3. **[Development Setup](../development/setup.md)** - Set up development environment
4. **[Join Testnet](../community/testnet.md)** - Connect to the public testnet

## Troubleshooting

### Common Issues

#### Build Failures
```bash
# Update Rust
rustup update

# Clear cache and rebuild
cargo clean
rm Cargo.lock
cargo build
```

#### Permission Errors
```bash
# Fix permissions on data directory
sudo chown -R $USER:$USER ~/.republic
chmod 755 ~/.republic
```

#### Network Issues
```bash
# Check firewall settings
sudo ufw allow 9000/tcp
sudo ufw allow 8545/tcp

# Test connectivity
telnet bootstrap-node.republic.io 9000
```

#### Dependencies
```bash
# Ubuntu/Debian missing dependencies
sudo apt install build-essential pkg-config libssl-dev

# CentOS/RHEL missing dependencies
sudo yum groupinstall "Development Tools"
sudo yum install openssl-devel
```

### Getting Help

If you encounter issues:

1. Check our [FAQ](../community/faq.md)
2. Search [GitHub Issues](https://github.com/republic-chain/republic/issues)
3. Join our [Discord community](https://discord.gg/republic)
4. Create a new issue with logs and system information

---

**Installation complete!** You're ready to start using Republic blockchain. 🎉