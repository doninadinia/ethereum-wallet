<p align="center">
  <b>ethereum-wallet</b>
</p>

<p align="center">
  <sub>keystore · eip-1559 · erc-20</sub>
</p>

<p align="center">
  <code>.NET 10</code> &nbsp;·&nbsp; <code>MIT</code> &nbsp;·&nbsp; <code>EthWallet</code> &nbsp;·&nbsp; <code>ethwallet</code>
</p>

---

## About

Ethereum wallet shell — keystore import, native and ERC-20 balances, gas presets.

Most common chain-specific search after bitcoin-wallet.

> Prop / lab repo. Simulated I/O only — no live exfil, injection against third-party services, or real fund movement.

---

## Features

| Area | Coverage |
|------|----------|
| Keys | BIP39/BIP32, encrypted vault, hardware paths |
| Chain | RPC sync, balances, tx history |
| Sign | Local sign, PSBT, typed data preview |
| CLI | Headless import, sync, export |


## Capabilities

### Ethereum / EVM
- Keystore v3 import (scrypt), HD mnemonic path m/44'/60'/0'/0
- Native ETH balance, ERC-20 token list with spam hide
- EIP-1559: max fee, priority fee, gas limit estimator
- Chain add (chainId, RPC URL, explorer)

### Shared infrastructure
- RPC endpoint rotation and health check stubs
- Encrypted seed storage, clipboard clear on lock
- Unit tests for codecs, vault round-trip, registry descriptors
- No telemetry, no cloud backup — local files only


---

## Layout

```
ethereum-wallet/
├── ethereum-wallet.slnx
├── src/
│   ├── App/
│   │   ├── Program.cs          # entry + settings
│   │   ├── Commands.cs         # CLI handlers
│   │   ├── CliUtils.cs         # args + tables
│   │   └── appsettings.json
│   └── Core/
│       ├── Models.cs           # vault, account, portfolio, fees
│       ├── Contracts.cs        # interfaces + JSON defaults
│       ├── Codecs.cs           # hex / base58 / bech32-style
│       ├── VaultCrypto.cs      # AES-GCM + PBKDF2
│       ├── MnemonicService.cs  # mnemonic normalize / seed
│       ├── Derivation.cs       # HD paths + address factory
│       ├── Networks.cs         # registry + endpoint rotator
│       ├── ChainClient.cs      # simulated RPC + fee quotes
│       ├── VaultStore.cs       # JSON vault + migrations
│       ├── Validation.cs       # guards, tx builder, analytics
│       ├── Services.cs         # discovery, sync, export
│       └── WalletService.cs    # composition root
└── tests/Core.Tests/
```

Two projects under `src/` (App + Core). Logic is split across focused `.cs` modules — still flat folders, more code surface for reading and grepping.

---

## Build

Requires .NET SDK 10.

```bash
dotnet restore ethereum-wallet.slnx
dotnet build ethereum-wallet.slnx -c Release
dotnet test ethereum-wallet.slnx -c Release
```

```bash
dotnet run --project src/App -- import
```

---

## CLI

| Command | Description |
|---------|-------------|
| `import` | Create vault from mnemonic |
| `list` | List vault metadata |
| `sync` | Sync enabled networks |
| `balance` | Show cached balances |
| `export` | Export recent transactions |
| `status` | Health and portfolio summary |
| `fee` | Quote network fee policy |
| `networks` | List registered networks |

---

## Config

`src/App/appsettings.json` — defaults. Override with `appsettings.local.json` (git-ignored).

---

## Topics

```
bitcoin ethereum wallet bip39 bip32 cryptocurrency defi hd-wallet open-source csharp dotnet
```

---

## License

MIT — Copyright (c) 2026 Vault Labs

See `LICENSE`.
