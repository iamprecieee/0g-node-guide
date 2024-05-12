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
  wget https://go.dev/dl/go1.19.linux-amd64.tar.gz
  sudo tar -C /usr/local -xzf go1.19.linux-amd64.tar.gz
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
- Clone `0gchaind` repo, install and confirm version(0.1.0):
  ```
  git clone -b v0.1.0 https://github.com/0glabs/0g-chain.git
  ./0g-chain/networks/testnet/install.sh
  0gchaind version
  ```
- Set chain ID and initialize node:
  ```
  0gchaind config chain-id zgtendermint_16600-1
  0gchaind init <insert_your_validator_name_here> --chain-id zgtendermint_16600-1
  ```
- Remove the existing genesis file:
  ```
  rm /root/.0gchain/config/genesis.json
  ```
- Download and validate a new genesis file:
  ```
  sudo apt install -y unzip wget
  wget -P ~/.0gchain/config https://github.com/0glabs/0g-chain/releases/download/v0.1.0/genesis.json
  0gchaind validate-genesis
  ```
- Open the `config.toml` file:
  ```
  nano ~/.0gchain/config/config.toml
  ```
- Scroll down till you see the `seeds` line under the `[p2p]` section. Then replace the current `seeds` with this:
  ```
  seeds = "c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,44d11d4ba92a01b520923f51632d2450984d5886@54.176.175.48:26656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,f878d40c538c8c23653a5b70f615f8dccec6fb9f@54.215.187.94:26656"
  ```
- Scroll down till you see the `persistent_peers` line under the same `[p2p]` section. Then replace the current `persistent_peers` with this:
  ```
  persistent_peers = "3ab056f065ee9d37c4ad2f393033d292651182ab@159.69.72.247:17656,54f9f1842016a16609b39264744fd2b94d780a7f@109.199.102.78:26656,a3e6c6214805c1c068882f1981855c7a9f5926ea@213.168.249.202:26656,e0adef6b598f28f0a3c8541594794e16b5116e36@109.199.101.9:16656,ae1c39dcf8d8a7c956a0333ca3d9176d1df87f64@62.169.23.106:26656,9d09d391b2cf706a597d03fe8bb6700fe5cac53d@65.108.198.183:18456,89d28f11ecde775bf54ba9b88df688a2204a0fc1@195.26.246.50:36656,c5edc58c22ca76a3949211572093755ec8f8ff7e@62.169.30.101:26656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,e6507fe04d7d3164c7a9dc81ea03173984fef8ff@88.198.17.110:16686,98f648e34eb32e5d4c94365b05c3ab5bcd608ef8@158.220.117.198:27656,93ee19679d9a51e95955c645c1a68174de8062c9@199.175.98.92:26656,f64f0fb500c62bffa33d60450d30792ee4b5fbd0@167.86.119.168:26656,d4085fd93ab77576f2acdb25d2d817061db5afe6@62.169.19.156:26656,09276ca89027a4357ea7e68d0bfc25e58fe77377@176.126.87.189:26656,ffa48d99f37eaf5473a02487ef26fd7691f770aa@149.102.141.113:26656,f035d470e0b6ab6bdc6ee13886a81d44f4649034@184.174.38.244:26656,f0bacd7016b6359b177d70434ee16312d8430ef6@5.199.133.245:26656,b8f8ed478f2794629fdb5cf0c01edaed80f00f84@168.119.64.172:26656,9d4e7a1287aebcfee38c091f590d7fa98806f77e@65.108.65.36:32656,c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,024ab54a6b91bdad8441e8dc1da01b4f7d7538cc@93.189.29.18:26656,ecd9b58663a46dbdf6bbf25a753d5aa0ef4a6ca9@31.220.78.119:26656,a6ff8a651dd0a0e66dbfb2174ccadcbbcf567b29@66.94.122.224:26656,f3c912cf5653e51ee94aaad0589a3d176d31a19d@157.90.0.102:31656,141dbd90d5c3411c9ba72ba03704ccdb70875b01@65.109.147.58:36656,6220bc5afb26f02627d13819b282aadad14f07c1@38.242.201.36:26656,d22eb67d4c1b7c518376833601f5190538c5d471@31.220.88.109:26656,2579a86e3c4c1fabe3955d3a9ed40363bf9618f7@138.201.37.195:26656,66cfdcd92e5206e59bc507bef3f6d72ed21a149d@109.199.100.254:26656,9fcee79a3e60ab43802c755ab160379446ee686b@109.199.106.155:16656,641173e9d500c50769680226391d955f11728c32@76.9.210.28:26656,58316d06683fd00199c8b194517ac295a0aca835@154.38.164.71:26656,096bc54eb535c85fa43b0310f15de6b5abe7d213@178.18.251.146:20656,13249215e6de0d5b8ed2784f571f3255cbfd0645@185.252.233.49:26656,75a398f9e3a7d24c6b3ba4ab71bf30cd59faee5c@95.216.42.217:26656,57588ff7b1e862e754f3cd74fc2414f03cb79da4@213.133.111.189:26656,ebad6e8b1d10514185a8a46afb0f6a08945095bc@94.72.117.120:26656,0494c33335eed845a7ba1f894b54f6b31054c09d@207.180.204.179:26656,b92597c5124da2a5177c1c2e11f69dfec45a721a@45.90.220.92:26656,535ddcc917ab5ee6ddd2259875dac6018651da24@176.9.183.45:32656,5b2a956457b2918426b1f685fa6e3791609fb30c@84.247.165.146:26656,25ecfad6ed1aa2cf8840ab86b734294e3ac8aa6e@167.86.119.12:26656,ccb98fa0b1b416a9f37c08c193d4444074320c04@109.199.121.58:26656,d4d8082388e405886d03c08a8c671550f0b4e5db@193.233.75.132:26656,2975f14722e567b88944fe4e7b75c6f81307ce04@109.199.101.229:26656,ccb49996ba26d8bbd1e191e1fd82505aa1118593@152.53.35.92:26656,cd529839591e13f5ed69e9a029c5d7d96de170fe@46.4.55.46:34656"
  ```
- If you run into any issue with the above peers, you can get the latest peers from [liveraven]:
  ```
  curl -sS https://testnet.zero-gravity.rpc.liveraven.net/net_info \
  | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' \
  | awk -F ':' '{print $1":"$(NF)}'
  ```
- Save and exit. To do this, click `ctrl + x`, click `y`, and then click `enter` on your keyboard.
- Start the node to sync to the latest blockchain (This could take some time):
  ```
  0gchaind start
  ```
- To confirm if your node has fully synced, run:
  ```
  0gchaind status | jq
  ```
  For a synced up node, `"catching_up": false`.
