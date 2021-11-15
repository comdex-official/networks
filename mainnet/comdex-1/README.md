# comdex-1
> This is comdex Main net chain

> PRE-GENESIS PUBLISHED :: Please use this genesis to generate gentx

> FINAL GENESIS NOT PUBLISHED

> PEERS NOT PUBLISHED

1st main net for comdex-official/comdex application.

## Hardware Requirements
* **Minimal**
    * 4 GB RAM
    * 100 GB SSD
    * 3.2 x4 GHz CPU
* **Recommended**
    * 8 GB RAM
    * 1 TB NVME SSD
    * 3.2 GHz x4 CPU

## Operating System

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
git checkout v0.0.3
```
* Install
```shell
make all
```

### Generate keys

`comdex keys add [key_name]`

or

`comdex keys add [key_name] --recover` to regenerate keys with your [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039) mnemonic


## Validator setup instructions for validators participating in the genesis

### GenTx : Accepting Now.

* [Install](#installation-steps) comdex core application
* Initialize node
* Replace the contents of your `${HOME}/.comdex/config/genesis.json` with that of mainnet/comdex-1/pre_genesis.json.

```shell
wget https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/comdex-1/pre_genesis.json
```

* Warning :: Please do not add more than ```10000000ucmdx``` in the genesis account and while gentx.

```shell
comdex init "{{NODE_NAME}}" --chain-id comdex-1
comdex add-genesis-account "{{KEY_NAME}}" 10000000ucmdx
comdex gentx "{{KEY_NAME}}" 10000000ucmdx \
--chain-id comdex-1 \
--moniker="{{VALIDATOR_NAME}}" \
--commission-max-change-rate=0.01 \
--commission-max-rate=0.01 \
--commission-rate=0.01 \
--details="XXXXXXXX" \
--security-contact="XXXXXXXX" \
--website="XXXXXXXX"
```

* Copy the contents of `${HOME}/.comdex/config/gentx/gentx-XXXXXXXX.json`.
* Fork the [repository](https://github.com/comdex-official/networks/)
* Create a file `gentx-{{VALIDATOR_NAME}}.json` under the mainnet/comdex-1/gentxs folder in the forked repo, paste the copied text into the file. Find reference file gentx-examplexxxxxxxx.json in the same folder.
* Run `comdex tendermint show-node-id` and copy your nodeID.
* Run `ifconfig` or `curl ipinfo.io/ip` and copy your publicly reachable IP address.
* Create a file `peers-{{VALIDATOR_NAME}}.json` under the mainnet/comdex-1/peers folder in the forked repo, paste the copied text from the last two steps into the file. Find reference file sample-peers.json in the same folder.
* Create a Pull Request to the `main` branch of the [repository](https://github.com/comdex-official/networks)
>**NOTE:** The Pull Request will be merged by the maintainers to confirm the inclusion of the validator at the genesis. The final genesis file will be published under the file mainnet/comdex-1/genesis_final.json.
* Replace the contents of your `${HOME}/.comdex/config/genesis.json` with that of mainnet/comdex-1/genesis_final.json.
* Copy below node as `persistent_peers` or `seeds` in `${HOME}/.comdex/config/config.toml`
 
```shell
TO BE PUBLISHED
```
* Copy below value as minimum-gas-prices in ${HOME}/.comdex/config/app.toml
```shell
0.025ucmdx
```

* Start comdex by running below command
```shell
comdex start
```

* Creating a `systemd` service to run comdex in background.

Sample systemd file

```toml
# /etc/systemd/system/comdex.service
[Unit]
Description=Comdex Node
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu
ExecStart=/home/ubuntu/go/bin/comdex start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

## Version
This chain is currently running on Comdex [v0.0.3](https://github.com/comdex-official/comdex/releases/tag/v0.0.3)
Commit Hash: c7c19f6366b859373c3ed8c06d4c760f2649feae
>Note: If your node is running on an older version of the application, please update it to this version at the earliest to avoid being exposed to security vulnerabilities /defects.

## Binary
We will be publishing binary for the mainnet.

## Explorer
The explorer for this chain is hosted [MainNet Explorer](TO BE PUBLISHED)

## Genesis Time
The genesis transactions sent before 1200HRS UTC 18th November 2021 will be used to publish the genesis_final.json on or before 1200HRS UTC 20th november 2021 and then start the chain at 14.30UTC (Tentative). We will be announcing on all the platforms for the same.
