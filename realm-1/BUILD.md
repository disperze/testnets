# Build gnoland binary

## Requirements

- [Go +1.18](https://go.dev/dl/)

## Build from source

```sh
git clone https://github.com/disperze/gno.git
cd gno
git checkout 22267102898c8f02c9b68fcd27139699bbcff635
make
```

## Download from releases

```sh
curl -LOJ https://github.com/disperze/testnets/releases/download/realm-1/gno_build_linux_amd64.tar.gz
tar -xf gno_build_linux_amd64.tar.gz

```

Files
```
|-build/
|     |- gnokey
|     |- gnoland
|
```
