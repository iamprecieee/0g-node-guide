# Installation-and-Syncing
## NB: This guide assumes you are using a vps/termius. To set up a vps please refer to [mzcat-vps-guide](https://medium.com/@mztacat/setting-up-a-vps-d030b2a28bab).

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
- Ensure your server timezone configuration is UTC. Check your timezone by running:
  ```
  timedatectl
  ```


### Node Setup
- Download pre-built `evmosd` binary:
  ```
  wget https://rpc-zero-gravity-testnet.trusted-point.com/evmosd
  chmod +x ./evmosd
  mv ./evmosd /usr/local/bin/
  evmosd version
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
  wget https://rpc-zero-gravity-testnet.trusted-point.com/genesis.json -O $HOME/.evmosd/config/genesis.json
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
- Set up persistent peers:
  ```
  PEERS="d813235cc2326983e0ea071ffa8acba341df0adb@89.117.56.219:16456,ac1d78038dfa515ec5e44db02831ceb2d1d1d57e@75.119.136.242:26656,da448d3b9fc80a5a4be747dd3d82f9be3812c544@144.126.213.37:16456,e47e39992ba47d7544797ec16eedcd24503a2629@144.91.84.170:26656,5a4d38ac71bf0546333c28ab2be75f069d72508f@84.247.177.86:26656,bf7bdcaf6cc807e53d3a63e64018ea3f57530bd5@213.199.40.126:26656,8ff0124d5f1881b708f459cc464f894b5bcc99be@38.242.212.146:26656,13b748e30700d662dd7516064d08f31a3a7c8e18@62.169.16.169:26656,f4231a379eb5b306210ee8dcde1cf9c1c5eeb965@37.60.228.142:26656,4091fc5a27a91c717b8ce84a3e76f81b96474df1@207.180.252.190:26656,ea224a77f8aa0805561da4047b0a8b2d89ecce2a@213.199.61.159:26656,6b644af890863f830d3e6b37a3e82d7b8847f342@173.212.221.121:16456,84ee5874d03a659dd18b886ea82c1c17b973db50@65.108.209.212:26656,1b06fd4dd3fcd7e530b60a2b6a7f228130906322@141.94.99.181:33656,64e84008878bb053f33b7b76c8684bb12ba53ec8@109.123.246.140:26656" && sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.evmosd/config/config.toml
  ```
- Start the node to sync to the latest blockchain (This could take a few hours):
  ```
  evmosd start
  ```
- To confirm if your node has fully synced, run:
  ```
  evmosd status | jq
  ```
  For a synced up node, `"catching_up": false`.
