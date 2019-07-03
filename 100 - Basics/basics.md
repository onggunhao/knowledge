# Git

```
// Adding submodule to git tree
git submodule add <url> <folder or path to store in>
// e.g.
git submodule add https://github.com/CA-bootcamp-s19/simple-bank-exercise-onggunhao.git simple-bank-exercise-onggunhao
```





# To Categorize

Cosmos - Ethereum: https://blog.cosmos.network/using-ethermint-with-truffle-984e6721e30d

Generalized State Channels for Ethereum: https://medium.com/statechannels/counterfactual-generalized-state-channels-on-ethereum-d38a36d25fc6



Possible Topics:

1. Storage, Memory, Calldata, Pass by Reference, Pass by Value, New keyword (spurred by EthAmal)
2. Debugging a single test with Truffle, Mocha and Debugger, with stepthroughs
3. 

Random things to build

1. Security audits for testnet eth
2. Zapier for Oracles + Smart Contract functions (use scratch)



Enable 

Writing tests for Crowdsale

https://hashnode.com/post/the-2018-guide-to-writing-and-testing-real-world-crowdsale-contracts-cjcs8dfes00apmdwthw03c2jq

NKN Network

https://www.nkn.org/#/developer









Method of attack:

1. Splunk codebase
2. Run truffle test independently (using cli.js)
3. Print out dlog statements
4. 



`cli.js` initializes a `Command` instance (from `./lib/command.js`), and passes in `./lib/commands/index.js` (all the different commands) as a constructor argument.

`Command` instance uses `yargs` to store / generate help files for each individual command 

`cli.js` takes input arguments on line 46, passes it down to `command.run` on line 56.

in `Command.run` (lib/command.js:74), we  run the test command in line 113.

in `lib/commands/test.js` we run the `.run` command. It picks out the files that we want to run, and calls `getFiles` . After getting the files, 

`/lib/commands/test.js:112` is the Test.run command (calls lib/test.js). Passes in the test_files 

`/lib/commands/test.js` runs at 143, calls environmentCallback (145) which then calls run() (129), which calls `lib/test.js`







`lib/command.js` runs `run` on line 74





lib/commands/test.js calls lib/test.js

in lib/commands/test.js, there is a way to get arguments. Find the mocha argument and log it out







Scalability

https://medium.com/sandor-report/a-gentle-introduction-to-blockchain-scalability-part-i-42d477898122

Main ways of scalability:

1. Block size upgrades (i.e. parameter changes)

   https://hackernoon.com/the-blockchain-scalability-problem-the-race-for-visa-like-transaction-speed-5cce48f9d44

2. Layer 2 solutions (i.e. Plasma Cash, exit games)

3. IOTA DAG type tools





Twitter video: between 20 seconds and 2 minutes

Linkedin video: 30 seconds and 5 minutes

Instagram videos: 60 seconds

You lose 33% of viewers by 30 second mark. Hubspot suggests 26 second videos have the best engagement (30 second limitation to explain a really complex topic)



Quick Insta

Simplified

Instasimplified

Explainagrams

Explainagram - twitter available, others available





SDK 

* deterministic state machine with modules

Tendermint

* handles replication over network
* Random validator V chosen to be proposer of next block. If 2/3 of validators sign a prevote and precommit, and if all transactions are valid, it is added to chain

ABCI

* Tendermint handles transaction bytes, has no knowledge on what it means
* 











**Today**

- [ ] Learn Go (overview tutorial) (1-3pm)
- [ ] Go through Cosmos SDK tutorial (3-5pm)
- [ ] React test for Ethereum smart contract

