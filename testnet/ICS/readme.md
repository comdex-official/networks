# `Document in progress` Date - 16th July 2024 . Time TBD

Comdex's launch on the Replicated Security Testnet will be a little different from previous consumer chain launches. Previous chain launches spawned a new chain from a fresh genesis state, but Comdex already exists as a sovereign chain.

### How will the sovereign -> consumer chain transition work on the Replicated Security testnet?

* Comdex side: The Comdex team is running their own testnet (chain-id: comdex-testnet-1). That testnet will perform a software upgrade and at the upgrade height (shortly after the spawn time) it will transition to the provider chain’s validator set.
* Provider side: There will be a consumer-addition proposal for the comdex chain. Shortly after the spawn time, we will receive the CCV state. This CCV state will be used to patch the original comdex chain’s genesis file.

### What do you need to do to participate in the launch on Tuesday?
See the table below for a breakdown of steps you'll need to follow throughout the process. 

## ⚠️  Complete STEP 1 (join comdex testnet with a full node) ASAP ⚠️
Follow along with Comdex's block explorer here: :: TODO

For step 1, you can try using Comdex’s joining script here: ::TODO

Otherwise you may manually join comdex-testnet-1 using these notes:
* Joining instructions: https://github.com/comdex-official/networks/tree/testnet/comdex-ics-testnet-1
* Genesis file: TODO
* Pre-transition comdex binary commit: `TODO`
* Comdex’s GitHub repository: https://github.com/comdex-official/comdex
* Building instructions for comdex’s binary: `make install`
* Go version: 1.21
* Persistent Peers = `TODO`

* Chain ID: comdex-testnet-1
* Post-upgrade comdex binary commit (run with this binary after the upgrade): [`TODO`](TODO)
  * You can also build with [TODO](TODO)
 
<details><summary>Detailed steps for joining Comdex Testnet</summary>
<br>
 

```sh
git clone https://github.com/comdex-official/comdex.git
cd comdex
git checkout 
make install
comdex init comdex-node --chain-id comdex-testnet-1

# Grab the genesis file
curl -L TODO -o $HOME/.comdex/config/genesis.json
```

add `TODO - add persistent peers seeds` as seed in `config.toml`

* Start comdex node, node should start catching up
* Node will panic at block -- ::TODO Block upgrade height--
* Stop the node

```sh
cd $HOME/comdex

git checkout "TODO - commit hash"

make install
```

replace the binary

```sh
mkdir -p $HOME/.sovereign/config

curl -L TODO genesis url -o $HOME/.sovereign/config/genesis.json
```
</details>

<details><summary>Detailed steps for transitioning comdex node from non-validator on Comdex testnet to validator on consumer chain</summary>
<br>


Download v15.0.0 Binary
```sh
cd comdex
git pull
git checkout ::TODO commithash
make install

#Should be v115.0.0
comdex version
```

Make directories in cosmovisor and copy binaries
```
mkdir -p $HOME/.comdex/cosmovisor/upgrades/v15/bin/
cp $HOME/go/bin/comdex $HOME/.comdex/cosmovisor/upgrades/v15/bin/
```

Download new Sovereign genesis
```
mkdir -p $HOME/.sovereign/config/
wget -O $HOME/.sovereign/config/genesis.json TODO genesis
```

Restart the Service
```
sudo service comdex restart && journalctl -u comdex -f -o cat
```

</details>

# Launch Sequence

| # | When? | Provider side | Comdex side |
| -- | --- | ----- | ---- |
| 1 | Now | | Join the Comdex testnet `comdex-testnet-1` with the pre-transition binary (commit hash `TODO`) as a full node (not validator) and sync to the tip of the chain (link to instructions below). |
| 2 | Now until software upgrade proposal passes on Comdex | | Build (or download) the target (post-transition) Comdex binary. <br><br>If you are using Cosmovisor, place place it in Cosmovisor `/upgrades/<upgrade-name>/bin` directory.<br><br>If you are not using Cosmovisor, be ready to manually switch your binary at the upgrade halt height. |
| 3 | Now until spawn time (15:00 UTC) | Submit assign-consensus-key for comdex-testnet-1 with the keys on your full node (**note: make sure to do this before spawn time!)** | |
| 4 | During voting period for  consumer-addition proposal on provider | You don’t have to do anything. Optionally, you may vote for the consumer-addition proposal | |
| 5 | During voting period for software upgrade on Comdex | | You don’t have to do anything. |
| 6 | After spawn time | | Place the newly generated “genesis” file (containing only the ccv state) in the ⚠️ `$HOME/.sovereign/config directory` ⚠️ Comdex will provide this.<br><br>Do NOT replace the existing genesis file. |
| 7 | When the software upgrade height is reached | | At the halt height, your node will halt.<br><br>Please upgrade to the  binary and ensure your genesis file has the CCV state from the provider chain |
