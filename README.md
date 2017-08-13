# geth-experiments
Experiments with geth and solc

## Install `geth` and `solc`
- `geth`: https://github.com/ethereum/go-ethereum/wiki/geth 
- `solc`: https://solidity.readthedocs.io/en/develop/

## Run a private net ([Reference](https://medium.com/taipei-ethereum-meetup/a-complete-guide-on-building-a-smart-contract-on-a-private-net-in-ethereum-726851c7c044))
- Start a network: `geth --rpc --rpcaddr 127.0.0.1 --rpcport 8545 --dev --datadir privchain`
Leave this window open. We will refer to this as the `node-window`

## Create accounts and mine
- Attach console to network: `geth attach ipc://Users/$USER/privchain/geth.ipc`
We will refer to this as the `console-window`

In the `console-window`:
- Create account: `personal.newAccount('')`
- Set this as default account: `web3.eth.defaultAccount = eth.accounts[0]`
- Do some mining: `miner.start()`, wait for a bit. In the `node-window`, you will see mining activity. `miner.stop()` to finish mining
- Check balance: `web3.eth.getBalance(eth.accounts[0])`

## Transfer some coin
In the `console-window`:
- Create account: `personal.newAccount('')`
- Transfer coin: `web3.eth.sendTransaction({to:eth.accounts[1],value:1000})`
- `miner.start()` then `miner.stop()`
- Check balance: `web3.eth.getBalance(eth.accounts[1])`

Complete API reference: https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethsendtransaction

