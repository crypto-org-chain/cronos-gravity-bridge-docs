

# Pioneer11 Relayer Deployment Guide

This guide is intended to assist the community validators with setting up Gravity Relayer between `Cronos Pioneer11` and `Ethereum Goerli` testnet.


## Prerequisites

### Ethereum node

You need to have access to the EVM RPC endpoint of an Ethereum node or host your own node with [go-ethereum](https://github.com/ethereum/go-ethereum/) or [openethereum](https://github.com/openethereum/openethereum).

You can use a nodes as a service provider as discussed [here](https://ethereum.org/en/developers/docs/nodes-and-clients/nodes-as-a-service/).


### Binaries

- `gorc`, the gravity bridge orchestrator cli, build instructions can be found [here](gorc-build.md). Alternatively, you can download Linux x86_64 binary from [here](https://github.com/crypto-org-chain/gravity-bridge/releases/tag/v2.0.0-cronos-alpha3)

- Above binaries setup in `PATH`.

## Generate Relayer Keys

You need to prepare one Ethereum account for the relayer. You should transfer some funds to the account, so the relayer can cover the gas fees of message relaying later on.

Please follow the [gorc-keystores](gorc-keystores.md) guide for this step. Note that you will not need a Cronos account to set up the relayer.

## Transfer funds to Relayer accounts

You should transfer funds to the Ethereum account generated earlier. Gravity Bridge is deployed between the `Cronos Pioneer11` and the `Ethereum Goerli` testnet.


## Trial Run Relayer

In order to run the relayer, you will need to set RELAYER_API_URL environment variable to point to Cronos public relayer API:

```bash
export RELAYER_API_URL=https://cronos.org/pioneer11/relayer/relayer
```

To read more about the relayer modes, you can check out [gravity-bridge-relayer-modes.md](gravity-bridge-relayer-modes.md).

To run the relayer:

```bash
gorc -c gorc.toml relayer start \
		--ethereum-key "relayerKeyName" \
		--mode Api
```

The relayer is running now.

## Run Relayer as a Service (Linux only)

To set up the Relayer as a service, you can run:

```bash
bash <(curl -s -L https://github.com/crypto-org-chain/cronos-gravity-bridge-docs/blob/cc55a1b103347d8f54afc0d29ec5d7be9e3dc016/pioneer11/systemd/setup-gorc-service.sh) -t relayer
```

You will be prompted for your ethereum key name set up earlier. After the service is created, you can run:

```bash
sudo systemctl start gorc
```

To view the logs:

```bash
journalctl -u gorc -f
```

## Reference:

### Contracts

```
Gravity: 0xAA1Ca02916a7C30d827226450540b6240E4a93CF (on Goerli) - To be used in `gorc.toml`
Eth Gravity Wrapper: 0xf9D339270A6369776dE9eA6F57222ea01c368D2F (on Goerli)
CroBridge: 0x38F05eb0c209c4c9Fe2D6E237f03ec503f65F088 (on Pioneer11)
```

Here are the deployed token mappings:

| ERC20 token | Goerli  | Pioneer11  |
| ------- | --- | --- |
| USDC | 0xD87Ba7A50B2E7E660f678A895E4B72E7CB4CCd9C | 0xE1E19D235D344De08Ab845e78656EB289a32875F |
| WETH | 0xB4FBF271143F4FBf7B91A5ded31805e42b2208d6 | 0x770b0139024e2C92D2E5dD4eA5DA5B52A32dA33f |
| USDT | 0xe802376580c10fE23F027e1E19Ed9D54d4C9311e | 0x23Cb66F1f767984520B29441f88FD92E896A2dF7 |
| WBTC | 0xC04B0d3107736C32e19F1c62b2aF67BE61d63a05 | 0x62ea2B07757FFA30daF475983976cDf7A4A27914 |
| DAI  | 0xdc31Ee1784292379Fbb2964b3B9C4124D8F89C60 | 0xB209F2ACF6783818D0BC5c74487BF052295F82a2 |

### Code

1. Gravity :
   - https://github.com/crypto-org-chain/gravity-bridge/blob/v2.0.0-cronos-alpha3/solidity/contracts/Gravity.sol

2. Eth Gravity Wrapper :
   -  https://github.com/crypto-org-chain/gravity-bridge/blob/v2.0.0-cronos-alpha3/solidity/contracts/EthGravityWrapper.sol

3. CroBridge :
   - https://github.com/crypto-org-chain/cronos/blob/v0.8.0-gravity-alpha3/integration_tests/contracts/contracts/CroBridge.sol

