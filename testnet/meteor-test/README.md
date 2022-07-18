## meteor-test Testnet Details

- **Chain-ID**: `meteor-test`
- **Current Comdex version**: `v2.1.0`
- **Genesis-file**: [Included in the repository](meteor-test/genesis.json)

## Endpoints

# `meteor.rpc.comdex.one` - RPC Endpoint
# `meteor.rest.comdex.one` - Rest Endpoint

## Peers

- [HERE](meteor-test/peers.txt)

## Explorers

## Steps to start meteor-test 

# For migrating existing validators to comets-test -> meteors-test

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
persistent_peers = "4202b41ccc3032011969005a215e1dbe36e3ba23@3.109.138.42:26656,223d534f0fd1daeea3578346ad3e49d9cec973b6@54.204.207.38:26656,efa67d2456e8e22e9b29bd127ed3024cffc7ede1@46.166.163.37:26656,494af55997cbb1df62cff1ed4f35b58c31277f63@46.166.172.230:26656"
```

* Start the service

```shell
sudo systemctl start cosmovisor
```

