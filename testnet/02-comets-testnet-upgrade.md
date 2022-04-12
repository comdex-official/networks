# Steps to upgrade comdex chain

* Follow the cosmovisor setup guide. LAST UPGRADE WAS DONE THROUGH COSMOVISOR HENCE SHOULD BE ALREADY THERE.

# Setup Cosmovisor (IF NOT ALREADY DONE)

1. Install the latest version of cosmovisor,using the below command:

```shell
    go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
```

2. Copy the cosmovisor binary to Go path bin directory IF NOT INSTALLED AT ($GOPATH/bin/cosmovisor)

```shell
    cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
```

## Set up folder structure

## Create the required directories

```shell
    mkdir -p ~/.comdex/cosmovisor
    mkdir -p ~/.comdex/cosmovisor/genesis
    mkdir -p ~/.comdex/cosmovisor/genesis/bin
    mkdir -p ~/.comdex/cosmovisor/upgrades
```    

## Cosmovisor expects a certain folder structure:

    .
    ├── current -> genesis or upgrades/<name>
    ├── genesis
    │   └── bin
    │       └── $DAEMON_NAME
    └── upgrades
        └── <name>
            └── bin
                └── $DAEMON_NAME


## Set up the environment variables:

```shell
    echo "# Setup Cosmovisor" >> ~/.profile
    echo "export DAEMON_NAME=comdex" >> ~/.profile
    echo "export DAEMON_HOME=$HOME/.comdex" >> ~/.profile
    echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> ~/.profile
    echo "export DAEMON_LOG_BUFFER_SIZE=512" >> ~/.profile
    echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.profile
    echo "export UNSAFE_SKIP_BACKUP=true" >> ~/.profile
    source ~/.profile
```    

## Copy the current(v0.1.0) comdex binary into the cosmovisor/genesis folder

```shell
    cp $GOPATH/bin/comdex ~/.comdex/cosmovisor/genesis/bin
```

## To check your work, ensure the version of cosmovisor and comdex should be same:

```shell
    cosmovisor version
    comdex version
```    

# Set Up Cosmovisor Service

# Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:

## create the service file

```shell
    sudo nano /etc/systemd/system/cosmovisor.service
```    

## IF COSMOVISOR ALREADY INSTALLED

## Change the contents of the below to match your setup - cosmovisor is likely at ~/go/bin/cosmovisor regardless of which installation path you took above, but it's worth checking.

```shell
    [Unit]
    Description=cosmovisor
    After=network-online.target
    [Service]
    User=ubuntu
    ExecStart=/home/ubuntu/go/bin/cosmovisor start
    Restart=always
    RestartSec=3
    LimitNOFILE=4096
    Environment="DAEMON_NAME=comdex"
    Environment="DAEMON_HOME=/home/ubuntu/.comdex"
    Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
    Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
    Environment="DAEMON_LOG_BUFFER_SIZE=512"
    [Install]
    WantedBy=multi-user.target
```    

## Finally, enable the service and start it.

```shell
    sudo -S systemctl daemon-reload
    sudo -S systemctl enable cosmovisor
```

## Start the Comdex chain Using Cosmovisor

```shell
    sudo systemctl start cosmovisor
```

## Check it is running using

```shell
    sudo systemctl status cosmovisor
```

## To view logs of the service

```shell
    journalctl -u cosmovisor -f
```

## To stop cosmovisor using

```shell
    sudo systemctl stop cosmovisor
````

# Create the updated Comdex binary of v0.1.2

```shell
    mkdir -p ~/.comdex/cosmovisor/upgrades/v0.1.2/bin
    cd $HOME/comdex
    git pull
    git fetch --tags
    git checkout v0.1.2
    make install
    make all
```

## check the new comdex version:

```shell
    comdex version
```

## Verify the comdex current version

```shell
    v0.1.0
```
## copy the new comdex(v0.1.2) binary to cosmovisor upgrades directory

```shell
    cp $GOPATH/bin/comdex ~/.comdex/cosmovisor/upgrades/v0.1.2/bin
```
