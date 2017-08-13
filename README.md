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
- Do some mining: `miner.start()`, wait for a bit. 
- In the `node-window`, you will see mining activity. `miner.stop()` to finish mining
- Check balance: `web3.eth.getBalance(eth.accounts[0])`

## Transfer some coin
In the `console-window`:
- Create account: `personal.newAccount('')`
- Transfer coin: `web3.eth.sendTransaction({to:eth.accounts[1],value:1000})`
- You might need to unlock account using `personal.unlockAccount(eth.accounts[0],account1, '')`
- `miner.start()` then `miner.stop()`
- Check balance: `web3.eth.getBalance(eth.accounts[1])`

Complete API reference: https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethsendtransaction

## Deploy a smart contract
- First get `solc` using `npm install -g solc solc-cli`
- Create multiply.solc:
```
pragma solidity ^0.4.15;

contract test {
  function multiply(uint a) returns(uint d){
    return a * 7;
  }
}
```
- Compile using `solc multiply.solc`
- Files would be output into `./contracts/*abi ./contracts/*.bin`
- Create contract using abi:
`MyContract = web3.eth.contract(<Copy as is from ./contracts/multiply*.abi>)`
- Load bytecode: `bytecode = "0x<Copy as is from ./contracts/multiply*.bin>"`

- Make sure bytecode is good using `web3.eth.estimateGas({data: bytecode})`
- Create contract:
```
contractInstance = MyContract.new({
    data: bytecode, gas: 1000000, from: account1
  }, function(e, contract){
    if(!e){
      if(!contract.address){
        console.log(
        "Contract transaction send: Transaction Hash: "+contract.transactionHash+
        " waiting to be mined...");
      } else {
        console.log("Contract mined! Address: "+contract.address);
        console.log(contract);
      }
    } else {
      console.log(e)
    }
  })
```
- Mine the contract using `miner.start()` then `miner.stop()`
