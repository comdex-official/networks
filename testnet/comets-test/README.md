# comets-test
> This is comdex testnet chain

> GENESIS PUBLISHED

> PEERS PUBLISHED

1st testnet for comdex-official/comdex application.

## Hardware Requirements
* **Minimal**
    * 4 GB RAM
    * 100 GB SSD
    * 3.2 x4 GHz CPU
* **Recommended**
    * 8 GB RAM
    * 100 GB NVME SSD
    * 4.2 GHz x6 CPU

## Operating System
* Linux/Windows/MacOS(x86)
* **Recommended**
    * Linux(x86_64)

## Installation Steps
>Prerequisite: go1.17+ required. [ref](https://golang.org/doc/install)

   Append the below lines to the file ${HOME}/.bashrc and execute the command source ${HOME}/.bashrc to reflect in the current Terminal session
   ```shell
   export GOROOT=/usr/local/go
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
git checkout v0.1.0
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
3659590cd1466671a49421089e55f1392e1cad0e@15.207.189.210:26656,8b1ccf5cf3a3ba65ee074f46ea8c6c164d867104@52.201.166.91:26656,5307ce50bd8a6f7bb5a922e3f7109b5f3241c425@13.51.118.56:26656,9c25a7ab94a315f683c3693e17aec6b2c91c851c@52.77.115.73:26656
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
3659590cd1466671a49421089e55f1392e1cad0e@15.207.189.210:26656,8b1ccf5cf3a3ba65ee074f46ea8c6c164d867104@52.201.166.91:26656,5307ce50bd8a6f7bb5a922e3f7109b5f3241c425@13.51.118.56:26656,9c25a7ab94a315f683c3693e17aec6b2c91c851c@52.77.115.73:26656
```

* Copy below value as minimum-gas-prices in ${HOME}/.comdex/config/app.toml
```shell
0.2ucmdx
```

### Bootstrap node from state-sync snapshot 

```
curl -s https://comets.rpc.comdex.one/status | \ 
 jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
```

* Example output:
```
{
  "trust_height": "3549879",
  "trust_hash": "461420F85D8A7A9833B5A1C1E7FCC461AC10247B840C7DD3BB53AC687E3AC0BB"
}
```

##### Set options in config.toml

* Set persistent-peers in config.toml. These are the list of peers in which there are snapshots.
```
persistent_peers = "3659590cd1466671a49421089e55f1392e1cad0e@15.207.189.210:26656,8b1ccf5cf3a3ba65ee074f46ea8c6c164d867104@52.201.166.91:26656,5307ce50bd8a6f7bb5a922e3f7109b5f3241c425@13.51.118.56:26656,9c25a7ab94a315f683c3693e17aec6b2c91c851c@52.77.115.73:26656"
```

* Set trust_height and trust_hash values from rpc/status output:
(If it throws a port error, append :443 to each rpc url's suffix)
```
[statesync]
enable = true
rpc_servers = "https://comets.rpc.comdex.one,https://comets.rpc.comdex.one"
trust_height = 3549879
trust_hash = "461420F85D8A7A9833B5A1C1E7FCC461AC10247B840C7DD3BB53AC687E3AC0BB"
trust_period = "168h0m0s"
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
This chain is currently running on Comdex [v0.0.2](https://github.com/comdex-official/comdex/releases/tag/v0.1.0)

>Note: If your node is running on an older version of the application, please update it to this version at the earliest to avoid being exposed to security vulnerabilities /defects.

## Explorer
The explorer for this chain is hosted [TestNet Explorer](https://comets-test.comdex.one/)

## Genesis Time
The genesis transactions sent before 1200HRS UTC 15th October 2021 will be used to publish the genesis_final.json at 1400HRS UTC 15th October 2021 and then start the chain at 14.30UTC once the quorum is reached.
