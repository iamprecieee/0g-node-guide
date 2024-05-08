### Validator-Creation
- Open a duplicate terminal, so your node still runs in the former.
- Create a key for your account:
  ```
  0gchaind keys add <insert_your_key_name_here> --eth
  ```
- Enter and confirm a safe passphrase (must be 8 characters in length). Save this passphrase as you'll need it later.
- Save the generated mnemonic phrase in a safe location.
- Use this command to get your private key:
  ```
  0gchaind keys unsafe-export-eth-key <insert_your_key_name_here>
  ```
- Import the returned private key to a wallet (e.g. Metamask) to get your public address. Use this to acquire testnet tokens from the [faucet](https://faucet.0g.ai).
- The next step is to create your validator:
  ```
  0gchaind tx staking create-validator \
  --amount 1000000ua0gi \
  --pubkey $(0gchaind tendermint show-validator) \
  --moniker "<insert_your_validator_name_here>" \
  --chain-id zgtendermint_16600-1 \
  --commission-rate 0.05 \
  --commission-max-rate 0.20 \
  --commission-max-change-rate 0.05 \
  --min-self-delegation 1 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.0025ua0gi \
  --from <insert_your_key_name_here> \
  -y
  ```
- Delegate some tokens to your validator:
  ```
  0gchaind tx staking delegate $(0gchaind keys show <insert_your_key_name_here> --bech val -a)  1000000ua0gi --from <insert_your_key_name_here> --gas auto --gas-prices 0.0025ua0gi -y
  ```
- Check if your validator made it to the 'active validators' set:
  ```
  0gchaind q staking validators -o json --limit=1000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '.tokens + " - " + .description.moniker' | sort -gr | nl
  ```
