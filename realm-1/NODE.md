# Gnoland Node

## GNO Code

You need the gno repo to start a node.
```sh
git clone https://github.com/disperze/gno.git
cd gno
git checkout b273a1f4739489ba5ee7bcbab5fdd5bb410e7695
```

:warning: **Keep the gno repo as your working directory to avoid issues.**

## Init config

You need the gnoland binary, see [build section](./BUILD.md)
```
./build/gnoland --chainid realm-1 --skip-start
```

## Download genesis

```sh
curl -s  https://raw.githubusercontent.com/disperze/testnets/master/realm-1/genesis.json > testdir/config/genesis.json
```

Verify genesis
```sh
sha256sum testdir/config/genesis.json
#
```

Reset data
```
rm -rf testdir/data/*.db
rm -rf testdir/data/*.wal
```

## Configure peers

Edit `testdir/config/config.toml` and replace:
```
persistent_peers = ""
```

## Service
Linux service

```
sudo nano /etc/systemd/system/gnotest.service
```

Set content, but replace user (default: `root`) or change gno directory (defaul: `/root/gno`)
```
[Unit]
Description=GNOTest Node
After=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/gno
ExecStart=/root/gno/build/gnoland --home /root/gno/testdir
Restart=on-failure
RestartSec=10
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

Enable service
```
sudo systemctl enable gnotest.service
```

## Run

Finally start the service
```
systemctl start gnotest
```
