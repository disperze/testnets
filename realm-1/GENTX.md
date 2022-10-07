# Gentx

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

Get generated pubkey
```sh
PUBKEY=$(cat testdir/config/priv_validator_key.json | jq -r '.pub_key.value')
echo $PUBKEY
# wcnqY2gw0ifaybJqBKhJO8LkQMkqelJ5XWANHI/PER4=
```

## Create gentx

:warning: Replace `VAL_NAME` with your validator name. 
```sh
./build/gnokey maketx call val --pkgpath "gno.land/r/validators" \
    --func CreateValidator \
    --args "VAL_NAME" \
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

