# Quick Start Guide

Get up and running with Republic blockchain in minutes.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Rust 1.70.0 or later**: [Install Rust](https://rustup.rs/)
- **Git**: [Install Git](https://git-scm.com/downloads)
- **Basic blockchain knowledge**: Understanding of blockchain concepts is helpful

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/republic-chain/republic.git
cd republic
```

### 2. Build the Project

```bash
# Build in debug mode
cargo build

# Or build optimized release version
cargo build --release
```

### 3. Run Tests

```bash
# Run all tests
cargo test

# Run with output
cargo test -- --nocapture
```

## Running Your First Node

### 1. Start a Development Node

```bash
# Start a single node for development
cargo run --bin node -- start --config ./config/dev.toml
```

### 2. Check Node Status

In another terminal:

```bash
# Query the current block height
cargo run --bin cli -- get-block-height

# Check node status
cargo run --bin cli -- node-status
```

## Your First Transaction

### 1. Create Test Accounts

```bash
# Generate a new key pair
cargo run --bin cli -- generate-key --output alice.json

# Generate another key pair
cargo run --bin cli -- generate-key --output bob.json
```

### 2. Fund the First Account

For development, the genesis block includes pre-funded accounts. Check the genesis configuration or use the built-in faucet:

```bash
# Check balance of genesis account
cargo run --bin cli -- get-balance --address 0x0000000000000000000000000000000000000001
```

### 3. Send a Transaction

```bash
# Send tokens from alice to bob
cargo run --bin cli -- send-tx \
  --from-key alice.json \
  --to 0xbob_address_here \
  --value 1000 \
  --gas-price 1
```

### 4. Verify the Transaction

```bash
# Check transaction status
cargo run --bin cli -- get-tx --hash 0xtransaction_hash_here

# Check updated balances
cargo run --bin cli -- get-balance --address 0xalice_address_here
cargo run --bin cli -- get-balance --address 0xbob_address_here
```

## Development Network

For multi-node testing, start a 3-node development network:

```bash
# Start the development network
./scripts/start-devnet.sh

# This will start 3 nodes on ports 9001, 9002, 9003
# RPC endpoints will be available on 8545, 8546, 8547
```

## Configuration

### Basic Configuration

Create a custom configuration file:

```toml
# config/custom.toml
[node]
data_dir = "./data"
bind_address = "127.0.0.1:9000"
rpc_address = "127.0.0.1:8545"

[consensus]
block_time = 2000  # milliseconds
validators = [
    "0x1111111111111111111111111111111111111111",
    "0x2222222222222222222222222222222222222222",
    "0x3333333333333333333333333333333333333333"
]

[network]
max_peers = 10
bootstrap_nodes = []
```

### Environment Variables

Set common environment variables:

```bash
export REPUBLIC_CONFIG=./config/custom.toml
export REPUBLIC_DATA_DIR=./data
export RUST_LOG=republic=debug
```

## Next Steps

Now that you have Republic running:

1. **[Read the Architecture Guide](../architecture/overview.md)** - Understand how Republic works
2. **[Explore the API](../api/json-rpc.md)** - Learn about available RPC methods
3. **[Join Development](../development/setup.md)** - Set up a development environment
4. **[Contribute](../community/contributing.md)** - Help build the future of democratic blockchain

## Troubleshooting

### Common Issues

**Build Errors**
```bash
# Update Rust toolchain
rustup update

# Clean and rebuild
cargo clean && cargo build
```

**Port Already in Use**
```bash
# Check what's using the port
lsof -i :8545

# Kill the process or change the port in config
```

**Database Errors**
```bash
# Clear the database
rm -rf ./data

# Restart the node
cargo run --bin node -- start --config ./config.toml
```

### Getting Help

- 📖 **Documentation**: Check our [full documentation](../index.md)
- 🐛 **Issues**: Report bugs on [GitHub Issues](https://github.com/republic-chain/republic/issues)
- 💬 **Discussions**: Join [GitHub Discussions](https://github.com/republic-chain/republic/discussions)
- 📧 **Contact**: Reach out to the [development team](../community/code-of-conduct.md)

---

**Congratulations! You're now running Republic blockchain.** 🎉