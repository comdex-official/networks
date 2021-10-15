# comets-test
> This is comdex testnet chain

> GENESIS PUBLISHED

> PEERS PUBLISHED

1st testnet for comdex-official/comdex application.

## Hardware Requirements
* **Minimal**
    * 1 GB RAM
    * 25 GB HDD
    * 1.4 GHz CPU
* **Recommended**
    * 2 GB RAM
    * 100 GB HDD
    * 2.0 GHz x2 CPU

## Operating System
* Linux/Windows/MacOS(x86)
* **Recommended**
    * Linux(x86_64)

## Installation Steps
>Prerequisite: go1.17+ required. [ref](https://golang.org/doc/install)

   Append the below lines to the file ${HOME}/.bashrc and execute the command source ${HOME}/.bashrc to reflect in the current Terminal session
   ```shell
   export GOROOT=/usr/lib/go
   export GOPATH=${HOME}/go
   export GOBIN=${GOPATH}/bin
   export PATH=${PATH}:${GOROOT}/bin:${GOBIN}
   ```

>Prerequisite: git. [ref](https://github.com/git/git)

>Optional requirement: GNU make. [ref](https://www.gnu.org/software/make/manual/html_node/index.html)

* Clone git repository
```shell
git clone https://github.com/comdex-official/comdex.git
```
* Checkout latest tag
```shell
cd comdex
git fetch --tags
git checkout v0.0.2
```
* Install
```shell
make all
```

### Generate keys

`comdex keys add [key_name]`

or

`comdex keys add [key_name] --recover` to regenerate keys with your [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039) mnemonic


## Validator setup instructions

### GenTx : No Accepting Now.

* [Install](#installation-steps) comdex core application
* Initialize node
```shell
comdex init {{NODE_NAME}} --chain-id comets-test
comdex add-genesis-account {{KEY_NAME}} 1000000000ucmdx
comdex gentx {{KEY_NAME}} 10000000ucmdx \
--chain-id comets-test \
--moniker="{{VALIDATOR_NAME}}" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.07 \
--details="XXXXXXXX" \
--security-contact="XXXXXXXX" \
--website="XXXXXXXX"
```
* Copy the contents of `${HOME}/.comdex/config/gentx/gentx-XXXXXXXX.json`.
* Fork the [repository](https://github.com/comdex-official/networks/)
* Create a file `gentx-{{VALIDATOR_NAME}}.json` under the testnet/comets-test/gentxs folder in the forked repo, paste the copied text into the file. Find reference file gentx-examplexxxxxxxx.json in the same folder.
* Run `comdex tendermint show-node-id` and copy your nodeID.
* Run `ifconfig` or `curl ipinfo.io/ip` and copy your publicly reachable IP address.
* Create a file `peers-{{VALIDATOR_NAME}}.json` under the testnet/comets-test/peers folder in the forked repo, paste the copied text from the last two steps into the file. Find reference file sample-peers.json in the same folder.
* Create a Pull Request to the `main` branch of the [repository](https://github.com/comdex-official/networks)
>**NOTE:** The Pull Request will be merged by the maintainers to confirm the inclusion of the validator at the genesis. The final genesis file will be published under the file testnet/comets-test/genesis_final.json.
* Replace the contents of your `${HOME}/.comdex/config/genesis.json` with that of testnet/comets-test/genesis_final.json.
* Copy below node as `persistent_peers` or `seeds` in `${HOME}/.comdex/config/config.toml`
 
```shell
479717ccfc49c626654b9a126adf73db2407365d@15.207.189.210:26656,9b85c06cc7e66a766784bb98b93539428011e76d@52.201.166.91:25565,b2315a0f1eab593a5761f5175be2d493b12cb57d@13.51.118.56:26656,9c25a7ab94a315f683c3693e17aec6b2c91c851c@52.77.115.73:26656
```
* Copy below value as minimum-gas-prices in ${HOME}/.comdex/config/app.toml
```shell
0.2ucmdx
```

* Start comdex by running below command or create a `systemd` service to run comdex in background.
```shell
comdex start
```


### Become a validator

* [Install](#installation-steps) comdex core application
* Initialize node
```shell
comdex init {{NODE_NAME}}
```
* Replace the contents of your `${HOME}/.comdex/config/genesis.json` with that of testnet/comets-test/genesis_final.json from the `main` branch of [repository](https://github.com/comdex-official/networks).
* Copy below node as `persistent_peers` or `seeds` in `${HOME}/.comdex/config/config.toml`
```shell
479717ccfc49c626654b9a126adf73db2407365d@15.207.189.210:26656,9b85c06cc7e66a766784bb98b93539428011e76d@52.201.166.91:25565,b2315a0f1eab593a5761f5175be2d493b12cb57d@13.51.118.56:26656,9c25a7ab94a315f683c3693e17aec6b2c91c851c@52.77.115.73:26656
```

* Copy below value as minimum-gas-prices in ${HOME}/.comdex/config/app.toml
```shell
0.2ucmdx
```

* Start comdex by running below command or create a `systemd` service to run comdex in background.
```shell
comdex start
```
* Acquire $ucmdx by sending a message to the validators channel in [Discord](https://discord.gg/gH6RTrnexk).
* Run `comdex tendermint show-validator` and copy your consensus public key.
* Send a create-validator transaction
```
comdex tx staking create-validator \
--from {{KEY_NAME}} \
--amount XXXXXXXXucmdx \
--pubkey $(comdex tendermint show-validator) \
--chain-id comets-test \
--moniker="{{VALIDATOR_NAME}}" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.07 \
--min-self-delegation="1" \
--details="XXXXXXXX" \
--security-contact="XXXXXXXX" \
--website="XXXXXXXX"
```

## Version
This chain is currently running on Comdex [v0.0.2](https://github.com/comdex-official/comdex/releases/tag/v0.0.2)
Commit Hash: 36e59abc8aff22a8eea2e851ee19e497c7f754ea
>Note: If your node is running on an older version of the application, please update it to this version at the earliest to avoid being exposed to security vulnerabilities /defects.

## Binary
The binary can be downloaded from [here](https://github.com/comdex-official/comdex/releases/tag/v0.0.2).

## Explorer
The explorer for this chain is hosted [here](::To be published)

## Genesis Time
The genesis transactions sent before 1200HRS UTC 15th October 2021 will be used to publish the genesis_final.json at 1400HRS UTC 15th October 2021 and then start the chain at 14.30UTC once the quorum is reached.
