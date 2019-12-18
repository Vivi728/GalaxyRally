## Web3.js call interface

Interact with the underlying chain through the web3 object provided by web3.js. On the underlying implementation, it communicates with the local node through RPC calls. web3.js can connect to any PlatON node that exposes the RPC interface.

### Ready to work

First, make sure that the nodeJS environment is successfully installed locally, and introduce web3 into the project project through the npm package management tool. Follow these steps:

- npm: `npm i https: // github.com / PlatONnetwork / client-sdk-js`

Then you need to create a web3 instance and set up a provider. To ensure that existing providers are not overwritten, you need to check whether the web3 instance already exists. You can refer to the following code:

```js
if(typeof web3! == 'undefined') {
  web3 = new Web3(web3.currentProvider);
} else {
  // set the provider you want from Web3.providers
  web3 = new Web3(new Web3.providers.HttpProvider("http://localhost: 6789"));
}
```

After successful introduction, web3 related APIs are now available.

### Using callbacks

Since this API is designed to interact with the local PlatON node, all functions request the RPC interface synchronously by default.

If needed, make an asynchronous request. Most functions allow passing optional callback functions to support asynchronous. The callback function supports the style of error first callback.

```js
web3.platon.getBlock(48, function(error, result) {
    if(! error)
        console.log(JSON.stringify(result));
    else
        console.error(error);
})
```

### Detailed use

#### web3

The `web3` object provides all methods.

##### Example:

```js
var Web3 = require('web3');
// create an instance of web3 using the HTTP provider.
// NOTE in mist web3 is already available, so check first if it's available before instantiating
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost: 8545"));
```

------

#### web3.version.api

##### transfer:

```js
web3.version.api
```

##### return value:

 `String`-API version number of PlatON js

##### Example:

```js
var version = web3.version.api;
console.log(version); // "0.2.0"
```

------

#### web3.version.node

##### transfer:

web3.version.node

// asynchronous mode

web3.version.getNode(callback(error, result) {...})

##### return value:

 `String`-client or node version information

##### Example:

```js
var version = web3.version.node;
console.log(version); // "Mist / v0.9.3 / darwin / go1.4.1"
```

------

#### web3.version.network

##### transfer:

web3.version.network

// asynchronous mode

web3.version.getNetwork(callback(error, result) {...})

##### return value:

 `String`-network protocol version

##### Example:

```js
var version = web3.version.network;
console.log(version); // 54
```

------

#### web3.version.platon

##### transfer:

web3.version.platon

// asynchronous mode

web3.version.getPlaton(callback(error, result) {...})

##### return value:

 `String`-Protocol version of PlatON

##### Example:

```js
var version = web3.version.platon;
console.log(version); // 60
```

------

#### web3.isConnected

Check if the connection exists.

##### transfer:

web3.isConnected()

##### Parameters:

no

##### return value:

 `Boolean`

##### Example:

```js
if(! web3.isConnected()) {
  
   // show some dialog to ask the user to start a node
 } else {
 
   // start web3 filters, calls, etc
}

```

------

#### web3.setProvider

Set the Provider.

##### transfer:

web3.setProvider(provider)

##### Parameters:

no

##### return value:

 `undefined`

##### Example:

```js
web3.setProvider(new web3.providers.HttpProvider('http://localhost: 8545')); // 8080 for cpp / AZ, 8545 for go / mist

```

------

#### web3.currentProvider

If a Provider has been set, returns the current Provider. This method can check whether the Provider has been set in the case of using the mist browser, etc., to avoid duplicate settings.

##### transfer: 

web3.currentProvider

##### return value:

 `Object`-` null` or `Provider` already set

##### Example:

```js
// Check if mist etc. already set a provider
if(! web3.currentProvider)
    web3.setProvider(new web3.providers.HttpProvider("http://localhost: 8545"));

```

------

#### web3.reset

Reset the status of web3. Reset everything except the manager. Uninstall filter and stop status polling.

##### transfer: 

web3.reset(keepIsSyncing)

##### Parameters:

1. `Boolean`-if set to true, all filters will be uninstalled, but state polling for web3.platon.isSyncing() will be retained

##### return value:

 `undefined`

##### Example:

```js
web3.reset();

```

------

#### web3.sha3

##### transfer: 

web3.sha3(string [, options])

##### Parameters:

1. `String`-incoming string that needs to be hashed using Keccak-256 SHA3 algorithm
2. `Object`-optional setting. If you want to parse a hex string in hex format. Need to set encoding to hex. Because 0x is ignored by default in JS

##### return value:

 `String`-hashed result using Keccak-256 SHA3 algorithm

##### Example:

```js
var hash = web3.sha3("Some string to be hashed");
console.log(hash); // "0xed973b234cf2238052c9ac87072c71bcf33abc1bbd721018e0cca448ef79b379"
var hashOfHash = web3.sha3(hash, {encoding: 'hex'});
console.log(hashOfHash); // "0x85dd39c91a64167ba20732b228251e67caed1462d4bcf036af88dc6856d0fdcc"

```

------

#### web3.toHex

Convert any value to Hex.

##### transfer: 

web3.toHex(mixed);

##### Parameters:

1. `String | Number | Object | Array | BigNumber`-the value to be converted to HEX. If it is an object or array type, it will first be converted to a string using JSON.stringify. If BigNumber is passed in, you will get the HEX of the corresponding Number

##### return value:

 `String`-String converted to Hex

##### Example:

```js
var str = web3.toHex((test: 'test'));
console.log(str); // '0x7b2274657374223a2274657374227d'

```

------

#### web3.toAscii

Convert HEX string to ASCII string.

##### transfer:

web3.toAscii(hexString);

##### Parameters:

1. `String`-hex string

##### return value:

 `String`-the ASCII value corresponding to the given hexadecimal string

##### Example:

```js
var str = web3.toAscii("0x506c61744f4ed");
console.log(str); // "PlatON"

```

------

#### web3.fromAscii

Convert any ASCII string into a HEX string.

##### transfer:

web3.fromAscii(string);

##### Parameters:

1. `String`-ASCII string

##### return value:

 `String`-the converted HEX string

##### Example:

```js
var str = web3.fromAscii('PlatON');
console.log(str); // "0x506c61744f4e"

```

------

#### web3.toDecimal

Convert a hexadecimal to a decimal number.

##### transfer:

web3.toDecimal(hexString);

##### Parameters:

1. `String`-hex string

##### return value:

`Number`-the hexadecimal value represented by the passed string

##### Example:

```js
var number = web3.toDecimal('0x15');
console.log(number); // 21

```

------

#### web3.fromDecimal

Converts a number, or a string of numbers, into a hexadecimal string.

##### transfer:

web3.fromDecimal(number);

##### Parameters:

1. `Number | String`-number

##### return value:

 `String`-the hexadecimal representation of the given number

##### Example:

```js
var value = web3.fromDecimal('21 ');
console.log(value); // "0x15"

```

------

#### web3.fromVon

Conversion between PlatON currency units. The number of von units is converted to the following units, which can take the following values:

-`von`
-`kvon`
-`mvon`
-`gvon`
-`microlat`
-`millilat`
-`lat`
-`klat`
-`mlat`
-`glat`
-`tlat`

##### transfer:

web3.fromVon(number, unit)

##### Parameters:

1. `Number | String | BigNumber`-number or BigNumber
2. `String`-unit string

##### return value:

`String | BigNumber`-depending on the parameters passed, either a string as a string, or a BigNumber

##### Example:

```js
var value = web3.fromVon('21000000000000', 'gvon');
console.log(value); // "21000"

```

------

#### web3.toVon

Convert to von by the corresponding currency. The following units can be selected:

-`von`
-`kvon`
-`mvon`
-`gvon`
-`microlat`
-`millilat`
-`lat`
-`klat`
-`mlat`
-`glat`
-`tlat`

```
'von': '1',
'kvon': '1000',
'mvon': '1000000',
'gvon': '1000000000',
'microlat': '1000000000000',
'millilat': '1000000000000000',
'lat': '1000000000000000000',
'klat': '1000000000000000000000',
'mlat': '1000000000000000000000000',
'glat': '1000000000000000000000000000',
'tlat': '1000000000000000000000000000000'

```

##### transfer:

web3.toVon(number, unit)

##### Parameters:

1. `Number | String | BigNumber`-number or BigNumber
2. `String`-string unit

##### return value:

 `String | BigNumber`-depending on the parameters passed, either a string as a string, or a BigNumber

##### Example:

```js
var value = web3.toVon('1', 'lat');
console.log(value); // "1000000000000000000"

```

------

#### web3.toBigNumber

Converts the given number or hexadecimal string to a BigNumber.

##### transfer:

web3.toBigNumber(numberOrHexString);

##### Parameters:

1. `Number | String`-number in hexadecimal format

##### return value:

`BigNumber`-an instance of BigNumber

##### Example:

