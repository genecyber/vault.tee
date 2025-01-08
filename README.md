# Vault TEE

A secure vault implementation with Trusted Execution Environment (TEE) support for managing cryptocurrency keys and addresses across multiple networks.

## Features

- Multi-network cryptocurrency address generation and management
- Secure key derivation using BIP39/BIP44 standards
- TEE integration for enhanced security
- Support for multiple cryptocurrency networks including:
  - Bitcoin (BTC)
  - Bitcoin Taproot (TAP)
  - Ethereum (ETH)
  - Litecoin (LTC)
  - Namecoin (NMC)
  - Dogecoin (DOGE)
  - Bitcoin Cash (BCH)
  - Monacoin (MONA)
  - Stacks (STX)
  - DigiByte (DGB)
  - Belacoin (BEL)
  - Tezos (XTZ)

## Installation

```bash
npm install
```

## Building

```bash
# Build the main library and tests
npm run build

# Build the LIT Action components
npm run build:lit
```

## Testing

```bash
# Run all tests
npm test

# Run LIT-specific tests
npm run test:lit

# Run browser tests
npm run test:browser
```

## Usage

### Basic Vault Generation

```javascript
const vault = await CryptoUtils.generateVault();
```

### Generate Vault with Specific Options

```javascript
const vault = await CryptoUtils.generateVault({
    phrase: "your mnemonic phrase",
    network: "BTC",
    pubkey: "optional public key"
});
```

### Multi-Network Address Generation

```javascript
const addresses = await CryptoUtils.generateMultiNetworkAddresses(
    "your phrase here",
    ["BTC", "ETH"]
);
```

### Address Validation

```javascript
const isValid = await CryptoUtils.validateAddress("1abc...", "BTC");
```

## LIT Action Integration

The vault supports secure operations within a Trusted Execution Environment (TEE) through LIT Protocol integration:

```javascript
const response = await litNodeClient.executeJs({
    code: vaultCode,
    jsParams: {
        operation: 'generateAddresses',
        params: {
            phrase: "your mnemonic phrase",
            networkList: ["BTC", "ETH"]
        }
    }
});
```

## LIT Action Integration Status

The LIT Action integration has been successfully implemented and all tests are now passing. The test suite provides comprehensive coverage of the vault functionality:

### Test Suite Coverage

The test suite covers the following areas:
- Known Good Data Tests
  - Seed generation from known phrases
  - Individual address generation for BTC and ETH
  - Multi-network address generation
- Address Generation Tests
  - Response structure validation
  - Address data format verification
  - Network-specific address validation
  - Deterministic generation verification
- Vault Generation Tests
  - Complete vault generation
  - Phrase-based vault generation
  - Unique vault generation verification
- Error Handling Tests
  - Invalid operation handling
  - Missing parameter validation

### Integration Status

All tests are now passing successfully, demonstrating:
1. Proper LIT Action initialization
2. Correct runtime environment setup
3. Successful integration between the vault library and LIT Protocol
4. Proper error handling for edge cases

The only remaining non-critical issue is a Buffer deprecation warning that can be addressed in future updates.

## Implementation Feature Matrix

| Feature                          | Standard Library | LIT Action |
|----------------------------------|-----------------|------------|
| **Known Good Data Tests**        |                 |            |
| - Seed Generation               | ✅ Passing      | ✅ Passing |
| - Bitcoin Address Generation    | ✅ Passing      | ✅ Passing |
| - Taproot Address Generation    | ✅ Passing      | ✅ Passing |
| - Ethereum Address Generation   | ✅ Passing      | ✅ Passing |
| - Tezos Address Generation     | ✅ Passing      | ✅ Passing |
| - Multi-Network Generation     | ✅ Passing      | ✅ Passing |
| **Address Validation**           |                 |            |
| - Known Good Addresses         | ✅ Passing      | ✅ Passing |
| - Invalid Address Rejection    | ✅ Passing      | ✅ Passing |
| **Derivation Path Tests**        |                 |            |
| - Path Verification           | ✅ Passing      | ✅ Passing |
| **End-to-End Tests**             |                 |            |
| - Vault Generation            | ✅ Passing      | ✅ Passing |
| - Deterministic Generation    | ✅ Passing      | ✅ Passing |
| **Error Handling**               |                 |            |
| - Invalid Operations          | ✅ Passing      | ✅ Passing |
| - Missing Parameters         | ✅ Passing      | ✅ Passing |

✅ = Test Passing
❌ = Test Failed
⚪ = Not Implemented

Note: All tests are now passing for both the standard library and LIT Action implementations, demonstrating full feature parity and successful TEE integration.

## Security Features

### Standard Library Implementation
- ✅ Memory wiping for sensitive data
- ✅ Input validation for all parameters
- ✅ Secure random number generation
- ✅ Local-only key operations
- ⚠️ No TEE protection (keys are in process memory)
- ⚠️ No rate limiting
- ⚠️ No access control

