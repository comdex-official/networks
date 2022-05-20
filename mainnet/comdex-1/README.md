# comdex-1 :: First main net.

## Genesis Time
The genesis transactions sent before 1200HRS UTC 18th November 2021 will be used to publish the genesis.json on or before 1805HRS UTC 19th november 2021 and then start the chain at 1400UTC. We will be announcing on all the platforms for the same. Please join our [Discord](https://discord.gg/gH6RTrnexk) and [Comdex-Validators-Group](https://t.me/joinchat/d3WeO-OzYG0zZGFl) to stay updated.

Thank you all for submiting the gentxs. We have received more than 75 gentxs, reviewed and accepted the gentxs of the validators as per the discussion our team had with validators.

> PRE-GENESIS PUBLISHED :: Make sure to replace this genesis with the final genesis, after following the instructions on [Part-2](#launch-instructions).

> FINAL GENESIS :: PUBLISHED

> PEERS :: PUBLISHED

> SEEDS :: PUBLISHED

## First part was to submit the gentx. WHICH IS CLOSED NOW.
## Please follow instructions in [Part-2](#launch-instructions) for the next steps.

## Hardware Requirements
* **Minimal**
    * 8 GB RAM
    * 100 GB SSD
    * 3.2 x4 GHz CPU
* **Recommended**
    * 16 GB RAM
    * 1 TB NVME SSD
    * 3.2 GHz x4 GHz CPU

## Operating System

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
```

```shell
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

Update comdex to v0.0.4. Pleae update all you sentry and validators node.

For the gentx creation, we used the v.0.03 tag.

For launch, please update to the v0.0.4 tag and rebuild your binaries. Changes in versions 
 - make file refactor to display version and include ldflags.
 - updated IBC version.

* Go to the comdex code repository location. If you are at /home/{username} then below command should work,assuming code was cloned at home location. OR YOU CAN ALSO CHECKOUT OUT THE LATEST CODE BY
```shell
git clone https://github.com/comdex-official/comdex.git
```

* Checkout latest tag : Commit Hash: 43b5ff06c05556b33628dce56cf6b8f9fc92d72e
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


## Initialize your node

```shell
comdex init "{{NODE_NAME}}" --chain-id comdex-1
```

## Remove old genesis file

```shell
rm ~/.comdex/config/genesis.json
```

## Download Final Genesis file

You can now download the "genesis" file for the chain. It is pre-filled with the entire genesis state and gentxs. 

```shell
curl https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/comdex-1/genesis.json > ~/.comdex/config/genesis.json
```

Verify Hash `88b5e9c2dc1b258791ea1fbbe0110b408f36fef7c62ffd2459b44cb000fb16d0`

```shell
jq -S -c -M ' ' ~/.comdex/config/genesis.json | shasum -a 256
```

* Verify the genesis at location `${HOME}/.comdex/config/genesis.json` is replaced with that of mainnet/comdex-1/genesis.json.

* Copy below node as `persistent_peers` in `${HOME}/.comdex/config/config.toml`
 
```shell
f74518ad134630da8d2405570f6a3639954c985f@65.0.173.217:26656,d478882a80674fa10a32da63cc20cae13e3a2a57@43.204.0.243:26656,61d743ea796ad1e1ff838c9e84adb38dfffd1d9d@15.235.9.222:26656,b8468f64788a17dbf34a891d9cd29d54b2b6485d@194.163.178.25:26656,d8b74791ee56f1b345d822f62bd9bc969668d8df@194.163.128.55:36656,81444353d70bab79742b8da447a9564583ed3d6a@164.68.105.248:26656,5b1ceb8110da4e90c38c794d574eb9418a7574d6@43.254.41.56:26656,98b4522a541a69007d87141184f146a8f04be5b9@40.112.90.170:26656,9a59b6dc59903d036dd476de26e8d2b9f1acf466@195.201.195.111:26656
```

* Copy below node as `seeds` in `${HOME}/.comdex/config/config.toml`
```shell
aef35f45db2d9f5590baa088c27883ac3d5e0b33@3.108.102.92:26656,7ca14a1d156299999eba9c394ca060368022d52f@54.194.178.110:26656
```

* Copy below value as `minimum-gas-prices` in ${HOME}/.comdex/config/app.toml
```shell
0.025ucmdx
```

### Bootstrap node from state-sync snapshot (Not recommended, use archive snapshots for quicksyncing it)

```
curl -s https://rpc.comdex.one/status | \ 
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

* Set persistent-peers in config.toml. Seeds node provide list of peers on which there is a snapshots.
```
persistent_peers = "086122c105c4d6e9152bf34558fe49e624ff87fe@13.235.40.129:26656,6ac3ae94528a63f73f5554913927d9d532992288@13.229.39.55:26656,6c1e652e2d9ebf303958e27d939d6167b7528366@99.80.214.210:26656"
```

* Set trust_height and trust_hash values from rpc/status output:
```
[statesync]
enable = true
rpc_servers = "https://rpc.comdex.one,https://rpc.comdex.one"
trust_height = 3549879
trust_hash = "461420F85D8A7A9833B5A1C1E7FCC461AC10247B840C7DD3BB53AC687E3AC0BB"
trust_period = "168h0m0s"
```

### Downloading Archive snapshots
* Download any retrieving package tool such as `wget` or `aria2c`
```
wget 'paste your link here'
```

* For faster downloads and needs double the storage space (multi-threaded download)
```
aria2c -x5 'paste your link here'
```

* Download the archives from our servers. Edit the URL suffix for downloading latest snapshot based on the given format (ie.,comdex-1-archive-YYYY/MM/DD.tar.gz)

For example :
```
https://comdex-archive-minio.s3.ap-south-1.amazonaws.com/comdex-1-archive-20220124.tar.gz
```

* Extract the archive containing /data folder inside it. It is best to extract it inside `./comdex/` directory to avoid issues related to permissions.
```
lz4 -d 'archivename' | tar xf -
```

### Start comdex node

* Start comdex by running below command for fresh node. If you have used state-sync or archive, don't use unsafe-reset-all. There won't be any data for fresh node, but to be sure please run reset-unsafe as mentioned below.
```shell
comdex unsafe-reset-all
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
The explorer for this chain is hosted [MainNet Explorer]

* [Mintscan](https://www.mintscan.io/comdex/)
* [Aneka - Vitwit](https://comdex.aneka.io/)
* [Look.Chill](https://look.chillvalidation.com/comdex)
* [Atomscan](https://atomscan.com/comdex)
* [ZenChainLabs](https://comdex.zenscan.io/)
