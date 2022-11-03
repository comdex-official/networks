## meteor-test Testnet Details

- **Chain-ID**: `meteor-test`
- **Current Comdex version**: `v5.0.0.beta`
- **Genesis-file**: [Included in the repository](genesis.json)

## Endpoints

- [meteor-rpc.zenscape.one](https://meteor-rpc.zenscape.one:443) - RPC Endpoint
- [meteor-rest.zenscape.one](https://meteor-rest.zenscape.one:443) - Rest Endpoint

## Peers

- [HERE](peers.txt)

## Explorers

- [meteor-explorer.comdex.one](https://meteor-explorer.comdex.one)

## Recommended Specifications:
   * 4 Core CPU
   * 16GB or more RAM
   * 400GB SSD (3000+ iops)

# Steps to start meteor-test 

## For migrating existing validators to comets-test -> meteors-test

* Stop the chain

* Reset the data and WAL of the current state and wasm cache

```shell
comdex tendermint reset-state 
rm -rf ${HOME}/.comdex/config/wasm
```

* Delete/Replace the exisiting genesis with the new genesis

```shell
rm ${HOME}/.comdex/config/genesis.json 
cd ${HOME}/.comdex/config/
wget https://raw.githubusercontent.com/comdex-official/networks/main/testnet/meteor-test/genesis.json
sha256sum genesis.json
```

* Verify the genesis hash 

```shell
b0744029560d65bd5d81dbe055704708a33c3d51594b02ea575cf4b56d7f506e
```

* Update the existing peers 

```shell
persistent_peers = "4202b41ccc3032011969005a215e1dbe36e3ba23@65.2.136.12:26656,223d534f0fd1daeea3578346ad3e49d9cec973b6@54.166.39.27:26656,efa67d2456e8e22e9b29bd127ed3024cffc7ede1@46.166.163.37:26656,494af55997cbb1df62cff1ed4f35b58c31277f63@46.166.172.230:26656"
```

* Restore the snapshot

```shell
cd ${HOME}/.comdex/
wget https://snapshot.zenscape.one/comdex-testnet/meteor-pruned-5723785.tar.lz4
lz4 -d meteor-pruned-5723785.tar.lz4 | tar xf -
```

* Start the service

```shell
sudo systemctl start cosmovisor
```

## For new users

* Prerequisites
  > - go1.18+ required. [ref](https://golang.org/doc/install)
  > - git. [ref](https://github.com/git/git)
  > - jq [ref](https://github.com/stedolan/jq)
  > - GNU make. [ref](https://www.gnu.org/software/make/manual/html_node/index.html)
  > - GCC [ref](https://gcc.gnu.org/releases.html)
  
* For Debian/Ubuntu based distros
  - `sudo apt-get update`
  - `sudo apt install git jq gcc make -y`


## Installation

* Clone git repository

  ```shell
  git clone https://github.com/comdex-official/comdex.git
  ```
  
* Checkout latest tag

  ```shell
  cd comdex
  git fetch --tags
  git checkout v5.0.0.beta
  ```
  
* Install

  ```shell
  make install
  ```
  
* Intialize the node

  ```shell
  comdex init {{NODE_NAME}} --chain-id meteor-test
  ```
  
* Download the latest genesis and replace it

  ```shell
  cd ${HOME}/.comdex/config
  rm genesis.json
  wget https://raw.githubusercontent.com/comdex-official/networks/main/testnet/meteor-test/genesis.json
  sha256sum genesis.json
  ```
  
* Verify the genesis hash 

  ```shell
  b0744029560d65bd5d81dbe055704708a33c3d51594b02ea575cf4b56d7f506e
  ```

* Update the existing peers in `${HOME}/.comdex/config/config.toml`

  ```shell
  persistent_peers = "4202b41ccc3032011969005a215e1dbe36e3ba23@65.2.136.12:26656,223d534f0fd1daeea3578346ad3e49d9cec973b6@54.166.39.27:26656,efa67d2456e8e22e9b29bd127ed3024cffc7ede1@46.166.163.37:26656,494af55997cbb1df62cff1ed4f35b58c31277f63@46.166.172.230:26656""
  ```
  
* Snapshot recovery

  ```shell
  cd ${HOME}/.comdex/
  wget https://snapshot.zenscape.one/comdex-testnet/meteor-pruned-5723785.tar.lz4
  lz4 -d meteor-pruned-5723785.tar.lz4 | tar xf -
  ```

* Advanced users, prefer statesync. Go with snapshots for first timers: (NOT NEEDED IF SNAPSHOT RECOVERY USED)

    ```
    curl -s https://meteor-rpc.zenscape.one/status | \ 
     jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
    ```

  - Example output:
  
    ```
    {
      "trust_height": "3549879",
      "trust_hash": "461420F85D8A7A9833B5A1C1E7FCC461AC10247B840C7DD3BB53AC687E3AC0BB"
    }
    ```

  - Set trust_height and trust_hash values from RPC/status output in `$(HOME)/.comdex/config.toml`
  
    ```
    [statesync]
    enable = true
    rpc_servers = "https://meteor-rpc.zenscape.one:443,https://meteor-rpc.zenscape.one:443"
    trust_height = 3549879
    trust_hash = "461420F85D8A7A9833B5A1C1E7FCC461AC10247B840C7DD3BB53AC687E3AC0BB"
    trust_period = "168h0m0s"
    ```
    
* Start the node/service

  ```shell
   comdex start
  ```
  
* For creating service / cosmovisor -> [ref](https://github.com/comdex-official/networks/blob/main/testnet/cosmovisor-setup.md)
  



