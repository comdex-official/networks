Requirements:
  - go >= 1.18

```shell
cd comdex
git fetch --tags && git checkout v0.1.3
go mod vendor
make install
  # this will return commit 996939cd711d1c71276f347d27bbc25672dad401
comdex version --long


sudo systemctl stop cosmovisor

cp $HOME/go/bin/comdex $DAEMON_HOME/cosmovisor/upgrades/v0.1.1/bin
$DAEMON_HOME/cosmovisor/upgrades/v0.1.1/bin/comdex version
  # this should return 0.1.3

sudo systemctl start cosmovisor
```
