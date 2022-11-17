# Comdex Chain Restart

## Backup your priv_validator_state.json

### In case you need a fresh snapshot, you can download it from
 - [Mirror #1]https://quicksync.ccvalidators.com/SNAPSHOTS/comdex-1_20221117_default.tar.lz4)
 - [Mirror #2]https://snapshot.zenscape.one/comdex/cryptocrew/comdex-1_20221117_default.tar.lz4

## Stop the chain

- Don't forget to backup your ~/.comdex/data/priv_validator_state.json file before replacing your data dir with the snapshot and place the file back before starting the validator.
```bash
  cp ~/.comdex/data/priv_validator_state.json ~/.comdex/priv_validator_state.json.bak
```
## Extract the data
```bash
  lz4 -d comdex-1_20221117_default.tar.lz4 | tar -xvf -
```

## Manually edit the app.toml file
 It is very important that you <b>VERIFY and ADD THESE LINES RIGHT BEFORE THE TELEMETRY PART IN YOUR app.toml FILE</b>
```bash
# IAVLDisableFastNode enables or disables the fast node feature of IAVL.
# Default is true.
iavl-disable-fastnode = false
```
Verify that the pruning is set to "nothing" (We want to be extra safe and we will change it back to custom if needed after the chain resumes)

```bash
# default: the last 100 states are kept in addition to every 500th state; pruning at 10 block intervals
# nothing: all historic states will be saved, nothing will be deleted (i.e. archiving node)
# everything: all saved states will be deleted, storing only the current state; pruning at 10 block intervals
# custom: allow pruning options to be manually specified through 'pruning-keep-recent', 'pruning-keep-every', and 'pruning-interval'
pruning = "nothing"
```

## Replace your priv_validator_state.json from the backup
```bash
  cp ~/.comdex/priv_validator_state.json.bak ~/.comdex/data/priv_validator_state.json
```

## Start the chain

## Start the node and update it the information on #🪐validators-zone discord channel

### Validators can use this snapshot, provided by CrpytoCrew (Fresh snapshot - statesync from 0.1.3, taken before the upgrade height)
### For RPC, we will provide a ```pruning = default``` snapshot soon from scratch; you can wait and sync your node with that snapshot
