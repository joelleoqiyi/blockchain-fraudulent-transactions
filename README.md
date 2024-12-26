# blockchain-fraudulent-transactions

some background about the data. 

source: https://gz.blockchair.com/bitcoin/

important: inputs track all the money being paid out, and outputs track all the money being recieved. hence all inputs must at some point be an output as well. ie all inputs must be formed from previous outputs

combined_data_input.csv: 
- `block_id`: the block number for which this transactions prev output came from
- `transaction_hash`: transaction hash for the previous outputs which form this particular input. 
- `value_usd`: the value of the transaction (at the point in time when the previous output was transacted)
- `recipient`: address of the payer for this current transaction
- `spending_block_id`: the block number for which this transaction is the input. 
- `spending_transaction_hash`: transaction hash for which this input is the input to the transaction
- `spending_index`: basically the index of the current row for all rows with the same `spending_transaction_hash` 
    * this happens when multiple inputs for one transaction. 
- `spending_value_usd`: dollar value of transaction (at the point in time when the current row is an input)
    * if this differs from `value_usd`, it is likely due to changes in btc-usd exchange rate between the previous output and current input time period. 
- `lifespan`: time between the previous output and the current input (ie how long was the btc not used)
- `cdd`: coin destroyed days -- how many days since the btc was last used
- `value`: amount of BTCs in satoshis (ie remove the effect of btc-usd exchange rate) 

combined_data_output.csv
- `block_id`: the block number of the current output 
- `transaction_hash`: transaction hash for which the current row is the output. 
    * this transaction hash should match the `spending_transaction_hash` of the `combined_data_input.csv`
- index: basically the index of the current row for all rows with the same `transaction_hash`     
    * this happens when multiple outputs for one transaction. 
- `value`: amount of BTCs in satoshis (ie remove the effect of btc-usd exchange rate)
- `is_from_coinbase`: basically whenever a new bitcoin is mined, a block reward is generated, (kind of like inventing money out of nowhere to incentive miners to work). hence these output "transactions", dont have an corresponding input "transaction"
    * TLDR: ignore all rows where `is_from_coinbase` is true since it doesnt really represent a actual transaction between people 

combined_data_transaction.csv
- `block_id`: block number for the current transaction
- `hash`: transaction hash 
    * this transaction hash should match the `spending_transaction_hash` of the `combined_data_input.csv`
- `input_count`: number of inputs to form this transaction (ie how many input rows in inputs.csv with this particular `spending_transaction_hash`)
- `input_total`: total amount of btc used as input (in satoshis) (ie the sum of all the satoshis from the various rows in inputs.csv with this particular `spending_transaction_hash`)
- `output_count`: number of outputs to form this transaction (ie how many output rows in outputs.csv with this particular `transaction_hash`)
- `output_count`: total amount of btc used as output (in satoshis) (ie the sum of all the satoshis from the various rows in outputs.csv with this particular `transaction_hash`)
- `fee`: transaction fee for this transaction
- `cdd_total`: total coins days destroyed.