```js
var value = web3.toBigNumber('200000000000000000000001');
console.log(value); // instanceOf BigNumber
console.log(value.toNumber()); // 2.0000000000000002e + 23
console.log(value.toString(10)); // '200000000000000000000001'

```

------

#### web3.isAddress

Checks if the given string is an address.

##### transfer:

web3.isAddress(HexString);

##### Parameters:

1. `String`-hex string

##### return value:

 `Boolean`-false if not a valid address format. Returns true if all lowercase or all uppercase valid addresses. If it is a mixed case address, check it with `web3.isChecksumAddress()`

##### Example:

```js
var isAddress = web3.isAddress("0x8888f1f195afa192cfee860698584c030f4c9db1");
console.log(isAddress); // true

```

------

#### web3.net

#### web3.net.listening

This property is read-only and indicates whether the currently connected node is listening to the network or not. listen can be understood as receiving.

##### transfer:

web3.net.listening

// asynchronous mode

web3.net.getListening(callback(error, result) {...})

##### return value:

`Boolean`-` true` indicates that the node on the connection is listening to the network request, otherwise returns false

##### Example:

```js
var listening = web3.net.listening;
console.log(listening); // true of false

```

------

#### web3.net.peerCount

The attribute is read-only and returns the number of other PlatON nodes to which the connected node is connected.

##### transfer:

web3.net.peerCount

// asynchronous mode

web3.net.getPeerCount(callback(error, result) {.....})

##### return value:

 `Number`-the number of other PlatON nodes connected by the connection node

##### Example:

```js
var peerCount = web3.net.peerCount;
console.log(peerCount); // 4

```

------

#### web3.platon

Contains methods related to the PlatON blockchain.

##### Example:

```js
var platon = web3.platon;

```

------

#### web3.platon.gasPrice

The property is read-only and returns the current gas price. This value is determined by the median gas price of the last few blocks.

##### transfer:

web3.platon.gasPrice

// asynchronous mode
web3.platon.getGasPrice(callback(error, result) {...})

##### return value:

`BigNumber`-BigNumber instance of current gas price in von

##### Example:

```js
var gasPrice = web3.platon.gasPrice;
console.log(gasPrice.toString(10)); // "10000000000000"

```

------

#### web3.platon.accounts

A read-only property that returns a list of accounts held by the current node.

##### transfer:

web3.platon.accounts

// asynchronous mode

web3.platon.getAccounts(callback(error, result) {...})

##### return value:

 `Array`-List of accounts held by the node

##### Example:

```js
var accounts = web3.platon.accounts;
console.log(accounts); // ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]

```

------

#### web3.platon.blockNumber

The property is read-only and returns the latest block number.

##### transfer:

web3.platon.blockNumber

// asynchronous mode

web3.platon.getBlockNumber(callback(error, result) {...})

##### return value:

`Number`-latest block number

##### Example:

```js
var number = web3.platon.blockNumber;
console.log(number); // 2744

```

------

#### web3.platon.getBalance

Get the balance of the given address at the specified block.

##### transfer:

web3.platon.getBalance(addressHexString [, defaultBlock] [, callback])

##### Parameters:

1. `String`-the address to query the balance
2. `Number | String`-(optional) if this value is not set
3. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

 `String`-a BigNumber instance containing the current balance of the given address, in von

##### Example:

```js
var balance = web3.platon.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(balance); // instanceof BigNumber
console.log(balance.toString(10)); // '1000000000000'
console.log(balance.toNumber()); // 1000000000000

```

------

#### web3.platon.getStorageAt

Gets the stored status value at a specified location of an address.

##### transfer:

web3.platon.getStorageAt(addressHexString, position [, defaultBlock] [, callback])

##### Parameters:

1. `String`-get the address of the storage
2. `Number`-the number of the storage to get
3. `Number | String`-(optional) If no parameter is passed, the block defined by web3.platon.defaultBlock is used by default, otherwise the specified block is used
4. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

`String`-Stored value at given position

##### Example:

```js
var state = web3.platon.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
console.log(state); // "0x03"

```

------

#### web3.platon.getCode

Get the code of the specified address.

##### transfer:

web3.platon.getCode(addressHexString [, defaultBlock] [, callback])

##### Parameters:

1. `String`-the address to get the code from
2. `Number | String`-(optional) if no parameter is passed, the block defined by web3.platon.defaultBlock is used by default, otherwise the specified block is used
3. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

`String`-the compiled byte code for the given address contract

##### Example:

```js
var code = web3.platon.getCode("0xd5677cf67b5aa051bb40496e68ad359eb97cfbf8");
console.log(code); // "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"

```

------

#### web3.platon.getBlock

Returns the block corresponding to the block number or block hash.

##### transfer:

web3.platon.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

##### Parameters:

- Number | String-(optional) if no parameter is passed, the block defined by web3.platon.defaultBlock is used by default, otherwise the specified block
- Boolean-(Optional) The default value is false. true returns all transactions contained in the block as objects. Otherwise only the hash of the transaction is returned
- Function-callback function for supporting asynchronous execution

##### return value:

- `Object`-The block object:
- Number-The block number. When this block is pending it will return null
- hash-string, the hash string of the block. When this block is pending it will return null
- parentHash-string, 32-byte hash of the parent block
- nonce-string, 8 bytes. POW generated hash. When this block is pending it will return null
- sha3Uncles-string, 32 bytes. Hash of uncle block
- logsBloom-string, Bloom filter for block logs 9. When this block is pending it will return null
- transactionsRoot-string, 32 bytes, root of block transaction prefix tree
- stateRoot-string, 32 bytes. The root of the block's final state prefix tree
- miner-String, 20 bytes. Rewarded miners for this block
- difficulty-BigNumber type. Difficulty of the current block, integer
- totalDifficulty-BigNumber type. Total difficulty from blockchain to current block, integer
- extraData-string. Extra data field of the current block
- size-Number. The byte size of the current block
- gasLimit-Number, the maximum gas allowed in the current block
- gasUsed-the total gas used by the current block
- timestamp-Number. Unix timestamp when the block is packed
- transactions-array. Trading partners. Or a 32-byte transaction hash
- uncles-array. Uncle hashed array

##### Example:

```js
var info = `Object`-The block object;
console.log(info);
/ *
{
  "number": 3,
  "hash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "parentHash": "0x2302e1c0b972d00932deb5dab9eb2982f570597d9d42504c05d9c2147eaf9c88",
  "nonce": "0xfb6e1a62d119228b",
  "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  "transactionsRoot": "0x3a1b03875115b79539e5bd33fb00d8f7b7cd61929d5a3c574f507b8acf415bee",
  "stateRoot": "0xf1133199d44695dfa8fd1bcfe424d82854b5cebef75bddd7e40ea94cda515bcb",
  "miner": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "difficulty": BigNumber,
  "totalDifficulty": BigNumber,
  "size": 616,
  "extraData": "0x",
  "gasLimit": 3141592,
  "gasUsed": 21662,
  "timestamp": 1429287689,
  "transactions": [
    "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b"
  ],
  "uncles": []
}
* /

```

------

#### web3.platon.getBlockTransactionCount

Returns the number of transactions in the specified block.

##### transfer:

web3.platon.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

##### Parameters:

- Number | String-(optional) if no parameter is passed, the block defined by web3.platon.defaultBlock is used by default, otherwise the specified block is used
- Function-callback function for supporting asynchronous execution

##### return value:

 `Number`-number of transactions for a given block

##### Example:

```js
var number = web3.platon.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1

```

------

##### web3.platon.getTransaction

Returns transactions that match the specified transaction hash.

##### transfer:

web3.platon.getTransaction(transactionHash [, callback])

##### Parameters:

- String-The hash value of the transaction
- Function-callback function for supporting asynchronous execution

##### return value:

- `Object`-A transaction object its hash` transactionHash`:
- hash: String-32 bytes, the hash value of the transaction
- nonce: Number-the number of transactions previously initiated by the transaction originator
- blockHash: String-32 bytes. The hash value of the exchange in the block. When this block is pending it will return null
- blockNumber: Number-The block number of the exchange in the block. When this block is pending it will return null
- transactionIndex: Number-an integer. The sequence number of the transaction in the block. When this block is pending it will return null
- from: String-20 bytes, the address of the transaction initiator
- to: String-20 bytes, the address of the recipient of the transaction. When this block is pending it will return null
- value: BigNumber-the amount of currency attached to the transaction, in von
- gasPrice: BigNumber-gas price configured by the transaction initiator, in von
- gas: Number-The gas provided by the transaction initiator
- input: String-data attached to the transaction

##### Example:

```js
var transaction = web3.platon.getTransaction('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(transaction);
/ *
{
  "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "nonce": 2,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "transactionIndex": 0,
  "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",
  "value": BigNumber,
  "gas": 314159,
  "gasPrice": BigNumber,
  "input": "0x57cb2fc4"
}
* /

```

------

