# POA Bridge - User Interface (UI) Application

[![Build Status](https://travis-ci.org/patitonar/bridge-ui.svg?branch=master)](https://travis-ci.org/patitonar/bridge-ui)
[![Gitter](https://badges.gitter.im/poanetwork/poa-bridge.svg)](https://gitter.im/poanetwork/poa-bridge?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Welcome to the POA Bridge! Following is an overview of the POA Bridge and Bridge UI Application, as well as [basic instructions for getting started](#getting-started).

## POA Bridge Overview

The POA Bridge allows users to transfer assets between two chains in the Ethereum ecosystem. It is composed of several elements which are located in different POA Network repositories. 

For a complete picture of the POA Bridge functionality, it is useful to explore each repository.

**Bridge Elements**
1. Bridge UI Application. A DApp interface to transfer tokens and coins between chains, located in this repository.
2. [Bridge Smart Contracts](https://github.com/poanetwork/poa-bridge-contracts). Solidity contracts used to manage bridge validators, collect signatures, and confirm asset relay and disposal.
3. [Token Bridge](https://github.com/poanetwork/token-bridge). The token bridge oracle written in NodeJS.
4. [Bridge Monitor](https://github.com/poanetwork/bridge-monitor). A tool for checking balances and unprocessed events in bridged networks.
5. [Bridge Deployment Playbooks](https://github.com/poanetwork/deployment-bridge). Manages configuration instructions for remote deployments and allows you to deploy separate bridge instances for validators.
6. [Bridge Monitor](https://github.com/poanetwork/bridge-monitor).

## Bridge UI Application

The UI provides an intuitive interface for assets transfer between networks running the Bridge smart contracts. Users can connect to a web3 wallet such as [Nifty Wallet](https://chrome.google.com/webstore/detail/nifty-wallet/jbdaocneiiinmjbjlgalhcelgbejmnid?hl=en) or [MetaMask](https://metamask.io/) and complete the transfer through a web browser.

The current implementation allows for several bridge modes.

  1. `Native-to-ERC20` Coins on a Home network can be converted to ERC20-compatible tokens on a Foreign network. Coins are locked on the Home side and the corresponding amount of ERC20 tokens are minted on the Foreign side. When the operation is reversed, tokens are burnt on the Foreign side and unlocked in the Home network.
  2. `ERC20-to-ERC20` ERC20-compatible tokens on the Foreign network are locked and minted as ERC20-compatible tokens (ERC677 tokens) on the Home network. When transferred from Home to Foreign, they are burnt on the Home side and unlocked in the Foreign network. This can be considered a form of atomic swap when a user swaps the token "X" in network "A" to the token "Y" in network "B".

  ![Bridge UI](bridge-ui.png)

### UI Features
- Shows daily limits in both networks
- Displays all events in both networks
- Filter events from a specific block number on both sides of the bridge
- Find a corresponding event on different sides of the bridge
- Submit a transaction from Home to Foreign network
- Submit a transaction from Foreign to Home network

### User Transactions
- Connect to the network you want to transfer coins from using a web3 wallet such as Nifty Wallet or MetaMask. This can be the Home or Foreign network. 

The wallet must be funded to cover gas costs related to the transfer. With the Native-to-ERC20 bridge, the wallet must contain the amount to transfer, and with the ERC20-to-ERC20 bridge, the wallet must contain tokens linked with the network you are transferring from.  

**Process**

- Specify the amount to send.
- Click the `Transfer` button.
- Confirm the transaction via the web3 wallet. 

The same address is used to send a coin from the Home network and receive a token on the Foreign Network. In order to send assets in the opposite direction, change the network in the web3 wallet. 

![Web3 Wallet Change Network](web3wallet_network.gif)


### Resources
 Some of the following resources are outdated, but provide a general sense of the UI and transactional flow.

- [Deployed URL for POA -> Ethereum Network Bridge](https://bridge.poa.net/)
- [Testnet Bridge URL](https://bridge-testnet.poa.net/)
- [Bridge UI Tutorial Videos](https://www.youtube.com/playlist?list=PLS5SEs8ZftgUqR3hVFiEXQLqE9QI8sIGz)
- [Article on the POA Bridge](https://medium.com/poa-network/cross-chain-bridges-paving-the-way-to-internet-of-blockchains-422ac94bc2e5)
- Wallet Resources
  - [MetaMask](https://consensys.zendesk.com/hc/en-us/categories/360001045692-Using-MetaMask)
  - [Nifty Wallet](https://poanet.zendesk.com/hc/en-us/articles/360008957634-Nifty-Wallet)

## Getting Started

The following is an example setup using the POA Sokol testnet as the Home network, and the Ethereum Kovan testnet as the Foreign network. The instructions for the Bridge UI are identical for an `ERC20-to-ERC20` configuration, but the smart contract deployment steps will vary.

### Dependencies

- [poa-bridge-contracts](https://github.com/poanetwork/poa-bridge-contracts)
- [token-bridge](https://github.com/poanetwork/token-bridge)
- [node.js](https://nodejs.org/en/download/)
- [Nifty Wallet](https://chrome.google.com/webstore/detail/nifty-wallet/jbdaocneiiinmjbjlgalhcelgbejmnid?hl=en) or [MetaMask](https://metamask.io/)

### Example Setup

1. Create an empty folder where you will be setting up your bridge. In this example we call it `sokol-kovan-bridge`.
`mkdir sokol-kovan-bridge && cd sokol-kovan-bridge`  

2. Prepare temporary ETH address(es) for deployment by creating new account(s) in Nifty Wallet or MetaMask. See the [wallet resources](#resources) if you need more information on this step.  

3. Fund the test account(s).
    * Fund Home accounts (`Validators`) using the [POA Sokol Faucet](https://faucet-sokol.herokuapp.com/)
    * Get free Kovan Coins from the [gitter channel](https://gitter.im/kovan-testnet/faucet) or [Iracus faucet](https://github.com/kovan-testnet/faucet) for Foreign Accounts. Get 5 Keth to 1 acc, and transfer from it to all other wallets.


4. Deploy the Sokol <-> Kovan Bridge contracts.
    * Go to the the `sokol-kovan-bridge` folder created in step 1 and `git clone https://github.com/poanetwork/poa-bridge-contracts`
    * Follow instructions in the [POA Bridge contracts repo](https://github.com/poanetwork/poa-bridge-contracts).
    * Set the parameters in the .env file.
      * `DEPLOYMENT_ACCOUNT_PRIVATE_KEY`: Export the private key from step 2
      * `HOME_RPC_URL`=https://sokol.poa.network
      * Wallet address(es) of validators. For testing, you can use the same address for all address values in the file. This includes:
        * `HOME_OWNER_MULTISIG`
        * `HOME_UPGRADEABLE_ADMIN_VALIDATORS`
        * `HOME_UPGRADEABLE_ADMIN_BRIDGE`
        * `FOREIGN_OWNER_MULTISIG`
        * `FOREIGN_UPGRADEABLE_ADMIN_VALIDATORS`
        * `FOREIGN_UPGRADEABLE_ADMIN_BRIDGE`
      * FOREIGN_RPC_URL=https://kovan.infura.io/mew
    * When deployment is finished, check that the `bridgeDeploymentResults.json` file exists in the `deploy` directory and includes the bridge contract addresses.  

5. Install and run the POA Token Bridge.
    * Got to the `sokol-kovan-bridge` folder and  `git clone https://github.com/poanetwork/token-bridge`
    * Follow instructions in the [POA Token Bridge repo](https://github.com/poanetwork/token-bridge).

If successful, you will see bridge processes run when you issue a command. For example run `npm run watcher:signature-request`

**Example NPM Output:**
```bash
[1539195000507] INFO (watcher-signature-request): Connected to redis
[1539195000545] INFO (watcher-signature-request): Connected to amqp Broker
[1539195006085] INFO (watcher-signature-request): Found 0 UserRequestForSignature events
[1539195011467] INFO (watcher-signature-request): Found 0 UserRequestForSignature events
```

**Example Docker Output:**

**Note:** The output will depend on your Docker configuration. You may need to access the container logs to view.

```bash
{"level":30,"time":1539366879816,"msg":"Connected to redis","validator":"0xC9f74f16F3743f4370c66DdA728D1751BCBa2616","name":"watcher-signature-request","v":1}
{"level":30,"time":1539366879880,"msg":"Connected to amqp Broker","validator":"0xC9f74f16F3743f4370c66DdA728D1751BCBa2616","name":"watcher-signature-request","v":1}
{"level":30,"time":1539366885587,"msg":"Found 0 UserRequestForSignature events","validator":"0xC9f74f16F3743f4370c66DdA728D1751BCBa2616","name":"watcher-signature-request","v":1}
```

6. Keep the bridge processes running. Open a separate terminal window and go to the `sokol-kovan-bridge` folder to install and unpack this repository.

    * `git clone https://github.com/poanetwork/bridge-ui.git`  
    * `cd bridge-ui`
    * Update submodules  
`git submodule update --init --recursive --remote` 
    * Install dependencies  
`npm install`  
    * Create a .env file from the example file[.env.example](.env.example)  
`cp .env.example .env`  
    * Insert the addresses from the bridgeDeploymentResults.json file into the .env file.
`cat ../poa-bridge-contracts/deploy/bridgeDeploymentResults.json`  
```bash
    # HomeBridge address in bridgeDeploymentResults.json
    REACT_APP_HOME_BRIDGE_ADDRESS=0x.. 
    # ForeignBridge address in bridgeDeploymentResults.json
    REACT_APP_FOREIGN_BRIDGE_ADDRESS=0x..
    # https public RPC node for Foreign network
    REACT_APP_FOREIGN_HTTP_PARITY_URL=https://kovan.infura.io/mew
    # public RPC node for Home network 
    REACT_APP_HOME_HTTP_PARITY_URL=https://sokol.poa.network 
    # Gas price speed option (slow, standard, fast, instant)
    REACT_APP_GAS_PRICE_SPEED_TYPE=fast
```
   * Run `npm run start`
   * Make sure your web3 wallet (Nifty Wallet or MetaMask) is funded and connected to the POA Sokol Network (see step 2)
   * Specify an amount and click `Transfer` to complete a cross-chain transaction from Sokol to Kovan

## Testing

`npm run test`

To run tests with coverage

`npm run coverage`

## Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) document for contribution, testing and pull request protocol.

## License

[![License: LGPL v3.0](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0)

This project is licensed under the GNU Lesser General Public License v3.0. See the [LICENSE](LICENSE) file for details.
