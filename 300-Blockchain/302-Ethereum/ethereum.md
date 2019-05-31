---
layout: default
title: Ethereum
parent: Blockchain
nav_order: 2
has_children: true
---

{:toc}

# todo

- [ ] [ScottWorks' Consensys notes](https://github.com/ScottWorks/ConsenSys-Academy-Notes)

# How it works

- Consensys Chap 2, 3, 4 etc
- Merkle trees et al

# Environment Setup

## Metamask

Source: [Consensys Academy Lesson](https://learn.consensys.net/unit/view/id:1906)

## Concept of Metamask

- Need way to sign a transaction w/ private key in the browser
- Metamask = chrome plugin that manages keys for user (previously: users would need to trust private keys to a website)
- Metamask injects a web3 API into websites, allows websites to read from blockchain using data,
- Metamask allows website to propose transactions to the user, shows user costs, allows user to accept or decline
- Metamask is a bridge that allows users to run Ethereum dApps in-browser without needing to run a full node

## Remix

- Browser-based Solidity IDE

### Loading local filesystem

You can connect Remix to your localhost filesystem. This is especially useful for working with truffle, where contracts are all in subfolders (or installed via `truffle install XXX`)

Folder is shared using websocket connection between Remix IDE and remixd. Remixd provides full read and write access to folder (possibly dangerous?)

https://remix.readthedocs.io/en/latest/remixd.html

```
remixd -s $(pwd) --remix-ide https://remix.ethereum.org
// Note: it will fail if URL contains trailing slash
```

## Deploying a Smart Contract w/o tooling

```javascript
// Start a private eth testnet (geth repl)
geth --dev console

// Check account (in geth repl)
eth.accounts[0]
eth.getBalance(eth.accounts[0])

// Create sol file (in new terminal tab)
// Grab sample solidity file from solidity docs
vi SimpleStorage.sol
// Compile using solc (solidity compiler)
echo var compiledStorage = `solc --combined-json abi,bin,interface SimpleStorage.sol` > simpleStorage.js

// Load ABI into geth repl
loadScript('simpleStorage.js')
var ABI = compiledStorage.contracts["SimpleStorage.sol:SimpleStorage"].abi

// Load binary into geth repl, add "0x" to indicate hex encoding
var bin = "0x" + compiledStorage.contracts["SimpleStorage.sol:SimpleStorage"].bin

// Deploy contract to blockchain
var deploymentTx = {
  from: eth.accounts[0],
  data: bin,
  gas: 1000000
}
var storageInterface = eth.contract(JSON.parse(ABI))
var storageInstance = storageInterface.new(deploymentTx)

// Get transaction receipts
var address = eth.getTransactionReceipt(storageInstance.transactionHash).contractAddress

// Interact with contract using contract ABI
var storage = storageInterface.at(address)
storage.get.call()
storage.set.sendTransaction(10, {
  from: eth.accounts[0],
  gas: 1000000
})

```

## Ganache-CLI

- Simulates private blockchain on local machine w/ funded accounts

**Cheatsheet**

```javascript
npm install -g ganache-cli

ganache-cli
ganache-cli -m <list of mnemonic words> // Useful for working w/ metamask, generates HD wallet addresses
```

## Truffle

"Ruby on Rails of smart contract development" (fml)

- [ ] Smart Contract Compilation
- [ ] Libary Linking
- [ ] Contract Testing
- [ ] Transaction Debugging
- [ ] Deployment
- [ ] Front-end Interaction (?)

### Truffle Cheatsheet

```javascript
truffle init
truffle unbox metacoin

truffle compile
truffle migrate // deploys contracts onto blockchain
```

```javascript
// To have a different truffle-config from the rest of the repo, untrack it in local repo
git update-index --assume-unchanged truffle-config.js
```

Truffle Test:

- Uses Mocha - testing / Chai - assertions OR solidity

4447 error: my answer
[https://ethereum.stackexchange.com/questions/69286/the-network-id-specified-in-the-truffle-config-4447-does-not-match-the-one-ret/71087#71087](https://ethereum.stackexchange.com/questions/69286/the-network-id-specified-in-the-truffle-config-4447-does-not-match-the-one-ret/71087#71087)

```javascript
// contract function instead of describe. Contracts are re-deployed ('clean room function')

contract("MetaCoin", accounts => {
  it("should put 10000 MetaCoin in the first account", async () => {
    const metaCoinInstance = await MetaCoin.deployed();
    const balance = await metaCoinInstance.getBalance.call(accounts[0]);

    assert.equal(balance.valueOf(), 10000, "10000 wasn't in the first account");
  });
  it("should call a function that depends on a linked library", async () => {
    const metaCoinInstance = await MetaCoin.deployed();
    const metaCoinBalance = (await metaCoinInstance.getBalance.call(
      accounts[0]
    )).toNumber();
    const metaCoinEthBalance = (await metaCoinInstance.getBalanceInEth.call(
      accounts[0]
    )).toNumber();

    assert.equal(
      metaCoinEthBalance,
      2 * metaCoinBalance,
      "Library function returned unexpected function, linkage may be broken"
    );
  });
  it("should send coin correctly", async () => {
    const metaCoinInstance = await MetaCoin.deployed();

    // Setup 2 accounts.
    const accountOne = accounts[0];
    const accountTwo = accounts[1];

    // Get initial balances of first and second account.
    const accountOneStartingBalance = (await metaCoinInstance.getBalance.call(
      accountOne
    )).toNumber();
    const accountTwoStartingBalance = (await metaCoinInstance.getBalance.call(
      accountTwo
    )).toNumber();

    // Make transaction from first account to second.
    const amount = 10;
    await metaCoinInstance.sendCoin(accountTwo, amount, { from: accountOne });

    // Get balances of first and second account after the transactions.
    const accountOneEndingBalance = (await metaCoinInstance.getBalance.call(
      accountOne
    )).toNumber();
    const accountTwoEndingBalance = (await metaCoinInstance.getBalance.call(
      accountTwo
    )).toNumber();

    assert.equal(
      accountOneEndingBalance,
      accountOneStartingBalance - amount,
      "Amount wasn't correctly taken from the sender"
    );
    assert.equal(
      accountTwoEndingBalance,
      accountTwoStartingBalance + amount,
      "Amount wasn't correctly sent to the receiver"
    );
  });
});
```

```javascript
/build // Created when truffle compile is run
  /contracts
/contracts
/migrations
/test
truffle-config.js
```

```javascript
// Migration scripts
// 1_initial_migration.js
```

- [ ] What does `deployer.link` do? Why does it need to link?

```javascript
// truffle-config.js
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 8545 // should be 7545 if truffle develop
      network_id: "*"
    }
  }
}
```

## EthPM integration

`truffle install <pkg>` installs EthPM packages in the `installed_packages` directory

- There is some stuff about EthPM v2 where you need to deploy packages yourself
- https://github.com/trufflesuite/truffle/issues/1883
- https://github.com/trufflesuite/trufflesuite.com/issues/398

## Ganache GUI

- Has nice way of seeing
  ![](../images/ganache-gui.png)

```
# Drizzle

- Hook up to chain via Web3
- Instantiating contracts
- Keep track of data


# Estimating Gas

https://ethgasstation.info/
```

# Solidity

# Writing Smart Contracts

# Smart Contract Security

# Concepts

## Metatransactions

## Plasma

- [ ] Build a Plasma chain tutorial https://medium.com/@dan_30977/new-chainshot-tutorial-plasma-mvp-a80629841936

# Cookbooks

## Time-based contract

https://ethereum.stackexchange.com/questions/413/can-a-contract-safely-rely-on-block-timestamp

# Crowdsale

reference contract for cryptocurrency is ERC20 standard (standard interface)
openzeppelin-solidity has reference contracts for crowdsales

tmux for terminal handling

```
init truffle
npm install openzeppelin-solidity
npm install all dependencies that openzep-solidity uses
```

.babelrc (use presets so we can use ES6)

truffle-config.js because truffle.js conflicts with windows

- only need development and ganache-cli (8545)
- specify solc version, with optimizer (TODO: what is that?), needs to be compatible with version of Truffle (find out by running truffle version)

Create a dapptoken.sol in /contracts

openzep-solidity library in node_modules -> openzeppelin -> DetailedERC20.sol

- importing it in custom contract file

```
truffle compile
truffle migrate
```

```
truffle console
DappToken.deployed().then((t) => { token = t })
token.address
```

Crowdsale contract

- Normal contract, only has 60,000 tokens that can be bought at 1 Dai each
  (how do we convert the rate?)

Validation

- CappedCrowdsale (in Dai)
- Whitelisted Crowdsale (only allow whitelisted participants to purchase tokens)
- TimedCrowdsale (i.e. from X time to Y time)

Distribution

- Default behavior: release tokens once loan is made
- RefundableCrowdsale?

Tokens

- Map addresses to balances

ERC20

- mapping(address => uint256) is fungible
- ERC20.sol implements, IERC20 is the interface, ERC20Detailed adds optional ERC20Detailed

ERC20 Burnable - allows you to burn the token (i.e. a donation?)
ERC20 Capped - allows you to say that there is a maximum cap on tokens

SafeERC20

ERC721

- Represent ownership that is non-fungible (e.g. company shares = common stock vs investor shares)
- IERC721 Receiver: contract needs to know how to handle ERC721 tokens
- each token has a tokenURI that resolves to a JSON document that has name, description, image

ERC20 vs ERC223 vs ERC777

- Cannot send funds to a smart contract account using `transfer`, which results in a successful transaction but contract never receives tokens
- ERC223 proposal: checks if `transfer` is sending tokens to a smart contract, will throw error as a result
