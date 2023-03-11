# Patch upgrade to v9.1.0

## Please follow instructions from the Step-1 below.

### Upgrade time : Anytime after BlockHeight

### Please update this sheet as per instructions

   [https://docs.google.com/spreadsheets/d/1MExGqk_2cYtBd1o1mLKubUO7VabetfplZseJ1SjKzyU/edit#gid=0](https://docs.google.com/spreadsheets/d/1MExGqk_2cYtBd1o1mLKubUO7VabetfplZseJ1SjKzyU/edit#gid=0)
    
# Now halt the chain and please follow the below upgrade steps, 

### Release Details
    * https://github.com/comdex-official/comdex/releases/tag/v9.1.0
    
## Step -1 : All upgrades will be done using cosmovisor.

* stop the comdex chain

```shell
   sudo systemctl stop cosmovisor
```

## Copy the updated Comdex binary of v9.1.0 and copy it in v9.0.0 directory of cosmovisor.

* Go to comdex directory if present else clone the repository

```shell
   git clone https://github.com/comdex-official/comdex.git
```

* Follow these steps if comdex repo already present

```shell
   cd $HOME/comdex
   git pull
   git fetch --tags
   git checkout v9.1.0
   go mod vendor
   make install
```

## Check current comdex version
```shell
   cosmovisor version
   # Output should be
   9.0.0
```

## Check the new comdex version, verify the latest commit hash

```shell
  name: comdex
   server_name: comdex
   version: v9.1.0
   commit: 
   build_tags: netgo,ledger
   go: go version go1.19.3 darwin/amd64

```

## Check if the wasm library is linked with the current version 

```shell
   ldd $GOPATH/bin/comdex
   # The output should be:
   go/pkg/mod/github.com/!cosm!wasm/wasmvm@v1.1.1/api/libwasmvm.x86_64.so

   # For using non-static binaries, place your wasmvm file library on /usr/lib (or) /usr/lib64 based on your distro
   cp $GOPATH/pkg/mod/github.com/!cosm!wasm/wasmvm@v1.1.1/api/libwasmvm.x86_64.so /usr/lib
```


## Copy the new comdex(v9.0.0) binary to cosmovisor upgrades directory

```shell
   cp $GOPATH/bin/comdex ~/.comdex/cosmovisor/upgrades/v9.0.0/bin
```

## Start the cosmovisor

```shell
   sudo systemctl start cosmovisor
```

All the communications will done in comdex validator group [validator-zone](https://discord.com/channels/890929797318967416/891998323416907786)
