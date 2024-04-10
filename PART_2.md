### Validator-Creation
- Open a duplicate terminal, so your node still runs in the former.
- Create a key for your account:
  ```
  evmosd keys add <insert_your_key_name_here>
  ```
- Enter and confirm a safe passphrase (must be 8 characters in length). Save this passphrase as you'll need it later.
- Save the generated mnemonic phrase in a safe location.
- Use this command to get your private key:
  ```
  evmosd keys unsafe-export-eth-key <insert_your_key_name_here>
  ```
- Import the returned private key to a wallet (e.g. Metamask) to get your public address. Use this to acquire testnet tokens from the [faucet](https://faucet.0g.ai).
- The next step is to create your validator:
  ```
  evmosd tx staking create-validator \\
  --amount=10000000000000000evmos \\
  --pubkey=$(evmosd tendermint show-validator) \\
  --moniker="<insert_your_validator_name_here>" \\
  --chain-id=zgtendermint_9000-1 \\
  --commission-rate="0.05" \\
  --commission-max-rate="0.10" \\
  --commission-max-change-rate="0.01" \\
  --min-self-delegation="1000000" \\
  --gas="5000000" \\
  --gas-prices="99999aevmos" \\
  --from=<insert_your_key_name_here>
  ```
- Delegate some tokens to your validator:
  ```
  evmosd tx staking delegate $(evmosd keys show <insert_your_key_name_here> --bech val -a)  1000000000000000000aevmos --from <insert_your_key_name_here> --gas=500000 --gas-prices=99999aevmos -y
  ```
- Check if your validator made it to the 'active validators' set:
  ```
  evmosd q staking validators -o json --limit=1000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '.tokens + " - " + .description.moniker' | sort -gr | nl
  ```
