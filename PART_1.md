# Installation-and-Syncing
## NB: This tutorial assumes you are using a vps/termius. To set up a vps please refer to [mzcat-vps-guide](https://medium.com/@mztacat/setting-up-a-vps-d030b2a28bab).

### Pre-requisites Installation
- Update and install necessary packages:
  ```
  sudo apt update && sudo apt install git -y && sudo apt install snapd && sudo apt install golang-go
  ```
- Set up your `GOPATH` and `PATH`:
  ```
  export GOPATH=$HOME/go
  export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
  ```
- Remove current GO version:
  ```
  sudo rm -rf /usr/local/go
  ```
- Download and install upgraded version:
  ```
  wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz
  sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz
  ```
- Export to `PATH`:
  ```
  export PATH=/usr/local/go/bin:$PATH
  ```
- Confirm go version:
  ```
  go version
  ```


### Node Setup
- Clone and install the evmos repository:
  ```
  git clone -b testnet https://github.com/0glabs/0g-evmos.git
  cd 0g-evmos/networks/testnet/
  ./install.sh
  ```
- Set chain ID and initialize node:
  ```
  evmosd config chain-id zgtendermint_9000-1
  evmosd init <insert_your_validator_name_here> --chain-id zgtendermint_9000-1
  ```
- Remove the existing genesis file:
  ```
  rm /root/.evmosd/config/genesis.json
  ```
- Download and validate a new genesis file:
  ```
  wget -P ~/.evmosd/config https://github.com/0glabs/0g-evmos/releases/download/v1.0.0-testnet/genesis.json
  evmosd validate-genesis
  ```
- Open the `config.toml` file:
  ```
  nano ~/.evmosd/config/config.toml
  ```
- Scroll down till you see the `seeds` line under the `[p2p]` section. Then replace the current `seeds` with this:
  ```
  seeds = "8c01665f88896bca44e8902a30e4278bed08033f@54.241.167.190:26656,b288e8b37f4b0dbd9a03e8ce926cd9c801aacf27@54.176.175.48:26656,8e20e8e88d504e67c7a3a58c2ea31d965aa2a890@54.193.250.204:26656,e50ac888b35175bfd4f999697bdeb5b7b52bfc06@54.215.187.94:26656"
  ```
- Save and exit. To do this, click `ctrl + x`, click `y`, and then click `enter` on your keyboard.
- Start the node to sync to the latest blockchain (This could take a few hours):
  ```
  evmosd start
  ```
- To confirm if your node has fully synced, run:
  ```
  evmosd status | jq
  ```
  For a synced up node, `"catching_up": false`.
