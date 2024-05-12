### Speeding-up-Node-Synchronization
- Stop your node using `ctrl + c`.
- Install `lz4`:
  ```
  sudo apt upgrade && sudo apt install lz4
  ```
- Download the latest snapshot:
  ```
  wget http://snapshots.liveraven.net/snapshots/testnet/zero-gravity/zgtendermint_16600-1_latest.tar.lz4
  ```
- Backup `priv_validator_state.json`:
  ```
  cp $HOME/.0gchain/data/priv_validator_state.json $HOME/.0gchain/priv_validator_state.json.backup
  ```
- Reset DB:
  ```
  0gchaind tendermint unsafe-reset-all --home $HOME/.0gchain --keep-addr-book
  ```
- Extract archived files:
  ```
  lz4 -d -c ./zgtendermint_16600-1_latest.tar.lz4 | tar -xf - -C $HOME/.0gchain
  ```
- Restore `priv_validator_state.json`:
  ```
  mv $HOME/.0gchain/priv_validator_state.json.backup $HOME/.0gchain/data/priv_validator_state.json
  ```
- Restart your node:
  ```
  0gchaind start
  ```
- Give your node a few minutes to sync. You can check status using:
  ```
  0gchaind status | jq
  ```



Refer to [liveraven](https://services.liveraven.net/cosmos-testnets/zero-gravity/snapshots) for more info.
