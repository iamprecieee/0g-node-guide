# VALIDATOR OPERATIONS
## Voting-on-Proposals
  ```
  0gchaind tx gov vote <insert_proposal_number_here> <insert_vote_here:yes_or_no> --from <insert_your_key_name_here> --gas auto --gas-prices 0.0025ua0gi --gas-adjustment 1.4 -y
  ```
## Querying-Transactions
  ```
  0gchaind query tx <insert_transaction_hash_here>
  ```
## Delegate-Tokens-to-Validator
  ```
  0gchaind tx staking delegate <insert_valoper_address_here> <insert_amount_here>ua0gi --from <insert_your_key_name_here> --chain-id zgtendermint_16600-1 --gas auto --gas-prices 0.0025ua0gi --gas-adjustment 1.4 -y
  ```
## Transfer-Tokens-Between-Wallets
  ```
  0gchaind tx bank send <insert_your_key_name_here> <insert_destination_wallet_address_here> <insert_amount_here>ua0gi --from <insert_your_key_name_here> --chain-id zgtendermint_16600-1 --gas auto --gas-prices 0.0025ua0gi --gas-adjustment 1.4 -y
  ```
