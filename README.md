# POA Bridge - User Interface (UI) Application

[![Build Status](https://travis-ci.org/patitonar/bridge-ui.svg?branch=master)](https://travis-ci.org/patitonar/bridge-ui)
[![Gitter](https://badges.gitter.im/poanetwork/poa-bridge.svg)](https://gitter.im/poanetwork/poa-bridge?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


Welcome to the POA Bridge! Following is an overview of the POA Bridge and Bridge UI Application, as well as [basic instructions for getting started](#getting-started).


## POA Bridge Overview

The POA Bridge allows users to transfer assets between two chains in the Ethereum ecosystem. It is composed of several elements which are located in different POA Network repositories. 

For a complete picture of the POA Bridge functionality, it is useful to explore each repository.

**Bridge Elements**
1. Bridge UI Application. A DApp interface to transfer tokens and coins between chains, located in this repository.
2. [Solidity smart contracts](https://github.com/poanetwork/poa-bridge-contracts). Used to manage bridge validators, collect signatures, and confirm asset relay and disposal.
3. [Bridge Oracle](https://github.com/poanetwork/token-bridge) An oracle written in NodeJS.
4. [Bridge Monitor](https://github.com/poanetwork/bridge-monitor). A tool for checking balances and unprocessed events in bridged networks.
5. [Bridge Deployment Playbooks](https://github.com/poanetwork/deployment-bridge). Manages configuration instructions for remote deployments and allows you to deploy separate bridge instances for validators.


## Bridge UI Application

The Bridge UI application runs on multiple computers; smart contracts are located in both sides of the bridge. The UI provides an intuitive interface for assets transfer between networks running the contracts. Users can connect to a web3 wallet such as [Nifty Wallet](https://chrome.google.com/webstore/detail/nifty-wallet/jbdaocneiiinmjbjlgalhcelgbejmnid?hl=en) or [MetaMask](https://metamask.io/) and complete the transfer through a web browser.

The current implementation allows users to tokenize the native coin on their EVM (Ethereum Virtual Machine) compatible "Home" network into an ERC20 token on another EVM compatible "Foreign" network. Tokens from the Foreign network can also be transferred back to native coins. 

### Features
- Show daily limits in both networks
- Display all events in both networks
- Filter events from a specific block number on both sides of the bridge
- Find a corresponding event on different sides of the bridge
- Submit a transaction from Home to Foreign network
- Submit a transaction from Foreign to Home network


### Resources
- [Deployed URL for POA -> Ethereum Network Bridge](https://bridge.poa.net/)
- [Testnet Bridge URL](https://bridge-testnet.poa.net/)
- [Bridge UI Tutorial Videos](https://www.youtube.com/playlist?list=PLS5SEs8ZftgUqR3hVFiEXQLqE9QI8sIGz)
- [Article on the POA Bridge]((https://medium.com/poa-network/cross-chain-bridges-paving-the-way-to-internet-of-blockchains-422ac94bc2e5)

### User Transactions

#### From Home to Foreign Network
To tokenize native POA coins a user must complete the following:
- Connect to the POA network using a web3 wallet such as Nifty Wallet or MetaMask. The wallet must be funded to include amount to transfer and any gas costs.
- Specify the amount to send
- Click the Transfer button
- Confirm the transaction via a web3 wallet 
- The same address is used to send a coin from the Home network and receive a token on the Foreign Network.

The amount sent must be within the daily limits provided by the contracts. When the transaction is validated, the user should see the Deposit event on the Home Network. To complete the transaction, validators submit signatures emitting a `SignedForDeposit` event to the Foreign Network. Once the required number of signatures is reached, a `Deposit` event is emitted on the Foreign Network. This results in the equivalent amount of ERC20 tokens minted on the Foreign Network with the corresponding depost address. 

#### From Foreign to Home Network
To burn a token and send it back to Home Network (ERC20 to POA20):
- Connect to the Foreign network using a web3 wallet such as Nifty Wallet or MetaMask. The wallet must be funded to include amount to transfer and any gas costs.
- Specify an Amount
- Click the Transfer button to send
- Confirm the transaction via a web3 wallet
- The same address is used to send a token from the Foreign network and receive a coin on the Home Network.

The amount sent must be within the daily limits provided by the contracts. When the transaction is validated, the user should expect to see a Withdrawal Event on the Foreign Network (right-side - Ethereum Foundation). To complete the transaction,  Validators submit signatures emitting a `SignedForWithdrawal` event to the Foreign Network. Once the required number of signatures is reached, the `CollectedSignatures` event is emitted on the Foreign Network. The ERC20 tokens are burned on the Foreign Network. This process generates a signed message which Validators submit on the Home Network. The user receives their Native Home network coin.


## Getting Started

The following is an example setup using the POA Sokol testnet as the Home network, and the Ethereum Kovan testnet as the Foreign network.

### Dependencies

- [poa-bridge-contracts](https://github.com/poanetwork/poa-bridge-contracts)
- [token-bridge](https://github.com/poanetwork/token-bridge)
- [node.js](https://nodejs.org/en/download/)
- [Nifty Wallet](https://chrome.google.com/webstore/detail/nifty-wallet/jbdaocneiiinmjbjlgalhcelgbejmnid?hl=en) or [MetaMask](https://metamask.io/)

1. Create an empty folder where you will be setting up your bridge. In this example we call it `sokol-kovan-bridge`.
`mkdir sokol-kovan-bridge && cd sokol-kovan-bridge`  

2. Prepare a temporary ETH address for deployment. 
  * `VALIDATORS` : Generate New Ethereum Keystore File JSON and its password https://www.myetherwallet.com/#generate-wallet

3. Fund the test accounts.
  * Fund Home accounts(`Validators`) using the [POA Sokol Faucet](  https://faucet-sokol.herokuapp.com/ )
  * Get free Kovan Coins from the [gitter channel](https://gitter.im/kovan-testnet/faucet) or [Iracus faucet](https://github.com/kovan-testnet/faucet) for Foreign Accounts. Get 5 Keth to 1 acc, and transfer from it to all others.
  * 

4. Deploy the Sokol <-> Kovan Bridge contracts.
  * In the `sokol-kovan-bridge` folder, git clone `https://github.com/poanetwork/poa-bridge-contracts`
  * Follow instructions in the [POA Bridge contracts repo](https://github.com/poanetwork/poa-bridge-contracts/blob/master/deploy/README.md).
  * Set the parameters in the .env file.
    * DEPLOYMENT_ACCOUNT_ADDRESS: Temporary address from step 2
    * DEPLOYMENT_ACCOUNT_PRIVATE_KEY: Private key from step 2
    * HOME_RPC_URL=https://sokol.poa.network
    * FOREIGN_RPC_URL=https://kovan.infura.io/mew

5. Install and run the POA Bridge Oracle.
  * In the `sokol-kovan-bridge` folder, git clone `https://github.com/poanetwork/token-bridge`
  * Follow instructions in the [POA NodeJS Oracle repo](https://github.com/poanetwork/token-bridge).


If successful, you should see some bridge processes run when you issue a command. For example run `npm run watcher:signature-request`

**Example Output:**
```bash

```

6. Keep the bridge processes running. Open a separate terminal window and go to the `sokol-kovan-bridge` folder

  1. `git clone https://github.com/poanetwork/bridge-ui.git`  
  2. `cd bridge-ui`  
  3. `npm install`  
  4. Create a .env file from the example file[.env.example](.env.example)  
`cp .env.example .env`  
  5. Insert addresses from the bridgeDeploymentResults.json file into the .env file.
`cat ../poa-bridge-contracts/deploy/bridgeDeploymentResults.json`  

```bash
REACT_APP_HOME_BRIDGE_ADDRESS=0x.. //HomeBridge address in bridgeDeploymentResults.json
REACT_APP_FOREIGN_BRIDGE_ADDRESS=0x.. //ForeignBridge address in bridgeDeploymentResults.json
REACT_APP_FOREIGN_HTTP_PARITY_URL=https://kovan.infura.io/mew //http public RPC node for Foreign network
REACT_APP_HOME_HTTP_PARITY_URL=https://sokol.poa.network //http public RPC node for Home network
REACT_APP_GAS_PRICE_SPEED_TYPE=fast // Gas price speed option (slow, standard, fast, instant)
```

  6. Run `npm run start`
  7. Make sure you have a web3 wallet installed (Nifty Wallet or MetaMask) and connected to the POA Sokol Network. Check that you are connected the funded account from step 2 in the web3 wallet
  9. Specify an amount and click Transfer to make a cross chain transaction from Sokol to Kovan

## Testing

`npm run test`

To run tests with coverage

`npm run coverage`

## Optional 

- Bridge Monitoring: https://github.com/poanetwork/bridge-monitor
  In order to setup PagerDuty alerts for all administrators of the bridge, there is monitoring server that needs to deployed separately to make sure it notifies all interested parties of the status of the bridge contracts.

## Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) document for contribution, testing and pull request protocol.

## License

[![License: LGPL v3.0](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0)

This project is licensed under the GNU Lesser General Public License v3.0. See the [LICENSE](LICENSE) file for details.
