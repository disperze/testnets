# Validator Gentx

## Build

You need [go +1.18](https://go.dev/dl/) installed.

```sh
git clone https://github.com/disperze/gno.git
cd gno
git checkout 22267102898c8f02c9b68fcd27139699bbcff635
make

```

:warning: **Keep the gno repo as your working directory to avoid issues.**

## Create/Restore validator wallet

Add new wallet
```
./build/gnokey add val

```

Restore existing wallet
```
./build/gnokey add val --recover

```

## Generate validator key

```sh
./build/gnoland --chainid realm-1 --skip-start

```

> You can take note of your Node ID to share with other nodes, you can find it at the end of log.    
    ```
    .level 1 .msg P2P Node ID [ID g1sr75dcex0w3579767w2v07p8uy6lfan779ruc9 file testdir/config/node_key.json [module p2p]]  
    ```

Get generated pubkey
```sh
PUBKEY=$(cat testdir/config/priv_validator_key.json | jq -r '.pub_key.value')
echo $PUBKEY
# wcnqY2gw0ifaybJqBKhJO8LkQMkqelJ5XWANHI/PER4=
```



## Create gentx

:warning: Replace `VAL_NAME` with your validator name. 
```sh
VALIDATOR="VAL_NAME"
./build/gnokey maketx call val --pkgpath "gno.land/r/validators" \
    --func CreateValidator \
    --args $VALIDATOR \
    --args $PUBKEY \
    --send 10000000ugnot \
    --gas-fee 1ugnot \
    --gas-wanted 2000000 > createval.tx.json
```

Sign gentx
```sh
./build/gnokey sign val --txpath createval.tx.json --chainid "realm-1" --number 0 --sequence 0 > gentx-$(date +%s).json
```

## Upload your genTx

Fork repo https://github.com/disperze/testnets and upload previous `gentx-*.json` to  
[realm-1/gentxs](https://github.com/disperze/testnets/tree/master/realm-1/gentxs) directory, and open a [new pull request](https://github.com/disperze/testnets/compare) 

## Share your peer id

Format: `<NODE_ID>@<IP>:<PORT>` (default port: 26656)

Example:
```
g1sr75dcex0w3579767w2v07p8uy6lfan779ruc9@153.81.24.5:26656
```
