# Configuration Guide

This guide covers configuration options for Republic blockchain nodes once implementation begins.

!!! note "Development Status"
    Republic is currently in Phase 0 (Design & Specification). Configuration options described here are planned features for future implementation phases.

## Overview

Republic will support flexible configuration through TOML files, environment variables, and command-line arguments. This allows operators to customize their node behavior for different network environments and use cases.

## Configuration File Structure

The main configuration file will be `config.toml` located in the node's data directory:

```toml
# Republic Node Configuration
# Phase 1+ Implementation

[node]
# Node identity and networking
node_id = "auto-generated"
data_dir = "./data"
bind_address = "0.0.0.0:9000"
external_address = "auto-detect"

[rpc]
# JSON-RPC server configuration
enabled = true
bind_address = "127.0.0.1:8545"
cors_origins = ["*"]
max_connections = 100

[consensus]
# Consensus algorithm parameters
algorithm = "leader-rotation-bft"
block_time = 2000  # milliseconds
max_validators = 100
validator_key_file = "./validator.json"

[network]
# P2P networking configuration
max_peers = 50
bootstrap_nodes = []
discovery_enabled = true
nat_traversal = true

[storage]
# Database configuration
backend = "rocksdb"
cache_size = "256MB"
compression = "lz4"
```

## Network Configurations

### Development Network
For local development and testing:

```toml
[network]
network_id = "devnet"
bootstrap_nodes = []
max_peers = 10

[consensus]
block_time = 1000  # Faster blocks for development
```

### Testnet Configuration
For connecting to the public testnet:

```toml
[network]
network_id = "testnet"
bootstrap_nodes = [
    "/ip4/testnet-1.republic.io/tcp/9000",
    "/ip4/testnet-2.republic.io/tcp/9000"
]

[consensus]
block_time = 2000
```

### Mainnet Configuration
For production mainnet deployment:

```toml
[network]
network_id = "mainnet"
bootstrap_nodes = [
    "/ip4/mainnet-1.republic.io/tcp/9000",
    "/ip4/mainnet-2.republic.io/tcp/9000"
]

[consensus]
block_time = 5000  # Conservative block time
```

## Environment Variables

Configuration can be overridden using environment variables:

```bash
# Node configuration
export REPUBLIC_DATA_DIR="/var/lib/republic"
export REPUBLIC_BIND_ADDRESS="0.0.0.0:9000"

# Network configuration
export REPUBLIC_NETWORK_ID="mainnet"
export REPUBLIC_MAX_PEERS="100"

# Logging
export RUST_LOG="republic=info,consensus=debug"
export RUST_BACKTRACE="1"
```

## Command-Line Arguments

Key settings can be specified via command line:

```bash
# Start node with custom configuration
republic-node start \
  --config /path/to/config.toml \
  --data-dir /var/lib/republic \
  --network mainnet \
  --bind-address 0.0.0.0:9000

# Override specific settings
republic-node start \
  --config config.toml \
  --rpc-address 127.0.0.1:8545 \
  --max-peers 50 \
  --block-time 3000
```

## Validator Configuration

For nodes participating in consensus:

```toml
[validator]
enabled = true
key_file = "./validator-key.json"
stake_amount = 1000000  # Initial stake

[slashing]
downtime_jail_duration = "24h"
slash_fraction_double_sign = 0.05
slash_fraction_downtime = 0.01
```

## Monitoring and Metrics

Configuration for monitoring and observability:

```toml
[metrics]
enabled = true
bind_address = "127.0.0.1:9090"
namespace = "republic"

[logging]
level = "info"
format = "json"
file = "./logs/node.log"
max_size = "100MB"
max_files = 10

[telemetry]
enabled = true
endpoint = "https://telemetry.republic.io"
```

## Security Configuration

Security-related settings:

```toml
[security]
# Rate limiting
max_requests_per_second = 100
max_connections_per_ip = 10

# API access control
rpc_whitelist = ["127.0.0.1", "10.0.0.0/8"]
admin_endpoints_enabled = false

# Cryptographic settings
signature_scheme = "ed25519"
hash_algorithm = "sha256"
```

## Advanced Configuration

### State Management
```toml
[state]
cache_size = "1GB"
snapshot_interval = 1000  # blocks
pruning_enabled = true
pruning_keep_recent = 10000  # blocks
```

### Performance Tuning
```toml
[performance]
worker_threads = 8
transaction_pool_size = 10000
block_cache_size = 100
max_block_size = "1MB"
```

### Development Features
```toml
[development]
debug_mode = false
profiling_enabled = false
test_mode = false
mock_time = false
```

## Configuration Validation

Republic will provide tools to validate configuration:

```bash
# Validate configuration file
republic-node config validate config.toml

# Generate default configuration
republic-node config generate --network testnet

# Show current configuration
republic-node config show
```

## Configuration Precedence

Settings are applied in the following order (highest to lowest priority):

1. Command-line arguments
2. Environment variables
3. Configuration file
4. Default values

## Common Configuration Patterns

### High-Performance Node
```toml
[performance]
worker_threads = 16
cache_size = "4GB"

[network]
max_peers = 100

[storage]
compression = "none"  # Trade storage for speed
```

### Resource-Constrained Node
```toml
[performance]
worker_threads = 2
cache_size = "128MB"

[network]
max_peers = 20

[state]
pruning_enabled = true
```

### Archive Node
```toml
[state]
pruning_enabled = false
snapshot_interval = 5000

[storage]
compression = "zstd"  # High compression
```

## Troubleshooting

### Common Configuration Issues

**Port Already in Use**
```bash
# Check what's using the port
lsof -i :8545
# Change port in configuration
```

**Permission Errors**
```bash
# Ensure data directory is writable
chmod 755 /path/to/data/dir
chown user:group /path/to/data/dir
```

**Network Connectivity**
```bash
# Test bootstrap node connectivity
telnet bootstrap-node.republic.io 9000
```

## Next Steps

Once Republic enters implementation phases:

1. **[Quick Start](quickstart.md)** - Get a node running quickly
2. **[Installation](installation.md)** - Detailed installation guide
3. **[Contributing](../development/contributing.md)** - Help develop Republic

---

*Configuration options will be finalized during Phase 1 implementation.*