#### web3.platon.getTransactionFromBlock

Returns the transaction with the specified sequence number in the specified block.

##### transfer:

getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

##### Parameters:

- String-The block number or hash. Or earliest, latest or pending. See web3.platon.defaultBlock for optional values
- Number-The serial number of the transaction
- Function-callback function for supporting asynchronous execution

##### return value:

 `Object`-transaction object, see web3.platon.getTransaction

##### Example:

```js
var transaction = web3.platon.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see web3.platon.getTransaction

```

------

#### web3.platon.getTransactionReceipt

Returns a receipt for a transaction via a transaction hash.

** Remarks ** For transactions in the pending state, receipts are not available.

##### transfer:

web3.platon.getTransactionReceipt(hashString [, callback])

##### Parameters:

- String-The hash of the transaction
- Function-callback function for supporting asynchronous execution

##### return value:

- `Object`-A transaction receipt object, or` null` when no receipt was found:
- blockHash: String-32 bytes, the block hash of this exchange
- blockNumber: Number-the block number of the exchange in the block
- transactionHash: String-32 bytes, transaction hash
- transactionIndex: Number-the sequence number of the transaction in the block, integer
- from: String-20 bytes, the address of the sender of the transaction
- to: String-20 bytes, the address of the recipient of the transaction. If it is a contract-created transaction, returns null
- cumulativeGasUsed: Number-the total amount of gas spent after the execution of the current transaction is 10
- gasUsed: Number-gas used to execute the current transaction alone
- contractAddress: String-20 bytes, the contract address created. If it is a contract creation transaction, the contract address is returned, otherwise-null
- logs: Array-array of log objects generated by this transaction

##### Example:

```js
var receipt = web3.platon.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(receipt);
{
  "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "transactionIndex": 0,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
  "status": "0x1"
}

```

------

#### web3.platon.getTransactionCount

Returns the transaction at the specified address in the specified block.

##### transfer:

web3.platon.getTransactionCount(addressHexString [, defaultBlock] [, callback])

##### Parameters:

- String-Address to get the number of transactions
- Number | String-(optional) if no parameter is passed, the block defined by web3.platon.defaultBlock is used by default, otherwise the specified block is used
- Function-callback function for supporting asynchronous execution

##### return value:

 `Number`-the number of transactions sent by the specified address

##### Example:

```js
var number = web3.platon.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1

```

------

#### web3.platon.sendTransaction

Send a transaction to the network.

##### transfer:

web3.platon.sendTransaction(transactionObject [, callback])

##### Parameters:

- `Object`-the transaction object to send
- from: String-The address of the specified sender. If not specified, use web3.platon.defaultAccount
- to: String-(Optional) The destination address of the transaction message. If the contract is created, leave it blank.
- value: Number | String | BigNumber-(Optional) The amount of currency carried in the transaction, in von. If the contract creates a transaction, the initial fund
- gas: Number | String | BigNumber-(optional) the default is automatic, available gas for transactions, unused gas will be returned
- gasPrice: Number | String | BigNumber-(optional) the default is automatically determined, the gas price of the transaction, the default is the average of the network gas price
- data: String-(optional) or a byte string containing related data, if it is contract creation, the code used for initialization
- nonce: Number-(optional) integer, using this value allows you to overwrite your own nonce, pending transactions
- `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

 `String`-A 32-byte transaction hash string. In hexadecimal

If the transaction is a contract creation, use web3.platon.getTransactionReceipt() to get the address of the contract after the transaction is completed

##### Example:

```js
var code = "603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b6007600435028060005260206000f3";
web3.platon.sendTransaction({data: code}, function(err, transactionHash) {
if(! err)
console.log(transactionHash); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
});

```

------

#### web3.platon.sendRawTransaction

Send a signed transaction.

##### transfer:

web3.platon.sendRawTransaction(signedTransactionData [, callback])

##### Parameters:

1. `String`-Signature transaction data in hexadecimal format
2. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

 `String`-32-byte hexadecimal format transaction hash string

##### Example:

```js
var Tx = require('ethereumjs-tx');
var privateKey = new Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688d3dcffdd2270c0fd109', 'hex')
 var rawTx = {
  nonce: '0x00',
  gasPrice: '0x09184e72a000',
  gasLimit: '0x2710',
  to: '0x0000000000000000000000000000000000000000',
  value: '0x00',
  data: '0x7f746573743200000000000000000000000000000000000000000000000000000000000057'
}
 var tx = new Tx(rawTx);
tx.sign(privateKey);
 var serializedTx = tx.serialize();
 //console.log(serializedTx.toString('hex '));
// f889808609184e72a00082271094000000000000000000000000000000000000000080a47f7465737432000000000000000000000000000000000000000000000000000000000000571600008ca08a8bbf888cfa37bbf0bb965423625641fc956967b81d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e3df1f1fff
 web3.platon.sendRawTransaction('0x' + serializedTx.toString('hex'), function(err, hash) {
  if(! err)
    console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385"
});

```

------

#### web3.platon.sign

Use the specified account to sign the data to be sent, and the account needs to be unlocked.

##### transfer:

web3.platon.sign(address, dataToSign, [, callback])

##### Parameters:

1. `String`-address used for signature
2. `String`-the data to be signed
3. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

 `String`-Signed data

The returned value corresponds to the ECDSA(Elliptic Curve Digital Signature Algorithm) signed string

```
r = signature [0:64]
s = signature [64: 128]
v = signature [128: 130]

```

It should be noted that if you use ecrecover, the v value here is 00 or 01, so if you want to use them, you need to convert the v value here to an integer, plus 27. The final value you will use will be 27 or 28.

##### Example:

```js
var result = web3.platon.sign("0x135a7de83802408321b74c322f8558db1679ac20",
    "0x9dd2c369a187b4e6b9c402f030e50743e619301ea62aa4c0737d4ef7e10a3d49"); // second argument is web3.sha3("xyz")
console.log(result); // "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"