### LIT Action Implementation
- ✅ All key operations occur within TEE
- ✅ Private keys never leave the secure environment
- ✅ Memory is securely wiped after operations
- ✅ Access control through PKP ownership
- ✅ Rate limiting protection
- ✅ Comprehensive input validation
- ✅ Network isolation through TEE

## Command Line Interface

The project includes a CLI tool with both interactive and command-line modes:

### Interactive Mode
```bash
./cli.js
```

Interactive mode provides a guided experience with the following operations:
- Generate complete vault
- Generate for specific network
- Get address
- Get public key
- Get private key
- List supported networks

For each operation, the CLI will guide you through:
1. Selecting the network (if applicable)
2. Choosing input method:
   - Generate new
   - Use existing phrase
   - Use entropy
   - Use public key (for address generation)
3. Setting output format (json/text)

### Command Line Mode
```bash
# Generate a complete vault
./cli.js generate [options]

Options:
  -p, --phrase <phrase>    Mnemonic phrase to use
  -e, --entropy <entropy>  Entropy to use
  -n, --network <network>  Specific network to generate for
  -k, --pubkey <pubkey>    Public key to derive address from
  --format <format>        Output format (json or text)
```

### Features
- ✨ Full interactive mode with guided prompts
- 🔄 Continuous operation (option to perform multiple actions)
- 🎯 Network-specific operations
- 📄 Flexible input methods:
  - New generation
  - Existing phrase
  - Entropy
  - Public key
- 🔍 Multiple output formats (JSON/Text)
- ⚡ Direct command mode for automation
- 🛡️ Error handling and validation

Note: The CLI operates using the standard library implementation, not the TEE-protected LIT Action implementation. Use caution when handling private keys.

## Development

- Built with Node.js and modern JavaScript
- Uses WebPack for bundling
- Comprehensive test suite with Mocha
- Browser compatibility through bundled distributions

## Scripts

- `npm start`: Start the application
- `npm test`: Run test suite
- `npm run build`: Build using webpack
- `npm run build:lit`: Build LIT Action components
- `npm run test:browser`: Run browser-based tests
- `npm run serve`: Start HTTP server for development

## License

This project is proprietary software. All rights reserved.

## Security Considerations

1. Never store private keys in plaintext
2. Always use secure memory wiping when possible
3. Avoid logging key material
4. Use cryptographically secure random number generation
5. Validate entropy length and format
6. Never reuse entropy values
7. Never transmit private keys over the network
8. Use secure channels for all communication
9. Validate all inputs before processing

## For AI Tools and Assistants

### Project Structure and Philosophy

#### Import Strategy
This project uses CommonJS (`require`) imports for maximum compatibility and simplicity:
```javascript
const CryptoUtils = require('./src/crypto-utils');
const { bitcoin } = require('bitcoinjs-lib');
```

#### TypeScript Usage
TypeScript is intentionally limited to the LIT Action pre-bundling phase only. This decision is based on:
- Maintaining simplicity in the core library
- Avoiding unnecessary build complexity
- Preserving direct debugging capabilities
- Keeping the codebase accessible to all contributors

TypeScript should only be introduced when absolutely necessary for type safety in complex integrations (like the LIT Protocol interface).

#### Debugging Approach
When debugging issues:
1. **Always start with package.json**
   - Review dependencies and their versions
   - Check npm scripts for build and test flows
   - Understand the project's entry points

2. **Follow Test Evidence**
   - Note which tests are passing vs failing
   - Use passing tests as reference implementations
   - Trace test data through the system

3. **Implementation Investigation**
   - Follow the logic flow from entry points
   - Compare working implementations with failing ones
   - Look for initialization and setup differences

4. **Infrastructure Stability**
   - Avoid infrastructure changes when debugging
   - Don't modify working patterns or flows
   - Focus on root causes rather than symptoms

#### Key Files for Context
- `package.json`: Project configuration and dependencies
- `test/**/*.test.js`: Test suites showing expected behavior
- `src/index.js`: Main entry point and API surface
- `src/lib/crypto.js`: Core cryptographic operations
- `src/lib/networks.js`: Network configurations and parameters
- `src/lib/taproot.js`: Taproot-specific implementations
- `src/lit/index.ts`: LIT Protocol TEE integration (TypeScript)
- `src/lit/polyfills.js`: Environment compatibility layer

#### Common Pitfalls to Avoid
1. Don't suggest TypeScript migrations for working code
2. Avoid suggesting dependency updates unless absolutely necessary
3. Don't recommend architectural changes when debugging specific features
4. Maintain the existing import system unless there's a critical reason to change

#### When Implementing New Features
1. Reference existing passing tests for patterns
2. Follow the established module format
3. Use CommonJS imports consistently
4. Add tests that mirror the existing structure

Remember: This project prioritizes simplicity, maintainability, and direct debugging capability over modern build tooling complexity.
