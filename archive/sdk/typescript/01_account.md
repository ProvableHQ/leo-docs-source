---
id: account
title: Account
sidebar_label: Account
---

<a name="Account"></a>

## Overview
<p>Key Management class. Enables the creation of a new Aleo Account, importation of an existing account from
an existing private key or seed, and message signing and verification functionality.</p>
<p>An Aleo Account is generated from a randomly generated seed (number) from which an account private key, view key,
and a public account address are derived. The private key lies at the root of an Aleo account. It is a highly
sensitive secret and should be protected as it allows for creation of Aleo Program executions and arbitrary value
transfers. The View Key allows for decryption of a user's activity on the blockchain. The Address is the public
address to which other users of Aleo can send Aleo credits and other records to. This class should only be used
in environments where the safety of the underlying key material can be assured.</p>

**Kind**: global class  

* [Account](#Account)
    * _instance_
        * [.encryptAccount(ciphertext)](#Account+encryptAccount) ⇒ <code>PrivateKeyCiphertext</code>
        * [.decryptRecord(ciphertext)](#Account+decryptRecord) ⇒ <code>Record</code>
        * [.decryptRecords(ciphertexts)](#Account+decryptRecords) ⇒ <code>Array.&lt;Record&gt;</code>
        * [.ownsRecordCiphertext(ciphertext)](#Account+ownsRecordCiphertext) ⇒ <code>boolean</code>
        * [.sign(message)](#Account+sign) ⇒ <code>Signature</code>
        * [.verify(message, signature)](#Account+verify) ⇒ <code>boolean</code>
    * _static_
        * [.fromCiphertext(ciphertext, password)](#Account.fromCiphertext) ⇒ <code>PrivateKey</code> \| <code>Error</code>

<a name="Account+encryptAccount"></a>

### account.encryptAccount(ciphertext) ⇒ <code>PrivateKeyCiphertext</code>
<p>Encrypt the account's private key with a password</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| ciphertext | <code>string</code> | 

**Example**  
```js
let account = new Account();
let ciphertext = account.encryptAccount("password");
```
<a name="Account+decryptRecord"></a>

### account.decryptRecord(ciphertext) ⇒ <code>Record</code>
<p>Decrypts a Record in ciphertext form into plaintext</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| ciphertext | <code>string</code> | 

**Example**  
```js
let account = new Account();
let record = account.decryptRecord("record1ciphertext");
```
<a name="Account+decryptRecords"></a>

### account.decryptRecords(ciphertexts) ⇒ <code>Array.&lt;Record&gt;</code>
<p>Decrypts an array of Records in ciphertext form into plaintext</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| ciphertexts | <code>Array.&lt;string&gt;</code> | 

**Example**  
```js
let account = new Account();
let record = account.decryptRecords(["record1ciphertext", "record2ciphertext"]);
```
<a name="Account+ownsRecordCiphertext"></a>

### account.ownsRecordCiphertext(ciphertext) ⇒ <code>boolean</code>
<p>Determines whether the account owns a ciphertext record</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| ciphertext | <code>RecordCipherText</code> \| <code>string</code> | 

**Example**  
```js
// Create a connection to the Aleo network and an account
let connection = new NodeConnection("vm.aleo.org/api");
let account = Account.fromCiphertext("ciphertext", "password");

// Get a record from the network
let record = connection.getBlock(1234);
let recordCipherText = record.transactions[0].execution.transitions[0].id;

// Check if the account owns the record
if account.ownsRecord(recordCipherText) {
    // Then one can do something like:
    // Decrypt the record and check if it's spent
    // Store the record in a local database
    // Etc.
}
```
<a name="Account+sign"></a>

### account.sign(message) ⇒ <code>Signature</code>
<p>Signs a message with the account's private key.
Returns a Signature.</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| message | <code>Uint8Array</code> | 

**Example**  
```js
let account = new Account();
let message = Uint8Array.from([104, 101, 108, 108, 111 119, 111, 114, 108, 100])
account.sign(message);
```
<a name="Account+verify"></a>

### account.verify(message, signature) ⇒ <code>boolean</code>
<p>Verifies the Signature on a message.</p>

**Kind**: instance method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| message | <code>Uint8Array</code> | 
| signature | <code>Signature</code> | 

**Example**  
```js
let account = new Account();
let message = Uint8Array.from([104, 101, 108, 108, 111 119, 111, 114, 108, 100])
let signature = account.sign(message);
account.verify(message, signature);
```
<a name="Account.fromCiphertext"></a>

### Account.fromCiphertext(ciphertext, password) ⇒ <code>PrivateKey</code> \| <code>Error</code>
<p>Attempts to create an account from a private key ciphertext</p>

**Kind**: static method of [<code>Account</code>](#Account)  

| Param | Type |
| --- | --- |
| ciphertext | <code>PrivateKeyCiphertext</code> \| <code>string</code> | 
| password | <code>string</code> | 

**Example**  
```js
let ciphertext = PrivateKey.newEncrypted("password");
let account = Account.fromCiphertext(ciphertext, "password");
```