```

------

#### web3.platon.call

In the node's VM, the message call transaction is performed directly. But the data will not be merged into the blockchain(such calls will not modify the state).

##### transfer:

web3.platon.call(callObject [, defaultBlock] [, callback])

##### Parameters:

1. `Object`-returns a transaction object, the same as web3.platon.sendTransaction. The difference from sendTransaction is that the from attribute is optional
2. `Number | String`-(optional) if this value is not set
3. `Function`-(optional) callback function for supporting asynchronous execution

##### return value:

 `String`-the value returned by the function call

##### Example:

```js
var result = web3.platon.call({
    to: "0xc4abd0339eb8d57087278718986382264244252f",
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"

```

------

#### web3.platon.estimateGas

Perform a message call, or transaction, in the node's VM node. But it will not be integrated into the blockchain. Returns the amount of gas used.

##### transfer:

web3.platon.estimateGas(callObject [, callback])

##### Parameters:

Same as web3.platon.sendTransaction, all properties are optional

##### return value:

 `Number`-Gas cost for simulated call / transcation

##### Example:

```js
var result = web3.platon.estimateGas({
    to: "0xc4abd0339eb8d57087278718986382264244252f",
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"

```

------

#### web3.platon.filter

```js
// can be 'latest' or 'pending'
var filter = web3.platon.filter(filterString);
// OR object are log filter options
var filter = web3.platon.filter(options);
 // watch for changes
filter.watch(function(error, result) {
  if(! error)
    console.log(result);
});
 // Additionally you can start watching right away, by passing a callback:
web3.platon.filter(options, function(error, result) {
  if(! error)
    console.log(result);
});

```

##### Parameters:

1. `String | Object`-A string's optional value [latest, pending]. latest means monitoring the latest block changes, pending means monitoring the pending block. If you need to filter by condition objects, as follows:

- `fromBlock`:` Number | String`-the starting block number(if the string latest is used, meaning the latest, block being packed), the default value is latest
- `toBlock`:` Number | String`-the termination block number(if the string latest is used, meaning the latest block being packed), the default value is latest
- `address`:` String`-single or multiple addresses. Get logs for specified account
- `topics`:` Array of Strings`-an array of strings that must appear in the log object. The order is very important. If you want to ignore topics, such as [null, '0x00 ...'], you can also pass a separate array of options for each topic, such as [null, ['option1', 'option1' ]]

##### return value:

 `Object`-A filter object with the following methods:

- `filter.get(callback)`: returns logs that meet the filter conditions
- `filter.watch(callback)`: listen for state changes that meet the conditions, and call the callback when the conditions are met
- `filter.stopWatching()`: Stop listening and clear the filter from the node. You should always perform this operation after listening

##### Watch callback return value

- `String`-When using the latest parameter. Returns the latest block hash value
- `String`-When using the pending parameter. Returns the latest transaction hash in pending
- `Object`-When using the manual filtering option, the following log objects will be returned
- logIndex: Number-The number of the log in the block. Null for pending logs
- transactionIndex: Number-The sequence number of the transaction that generated the log in the block. Null for pending logs
- transactionHash: String, 32 bytes-transaction hash value of the generated log
- blockHash: String, 32 bytes-The hash of the block where the log is located. Null for pending logs
- blockNumber: Number-The block number of the block where the log is located. Null for pending logs
- address: String, 32 bytes-the contract address generated by the log
- data: string-contains one or more 32-byte non-indexed parameters of the log
- topics: String []-One to four 32-byte indexed log parameter arrays(in Solidity, the first topic is the signature of the entire event(eg, Deposit(address, bytes32, uint256)), but if you use Except when defining events anonymously)

##### Example:

```js
var filter = web3.platon.filter((toBlock: 'pending'));
 filter.watch(function(error, log) {
  console.log(log); // {"
address ":" 0x0000000000000000000000000000000000000000000000 "," data ":" 0x0000000000000000000000000000000000000000000000000000000000000000 ", ...}
});
 // get all past logs again.
var myResults = filter.get(function(error, logs) {...});
 ...
 // stops and uninstalls the filter
filter.stopWatching();

```

------

## Built-in contract calling guide

### Overview

The call or send function of the call object ppos(the built-in contract related to the economic model) converts the parameters passed into the parameters required by the rpc interface platon_call or platon_sendRawTransaction call, and then sends the transaction to the chain. And some helper functions needed to complete the call and send parameters.

### Brief use

When calling `const web3 = new Web3('http://127.0.0.1:6789');` when instantiating a web3, the system will automatically append a ppos object to web3. In other words, you can use web3.ppos to call some methods of ppos. But if you want to use the ppos object to send transactions on the chain, in addition to the provider passed in when instantiating `web3`, you also need to send at least the private key and chain id required for transaction signatures, where the chain id can be passed through the rpc interface` `chainName 'returned by admin_nodeInfo`: xxx`.

Of course, in order to satisfy multiple ppos that can be instantiated arbitrarily(for example, if I want to instantiate 3 ppos to different chains and send transaction calls at the same time), I will also attach a PPOS object to the web3 object(note that all capitalization). You can call `new PPOS(setting)` to instantiate a ppos object. An example call is as follows:

```JavaScript
(async() => {
    const Web3 = require('web3');
    const web3 = new Web3('http://192.168.120.164:6789');
    const ppos = web3.ppos; // I will use ppos as the object in the following examples. It will not be written as web3.ppos.

    // Update the configuration of ppos. This step must be performed when sending an on-chain transaction.
    // Since the provider was passed in when instantiating web3, it is not necessary to pass in the provider.
    ppos.updateSetting({
        privateKey: 'acc73b693b79bbb56f89f63ccc3a0c00bf1b8380111965bfe8ab22e32045600c',
        chainId: 100,
    })

    let data, reply;

    // Pass parameters to send transactions as objects: 1000. createStaking(): initiate pledge
    const benefitAddress = '0xe6F2ce1aaF9EBf2fE3fbA8763bABaDf25e3fb5FA';
    const nodeId = '80f1fcee54de74dbf7587450f31c31c0e057bedd4faaa2a10c179d52c900ca01f0fb255a630c49d83b39f970d175c42b12a341a37504be248d76ecf592d32bc0';
    const amount = '10000000000000000000000000000';
    const blsPubKey = 'd2459db974f49ca9cbf944d4d04c2d17888aef90858b62d6aec166341a6e886e8c0c0cfae9e469c2f618f5d9b7a249130d10047899da6154288c9cde07b576acacd75fefaa5ef5a7e0f5a0e5b0a5bf
    data = {
        funcType: 1000,
        typ: 0,
        benefitAddress: ppos.hexStrBuf(benefitAddress),
        nodeId: ppos.hexStrBuf(nodeId),
        externalId: 'externalId',
        nodeName: 'Me',
        website: 'www.platon.network',
        details: 'staking',
        amount: ppos.bigNumBuf(amount),
        programVersion: undefined, // rpc get
        programVersionSign: undefined, // rpc gets
        blsPubKey: ppos.hexStrBuf(blsPubKey),
        blsProof: undefined, // rpc get
    }
    let pv = await ppos.rpc('admin_getProgramVersion');
    let blsProof = await ppos.rpc('admin_getSchnorrNIZKProve');
    data.programVersion = pv.Version;
    data.programVersionSign = pv.Sign;
    data.blsProof = ppos.hexStrBuf(blsProof);
    reply = await ppos.send(data);
    console.log('createStaking params object reply:', JSON.stringify(reply, null, 2));

    // Pass parameters to send transactions as an array: 1000. createStaking(): initiate a pledge
    data = [
        1000,
        0,
        ppos.hexStrBuf(benefitAddress),
        ppos.hexStrBuf(nodeId),
        'externalId',
        'Me',
        'www.platon.network',
        'staking',
        ppos.bigNumBuf(amount),
        pv.Version,
        pv.Sign,
        ppos.hexStrBuf(blsPubKey),
        ppos.hexStrBuf(blsProof)
    ];
    // As it has been called above, the trade fair will be on the chain, but the business will fail
    reply = await ppos.send(data);
    console.log('createStaking params array reply:', reply);

    // Passing parameters is called as an object: 1102. getCandidateList(): Query all real-time candidate lists
    data = {
        funcType: 1102,
    }
    reply = await ppos.call(data);
    console.log('getCandidateList params object reply:', reply);

    // Pass the parameter as an array: 1102. getCandidateList(): Query the list of all real-time candidates
    data = [1102];
    reply = await ppos.call(data);
    console.log('getCandidateList params array reply:', reply);

    // Re-instantiate a ppos1 object and call it
    const ppos1 = new web3.PPOS({
        provider: 'http://127.0.0.1:6789',
        privateKey: '9f9b18c72f8e5154a9c59af2a35f73d1bdad37b049387fc6cea2bac89804293b',
        chainId: 100,
    })
    reply = await ppos1.call(data);
})()

```

The log information is output as follows. In order to save space, there are cuts

```
createStaking params object reply: {
  "blockHash": "0xdddd6b12919b69169b63d17fece52e8632fe3d8b48166c8b4ef8fdee39a1f35c",
  "blockNumber": "0xb",
  "contractAddress": null,
  "cumulativeGasUsed": "0x14f34",
  "from": "0x714de266a0effa39fcaca1442b927e5f1053eaa3",
  "gasUsed": "0x14f34",
  "logs": [
    {
      "address": "0x1000000000000000000000000000000000000002",
      "topics": [
        "0xd63087bea9f1800eed943829fc1d61e7869764805baa3259078c1caf3d4f5a48"
      ],
      "data": "0xe3a27b22436f6465223a302c2244617461223a22222c224572724d7367223a226f6b227d",
      "blockNumber": "0xb",
      "transactionHash": "0x4bee71e351076a81482e2576e469a8dfaa76da9b6cc848265c10968d6de67364",
      "transactionIndex": "0x0",
      "blockHash": "0xdddd6b12919b69169b63d17fece52e8632fe3d8b48166c8b4ef8fdee39a1f35c",
      "logIndex": "0x0",
      "removed": false,
      "dataStr": {
        "Code": 0,
        "Data": "",
        "ErrMsg": "ok"
      }
    }
  ],
  "logsBloom": "",
  "root": "0x3b7a41cea97f90196039586a3068f6a64c09aa7597898440c3c241a095e37984",
  "to": "0x1000000000000000000000000000000
000000002 ",
  "transactionHash": "0x4bee71e351076a81482e2576e469a8dfaa76da9b6cc848265c10968d6de67364",
  "transactionIndex": "0x0"
}

createStaking params array reply: {blockHash:
   '0x43351e4a9f1b7173552094bacfd5b6f84f18a6c7c0c02d8a10506e3a61041117',
  blockNumber: '0x10',
  contractAddress: null,
  cumulativeGasUsed: '0x14f34',
  from: '0x714de266a0effa39fcaca1442b927e5f1053eaa3',
  gasUsed: '0x14f34',
  logs:
   [{address: '0x1000000000000000000000000000000000000002',
       topics: [Array],
       data:
        '0xf846b8447b22436f6465223a3330313130312c2244617461223a22222c224572724d7367223a22546869732063616e64696461746520697320616c7265616479206578697374227d',
       blockNumber: '0x10',
       transactionHash:
        '0xe5cbc728d6e284464c30ce6f0bbee5fb2b30351a591424f3a0edd37cc1bbdc05',
       transactionIndex: '0x0',
       blockHash:
        '0x43351e4a9f1b7173552094bacfd5b6f84f18a6c7c0c02d8a10506e3a61041117',
       logIndex: '0x0',
       removed: false,
       dataStr: [Object]}],
  logsBloom: '',
  root:
   '0x45ffeda340b68a0d54c5556a51f925b0787307eab1fb120ed141fd8ba81183d4',
  to: '0x1000000000000000000000000000000000000002',
  transactionHash:
   '0xe5cbc728d6e284464c30ce6f0bbee5fb2b30351a591424f3a0edd37cc1bbdc05',
  transactionIndex: '0x0'}

getCandidateList params object reply: {
  Code: 0,
  Data:
   [{candidate1 info ...},
     {candidate2 info ...},
     {candidate3 info ...},
     {candidate4 info ...}
   ],
  ErrMsg: 'ok'}

getCandidateList params array reply: {
  Code: 0,
  Data:
   [{candidate1 info ...},
     {candidate2 info ...},
     {candidate3 info ...},
     {candidate4 info ...}
   ],
  ErrMsg: 'ok'}

```

### API call details

#### `updateSetting(setting)`

Update the configuration parameters of the ppos object. If you only need to send a call, you only need to pass in the provider. If you passed in the provider when instantiating web3. Then the provider of ppos defaults to the provider you instantiated from web3. Of course you can also update the provider at any time.

If you are sending a send transaction, in addition to the provider, you must also pass in the private key and chain id required by the sending exchange. Of course, the four parameters of gas, gasPrice, retry, and interval that need to be set for sending transactions are detailed in the `async send(params, [other])` description.

For the parameters passed in, you can choose to partially update. For example, if you want to use the private key A when sending a transaction to a ppos object, then you execute `ppos.updateSetting` before calling` send(params, [other]) `.({privateKey: youPrivateKeyA}) `to update the private key. Once updated, the current configuration will be overwritten. Later calls to the send transaction interface will default to the last updated configuration.

Entry parameters:

- setting Object
- provider String link
- privateKey String private key
- chainId String chain id
- gas String Maximum fuel consumption, please enter a hexadecimal string, such as '0x76c0000'
- gasPrice String Fuel price, please enter a hexadecimal string, such as '0x9184e72a000000'
- retry Number Query the number of transaction receipt objects.
- interval Number Query the interval of the transaction receipt object, the unit is ms.

No attendance.

Call example

```JavaScript
// Update privateKey, chainId at the same time
ppos.updateSetting({
    privateKey: 'acc73b693b79bbb56f89f63ccc3a0c00bf1b8380111965bfe8ab22e32045600c',
    chainId: 100,
})

// Update only privateKey
ppos.updateSetting({
    privateKey: '9f9b18c72f8e5154a9c59af2a35f73d1bdad37b049387fc6cea2bac89804293b'
})

```

------

#### `getSetting()`

Query the parameters you configured

No entry

Participate

- setting Object
- provider String link
- privateKey String private key
- chainId String chain id
- gas String maximum fuel consumption
- gasPrice String fuel price
- retry Number Query the number of transaction receipt objects.
- interval Number Query the interval of the transaction receipt object, the unit is ms.

Call example

```JavaScript
let setting = ppos.getSetting();

```

------

#### `async rpc(method, [params])`

Initiate an rpc request. An auxiliary function, because in the process of calling ppos to send a transaction, some parameters need to be obtained through rpc, so an rpc is intentionally encapsulated for calling. Note that this interface is async function, you need to add await to return the result of the call, otherwise it returns a Promise object.

Entry parameters:

- method String method name
- params Array The parameters required to call the rpc interface. If no parameters are required to call this rpc port, this parameter can be omitted.

Participate

- reply rpc call return result

Call example

```JavaScript
// Get the program version
let reply = await ppos.rpc('admin_getProgramVersion');

// Get all accounts
let reply = await ppos.rpc('platon_accounts')

// Get the amount of an account
let reply = await ppos.rpc('platon_getBalance', ["0x714de266a0effa39fcaca1442b927e5f1053eaa3", "latest"])

```

------

#### `bigNumBuf(intStr)`

Converts a large decimal integer into a buffer object that can be accepted by RLP encoding. A helper function. Because JavaScript's positive number range can only be represented as a maximum of 2 ^ 53, in order for RLP to encode large integers, you need to convert the decimal large integer of the string into the corresponding Buffer. Note that this interface can only convert large decimal integers to Buffer temporarily. If it is a hexadecimal string, you need to convert it to a decimal string first.

Entry parameters:

- intStr String String decimal large integer.

Participate

- buffer Buffer A buffer area.

Call example

```JavaScript
let buffer = ppos.bigNumBuf('1000000000000000000000000000000000000000000');

```

------

#### `hexStrBuf(hexStr)`

Converts a hexadecimal string to a buffer object that can be accepted by RLP encoding. A helper function. In the process of sending transactions by ppos, many parameters need to be transmitted as bytes instead of strings, such as `nodeId 64bytes pledged node Id(also called candidate node Id)`. The nodeId when writing code can only be expressed as a string. It needs to be converted into a 64 bytes Buffer.

Note: If the string you pass in starts with 0x or 0X, the system will assume that you are representing a hexadecimal string without encoding the first two letters. If you really want to encode 0x or 0X, you must prefix the string with 0x. For example, if you want to encode the full string 0x31c0e0(4 bytes), you must pass in 0x0x31c0e0.

Entry parameters:

- hexStr String A hexadecimal string.

Participate

- buffer Buffer A buffer area.

Call example

```JavaScript
const nodeId = '80f1fcee54de74dbf7587450f31c31c0e057bedd4faaa2a10c179d52c900ca01f0fb255a630c49d83b39f970d175c42b12a341a37504be248d76ecf592d32bc0';
let buffer = ppos.hexStrBuf(nodeId);

```

------

#### `async call(params)`

Send a call to ppos. Not on the chain. So you need to distinguish between querying and sending transactions. The input parameters can select objects or arrays. If you choose to pass in an object, then you need to use the specified string key, but the order of the keys is not required. You can write `{a: 1, b: 'hello'}` or `{b: 'hello', a: 1}`.

If you choose to use an array as the input parameter, you must put the parameters into the array in the order of the input parameters strictly **. Note that for some string large integers and bytes that need to be passed in, please choose the interface `bigNumBuf(intStr)` and `hexStrBuf(hexStr)` provided above to convert them by themselves.

Note that this interface is async function, you need to add await to return the result of the call, otherwise it returns a Promise object.

Entry parameters:

- params Object | Array call parameters.

Participate

- The reply result of the reply Object call. Note that I have turned the returned results into Object objects.
- Code Number call return code, 0 means the call result is normal.
- Data Array | Object | String | Number ... Returns the corresponding type according to the result of the call
- ErrMsg String Call prompt message.

The interface of querying the NodeID and pledged Id of the node commissioned by the current account address is called.

| Name     | Type                    | Description                           |
| -------- | ----------------------- | ------------------------------------- |
| funcType | uint16(2bytes)          | Represents the method type code(1103) |
| addr     | common.address(20bytes) | Client's account address              |

Call example

```JavaScript
let params, reply;

// Called by passing into the object(the order is not required for the key)
params = {
    funcType: 1103,
    addr: ppos.hexStrBuf("0xe6F2ce1aaF9EBf2fE3fbA8763bABaDf25e3fb5FA")
}
reply = await ppos.call(params);

// call as an array object
params = [1103, ppos.hexStrBuf("0xe6F2ce1aaF9EBf2fE3fbA8763bABaDf25e3fb5FA")];
reply = await ppos.call(params);

```

------

#### `async send(params, [other])`

Send a ppos send to send the transaction call. On the chain. So you need to distinguish between querying and sending transactions. The input parameters can select objects or arrays. For the incoming rules, please see the `async call(params)` call above.

Since it is a transaction, it will involve some parameters needed to call the transaction, such as gas, gasPrice. After the transaction is sent, in order to confirm whether the transaction is on the chain, you need to continuously poll the result on the chain through the transaction hash. There is an interval between the number of polls retry and each poll.

For the above four parameters of gas, gasPrice, retry, interval, if the other input parameters are specified, the other specified by the other will be used. If the other input parameter is not specified, the parameter specified by `updateSetting(setting)` is used when calling the function, otherwise the default value is used.

Note that this interface is async function, you need to add await to return the result of the call, otherwise it returns a Promise object.

Entry parameters:

- params Object | Array call parameters.
- other Object
- gas String Fuel limit, default '0x76c0000'.
- gasPrice String Fuel price, default '0x9184e72a000000'.
- retry Number Query the number of transaction receipt objects. The default is 600 times.
- interval Number Query the interval of the transaction receipt object, the unit is ms. The default is 100 ms.

Participate

- reply Object called successfully! The send method returns the receipt object for the specified transaction
- status-Boolean: true for successful transactions, false if EVM rolls back the transaction
- blockHash 32 Bytes-String: The hash value of the exchange in blocks
- blockNumber-Number: the block number of the exchange
- transactionHash 32 Bytes-String: hash of the transaction
- transactionIndex-Number: the index position of the transaction in the block
- from-String: The address of the sender of the transaction
- to-String: The address of the recipient of the transaction. For transactions that create a contract, the value is null
- contractAddress-String: For the transaction that created the contract, this value is the address of the contract created, otherwise null
- cumulativeGasUsed-Number: Cumulative total gas usage of the block where the transaction was executed
- gasUsed- Number: The total amount of gas for this transaction
- logs-Array: array of log objects generated by this transaction
- errMsg String call failed! If there is no receipt after the sending transaction is returned, the error message `no hash` is returned. If there is a receipt after sending the transaction, but the receipt object is not found within the specified time, then getTransactionReceipt txHash $ {hash} interval $ {interval} ms by $ {retry} retry failed`

To call the interface of `initiating a delegate`, the input parameters are from top to bottom, and the input parameters are as follows:

| Parameter | Type           | Description                                                  |
| --------- | -------------- | ------------------------------------------------------------ |
| funcType  | uint16(2bytes) | Represents the method type code(1004)                        |
| typ       | uint16(2bytes) | Indicates whether to use the account free amount or the account's locked amount as a commission, 0: free amount; 1: locked amount |
| nodeId    | 64bytes        | NodeId of the pledged node                                   |
| amount    | big.Int(bytes) | Amount entrusted(calculated in the smallest unit, 1LAT = 10 ^ 18 von) |

Call example

```JavaScript
const nodeId = "f71e1bc638456363a66c4769284290ef3ccff03aba4a22fb60ffaed60b77f614bfd173532c3575abe254c366df6f4d6248b929cb9398aaac00cbcc959f7b2b7c";
let params, others, reply;

// Called by passing into the object(the order is not required for the key)
params = {
    funcType: 1004,
    typ: 0,
    nodeId: ppos.hexStrBuf(nodeId),
    amount: ppos.bigNumBuf("10000000000000000000000")
}
reply = await ppos.send(params);

// call as an array object
params = [1004, 0, ppos.hexStrBuf(nodeId), ppos.bigNumBuf("10000000000000000000000")];
reply = await ppos.send(params);

// I don't want the default polling
other = {
    retry: 300, // only poll 300 times
    interval: 200 // 200ms per poll
}
params = [1004, 0, ppos.hexStrBuf(nodeId), ppos.bigNumBuf("10000000000000000000000")];
reply = await ppos.send(params, other);

```

### Built-in contract entry parameters detailed description

#### Pledge

- Initiate pledge and send transaction.

| Parameter          | Type             | Description                                                  |
| ------------------ | ---------------- | ------------------------------------------------------------ |
| funcType           | uint16(2bytes)   | Represents the method type code(1000)                        |
| typ                | uint16(2bytes)   | Indicates whether to use the free amount of the account or the locked amount of the account for pledge, 0: free amount; 1: locked amount |
| benefitAddress     | 20bytes          | Profit account for accepting block rewards and pledged rewards |
| nodeId             | 64bytes          | Pledged node Id(also called candidate node Id)               |
| externalId         | string           | External Id(the length is limited, and the third-party pull node description Id) |
| nodeName           | string           | The name of the node being pledged(the length is limited, indicating the name of the node) |
| website            | string           | The third-party homepage of the node(the length is limited, indicating the homepage of the node) |
| details            | string           | Description of the node(the length is limited, it means the description of the node) |
| amount             | * big.Int(bytes) | Pledged von                                                  |
| programVersion     | uint32           | Real version of the program, governance rpc acquisition      |
| programVersionSign | 65bytes          | Real version signature of the program, governance rpc acquisition |
| blsPubKey          | 96bytes          | bls's public key                                             |
| blsProof           | 64bytes          | bls proof, obtained by pulling the proof interface           |

- Modify the pledge information and send the transaction.

| Parameter      | Type           | Description                                                  |
| -------------- | -------------- | ------------------------------------------------------------ |
| funcType       | uint16(2bytes) | Represents the method type code(1001)                        |
| benefitAddress | 20bytes        | Profit account for accepting block rewards and pledged rewards |
| nodeId         | 64bytes        | Pledged node Id(also called candidate node Id)               |
| externalId     | string         | External Id(the length is limited, and the third-party pull node description Id) |
| nodeName       | string         | The name of the node being pledged(the length is limited, indicating the name of the node) |
| website        | string         | The third-party homepage of the node(the length is limited, indicating the homepage of the node) |
| details        | string         | Description of the node(the length is limited, it means the description of the node) |

- Increase pledge and send transaction.

Entry parameters:

| Parameter | Type             | Description                                                  |
| --------- | ---------------- | ------------------------------------------------------------ |
| funcType  | uint16(2bytes)   | Represents the method type code(1002)                        |
| nodeId    | 64bytes          | Pledged node Id(also called candidate node Id)               |
| typ       | uint16(2bytes)   | Indicates whether to use the free amount of the account or the locked amount of the account for pledge, 0: free amount; 1: locked amount |
| amount    | * big.Int(bytes) | Overweight von                                               |

- Cancellation of pledge(initiate all cancellations at one time, multiple times to the account), send the transaction.

| Parameter | Type           | Description                           |
| --------- | -------------- | ------------------------------------- |
| funcType  | uint16(2bytes) | Represents the method type code(1003) |
| nodeId    | 64bytes        | NodeId of the pledged node            |

- Initiate commission and send send transaction.

| Parameter | Type             | Description                                                  |
| --------- | ---------------- | ------------------------------------------------------------ |
| funcType  | uint16(2bytes)   | Represents the method type code(1004)                        |
| typ       | uint16(2bytes)   | Indicates whether to use the account free amount or the account's locked amount as a commission, 0: free amount; 1: locked amount |
| nodeId    | 64bytes          | NodeId of the pledged node                                   |
| amount    | * big.Int(bytes) | Amount entrusted(calculated in the smallest unit, 1LAT = 10 ** 18 von) |

- Reduction / revocation of commission(all reductions are revocations), and send sends the transaction.

| Parameter       | Type             | Description                                                  |
| --------------- | ---------------- | ------------------------------------------------------------ |
| funcType        | uint16(2bytes)   | Represents the method type code(1005)                        |
| stakingBlockNum | uint64(8bytes)   | Represents the only sign of a pledge of a node               |
| nodeId          | 64bytes          | NodeId of the pledged node                                   |
| amount          | * big.Int(bytes) | The amount of the reduced holding commission(calculated in the smallest unit, 1LAT = 10 ** 18 von) |

- Query the validator queue of the current settlement cycle, call query

Entry parameters:

| Name     | Type           | Description                           |
| -------- | -------------- | ------------------------------------- |
| funcType | uint16(2bytes) | Represents the method type code(1100) |

** Uniform query result format **

| Name | Type   | Description                                                  |
| ---- | ------ | ------------------------------------------------------------ |
| Code | uint32 | Represents the error code returned by the built-in ppos contract |

| Data | string | The query result of json string. For the specific format, please refer to the following query-related interface return value.
| ErrMsg | string | Error Message |

> Note: If there is no special declaration for the following query interface(the interface called by platon_call), the return parameters are returned in the above format.

Return: List

| Name   | Type    | Description                                    |
| ------ | ------- | ---------------------------------------------- |
| NodeId | 64bytes | Pledged Node Id(also called Candidate Node Id) |
| StakingAddress | 20bytes | The account used when initiating the pledge(the subsequent operation of the pledge information can only use this account, when the pledge is cancelled, von will be returned to the account or the account lock information)|
| BenefitAddress | 20bytes | Profit account for accepting block rewards and pledged rewards |
| StakingTxIndex | uint32(4bytes) | Transaction index when pledge is initiated |
| ProgramVersion | uint32 | The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance) |
| StakingBlockNum | uint64(8bytes) | Block height when initiating a pledge |
| Shares | * big.Int(bytes) | Total pledge of current candidate plus number of entrusted von |
| ExternalId | string | External Id(Id with a length limitation, the node description is pulled to a third party) |
| NodeName | string | The name of the node being pledged(with a length limit, indicating the name of the node) |
| Website | string | The third-party homepage of the node(the length of the node is the homepage of the node) |
| Details | string | Description of the node(with a length limit, indicating the description of the node) |
| ValidatorTerm | uint32(4bytes) | Term of Validator(1 in settlement cycle)

- Query current single commission information, call query

Entry parameters:

| Name            | Type           | Description                           |
| --------------- | -------------- | ------------------------------------- |
| funcType        | uint16         | Represents the method type code(1104) |
| stakingBlockNum | uint64(8bytes) | Block height when initiating a pledge |
| delAddr         | 20bytes        | Client Account Address                |
| nodeId          | 64bytes        | Verifier's Node Id                    |

Return: List

| Name               | Type                         | Description                                                  |
| ------------------ | ---------------------------- | ------------------------------------------------------------ |
| Addr               | 20bytes                      | Client's account address                                     |
| NodeId             | 64bytes                      | Verifier's Node Id                                           |
| StakingBlockNum    | uint64(8bytes)               | Block height when initiating a pledge                        |
| DelegateEpoch      | uint32(4bytes)               | The settlement cycle when the last commission for this candidate was initiated |
| Released           | string(0xhexadecimal string) | Initiate a free amount of lock-in period commissioned by the commissioning account |
| ReleasedHes        | string(0xhexadecimal string) | Initiate the hesitation period of the free amount of the commission account von |
| RestrictingPlan    | string(0xhexadecimal string) | Initiate the von commissioned by the lock period of the locked account amount of the commission account |
| RestrictingPlanHes | string(0xhexadecimal string) | Initiated hesitant period commissioned by the hedging amount of the commissioned account |
| Reduction          | string(0x hex string)        | von in revocation plan                                       |

- Query the pledge information of the current node, call query

Entry parameters:

| Name     | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| funcType | uint16  | Represents the method type code(1105) |
| nodeId   | 64bytes | Verifier's Node Id                    |

Return: List

| Name   | Type    | Description                                    |
| ------ | ------- | ---------------------------------------------- |
| NodeId | 64bytes | Pledged Node Id(also called Candidate Node Id) |

| StakingAddress | 20bytes | The account used when initiating the pledge(the subsequent operation of the pledge information can only use this account, when the pledge is cancelled, von will be returned to the account or the account lock information)
| BenefitAddress | 20bytes | Profit account for accepting block rewards and pledged rewards |
| StakingTxIndex | uint32(4bytes) | Transaction index when pledge is initiated |
| ProgramVersion | uint32(4bytes) | The real version number of the PlatON process of the pledged node(the interface for obtaining the version number is provided by the governance) |
| Status | uint32(4bytes) | The status of the candidate(the status is placed according to the 32 bit of uint32, there can be multiple states at the same time, the value is the sum of multiple simultaneous state values [0: node available(32 bits(All are 0); 1: the node is unavailable(only the last bit is 1); 2: the node's block generation rate is low but the removal condition is not met(only the penultimate bit is 1); 4: the node's von is not the lowest Pledge threshold(only the penultimate bit is 1); 8: the node is reported with double signing(only the penultimate bit is 1); 16: the node's block generation rate is low and the removal condition is met(the penultimate bit is 1 ); 32: The node actively initiates the revocation(only the penultimate bit is 1)] |
| StakingEpoch | uint32(4bytes) | Current settlement cycle when changing pledge amount |
| StakingBlockNum | uint64(8bytes) | Block height when initiating a pledge |
| Shares | string(0xhexadecimal string) | Total pledge of current candidate plus number of vons entrusted |
| Released | string(0xhexadecimal string) | Initiate a free amount locked period pledged von |
| ReleasedHes | string(0xhexadecimal string) | Initiate a free amount of hesitation period pledged von |
| RestrictingPlan | string(0xhexadecimal string) | Initiate the lock-up period of the locked amount of the pledged account von |
| RestrictingPlanHes | string(0xhexadecimal string) | Initiating the hedging period of the locked amount of the pledge account
| ExternalId | string | External Id(Id with a length limitation, the node description is pulled to a third party) |
| NodeName | string | The name of the node being pledged(with a length limit, indicating the name of the node) |
| Website | string | The third-party homepage of the node(the length of the node is the homepage of the node) |
| Details | string | Description of the node(with a length limit, indicating the description of the node) |



#### Governance

- Submit text proposal and send send transaction.

| Parameter | Type                     | Description                              |
| --------- | ------------------------ | ---------------------------------------- |
| funcType  | uint16(2bytes)           | Represents the method type code(2000)    |
| verifier  | discover.NodeID(64bytes) | The validator who submitted the proposal |
| pIDID     | string(uint64)           | PIPID                                    |

- Submit an upgrade proposal and send the transaction.

| Parameter       | Type                     | Description                                                  |
| --------------- | ------------------------ | ------------------------------------------------------------ |
| funcType        | uint16(2bytes)           | Represents the method type code(2001)                        |
| verifier        | discover.NodeID(64bytes) | The validator who submitted the proposal                     |
| pIDID           | string(uint64)           | PIPID                                                        |
| newVersion      | uint32(4bytes)           | Upgrade version                                              |
| endVotingRounds | uint64                   | Number of voting consensus rounds. Explanation: Suppose that the transaction that submitted the proposal is round1 when the consensus round number is packed into the block, the proposal voting deadline block is high, which is round1 + endVotingRounds. Unveiled 20 blocks in advance, 250, 20 are configurable), where 0 <endVotingRounds <= 4840(about 2 weeks, actual discussion can be calculated based on configuration), and is an integer) |

- Submit cancellation proposal and send send transaction.

| Parameter              | Type                     | Description                                                  |
| ---------------------- | ------------------------ | ------------------------------------------------------------ |
| funcType               | uint16(2bytes)           | Represents the method type code(2005)                        |
| verifier               | discover.NodeID(64bytes) | The validator who submitted the proposal                     |
| pIDID                  | string(uint64)           | PIPID                                                        |
| endVotingRounds        | uint64                   | Number of voting consensus rounds. Refer to the description of submitting an upgrade proposal. At the same time, the value of this parameter in this interface cannot be greater than the value in the corresponding upgrade proposal |
| tobeCanceledProposalID | common.hash(32bytes)     | Upgrade proposal ID to be canceled                           |

- Vote on the proposal and send the transaction.

| Parameter      | Type                       | Description                                                  |
| -------------- | -------------------------- | ------------------------------------------------------------ |
| funcType       | uint16(2bytes)             | Represents the method type code(2003)                        |
| verifier       | discover.NodeID(64bytes)   | Vote validator                                               |
| proposalID     | common.Hash(32bytes)       | Proposal ID                                                  |
| option         | uint8(1byte)               | Voting options                                               |
| programVersion | uint32(4bytes)             | Node code version, get programVersion interface of rpc       |
| versionSign    | common.VesionSign(65bytes) | Code version signature, get by RPC's getProgramVersion interface |

- Version statement, send send transaction.

| Parameter      | Type                       | Description                                                  |
| -------------- | -------------------------- | ------------------------------------------------------------ |
| funcType       | uint16(2bytes)             | Represents the method type code(2004)                        |
| verifier       | discover.NodeID(64bytes)   | The declared node can only be a validator / candidate        |
| programVersion | uint32(4bytes)             | Declared version, obtained by rpc's getProgramVersion interface |
| versionSign    | common.VesionSign(65bytes) | The version signature of the statement, which can be obtained by the rpc getProgramVersion interface |

- Query proposal, call query

Entry parameters:

| Name       | Type                 | Description |
| ---------- | -------------------- | ----------- |
| funcType   | uint16(2bytes)       | 2100)       |
| proposalID | common.Hash(32bytes) | Proposal ID |

Return parameter: Proposal interface implementation object json string

- Query proposal results, call query

Entry parameters:

| Name       | Type                 | Description                           |
| ---------- | -------------------- | ------------------------------------- |
| funcType   | uint16(2bytes)       | Represents the method type code(2101) |
| proposalID | common.Hash(32bytes) | Proposal ID                           |

Return parameter: json string of TallyResult object

- Query proposal list, call query

Entry parameters:

| Name     | Type           | Description                           |
| -------- | -------------- | ------------------------------------- |
| funcType | uint16(2bytes) | Represents the method type code(2102) |

Return parameter: Proposal interface implements the json string of the object list.

- Query the valid version of the node chain, call query

Entry parameters:

| Name     | Type           | Description                           |
| -------- | -------------- | ------------------------------------- |
| funcType | uint16(2bytes) | Represents the method type code(2103) |

Return parameter: The json string of the version number, such as {65536}, indicating that the version is: 1.0.0.
When parsing, ver needs to be converted into 4 bytes. Major version: second byte; minor version: third byte, patch version, fourth byte.

- Query the cumulative voteable number of proposals, call query

Entry parameters:

| Name       | Type                 | Description                           |
| ---------- | -------------------- | ------------------------------------- |
| funcType   | uint16(2bytes)       | Represents the method type code(2105) |
| proposalID | common.Hash(32bytes) | Proposal ID                           |
| blockHash  | common.Hash(32bytes) | block hash                            |

Back to reference:
Is a [] uint16 array

| Name | Type   | Description           |
| ---- | ------ | --------------------- |
|      | uint16 | Cumulative votes      |
|      | uint16 | Votes in favour       |
|      | uint16 | Number of negatives   |
|      | uint16 | Number of abstentions |

**ProposalType proposal type definition**

| Type            | definition | description      |
| --------------- | ---------- | ---------------- |
| TextProposal    | 0x01       | Text Proposal    |
| VersionProposal | 0x02       | Upgrade Proposal |
| CancelProposal  | 0x04       | Cancel Proposal  |

**ProposalStatus definition of proposal status**

For text proposals, there are three states: 0x01,0x02, 0x03;
For the upgrade proposal, there are four states: 0x01,0x03, 0x04, 0x05, 0x06.
For cancellation proposals, there are three states: 0x01,0x02, 0x03;

| Type      | definition | description                      |
| --------- | ---------- | -------------------------------- |
| Voting    | 0x01       | Voting                           |
| Pass      | 0x02       | Vote for                         |
| Failed    | 0x03       | Vote failed                      |
| PreActive | 0x04       | (Upgrade Proposal) Pre-Effective |
| Active    | 0x05       | (upgrade proposal) takes effect  |
| Canceled  | 0x06       | (Upgrade Proposal) Cancelled     |

**VoteOption definition**

| Type        | definition | description |
| ----------- | ---------- | ----------- |
| Yeas        | 0x01       | Support     |
| Nays        | 0x02       | Oppose      |
| Abstentions | 0x03       | Abstain     |

**Proposal interface Proposal definition**

Subclass TextProposal: text proposal

Field description:

| Field          | type                   | description                                                  |
| -------------- | ---------------------- | ------------------------------------------------------------ |
| ProposalID     | common.Hash(32bytes)   | Proposal ID                                                  |
| Proposer       | common.NodeID(64bytes) | Proposer Node ID                                             |
| ProposalType   | byte                   | Proposal type, 0x01: text proposal; 0x02: upgrade proposal; 0x03 parameter proposal; 0x04 cancel proposal. |
| PIPID          | string                 | Proposal PIPID                                               |
| SubmitBlock    | 8bytes                 | Block Height for Submitting Proposal                         |
| EndVotingBlock | 8bytes                 | The block height of the proposal voting end, the system is based on SubmitBlock |

Subclass VersionProposal: upgrade proposal

Field description:

| Field           | type                   | description                                                  |
| --------------- | ---------------------- | ------------------------------------------------------------ |
| ProposalID      | common.Hash(32bytes)   | Proposal ID                                                  |
| Proposer        | common.NodeID(64bytes) | Proposer Node ID                                             |
| ProposalType    | byte                   | Proposal type, 0x01: text proposal; 0x02: upgrade proposal; 0x03 parameter proposal; 0x04 cancel proposal. |
| PIPID           | string                 | Proposal PIPID                                               |
| SubmitBlock     | 8bytes                 | Block Height for Submitting Proposal                         |
| EndVotingRounds | 8bytes                 | Number of Voting Consensus Cycles Consistent                 |
| EndVotingBlock  | 8bytes                 | The block height at the end of proposal voting is calculated by the system according to SubmitBlock, EndVotingRounds |
| ActiveBlock     | 8bytes                 | The effective block height of the proposal is calculated by the system based on EndVotingBlock |
| NewVersion      | uint                   | Upgraded version                                             |

Subclass CancelProposal: Cancel proposal

Field description:

| Field           | type                   | description                                                  |
| --------------- | ---------------------- | ------------------------------------------------------------ |
| ProposalID      | common.Hash(32bytes)   | Proposal ID                                                  |
| Proposer        | common.NodeID(64bytes) | Proposer Node ID                                             |
| ProposalType    | byte                   | Proposal type, 0x01: text proposal; 0x02: upgrade proposal; 0x03 parameter proposal; 0x04 cancel proposal. |
| PIPID           | string                 | Proposal PIPID                                               |
| SubmitBlock     | 8bytes                 | Block Height for Submitting Proposal                         |
| EndVotingRounds | 8bytes                 | Number of Voting Consensus Cycles Consistent                 |
| EndVotingBlock  | 8bytes                 | The block height at the end of proposal voting is calculated by the system according to SubmitBlock, EndVotingRounds |
| TobeCanceled    | common.Hash(32bytes)   | Upgrade Proposal ID for proposal to cancel                   |

**Vote Voting Definition**

| Field      | type                 | description    |
| ---------- | -------------------- | -------------- |
| voter      | 64bytes              | Vote validator |
| proposalID | common.Hash(32bytes) | Proposal ID    |
| option     | VoteOption           | Voting Options |

**TallyResult voting result definition**

| Field         | type                 | description                                                  |
| ------------- | -------------------- | ------------------------------------------------------------ |
| proposalID    | common.Hash(32bytes) | Proposal ID                                                  |
| yeas          | uint16(2bytes)       | Yes                                                          |
| nays          | uint16(2bytes)       | No                                                           |
| abstentions   | uint16(2bytes)       | Abstain                                                      |
| accuVerifiers | uint16(2bytes)       | Total number of validators eligible to vote throughout the voting period |
| status        | byte                 | status                                                       |
| canceledBy    | common.Hash(32bytes) | When status = 0x06, record the ProposalID that initiated the cancellation |

#### Report punishment

- Report double sign, send send transaction.

Parameters | Type | Description |
| -------- | ------ | --------------------------------------- |
|funcType | uint16(2bytes) | represents the method type code(3000) |
|typ | uint8 | represents the double sign type:<br/> 1:prepareBlock, 2:prepareVote, 3:viewChange|
|data | string | json value of single evidence, format refer to [RPC interface Evidences] [evidences_interface] |

- Query whether the node has been reported to be oversigned, call query

Entry parameters:

Parameters | Type | Description |
| ----------- | -------------- | ---------------------- |
|funcType | uint16(2bytes) | represents the method type code(3001) |
typ | uint32 | represents the double sign type, <br /> 1: prepareBlock, 2: prepareVote, 3: viewChange |
addr | 20bytes | Reporting Node Address |
blockNumber | uint64 | Multi-Signed Block Height |

Return parameters:

| Type | Description |
| ------ | -------------- |
Hex | Reported Transaction Hash |



#### Locked

- Create a hedging plan and send the transaction.

Entry parameters:

Parameter | Type | Description |
| ------- | -------------- | --------------------------|
account | 20bytes | `Unlock the account to the account` |
plan | [] RestrictingPlan | plan is a list(array) of type RestrictingPlan. RestrictingPlan is defined as follows: <br> type RestrictingPlan struct {<br/> Epoch uint64 <br/> Amount: \ * big.Int <br/>} <br/> Among them, Epoch: represents a multiple of the settlement cycle. The product of the number of blocks produced per settlement cycle indicates the release of locked funds at the height of the target block. Epoch \ * The number of blocks per cycle must be at least greater than the highest irreversible block height. <br> Amount: indicates the amount to be released on the target block. |

- Get lock position information, call query

Note: This interface supports obtaining historical data. Block height can be attached to the request. By default, the latest block data is queried.

Entry parameters:

Parameter | Type | Description |
| ------- | ------- | ------------------ |
account | 20bytes | `Unlock the account to the account` |

Back to reference:

Returns a json format string with the following fields

| Name | Type | Description |
| ------- | --------------- | ------------------------- |
balance | string(0xhexadecimal string) | total locked balance-released amount |
| pledge | string(0x hex string) | Pledge / mortgage amount |
| debt | string(0x hex string) | Amount due for release |
plans | bytes | lock entry information, json array: [{"blockNumber": "", "amount": ""}, ..., {"blockNumber": "", "amount": ""} ]. Where: <br blockNumber: \*big.Int, release block height<br/> amount: \ string(0x hex string), release amount |

### Built-in contract error code description

| Error Code | Explanation                                          |
| ---------- | ---------------------------------------------------- |
| 301000     | Wrong bls public key                                 |
| 301001     | Wrong bls public key proof                           |
| 301002     | The Description length is wrong                      |
| 301003     | The program version sign is wrong                    |
| 301004     | The program version of the relates node's is too low |
| 301005     | DeclareVersion is failed on create staking           |
| 301006     | The address must be the same as initiated staking    |
| 301100     | Staking deposit too low                              |
| 301101     | This candidate is already exist                      |
| 301102     | This candidate is not exist                          |
| 301103     | This candidate status was invalided                  |
| 301104     | IncreaseStake von is too low                         |
| 301105     | Delegate deposit too low                             |
| 301106     | The account is not allowed to be used for delegating |
| 301107     | The candidate does not accept the delegation         |
| 301108     | Withdrew delegation von is too low                   |
| 301109     | This delegation is not exist                         |
| 301110     | The von operation type is wrong                      |
| 301111     | The von of account is not enough                     |
| 301112     | The blockNumber is disordered                        |
| 301113     | The von of delegation is not enough                  |
| 301114     | Withdrew delegation von calculation is wrong         |
| 301115     | The validator is not exist                           |
| 301116     | The fn params is wrong                               |
| 301117     | The slashing type is wrong                           |
| 301118     | Slashing amount is overflow                          |
| 301119     | Slashing candidate von calculate is wrong            |
| 301200     | Getting verifierList is failed                       |
| 301201     | Getting validatorList is failed                      |
| 301202     | Getting candidateList is failed                      |
| 301203     | Getting related of delegate is failed                |
| 301204     | Query candidate info failed                          |
| 301205     | Query delegate info failed                           |

### Other

You can configure the template according to the config.default.js file under the test directory and save the config.js file in the same directory. Then execute `npm run ppos` to execute the unit test. For more invocation examples, please refer to the unit test ppos.js file written under the test directory.
