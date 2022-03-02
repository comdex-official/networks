# Steps to create Validator after gentx creation

This is a detailed step-by-step guide for setting up a Comdex validator. Please be aware that while it is easy to set up a validating node, running a production-quality validator node with a robust architecture and security features requires an extensive setup.

# Generate a key wallet for your validator

`comdex keys add [key_name]`

or

`comdex keys add [key_name] --recover` to regenerate keys with your [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039) mnemonic

# Prerequisites

- You are familiar with comdex CLI

- You have completed how to run a full Comdex node, which outlines how to install, connect, and configure a node.

- Check if your validator and sentry nodes (if sentry architecture) followed is synced up to the latest block height

   ```comdex status``` 

   If `'catching_up':false`, it has reached the latest block height and in sync with the chain

# Create Validator

```shell
comdex tx staking create-validator \
  --amount=10000000ucmdx \
  --pubkey='$(comdex tendermint show-validator)' \
  --moniker=' ' \ 
  --chain-id=comdex-1 \
  --commission-rate="0.02" \
  --commission-max-rate="0.05" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000" \
  --gas="auto" \
  --gas-prices="0.025ucmdx" \
  --from=(yourkeyname) \
  --node=https://rpc.comdex.one:443 \
  -–gas-adjustment="1.15"
```

If you need further explanation for each of these command flags:


- `amount` flag is the amount you will place in your own validator in ucmdx (in the example, 10000000ucmdx is 10cmdx)
- `pubkey` is the validator public key
- `moniker` is a human readable name you choose for your validator
- `security-contact` is an email your delegates are able to contact you at
- `chain-id` is whatever chain-id you are working with (in the comdex mainnet case it is comdex-1)
- `commission-rate` is the rate you will charge your delegates (in the example above, 2 percent)
- `commission-max-rate` is the most you are allowed to charge your delegates (in the example above, 5 percent)
- `commission-max-change-rate` is how much you can increase your commission rate in a 24 hour period (in the example above, 1 percent per day until reaching the max rate)
- `from` flag is the KEY_NAME you created when initializing the key on your keyring
- `gas-prices` is the amount of gas used to send this create-validator transactiom
- `node` is the RPC endpoint which are using for interacting with the chain

# Edit Validator

```shell
comdex tx staking edit-validator
  --moniker="choose a moniker" \
  --website="https://comdex.one" \
  --identity=(your pgp key from keybase) \
  --details="To the moon!" \
  --chain-id=<chain_id> \
  --gas="auto" \
  --gas-prices="0.0025ucmdx" \
  --from=<key_name> \
  --commission-rate="0.12"
```

Note: The `--identity` can be used as to verify identity with systems like Keybase or UPort for adding your logo. When using Keybase, --identity should be populated with a 16-digit string that is generated with a keybase.io account. It's a cryptographically secure method of verifying your identity across multiple online networks. The Keybase API allows us to retrieve your Keybase avatar.

# View Validator

```shell
comdex query staking validator (your comdexvaloper address)
```

 
