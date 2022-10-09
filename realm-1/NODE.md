# Gnoland Node

## Minimum Requirements

- 1GB RAM / 10GB SSD
- 1CPU

## GNO Code

You need the gno repo to start a node.
```sh
git clone https://github.com/disperze/gno.git
cd gno
git checkout 22267102898c8f02c9b68fcd27139699bbcff635
```

:warning: **Keep the gno repo as your working directory to avoid issues.**

## Init config

You need the gnoland binary, see [build section](./BUILD.md).
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
# 3317ef086c3f6cead4d74e0453a63ae308aa11061088fe7a6db3fdd5c92f5f7a  testdir/config/genesis.json
```

Reset data
```
rm -rf testdir/data/*.db
rm -rf testdir/data/*.wal
```

## Configure peers

Edit `testdir/config/config.toml` and replace:
```
persistent_peers = "g190w8e50hgujrhz2wmeexdvxfk75qtq3q7948cw@65.109.48.19:26656"
```

## Service
Linux service

```
sudo nano /etc/systemd/system/gnotest.service
```

Set content, but replace user (default: `root`) or change gno directory (defaul: `/root/gno`).
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

## Open p2p port
Allow p2p port (default: 26656)

Example:
```
ufw allow 26656
```

You can change p2p port in `testdir/config/config.toml`  
```toml
##### peer to peer configuration options #####
[p2p]

# Address to listen for incoming connections
laddr = "tcp://0.0.0.0:26656"
```

## Run

Finally start the service
```
systemctl start gnotest
```
