## comdex-test3 Testnet Details

- **Chain-ID**: `comdex-test3`
- **Current Comdex version**: `v14.0.0`
- **Genesis-file**: [Included in the repository](genesis.json)
- [Faucet](https://faucet.comdex.one/)

## Endpoints

- [test3-rpc.comdex.one](https://test3-rpc.comdex.one:443) - RPC Endpoint
- [test3-rest.comdex.one](https://test3-rest.comdex.one:443) - Rest Endpoint

## Peers

- [HERE](peers.txt)

## Explorers

- [https://explorer.comdex.one/comdex-test3](https://explorer.comdex.one/comdex-test3)

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
  git checkout v14.0.0
  ```
  
* Install

  ```shell
  go mod vendor
  make install
  ```
  
* Intialize the node

  ```shell
  comdex init {{NODE_NAME}} --chain-id comdex-test3
  ```
  
* Download the latest genesis and replace it

  ```shell
  cd ${HOME}/.comdex/config
  rm genesis.json
  wget https://raw.githubusercontent.com/comdex-official/networks/main/testnet/testnet-3/genesis.json
  sha256sum genesis.json
  ```
  
* Verify the genesis hash 

  ```shell
  ffa04afce848dd0716af119a7f848ec9532668e5f9b648fd170cec03023c6de0  genesis.json
  ```

* Update the existing peers in `${HOME}/.comdex/config/config.toml` and gas-prices in `${HOME}/.comdex/config/app.toml`

  ```shell
  peers="8df6bd8c97822de7e65743fa13af6cb9d505de07@46.166.172.243:26656,dc271ed63533affc1ed9a33e56fb9f5906a2ad64@5.199.163.67:26656,1505424d58e0bd49a85cbf013a1469eebbbbb4db@5.199.163.68:26656,6a5a7e9d6d36b64d6b836c250e65232446d098f8@149.56.36.207:26656,94173b71b3719112d30bcc290289c935152febb1@188.214.134.116:26656"
  sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.comdex/config/config.toml
  sed -i.bak 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025ucmdx"/' $HOME/.comdex/config/app.toml
  ```
  
* If you are an existing validator from testnet-2, kindly use your same priv_validator_key.json.

* Statesync 

    ```
    curl -s https://test3-rpc.comdex.one/status | jq '.result .sync_info | {trust_height: .latest_block_height, trust_hash: .latest_block_hash} | values'
    ```

  - Example output:
  
    ```
    {
      "trust_height": "12854000",
      "trust_hash": "32945FD94C426805227973CAFC113AA147B1CF7F37D6282C36191BCC5216C275"
    }
    ```

  - Set trust_height and trust_hash values from RPC/status output in `$(HOME)/.comdex/config.toml`
  
    ```
    [state-sync]
    enable = true
    rpc_servers = "https://test3-rpc.comdex.one:443,https://test3-rpc.comdex.one:443"
    trust_height = 12854000 
    trust_hash = "32945FD94C426805227973CAFC113AA147B1CF7F37D6282C36191BCC5216C275"
    # 2/3 of unbonding time
    trust_period = "168h"
    ```
    ##### Latest statesync details are available at https://explorer.comdex.one/comdex-test3/statesync
    
* Start the node/service

  ```shell
   comdex start
  ```
  
* For creating service / cosmovisor -> [ref](https://github.com/comdex-official/networks/blob/main/testnet/cosmovisor-setup.md)
  



