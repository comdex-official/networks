# Comdex Testnet joining ICS (PSS, sovereign transition) upgrade version - v15.1.0
# Spawn Time - 06th August 2024 (Tuesday)  15:00 UTC
# Upgrade Date - 06th August 2024 (Tuesday)  15:00 UTC
# CCV.json file to be shared post spawn time

Comdex's launch on the Replicated Security Testnet will be a little different from previous consumer chain launches. Previous chain launches spawned a new chain from a fresh genesis state, but Comdex already exists as a sovereign chain.

### How will the sovereign -> consumer chain transition work on the Replicated Security testnet?

* Comdex side: The Comdex team is running their own testnet (chain-id: comdex-test-4). That testnet will perform a software upgrade and at the upgrade height (shortly after the spawn time) it will transition to the provider chain’s validator set.
* Provider side: There will be a consumer-addition proposal for the comdex chain. Shortly after the spawn time, we will receive the CCV state. This CCV state will be used to patch the original comdex chain’s genesis file.

### What do you need to do to participate in the launch on Tuesday?
See the table below for a breakdown of steps you'll need to follow throughout the process. 

## STEP 1 (join comdex testnet with a full node or as a validator node, if you wish to use existing comdex priv keys) - Complete this step ASAP
Follow along with Comdex's block explorer here: [Testnet Explorer](https://testnet-explorer.comdex.one/comdex-test-4)

You can manually join comdex-test-4 using these notes:
* Chain ID: comdex-test-4
* [Faucet](https://faucet.comdex.one/)
* Joining instructions: https://github.com/comdex-official/networks/blob/main/testnet/testnet-4/README.md
 
## STEP 2 (Opt in Tx on Provider Chain Before the Spawn Time - 06th August 2024 (Tuesday)  15:00 UTC)

## IMPORTANT: ⚠️ **If you did not use the key assignment feature before spawn time, do not use it until the chain is live, stable and receiving VSCPackets from the provider! **⚠️

This cannot be emphasized enough. Do _not_ try to do key assignment after the spawn time, but before Comdex is ICS secured. You can do key assignment before spawn time, or 1 week after spawn time. This problem created liveness problems for Neutron.

If you do not wish to reuse the private validator key from your provider chain, an alternative method is to use multiple keys managed by the Key Assignment feature.

⚠️ Ensure that the `priv_validator_key.json` on the consumer node is different from the key that exists on the provider node.

⚠️ The `AssignConsumerKey` transaction must be sent to the provider chain before the consumer chain's spawn time.

	# run this on the machine that you will use to run Comdex
	# the command gets the public key to use for Comdex
	$ comdex tendermint show-validator
	{"@type":"/cosmos.crypto.ed25519.PubKey","key":"qVifseOYMsfeKnzSHlkEb+0ZZeuZrVPJ7sqMZJHAbBc="}
	
	# do this step on the provider machine
	# you should have a key available on the provider that you can use to sign the key assignment transaction
	$ COMDEX_KEY='{"@type":"/cosmos.crypto.ed25519.PubKey","key":"qVifseOYMsfeKnzSHlkEb+0ZZeuZrVPJ7sqMZJHAbBc="}'
	$ gaiad tx provider opt-in comdex-test-4 $COMDEX_KEY --from <tx-signer> --chain-id <provider-chain-id>
	$ gaiad tx provider assign-consensus-key comdex-test-4 $COMDEX_KEY --from <tx-signer> --home <home_dir> --gas 900000 -y -o json
	
	# confirm your key has been assigned
	$ GAIA_VALCONSADDR=$(gaiad tendermint show-address --home ~/.gaia)
	$ gaiad query provider validator-consumer-key comdex-test-4 $GAIA_VALCONSADDR
	consumer_address: "<your_address>"


Read more on [Key Assignment](https://github.com/cosmos/interchain-security/blob/main/docs/docs/features/key-assignment.md). 



##  STEP 3 On the day of upgrade (Transitioning existing validator node ) if you are transitioning a non-validator node please skip to STEP-4

<details><summary>Detailed steps for transitioning comdex node from validator on Comdex testnet to validator on consumer chain</summary>
<br>

Download v15.1.0 Binary
```shell
cd comdex
git pull
git checkout v15.1.0
make install

#Should be v15.1.0
comdex version
```

Download and Copy the ccv.json file [It will be shared after Spawn Time]
```shell
wget -O $HOME/.comdex/config/ccv.json https://raw.githubusercontent.com/comdex-official/networks/main/testnet/ICS/ccv.json
```

Make directories in cosmovisor and copy binaries
```shell
mkdir -p $HOME/.comdex/cosmovisor/upgrades/v15.1.0/bin/
cp $HOME/go/bin/comdex $HOME/.comdex/cosmovisor/upgrades/v15.1.0/bin/
```

Start the node/service

```shell
comdex start
```
</details>

## STEP 4 Transitioning existing non-validator node

<details><summary>Detailed steps for transitioning comdex node from non-validator on Comdex testnet to validator on consumer chain</summary>
<br>


Download v15.1.0 Binary
```shell
cd comdex
git pull
git checkout v15.1.0
make install

#Should be v15.1.0
comdex version
```

Make directories in cosmovisor and copy binaries
```
mkdir -p $HOME/.comdex/cosmovisor/upgrades/v15.1.0/bin/
cp $HOME/go/bin/comdex $HOME/.comdex/cosmovisor/upgrades/v15.1.0/bin/
```

Download new Sovereign genesis
```
mkdir -p $HOME/.comdex/config/
wget -O $HOME/.comdex/config/ccv.json 
```

Restart the Service
```
sudo service comdex restart && journalctl -u comdex -f -o cat
```

</details>

# Launch Sequence

| # | When? | Provider side | Comdex side |
| -- | --- | ----- | ---- |
| 1 | Now | | Join the Comdex testnet `comdex-test-4` with the pre-transition binary (tag v14.1.0) as a full node (not validator) and sync to the tip of the chain (link to instructions below). |
| 2 | Now until software upgrade proposal passes on Comdex | | Build (or download) the target (post-transition) Comdex binary. <br><br>If you are using Cosmovisor, place place it in Cosmovisor `/upgrades/<upgrade-name>/bin` directory.<br><br>If you are not using Cosmovisor, be ready to manually switch your binary at the upgrade halt height. |
| 3 | Now until spawn time (15:00 UTC) | Submit assign-consensus-key for comdex-test-4 with the keys on your full node (**note: make sure to do this before spawn time!)** | |
| 4 | During voting period for  consumer-addition proposal on provider | You don’t have to do anything. Optionally, you may vote for the consumer-addition proposal | |
| 5 | During voting period for software upgrade on Comdex | | You don’t have to do anything. |
| 6 | After spawn time | | Place the newly generated “genesis” file (containing only the ccv state) in the ⚠️ `$HOME/.comdex/config directory` ⚠️ Comdex will provide this.<br><br>Do NOT replace the existing genesis file. |
| 7 | When the software upgrade height is reached | | At the halt height, your node will halt.<br><br>Please upgrade to the  binary and ensure your genesis file has the CCV state from the provider chain |
