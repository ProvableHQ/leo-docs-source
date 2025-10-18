---
id: aleo_network_client
title: Aleo Network Client
sidebar_label: Aleo Network Client
---

<a name="AleoNetworkClient"></a>

## Overview
<p>Connection management class that encapsulates REST calls to publicly exposed endpoints of Aleo nodes.
The methods provided in this class provide information on the Aleo Blockchain</p>

**Kind**: global class  

* [AleoNetworkClient](#AleoNetworkClient)
    * [new AleoNetworkClient(host)](#new_AleoNetworkClient_new)
    * [.setAccount(host)](#AleoNetworkClient+setHost)
    * [.setAccount(account)](#AleoNetworkClient+setAccount)
    * [.getAccount()](#AleoNetworkClient+getAccount)
    * [.getBlock(height)](#AleoNetworkClient+getBlock)
    * [.getBlockRange(start, end)](#AleoNetworkClient+getBlockRange)
    * [.getProgram(programId)](#AleoNetworkClient+getProgram)
    * [.getProgramObject(programId)](#AleoNetworkClient+getProgramObject)
    * [.getProgramImports(programId)](#AleoNetworkClient+getProgramImports)
    * [.getDeploymentTransactionIDForProgram(programId)](#AleoNetworkClient+getDeploymentTransactionIDForProgram)
    * [.getDeploymentTransactionForProgram(programId)](#AleoNetworkClient+getDeploymentTransactionForProgram)
    * [.getProgramMappingNames(programId)](#AleoNetworkClient+getProgramMappingNames)
    * [.getProgramMappingValue(programId, mappingName, key)](#AleoNetworkClient+getProgramMappingValue)
    * [.getLatestBlock()](#AleoNetworkClient+getLatestBlock)
    * [.getLatestHeight()](#AleoNetworkClient+getLatestHeight)
    * [.getStateRoot()](#AleoNetworkClient+getStateRoot)
    * [.getTransaction(id)](#AleoNetworkClient+getTransaction)
    * [.getTransactions(height)](#AleoNetworkClient+getTransactions)
    * [.getTransactionsInMempool()](#AleoNetworkClient+getTransactionsInMempool)
    * [.getTransitionId()](#AleoNetworkClient+getTransitionId)
    * [.findUnspentRecords()](#AleoNetworkClient+findUnspentRecords)

<a name="new_AleoNetworkClient_new"></a>

### new AleoNetworkClient(host)

| Param | Type |
| --- | --- |
| host | <code>string</code> | 

**Example**  
```js
// Connection to a local node
let local_connection = new AleoNetworkClient("http://localhost:3030");

// Connection to a public beacon node
let public_connection = new AleoNetworkClient("https://api.explorer.aleo.org/v1");
```
<a name="AleoNetworkClient+setHost"></a>

### aleoNetworkClient.setHost(host)
<p>Set a new host for the networkClient</p>

| Param | Type |
| --- | --- |
| host | <code>string</code> | 

**Example**  
```js
// New connection to a public beacon node
let public_connection = AleoNetworkClient.setHost("https://api.explorer.aleo.org/v1");
```
<a name="AleoNetworkClient+setAccount"></a>

### aleoNetworkClient.setAccount(account)
<p>Set an account</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| account | <code>Account</code> | 

**Example**  
```js
let account = new Account();
connection.setAccount(account);
```
<a name="AleoNetworkClient+getAccount"></a>

### aleoNetworkClient.getAccount()
<p>Return the Aleo account used in the node connection</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let account = connection.getAccount();
```
<a name="AleoNetworkClient+getBlock"></a>

### aleoNetworkClient.getBlock(height)
<p>Returns the block contents of the block at the specified block height</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| height | <code>number</code> | 

**Example**  
```js
let block = connection.getBlock(1234);
```
<a name="AleoNetworkClient+getBlockRange"></a>

### aleoNetworkClient.getBlockRange(start, end)
<p>Returns a range of blocks between the specified block heights</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| start | <code>number</code> | 
| end | <code>number</code> | 

**Example**  
```js
let blockRange = connection.getBlockRange(2050, 2100);
```
<a name="AleoNetworkClient+getProgram"></a>

### aleoNetworkClient.getProgram(programId)
<p>Returns the source code of a program</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let program = connection.getProgram("foo.aleo");
```

<a name="AleoNetworkClient+getProgramObject"></a>

### aleoNetworkClient.getProgramObject(programId)
<p>Returns a program object from a program ID or program source code</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let program = connection.getProgramObject("foo.aleo");
```

<a name="AleoNetworkClient+getProgramImports"></a>

### aleoNetworkClient.getProgramImports(programId)
<p>Returns an object containing the source code of a program and the source code of all programs it imports</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let program = connection.getProgramImports("foo.aleo");
```

<a name="AleoNetworkClient+getDeploymentTransactionIDForProgram"></a>

### aleoNetworkClient.getDeploymentTransactionIDForProgram(programId)
<p>Returns the deployment transaction id associated with the specified program</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let program = connection.getDeploymentTransactionIDForProgram("foo.aleo");
```

<a name="AleoNetworkClient+getDeploymentTransactionForProgram"></a>

### aleoNetworkClient.getDeploymentTransactionForProgram(programId)
<p>Returns the deployment transaction associated with a specified program</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let program = connection.getDeploymentTransactionForProgram("foo.aleo");
```

<a name="AleoNetworkClient+getProgramMappingNames"></a>

### aleoNetworkClient.getProgramMappingNames(programId)
<p>Returns the names of the mappings of a program</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let mappings = connection.getProgramMappingNames("credits.aleo");
```

<a name="AleoNetworkClient+getProgramImportNames"></a>

### aleoNetworkClient.getProgramImportNames(programId)
<p>Get a list of the program names that a program imports</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 

**Example**  
```js
let mappings = connection.getProgramImportNames("foo.aleo");
```

<a name="AleoNetworkClient+getProgramMappingValue"></a>

### aleoNetworkClient.getProgramMappingValue(programId, mappingName, key)
<p>Returns the value of a program's mapping for a specific key</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| programId | <code>string</code> | 
| mappingName | <code>string</code> | 
| key | <code>string</code> | 

**Example**  
```js
## Get public balance of an account
let mappingValue = connection.getProgramMappingValue("credits.aleo", "account", "aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px");
```
<a name="AleoNetworkClient+getLatestBlock"></a>

### aleoNetworkClient.getLatestBlock()
<p>Returns the block contents of the latest block</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let latestHeight = connection.getLatestBlock();
```

<a name="AleoNetworkClient+getLatestHeight"></a>

### aleoNetworkClient.getLatestHeight()
<p>Returns the latest block height</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let latestHeight = connection.getLatestHeight();
```

<a name="AleoNetworkClient+getLatestCommittee"></a>

### aleoNetworkClient.getLatestCommittee()
<p>Returns the latest committee</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let latestCommittee = connection.getLatestCommittee();
```
<a name="AleoNetworkClient+getStateRoot"></a>

### aleoNetworkClient.getStateRoot()
<p>Returns the latest state/merkle root of the Aleo blockchain</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let stateRoot = connection.getStateRoot();
```
<a name="AleoNetworkClient+getTransaction"></a>

### aleoNetworkClient.getTransaction(id)
<p>Returns a transaction by its unique identifier</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| id | <code>string</code> | 

**Example**  
```js
let transaction = connection.getTransaction("at1handz9xjrqeynjrr0xay4pcsgtnczdksz3e584vfsgaz0dh0lyxq43a4wj");
```

<a name="AleoNetworkClient+getTransactions"></a>

### aleoNetworkClient.getTransactions(height)
<p>Returns the transactions present at the specified block height</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  

| Param | Type |
| --- | --- |
| height | <code>number</code> | 

**Example**  
```js
let transactions = connection.getTransactions(654);
```
<a name="AleoNetworkClient+getTransactionsInMempool"></a>

### aleoNetworkClient.getTransactionsInMempool()
<p>Returns the transactions in the memory pool.</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let transactions = connection.getTransactionsInMempool();
```
<a name="AleoNetworkClient+getTransitionId"></a>

### aleoNetworkClient.getTransitionId()
<p>Returns the transition id by its unique identifier</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
let transition = connection.getTransitionId("2429232855236830926144356377868449890830704336664550203176918782554219952323field");
```
<a name="AleoNetworkClient+findUnspentRecords"></a>

### aleoNetworkClient.findUnspentRecords()
<p>Attempts to find unspent records in the Aleo blockchain for a specified private key</p>

**Kind**: instance method of [<code>AleoNetworkClient</code>](#AleoNetworkClient)  
**Example**  
```js
// Find all unspent records
const privateKey = "[PRIVATE_KEY]";
let records = connection.findUnspentRecords(0, undefined, privateKey);

// Find specific amounts
const startHeight = 500000;
const amounts = [600000, 1000000];
let records = connection.findUnspentRecords(startHeight, undefined, privateKey, amounts);

// Find specific amounts with a maximum number of cumulative microcredits
const maxMicrocredits = 100000;
let records = connection.findUnspentRecords(startHeight, undefined, privateKey, undefined, maxMicrocredits);
```
