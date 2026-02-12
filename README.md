# ₿ Bitcoin Vanity Address Generator

[![Rust](https://img.shields.io/badge/Rust-2024%20Edition-orange?logo=rust)](https://www.rust-lang.org/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

> A command-line tool to generate custom Bitcoin addresses (vanity addresses). Written in Rust for optimal performance with multithreading support.

## What is a Vanity Address?

A **vanity address** is a cryptocurrency address that contains a custom pattern chosen by the user. For example:

| Example Address | Pattern |
|---|---|
| `1Love...` | Starts with "Love" |
| `1BTC...` | Starts with "BTC" |

Generation works by **brute force**: the program generates random key pairs in a loop until it finds an address matching the desired pattern.

> **Security Note**: Vanity addresses are safe as long as you are the only one holding the private key. **Never** trust a third-party service to generate a vanity address for you.

## Features

- Bitcoin address generation (P2PKH, Bech32)
- 🔍 Search by **prefix**, **suffix**, or **regular expression**
- **Multithreading** to leverage all CPU cores
- **Real-time statistics**: generation speed, estimated time, attempts
- Export results (private key + address) to a file

## Prerequisites

- [Rust](https://www.rust-lang.org/tools/install) (2024 edition or later)
- Cargo (included with Rust)

## Installation

### From source

```bash
git clone https://github.com/nejos97/bitcoin-vanity-address-generator.git
cd bitcoin-vanity-address-generator
cargo build --release
```

The compiled binary will be located at `target/release/bitcoin-vanity-address-generator`.

### Via Cargo

```bash
cargo install --path .
```

## Usage

### Basic command

```bash
bitcoin-vanity-address-generator [OPTIONS] <PATTERN>
```

### Options

| Option | Short | Description | Default |
|---|---|---|---|
| `--pattern` | `-p` | Pattern to search for in the address | *required* |
| `--format` | `-f` | Bitcoin address format (`p2pkh`, `bech32`) | `p2pkh` |
| `--threads` | `-t` | Number of threads to use | Number of CPU cores |
| `--output` | `-o` | Output file to save the result | *none* |
| `--network` | `-n` | Bitcoin network (`mainnet`, `testnet`) | `mainnet` |

### Examples

#### Generate a Bitcoin address starting with "1Love"

```bash
bitcoin-vanity-address-generator -p Love
```

#### Bech32 (SegWit) address on testnet

```bash
bitcoin-vanity-address-generator -p test -f bech32 -n testnet
```

#### Save the result to a file

```bash
bitcoin-vanity-address-generator -p BTC -o result.txt
```

### Example output

```
Searching for a Bitcoin address starting with "Love"...
Threads: 8 | Network: mainnet | Format: P2PKH

Real-time statistics:
   Speed: 45,230 addresses/sec
   Attempts: 1,523,456
   Elapsed time: 33.7s

Address found!

   Address:     1Love8kRr9iu2vSMFebGY5FGKM4tFbNYVq
   Private key: 5KJvsngHe[...hidden...]
   Public key:  04a1b2c3d4[...]
   Attempts:    1,523,456
   Time:        33.7 seconds
```

## Project Architecture

```
src/
├── main.rs              # Entry point, CLI parsing
├── cli.rs               # CLI argument definitions (clap)
├── generator/
│   ├── mod.rs           # Generation module
│   ├── bitcoin.rs       # Bitcoin key/address generation
│   └── ethereum.rs      # Ethereum key/address generation
├── matcher/
│   ├── mod.rs           # Matching module
│   ├── prefix.rs        # Prefix matching
│   ├── suffix.rs        # Suffix matching
│   └── regex.rs         # Regex matching
├── worker/
│   ├── mod.rs           # Multithreading orchestration
│   └── stats.rs         # Performance statistics
├── output/
│   ├── mod.rs           # Output module
│   ├── console.rs       # Console display / progress bar
│   └── file.rs          # File export
├── crypto/
│   ├── mod.rs           # Cryptographic module
│   ├── secp256k1.rs     # Elliptic curve operations
│   ├── hash.rs          # Hash functions (SHA256, RIPEMD160, Keccak)
│   └── base58.rs        # Base58Check encoding
└── error.rs             # Custom error types
```

## Tests

```bash
# Run all tests
cargo test

# Run tests with detailed output
cargo test -- --nocapture
```

## Key Concepts Learned

This project is an excellent way to learn **Rust** and **Bitcoin fundamentals** simultaneously:

### Rust Side
- Memory management and ownership
- Concurrent programming with `std::thread` and `std::sync`
- Pattern matching and error handling (`Result`, `Option`)
- Traits and generics
- CLI with `clap`
- Unit and integration testing
- Benchmarking with `criterion`

### Bitcoin
- Elliptic curve cryptography (secp256k1)
- Private/public key generation
- Hash functions (SHA-256, RIPEMD-160, Keccak-256)
- Base58Check encoding
- Address formats (P2PKH, Bech32)
- Mainnet vs testnet differences

## License

This project is distributed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for more details.
