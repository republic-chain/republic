# Contributing to Republic

Thank you for your interest in contributing to Republic! This guide will help you get started with contributing to our democratic blockchain project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contributing Process](#contributing-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Community](#community)

## Code of Conduct

By participating in this project, you agree to abide by our [Code of Conduct](../community/code-of-conduct.md). We are committed to providing a welcoming and inclusive environment for all contributors.

## Getting Started

### Prerequisites

- Rust 1.70.0 or later
- Git
- Familiarity with blockchain concepts
- Understanding of consensus algorithms (helpful)

### First Contributions

Good first contributions include:

- Documentation improvements
- Writing tests
- Fixing typos or small bugs
- Adding code comments
- Improving error messages

Look for issues labeled `good-first-issue` or `help-wanted`.

## Development Setup

1. **Fork and Clone**
   ```bash
   git clone https://github.com/republic-chain/republic.git
   cd republic
   ```

2. **Install Dependencies**
   ```bash
   cargo build
   ```

3. **Run Tests**
   ```bash
   cargo test
   ```

4. **Check Code Style**
   ```bash
   cargo fmt --check
   cargo clippy -- -D warnings
   ```

## Contributing Process

### 1. Issue First

For significant changes, please create an issue first to discuss:
- New features
- Breaking changes
- Major refactoring
- Performance improvements

### 2. Branch Naming

Use descriptive branch names:
- `feature/add-consensus-algorithm`
- `fix/transaction-validation-bug`
- `docs/improve-setup-guide`
- `perf/optimize-state-updates`

### 3. Commits

- Write clear, descriptive commit messages
- Use conventional commit format when possible:
  ```
  feat: add leader rotation to consensus
  fix: resolve memory leak in p2p layer
  docs: update API documentation
  test: add integration tests for mempool
  ```

### 4. Pull Requests

- Fill out the PR template completely
- Keep PRs focused and reasonably sized
- Include tests for new functionality
- Update documentation as needed
- Ensure CI passes

### 5. Code Review

- Address reviewer feedback promptly
- Ask questions if feedback is unclear
- Be open to suggestions and improvements
- Maintain a collaborative attitude

## Coding Standards

### Rust Guidelines

- Follow Rust naming conventions
- Use `cargo fmt` for formatting
- Address all `cargo clippy` warnings
- Write idiomatic Rust code
- Prefer explicit error handling over panics

### Code Organization

- Keep modules focused and cohesive
- Use clear, descriptive names
- Add documentation for public APIs
- Include examples in documentation
- Organize imports consistently

### Error Handling

- Use appropriate error types
- Provide meaningful error messages
- Handle errors at appropriate levels
- Don't ignore errors silently

### Performance

- Avoid premature optimization
- Profile before optimizing
- Consider memory usage
- Use appropriate data structures
- Document performance characteristics

## Testing Guidelines

### Test Categories

1. **Unit Tests**
   - Test individual functions/methods
   - Mock external dependencies
   - Fast and isolated

2. **Integration Tests**
   - Test component interactions
   - Use real implementations where possible
   - Cover common workflows

3. **Network Simulation Tests**
   - Test consensus scenarios
   - Simulate network partitions
   - Test Byzantine behavior

### Test Requirements

- All new code must include tests
- Maintain or improve test coverage
- Tests should be deterministic
- Use descriptive test names
- Include both positive and negative test cases

### Running Tests

```bash
# Run all tests
cargo test

# Run specific test module
cargo test consensus

# Run tests with output
cargo test -- --nocapture

# Run tests in single thread
cargo test -- --test-threads=1
```

## Documentation

### Code Documentation

- Document all public APIs
- Include usage examples
- Explain complex algorithms
- Document error conditions
- Use proper rustdoc formatting

### User Documentation

- Keep documentation up-to-date
- Include practical examples
- Explain configuration options
- Document troubleshooting steps

## Security

### Security Best Practices

- Validate all inputs
- Use secure randomness
- Follow cryptographic best practices
- Avoid timing attacks
- Handle secrets securely

### Reporting Security Issues

**Do not report security vulnerabilities through public GitHub issues.**

Instead, please send an email to security@republic-chain.org with:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

We will respond within 48 hours and work with you to resolve the issue.

## Community

### Communication Channels

- GitHub Issues: Bug reports, feature requests
- GitHub Discussions: General questions, ideas
- GitHub PRs: Code contributions, reviews

### Getting Help

- Check existing issues and documentation first
- Use issue templates when reporting bugs
- Provide minimal reproducible examples
- Be patient and respectful when asking for help

### Recognition

Contributors will be recognized in:
- Release notes for significant contributions
- Contributors list in the repository
- Special recognition for outstanding contributions

## Development Phases

Republic is currently in **Phase 0 (Design & Specification)**. Contributions should align with the current phase:

### Phase 1: Single-node Prototype
- Basic state machine
- Transaction processing
- Simple consensus (single node)

### Phase 2: Multi-node Devnet
- P2P networking
- Multi-node consensus
- Mempool implementation

### Phase 3: Hardening
- Performance optimizations
- Security improvements
- Comprehensive testing

## Legal

By contributing to Republic, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for contributing to Republic! Your efforts help build a better decentralized future.