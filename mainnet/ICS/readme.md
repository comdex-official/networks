# Comdex Sovereign Chain joining ICS (PSS, sovereign transition) upgrade version - v15.3.0
# Spawn Time - 5th December 2024 (xxxxday)  xx:xx UTC
# Upgrade Date - 5th December 2024 (xxxxday)  xx:xx  UTC
# CCV.json file to be shared post spawn time

Comdex's launch on the Replicated Security will be a little different from previous consumer chain launches. Previous chain launches spawned a new chain from a fresh genesis state, but Comdex already exists as a sovereign chain.

### How will the sovereign -> consumer chain transition work on the Replicated Security testnet?

* Comdex Side: The Comdex chain (comdex-1) will undergo a software upgrade and at the upgrade height (TBA) it will transition to the Hub chain’s validator set.
* Hub Side: There will be a consumer-addition transaction for adding Comdex Sovereign chain as a consumer chain. Spawn time will be updated using the update consumer transaction on the Hub side (Need not be in the future). Once the spawn time is updated, we will receive the CCV state. Then a Reward denom will be added to the received CCV state to get CCV Denom. This CCV Denom will be used to patch the original Comdex chain’s genesis file.

### What do you need to do to participate in the launch on xxxxday?
See the table below for a breakdown of steps you'll need to follow throughout the process. 
 
## STEP 1 (Opt in Tx on Hub Chain Before the Update Tx - 5th December 2024 (xxxxday)  xx:xx  UTC)

⚠️ Ensure that the `priv_validator_key.json` on the Comdex node is different from the key that exists on the Hub node.

	# run this on the machine that you will use to run Comdex
	# the command gets the public key to use for Comdex
	$ comdex tendermint show-validator
	{"@type":"/cosmos.crypto.ed25519.PubKey","key":"qVifseOYMsfeKnzSHlkEb+0ZZeuZrVPJ7sqMZJHAbBc="}
	
	# do this step on the Hub machine
	# you should have a key available on the provider that you can use to perform the opt-in transaction
	$ COMDEX_KEY='{"@type":"/cosmos.crypto.ed25519.PubKey","key":"qVifseOYMsfeKnzSHlkEb+0ZZeuZrVPJ7sqMZJHAbBc="}'
	$ gaiad tx provider opt-in <consumer-chain-id> $COMDEX_KEY --from <tx-signer> --chain-id cosmoshub-4
	
	# get node's CometBFT validator consensus address
	$ gaiad tendermint show-address
	cosmosvalcons1l6lwnh9akhytg88mz5zuqwxgmczfrt6crqe3mm

	# confirm your validator has opted-in using the following command (the above consensus address should be present in the list)
	$ gaiad q provider consumer-opted-in-validators <consumer-chain-id>
	validators_provider_addresses: cosmosvalcons1l6lwnh9akhytg88mz5zuqwxgmczfrt6crqe3mm

##  STEP 3 On the day of upgrade (Transitioning existing validator node)

<details><summary>Detailed steps for transitioning comdex node from validator on Comdex Sovereign chain to validator on Comdex Consumer chain</summary>
<br>

Download v15.3.0 Binary
```shell
cd comdex
git pull
git checkout v15.3.0
make install

#Should be v15.3.0
comdex version
```

Download and Copy the ccv.json file [Will Be Updated]
```shell
wget -O $HOME/.comdex/config/ccv.json https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/ICS/ccv.json
```

Make directories in cosmovisor and copy binaries
```shell
mkdir -p $HOME/.comdex/cosmovisor/upgrades/v15.3.0/bin/
cp $HOME/go/bin/comdex $HOME/.comdex/cosmovisor/upgrades/v15.3.0/bin/
```

Start the node/service

```shell
comdex start
```
</details>

<details><summary>Detailed steps for transitioning comdex node from non-validator on Comdex testnet to validator on consumer chain</summary>
<br>


Download v15.3.0 Binary
```shell
cd comdex
git pull
git checkout v15.3.0
make install

#Should be v15.3.0
comdex version
```

Make directories in cosmovisor and copy binaries
```
mkdir -p $HOME/.comdex/cosmovisor/upgrades/v15.3.0/bin/
cp $HOME/go/bin/comdex $HOME/.comdex/cosmovisor/upgrades/v15.3.0/bin/
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
