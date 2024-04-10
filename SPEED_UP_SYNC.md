### Speeding-up-Node-Synchronization
- Stop your node using `ctrl + c`.
- Install `lz4`:
  ```
  sudo apt upgrade && sudo apt install lz4
  ```
- Download the latest snapshot:
  ```
  wget https://rpc-zero-gravity-testnet.trusted-point.com/latest_snapshot.tar.lz4
  ```
- Backup `priv_validator_state.json`:
  ```
  cp $HOME/.evmosd/data/priv_validator_state.json $HOME/.evmosd/priv_validator_state.json.backup
  ```
- Reset DB:
  ```
  evmosd tendermint unsafe-reset-all --home $HOME/.evmosd --keep-addr-book
  ```
- Extract archived files:
  ```
  lz4 -d -c ./latest_snapshot.tar.lz4 | tar -xf - -C $HOME/.evmosd
  ```
- Restore `priv_validator_state.json`:
  ```
  mv $HOME/.evmosd/priv_validator_state.json.backup $HOME/.evmosd/data/priv_validator_state.json
  ```
- Restart your node:
  ```
  evmosd start
  ```
- Give your node a few minutes to sync. You can check status using:
  ```
  evmosd status | jq
  ```



Refer to [trusted-point](https://github.com/trusted-point/0g-tools?tab=readme-ov-file#download-snapshot) for more info.
