# Steps to create Validator after gentx creation

This is a detailed step-by-step guide for setting up a Comdex validator. Please be aware that while it is easy to set up a validating node, running a production-quality validator node with a robust architecture and security features requires an extensive setup.

## Hardware Requirements
* **Minimal**
    * 32 GB RAM
    * 500 GB SSD
    * 3.2 x4 GHz CPU
* **Recommended**
    * 64 GB RAM
    * 1 TB NVME SSD
    * 3.2 GHz x4 GHz CPU

## Operating System

* **Recommended**
    * Linux(x86_64)

## Installation Steps
>Prerequisite: go1.19+ required. [ref](https://golang.org/doc/install)

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
* Checkout latest tag (comdex-1 is runing on v11.5.1)
```shell
cd comdex
git fetch --tags
git checkout v11.5.1
go mod vendor
```
* Install
```shell
make install
```

## Initialize your node

```shell
comdex init "{{NODE_NAME}}" --chain-id comdex-1
```

## Remove your old genesis file

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
f7fb364f9a73c1e294deb7c99d0e630fd849e1ae@3.110.67.238:26656,c71be7ba7eddac04723bc5bfbd0c6d9ae1ddc115@34.244.204.76:26656,e091e09473674dfdf46e67333bb664aa2b2b7e14@3.236.44.146:26656,0b88c5f8f653df0dcef75f47de8587fa4c46e3a4@188.214.134.115:26656,d8b74791ee56f1b345d822f62bd9bc969668d8df@194.163.128.55:36656,81444353d70bab79742b8da447a9564583ed3d6a@164.68.105.248:26656,5b1ceb8110da4e90c38c794d574eb9418a7574d6@43.254.41.56:26656,98b4522a541a69007d87141184f146a8f04be5b9@40.112.90.170:26656,9a59b6dc59903d036dd476de26e8d2b9f1acf466@195.201.195.111:26656
```

* Copy below node as `seeds` in `${HOME}/.comdex/config/config.toml`
```shell
bca2cf752ee34472c42ab96503fd10ef25d921c1@46.166.172.232:2014
```

* Copy below value as `minimum-gas-prices` in ${HOME}/.comdex/config/app.toml
```shell
0.025ucmdx
```

# Setup Cosmovisor

1. Install the latest version of cosmovisor,using the below command:

```shell
    go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
```

2. Copy the cosmovisor binary to Go path bin directory IF NOT INSTALLED AT ($GOPATH/bin/cosmovisor)

```shell
    cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
```

## Set up folder structure

## Create the required directories

```shell
    mkdir -p ~/.comdex/cosmovisor
    mkdir -p ~/.comdex/cosmovisor/genesis
    mkdir -p ~/.comdex/cosmovisor/genesis/bin
    mkdir -p ~/.comdex/cosmovisor/upgrades
```    

## Cosmovisor expects a certain folder structure:

    .
    ├── current -> genesis or upgrades/<name>
    ├── genesis
    │   └── bin
    │       └── $DAEMON_NAME
    └── upgrades
        └── <name>
            └── bin
                └── $DAEMON_NAME


## Set up the environment variables:

```shell
    echo "# Setup Cosmovisor" >> ~/.profile
    echo "export DAEMON_NAME=comdex" >> ~/.profile
    echo "export DAEMON_HOME=$HOME/.comdex" >> ~/.profile
    echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> ~/.profile
    echo "export DAEMON_LOG_BUFFER_SIZE=512" >> ~/.profile
    echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.profile
    echo "export UNSAFE_SKIP_BACKUP=true" >> ~/.profile
    source ~/.profile
```   
#### NOTE: You may use UNSAFE_SKIP_BACKUP=true if you wish to skip backup, backup takes a decent amount of time and public snapshots of old states are available. 

## Copy the current(v11.5.1) comdex binary into the cosmovisor/genesis folder

```shell
    cp $GOPATH/bin/comdex ~/.comdex/cosmovisor/genesis/bin
```

## To check your work, ensure the version of cosmovisor and comdex should be same:

```shell
    cosmovisor version
    comdex version
```    

# Set Up Cosmovisor Service

## Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:

## create the service file

```shell
    sudo nano /etc/systemd/system/cosmovisor.service
```    

## Change the contents of the below to match your setup - cosmovisor is likely at ~/go/bin/cosmovisor regardless of which installation path you took above, but it's worth checking.

```shell
    [Unit]
    Description=cosmovisor
    After=network-online.target
    [Service]
    User=ubuntu
    ExecStart=/home/ubuntu/go/bin/cosmovisor start
    Restart=always
    RestartSec=3
    LimitNOFILE=4096
    Environment="DAEMON_NAME=comdex"
    Environment="DAEMON_HOME=/home/ubuntu/.comdex"
    Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
    Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
    Environment="DAEMON_LOG_BUFFER_SIZE=512"
    Environment="UNSAFE_SKIP_BACKUP=true"
    [Install]
    WantedBy=multi-user.target
```    

## Finally, enable the service and start it.

```shell
    sudo -S systemctl daemon-reload
    sudo -S systemctl enable cosmovisor
```

## Start the Comdex chain Using Cosmovisor

```shell
    sudo systemctl start cosmovisor
```

## Check it is running using

```shell
    sudo systemctl status cosmovisor
```

## To view logs of the service

```shell
    journalctl -u cosmovisor -f
```

## To stop cosmovisor using

```shell
    sudo systemctl stop cosmovisor
```

## For future upgrades, follow this structure and keep the updated binary 

```shell
cp $GOPATH/bin/comdex ~/.comdex/cosmovisor/upgrades/<updated-version>/bin
```

If you want to skip syncing the node from the start to latest height, you can download a snapshot or statesync from our nodes.
Regarding statesync, as it's in Work in Progress, people can still request information regarding it in our Discord channel and experiment it.

## Downloading Validator snapshots

* Download any retrieving package tool such as `wget` or `aria2c`
```
wget 'paste your link here'
```

* For faster downloads and needs double the storage space (multi-threaded download)
```
aria2c -x5 'paste your link here'
```

* For running validators, you can download snapshots provided by Polkachu. It's minimal, lower disk storage, just the bare-minimum for you to start the validator.
Refer this for more [info](https://polkachu.com/tendermint_snapshots/comdex) and the instructions.

## Archival snapshots

* Download the archives from our servers (updated Monday on every week). Edit the URL suffix for downloading latest snapshot based on the given format (ie.,comdex-1-archive-YYYY/MM/DD.tar.gz)

For example :
```
https://comdex-archive-minio.s3.ap-south-1.amazonaws.com/comdex-1-archive-20220124.tar.gz
```

* Extract the archive containing /data folder inside it. It is best to extract it inside `./comdex/` directory to avoid issues related to permissions.
```
lz4 -d 'archivename' | tar xf -
```

## Start the node after the snapshot syncs

```shell
systemctl start cosmovisor
```

*These steps provided above will be enough for running your own full nodes. If you wish to convert it or start a validator node, follow the steps below.

## Generate a key wallet for your validator

`comdex keys add [key_name]`

or

`comdex keys add [key_name] --recover` to regenerate keys with your [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039) mnemonic

## Prerequisites

- You are familiar with comdex CLI

- You have completed how to run a full Comdex node, which outlines how to install, connect, and configure a node.

- Check if your validator and sentry nodes (if sentry architecture) followed is synced up to the latest block height

   ```comdex status``` 
   
   If `'catching_up':false`, it has reached the latest block height and in sync with the chain

## Create Validator

```shell
comdex tx staking create-validator \
  --amount=10000000ucmdx \
  --pubkey='$(comdex tendermint show-validator)' \
  --moniker=' ' \ 
  --chain-id=comdex-1 \
  --commission-rate="0.02" \
  --commission-max-rate="0.010" \
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

## Edit Validator

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

## View Validator

```shell
comdex query staking validator (your comdexvaloper address)
```



 
