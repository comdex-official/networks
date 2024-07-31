## comdex-test-4 Testnet Details

- **Chain-ID**: `comdex-test-4`
- **Current Comdex version**: `v14.1.0`
- **Genesis-file**: [Included in the repository](genesis.json)


## Endpoints

- [testnet-rpc.comdex.one](https://testnet-rpc.comdex.one:443) - RPC Endpoint
- [testnet-rest.comdex.one](https://testnet-rest.comdex.one:443) - REST Endpoint

## Peers

- [HERE](peers.txt)

## Explorers

- [https://testnet-explorer.comdex.one/comdex-test-4](https://testnet-explorer.comdex.one/comdex-test-4)

## Recommended Specifications:
   * 4 Core CPU
   * 16GB or more RAM
   * 400GB SSD (3000+ iops)

## For joining the testnet

* Prerequisites
  > - go1.21+ required. [ref](https://golang.org/doc/install)
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
  git checkout v14.1.0
  ```
  
* Install

  ```shell
  go mod vendor
  make install
  ```

* Check Binary Version, Verify the Latest Commit Hash

  ```shell
   $GOPATH/bin/comdex version --long

   name: comdex
   server_name: comdex
   version: v14.1.0
   cosmos_sdk_version: v0.47.9
   commit: 7841a9e06926a46cd2ac48ffb095e78a3afd0b81
   build_tags: netgo,ledger
   go: go version go1.21.7 linux/amd64
  ```

  
* Intialize the node

  ```shell
  comdex init {{NODE_NAME}} --chain-id comdex-test-4
  ```
  
* Download the latest genesis and replace it

  ```shell
  cd ${HOME}/.comdex/config
  rm genesis.json
  wget https://raw.githubusercontent.com/comdex-official/networks/main/testnet/testnet-4/genesis.json
  sha256sum genesis.json
  ```
  
* Verify the genesis hash 

  ```shell
  d4b993c85825d7abf5332fe164a29bb86a00c28c9a0a188d653d5fbc555a0238  genesis.json
  ```

* Update the existing peers in `${HOME}/.comdex/config/config.toml` and gas-prices in `${HOME}/.comdex/config/app.toml`

  ```shell
  peers="04b2689981bc7053a23684ca61924eadcab651bb@188.214.134.115:26656,1c5bd1a2ea0fcd2fc734ee77668bdead8cf9cff8@46.166.172.246:26656,5b028a30122c20fd8e68b16a34d404cfd7aaa896@46.166.172.245:26656,ba14a9a147728aaf126dc486e44e3ebf3fe99da3@46.166.172.243:26656"
  sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.comdex/config/config.toml
  sed -i.bak 's/minimum-gas-prices =.*/minimum-gas-prices = "2ucmdx"/' $HOME/.comdex/config/app.toml
  ```
  
* Statesync 

    ```
    curl -s https://testnet-rpc.comdex.one/status | jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
    ```

  - Example output:
  
    ```
    {
        "trust_height": "94437",
        "trust_hash": "56CC1ED8246BFA79261B616C67920F0D8DAD0A0C6C947D2E5015B1DADC4F3275"
    }
    ```

  - Set trust_height and trust_hash values from RPC/status output in `$(HOME)/.comdex/config.toml`
  
    ```
    [state-sync]
    enable = true
    rpc_servers = "https://testnet-rpc.comdex.one:443,https://testnet-rpc.comdex.one:443"
    trust_height = 94437 
    trust_hash = "56CC1ED8246BFA79261B616C67920F0D8DAD0A0C6C947D2E5015B1DADC4F3275"
    # 2/3 of unbonding time
    trust_period = "168h"
    ```
    ##### Latest statesync details are available at https://testnet-explorer.comdex.one/comdex-test-4/statesync
    
* Start the node/service

  ```shell
  comdex start
  ```
  
* For creating service / cosmovisor -> [ref](https://github.com/comdex-official/networks/blob/main/testnet/cosmovisor-setup.md)
  



