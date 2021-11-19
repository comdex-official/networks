# comdex-1 :: First main net.

## Genesis Time
The genesis transactions sent before 1200HRS UTC 18th November 2021 will be used to publish the genesis.json on or before 1000HRS UTC 20th november 2021 and then start the chain at 1400UTC. We will be announcing on all the platforms for the same. Please join our [Discord](https://discord.gg/gH6RTrnexk) and [Comdex-Validators-Group](https://t.me/joinchat/d3WeO-OzYG0zZGFl) to stay updated.

Thank you all for submiting the gentxs. We have received more than 75 gentxs, reviewed and accepted the gentxs of the validators as per the discussion our team had with validators.

> PRE-GENESIS PUBLISHED :: Make sure to replace this genesis with the final genesis, after following the instructions on [Part-2](#launch-instructions).

> FINAL GENESIS :: PUBLISHED

> PEERS :: PUBLISHED

> SEEDS :: PUBLISHED

## First part was to submit the gentx. WHICH IS CLOSED NOW.
## Please follow instructions in [Part-2](#launch-instructions) for the next steps.

## Hardware Requirements
* **Minimal**
    * 4 GB RAM
    * 100 GB SSD
    * 3.2 x4 GHz CPU
* **Recommended**
    * 8 GB RAM
    * 1 TB NVME SSD
    * 3.2 GHz x4 GHz CPU

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

```shell
comdex init "{{NODE_NAME}}" --chain-id comdex-1
```

* Replace the contents of your `${HOME}/.comdex/config/genesis.json` with that of mainnet/comdex-1/pre_genesis.json.

```shell
wget https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/comdex-1/pre_genesis.json
```

* Warning :: Please do not add more than ```10000000ucmdx``` in the genesis account and while gentx.

```shell
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
* Create a file `peers-{{VALIDATOR_NAME}}.json` under the mainnet/comdex-1/peers folder in the forked repo, paste the copied text from the last two steps into the file. Find reference file sample-peers.json in the same folder. (e.g. fd4351c2e9928213b3d6ddce015c4664e6138@3.127.204.206)

* Create a Pull Request to the `main` branch of the [repository](https://github.com/comdex-official/networks)
>**NOTE:** The Pull Request will be merged by the maintainers to confirm the inclusion of the validator at the genesis.Maximum number of validators - 64. The final genesis file will be published under the file mainnet/comdex-1/genesis_final.json.

# Part-2

## Launch Instructions

Update comdex to v0.0.4

For the gentx creation, we used the v.0.03 tag.

For launch, please update to the v0.0.4 tag and rebuild your binaries. Changes in versions 
 - make file refactor to display version and include ldflags.
 - updated IBC version.

* Go to the comdex code repository location. If you are at /home/{username} then below command should work,assuming code was cloned at home location. OR YOU CAN ALSO CHECKOUT OUT THE LATEST CODE BY
```shell
git clone https://github.com/comdex-official/comdex.git
```

* Checkout latest tag
```shell
cd comdex
git fetch --tags
git checkout v0.0.4
```
* Install
```shell
make all
```
* Verify version
```shell
comdex version
```
## Verify Your Installation

Verify that everything is OK. If you get something like the following, you've successfully installed comdex on your system.

```shell
v0.0.4
```
If the software version does not match, then please check your $PATH to ensure the correct comdex is running.

## Remove old genesis file

```shell
rm ~/.comdex/config/genesis.json
```

## Download Final Genesis file

You can now download the "genesis" file for the chain. It is pre-filled with the entire genesis state and gentxs.

```shell
curl https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/comdex-1/genesis.json > ~/.comdex/config/genesis.json
```

* Verify the genesis at location `${HOME}/.comdex/config/genesis.json` is replaced with that of mainnet/comdex-1/genesis.json.

* Copy below node as `persistent_peers` in `${HOME}/.comdex/config/config.toml`
 
```shell
f74518ad134630da8d2405570f6a3639954c985f@65.0.173.217:26656,d478882a80674fa10a32da63cc20cae13e3a2a57@43.204.0.243:26656,61d743ea796ad1e1ff838c9e84adb38dfffd1d9d@15.235.9.222:26656,b8468f64788a17dbf34a891d9cd29d54b2b6485d@194.163.178.25:26656,d8b74791ee56f1b345d822f62bd9bc969668d8df@194.163.128.55:36656,81444353d70bab79742b8da447a9564583ed3d6a@164.68.105.248:26656,5b1ceb8110da4e90c38c794d574eb9418a7574d6@43.254.41.56:26656,98b4522a541a69007d87141184f146a8f04be5b9@40.112.90.170:26656,9a59b6dc59903d036dd476de26e8d2b9f1acf466@195.201.195.111:26656
```

* Copy below node as `seeds` in `${HOME}/.comdex/config/config.toml`
```shell
aef35f45db2d9f5590baa088c27883ac3d5e0b33@3.108.102.92:26656
```

* Copy below value as `minimum-gas-prices` in ${HOME}/.comdex/config/app.toml
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
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
```
## Then update and start node

```shell
sudo -S systemctl daemon-reload
sudo -S systemctl enable comdex
sudo -S systemctl start comdex

To restart -
sudo -S systemctl restart comdex
To Stop -
sudo -S systemctl stop comdex
```

## Version
This chain is currently running on Comdex [v0.0.4](https://github.com/comdex-official/comdex/releases/tag/v0.0.4)

Commit Hash: 43b5ff06c05556b33628dce56cf6b8f9fc92d72e
>Note: If your node is running on an older version of the application, please update it to this version at the earliest to avoid being exposed to security vulnerabilities /defects.

## Explorer
The explorer for this chain is hosted [MainNet Explorer](TO BE PUBLISHED)
