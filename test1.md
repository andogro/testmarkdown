The following variables are configured in the `.env` file when deploying an ERC20-to-ERC20 bridge.  

| Variable | Value | Description |
|----------------------------------------|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `BRIDGE_MODE` | NATIVE_TO_ERC | Type of bridge - defines the set of contracts to be deployed. |
| `DEPLOYMENT_ACCOUNT_PRIVATE_KEY` | Hex value | Private key hex value of the account responsible for contracts deployment and initial configuration. The account's balance must contain funds for both networks. |
| `DEPLOYMENT_GAS_LIMIT` | Integer | The "gas" parameter set in every deployment/configuration transaction |
| `HOME_DEPLOYMENT_GAS_PRICE` | Integer | The "gasPrice" parameter set in every deployment/configuration transaction on Home network (in Wei) |
| `FOREIGN_DEPLOYMENT_GAS_PRICE` | Integer | The "gasPrice" parameter set in every deployment/configuration transaction on Foreign network (in Wei). |
| `GET_RECEIPT_INTERVAL_IN_MILLISECONDS` | Integer | Timeout limit to wait for receipt of the deployment/configuration transaction. |
|  |  |  |
| `BRIDGEABLE_TOKEN_NAME` | String | Name of the ERC677 token to be deployed on the Foreign network. |
| `BRIDGEABLE_TOKEN_SYMBOL` | String | Symbol name of the ERC677 token to be deployed on the Foreign network. |
| `BRIDGEABLE_TOKEN_DECIMALS` | Integer | Number of supportable decimal digits after the "point" in the ERC677 token to be deployed on the Foreign network |
|  |  |  |
| `HOME_RPC_URL` | URL | RPC channel to a Home node able to handle deployment/configuration transactions |
| `HOME_BRIDGE_OWNER` | Address beginning with 0x | Address on Home network with permissions to change parameters of the bridge contract. For extra security we recommended using a multi-sig wallet contract address here. |
| `HOME_VALIDATORS_OWNER` | Address beginning with 0x | Address on Home network with permission to change the parameters of bridge validator contract. |
| `HOME_UPGRADEABLE_ADMIN` | Address beginning with 0x | Address on Home network with permission to upgrade the bridge contract and the bridge validator contract. |
| `HOME_DAILY_LIMIT` | INT in Wei | Daily transaction limit in Wei. As soon as this limit is exceeded, any transaction requesting to relay assets will fail. |
| `HOME_MAX_AMOUNT_PER_TX` | INT in Wei | The maximum limit for one transaction in Wei. If a single transaction tries to relay funds exceeding this limit it will fail. |
| `HOME_MIN_AMOUNT_PER_TX` | INT in Wei | The minimum limit for one transaction in Wei. If a transaction tries to relay funds below this limit it will fail. This is required to prevent dryout validator accounts. |
| `HOME_REQUIRED_BLOCK_CONFIRMATIONS` | INT | The finalization threshold. The number of blocks issued after the block with the corresponding deposit transaction to guarantee the transaction will not be rolled back. |
| `HOME_GAS_PRICE` | INT in Wei | The default gas price (in Wei) used to send Home Network signature transactions for deposit or withdrawal confirmations. This price is used if the Gas price oracle is unreachable. |
| `BLOCK_REWARD_ADDRESS` | not used | **Not used in this mode, comment out or delete this variable.** |
|  |  |  |
| `FOREIGN_RPC_URL` | URL | The RPC channel to a Foreign node able to handle deployment/configuration transactions. |
| `FOREIGN_BRIDGE_OWNER` | Address beginning with 0x | Address on Foreign network with permissions to change parameters of the bridge contract. For extra security we recommended using a multi-sig wallet contract address here. |
| `FOREIGN_VALIDATORS_OWNER` | Address beginning with 0x | Address on Foreign network with permissions to change parameters of bridge validator contract. |
| `FOREIGN_UPGRADEABLE_ADMIN` | Address beginning with 0x | Address on Foreign network with permissions to upgrade the bridge contract and the bridge validator contract. |
| `FOREIGN_DAILY_LIMIT` | INT in Wei | The daily transaction limit in Wei. This is used on the Home side to check the bridge validator's actions. |
| `FOREIGN_MAX_AMOUNT_PER_TX` | INT in Wei | The maximum limit for one transaction in Wei. `FOREIGN_MAX_AMOUNT_PER_TX` must be less than `FOREIGN_DAILY_LIMIT` |
| `FOREIGN_MIN_AMOUNT_PER_TX` | INT in Wei | The minimum limit for one transaction in Wei. If a transaction tries to relay funds below this limit it will fail. This is required to prevent dryout validator accounts. |
| `FOREIGN_REQUIRED_BLOCK_CONFIRMATIONS` | INT | The finalization threshold. The number of blocks issued after the block with the corresponding deposit transaction to guarantee the transaction will not be rolled back. |
| `FOREIGN_GAS_PRICE` | INT in Wei | The default gas price (in Wei) used to send Foreign network transactions finalizing asset deposits. This price is used if the Gas price oracle is unreachable. |
| `ERC20_TOKEN_ADDRESS` | not used | **Not used in this mode, comment out or delete this variable.** |
|  |  |  |
| `REQUIRED_NUMBER_OF_VALIDATORS` | Integer | The minimum number of validators required to send their signatures confirming the relay of assets. The same number of validators is expected on both sides of the bridge. Minimum is 1.  |
| `VALIDATORS` | Addresses starting with 0x, separated by a space | The set of validators' addresses. It is assumed that signatures from these addresses are collected on the Home side. The same addresses are used on the Foreign network to confirm that the finalized agreement was transferred correctly to the Foreign network. |
