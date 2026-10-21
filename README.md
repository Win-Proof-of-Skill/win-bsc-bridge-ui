# Win Proof of Skill Bridge UI

**Website:** [win-pskill.com](https://win-pskill.com)

Skeleton UI for the Win Proof of Skill (WIN-PSKILL) vault bridge app.

## Fork Notice

This project is a **fork of [liberdus-bsc-bridge-ui](https://github.com/Liberdus/liberdus-bsc-bridge-ui)**. We are grateful to the Liberdus team for the foundation this project builds on.

Win Proof of Skill is an independent project and is not affiliated with or endorsed by the Liberdus team. Original copyright and license notices from upstream are retained.

- Upstream project: [Liberdus/liberdus-bsc-bridge-ui](https://github.com/Liberdus/liberdus-bsc-bridge-ui)
- Wallet module (submodule): [`liberdus-wallet-module`](vendor/liberdus-wallet-module)

## Current Scope

### Functional now

- Header and wallet connect button
- Injected browser wallet flow with explicit wallet selection, silent restore, and disconnect
- Config-driven source-network gating (no switch on connect; no popup network switching)
- Read-only contract wiring to the configured source Vault contract (set in `js/config.js`)
- Contract status tab with live reads for: `getChainId`, `token`, `REQUIRED_SIGNATURES`, `bridgeOutEnabled`, `halted`, `maxBridgeOutAmount`, `getVaultBalance`, `signers(0..3)`

### Placeholder only (coming next)

- Bridge Out interaction flow
- Multisig operation request and signature flows
- Action tab business logic

> **Note:** The contract address shipped with the upstream Liberdus repository belongs to a Liberdus deployment. Replace it in `js/config.js` with your own Win Proof of Skill Vault address before use.

## Local Run

Use any static server (ES modules require HTTP, not `file://`).

Clone with submodules:

```bash
git clone --recurse-submodules <YOUR-WIN-PSKILL-BRIDGE-UI-REPO-URL> win-pskill-bridge-ui
cd win-pskill-bridge-ui
python3 -m http.server 8080
```

If the repo is already cloned:

```bash
git submodule update --init --recursive
python3 -m http.server 8080
```

Open `http://localhost:8080`.

## Project Layout

- `index.html`: app shell
- `css/`: shared reference styles
- `js/app.js`: bootstrap and global manager wiring
- `js/wallet/`: injected-wallet discovery, connection, and network management (via [`liberdus-wallet-module`](vendor/liberdus-wallet-module))
- `docs/SUBMODULE.md`: submodule maintenance guide
- `js/contracts/contract-manager.js`: ABI, provider, and contract wiring
- `js/components/`: tabs, header, and toast components
- `abi/source-vault.json`: source Vault ABI
- `abi/destination-liberdus.json`: destination token ABI

## Configuration

Edit `js/config.js` to change network and contract settings:

- `CONFIG.RUNTIME.PROFILE` selects `dev` or `prod`
- `PROFILES.<profile>.SOURCE_NETWORK` and `PROFILES.<profile>.DESTINATION_NETWORK`
- `PROFILES.<profile>.SOURCE_CONTRACT` and `PROFILES.<profile>.DESTINATION_CONTRACT`
- `PROFILES.<profile>.BRIDGE.OBSERVER_URL`
- Runtime source aliases `CONFIG.NETWORK` and `CONFIG.CONTRACT` are derived from `SOURCE_*` for source-chain internals
- Canonical bridge runtime config lives at `CONFIG.BRIDGE.CHAINS.{SOURCE,DESTINATION}` and `CONFIG.BRIDGE.CONTRACTS.{SOURCE,DESTINATION}`

Wallet connect only requests account access. If the wallet is on the wrong chain, the app stays connected, and transaction actions will prompt a switch to the configured source network when used.

## Next Implementation Steps

1. Implement the Bridge Out transaction path (`approve` + `bridgeOut`).
2. Implement operations tab actions (`requestOperation`, `submitSignature`).
3. Add richer success and error transaction UX and status polling.

## Contributing

Issues and pull requests are welcome. If a fix or feature is relevant to the shared codebase, please consider contributing it upstream to [Liberdus/liberdus-bsc-bridge-ui](https://github.com/Liberdus/liberdus-bsc-bridge-ui) as well.

## Links

- Website: [win-pskill.com](https://win-pskill.com)
- Upstream: [github.com/Liberdus/liberdus-bsc-bridge-ui](https://github.com/Liberdus/liberdus-bsc-bridge-ui)

## License

This project inherits the license of the upstream Liberdus repository. See the `LICENSE` file for details.
