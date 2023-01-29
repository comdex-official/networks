## meteor-test Testnet Details

- **Chain-ID**: `comdex-test2`
- **Current Comdex version**: `v8.0.0`
- **Genesis-file**: [Included in the repository](genesis.json)

## Endpoints

- [test2-rpc.comdex.one](https://test2-rpc.comdex.one:443) - RPC Endpoint
- [test2-rest.comdex.one](https://test2-rest.comdex.one:443) - Rest Endpoint

## Peers

- [HERE](peers.txt)

## Explorers

- [https://test2-explorer.comdex.one/comdex-test2](https://test2-explorer.comdex.one/comdex-test2)

## Recommended Specifications:
   * 4 Core CPU
   * 16GB or more RAM
   * 400GB SSD (3000+ iops)

## For joining the testnet

* Prerequisites
  > - go1.19+ required. [ref](https://golang.org/doc/install)
  > - git. [ref](https://github.com/git/git)
  > - jq [ref](https://github.com/stedolan/jq)
  > - GNU make. [ref](https://www.gnu.org/software/make/manual/html_node/index.html)
  > - GCC [ref](https://gcc.gnu.org/releases.html)
  
* For Debian/Ubuntu based distros
  - `sudo apt-get update`
  - `sudo apt install git jq gcc make -y`

* For Fedora distro
  - `sudo dnf install git make gcc aria2 jq -y`


## Installation

* Clone git repository

  ```shell
  git clone https://github.com/comdex-official/comdex.git
  ```
  
* Checkout latest tag

  ```shell
  cd comdex
  git fetch --tags
  git checkout v8.0.0
  ```
  
* Install

  ```shell
  make install
  ```
  
* Intialize the node

  ```shell
  comdex init {{NODE_NAME}} --chain-id comdex-test2
  ```
  
* Download the latest genesis and replace it

  ```shell
  cd ${HOME}/.comdex/config
  rm genesis.json
  wget https://raw.githubusercontent.com/comdex-official/networks/main/testnet/testnet-2/genesis.json
  sha256sum genesis.json
  ```
  
* Verify the genesis hash 

  ```shell
  86f1532b5e7ad70467c11c5a08ccf51d62e780cbc0e780aef57fa91200a9fc0b  genesis.json
  ```

* Update the existing peers in `${HOME}/.comdex/config/config.toml` and gas-prices in `${HOME}/.comdex/config/app.toml`

  ```shell
  peers="e801945d20c9cf736d4ff096f7e74a435548f3d0@peer-test2-in.comdex.one:26656,86ca6b17f5d3fee15309e1c1aecc8ba716008c95@peer-test2-nv.comdex.one:26656,6d9de52e9f413bb9affda12b333deda302d4568a@peer-test2-oh.comdex.one:26656"
  sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.comdex/config/config.toml
  sed -i.bak 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025ucmdx"/' $HOME/.comdex/config/app.toml
  ```
  
* Statesync 

    ```
    curl -s https://test2-rpc.comdex.one/status | jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
    ```

  - Example output:
  
    ```
    {
      "trust_height": "820000",
      "trust_hash": "01f7e85f9b7df173ddae4edfd8a2c211452914fc1f98e649eb586617bbb3e284"
    }
    ```

  - Set trust_height and trust_hash values from RPC/status output in `$(HOME)/.comdex/config.toml`
  
    ```
    [statesync]
    enable = true
    rpc_servers = "https://test2-rpc.comdex.one:443,https://test2-rpc.comdex.one:443"
    trust_height = 820000
    trust_hash = "01f7e85f9b7df173ddae4edfd8a2c211452914fc1f98e649eb586617bbb3e284"
    trust_period = "168h"  # 2/3 of unbonding time
    ```
    ##### Latest statesync details are available at https://test2-explorer.comdex.one/comdex-test2/statesync
    
* Start the node/service

  ```shell
   comdex start
  ```
  
* For creating service / cosmovisor -> [ref](https://github.com/comdex-official/networks/blob/main/testnet/cosmovisor-setup.md)
  



