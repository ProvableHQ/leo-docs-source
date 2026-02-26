---
id: nodejs
title: NodeJS + Browser (Single-Thread)
sidebar_label: NodeJS + Browser
---


## Classes

<dl>
<dt><a href="#Address">Address</a></dt>
<dd></dd>
<dt><a href="#ExecutionResponse">ExecutionResponse</a></dt>
<dd><p>Webassembly Representation of an Aleo function execution response</p>
<p>This object is returned by the execution of an Aleo function off-chain. It provides methods for
retrieving the outputs of the function execution.</p>
</dd>
<dt><a href="#KeyPair">KeyPair</a></dt>
<dd></dd>
<dt><a href="#PrivateKey">PrivateKey</a></dt>
<dd></dd>
<dt><a href="#PrivateKeyCiphertext">PrivateKeyCiphertext</a></dt>
<dd><p>Private Key in ciphertext form</p>
</dd>
<dt><a href="#Program">Program</a></dt>
<dd><p>Webassembly Representation of an Aleo program</p>
<p>This object is required to create an Execution or Deployment transaction. It includes several
convenience methods for enumerating available functions and each functions&#39; inputs in a
javascript object for usage in creation of web forms for input capture.</p>
</dd>
<dt><a href="#ProvingKey">ProvingKey</a></dt>
<dd></dd>
<dt><a href="#RecordCiphertext">RecordCiphertext</a></dt>
<dd><p>Encrypted Aleo record</p>
</dd>
<dt><a href="#RecordPlaintext">RecordPlaintext</a></dt>
<dd><p>Aleo record plaintext</p>
</dd>
<dt><a href="#Signature">Signature</a></dt>
<dd></dd>
<dt><a href="#Transaction">Transaction</a></dt>
<dd><p>Webassembly Representation of an Aleo transaction</p>
<p>This object is created when generating an on-chain function deployment or execution and is the
object that should be submitted to the Aleo Network in order to deploy or execute a function.</p>
</dd>
<dt><a href="#VerifyingKey">VerifyingKey</a></dt>
<dd></dd>
<dt><a href="#ViewKey">ViewKey</a></dt>
<dd></dd>
</dl>

<a name="Address"></a>

## Address
**Kind**: global class

* [Address](#Address)
    * _instance_
        * [.to_string()](#Address+to_string) ⇒ <code>string</code>
        * [.verify(message, signature)](#Address+verify) ⇒ <code>boolean</code>
    * _static_
        * [.from_private_key(private_key)](#Address.from_private_key) ⇒ [<code>Address</code>](#Address)
        * [.from_view_key(view_key)](#Address.from_view_key) ⇒ [<code>Address</code>](#Address)
        * [.from_string(address)](#Address.from_string) ⇒ [<code>Address</code>](#Address)

<a name="Address+to_string"></a>

### address.to\_string() ⇒ <code>string</code>
**Kind**: instance method of [<code>Address</code>](#Address)
<a name="Address+verify"></a>

### address.verify(message, signature) ⇒ <code>boolean</code>
**Kind**: instance method of [<code>Address</code>](#Address)

| Param | Type |
| --- | --- |
| message | <code>Uint8Array</code> |
| signature | [<code>Signature</code>](#Signature) |

<a name="Address.from_private_key"></a>

### Address.from\_private\_key(private_key) ⇒ [<code>Address</code>](#Address)
**Kind**: static method of [<code>Address</code>](#Address)

| Param | Type |
| --- | --- |
| private_key | [<code>PrivateKey</code>](#PrivateKey) |

<a name="Address.from_view_key"></a>

### Address.from\_view\_key(view_key) ⇒ [<code>Address</code>](#Address)
**Kind**: static method of [<code>Address</code>](#Address)

| Param | Type |
| --- | --- |
| view_key | [<code>ViewKey</code>](#ViewKey) |

<a name="Address.from_string"></a>

### Address.from\_string(address) ⇒ [<code>Address</code>](#Address)
**Kind**: static method of [<code>Address</code>](#Address)

| Param | Type |
| --- | --- |
| address | <code>string</code> |

<a name="ExecutionResponse"></a>

## ExecutionResponse
Webassembly Representation of an Aleo function execution response

This object is returned by the execution of an Aleo function off-chain. It provides methods for
retrieving the outputs of the function execution.

**Kind**: global class
<a name="ExecutionResponse+getOutputs"></a>

### executionResponse.getOutputs() ⇒ <code>Array.&lt;any&gt;</code>
Get the outputs of the executed function

**Kind**: instance method of [<code>ExecutionResponse</code>](#ExecutionResponse)
<a name="KeyPair"></a>

## KeyPair
**Kind**: global class

* [KeyPair](#KeyPair)
    * [new KeyPair(proving_key, verifying_key)](#new_KeyPair_new)
    * [.provingKey()](#KeyPair+provingKey) ⇒ [<code>ProvingKey</code>](#ProvingKey)
    * [.verifyingKey()](#KeyPair+verifyingKey) ⇒ [<code>VerifyingKey</code>](#VerifyingKey)

<a name="new_KeyPair_new"></a>

### new KeyPair(proving_key, verifying_key)
Create new key pair from proving and verifying keys


| Param | Type |
| --- | --- |
| proving_key | [<code>ProvingKey</code>](#ProvingKey) |
| verifying_key | [<code>VerifyingKey</code>](#VerifyingKey) |

<a name="KeyPair+provingKey"></a>

### keyPair.provingKey() ⇒ [<code>ProvingKey</code>](#ProvingKey)
Get the proving key

**Kind**: instance method of [<code>KeyPair</code>](#KeyPair)
<a name="KeyPair+verifyingKey"></a>

### keyPair.verifyingKey() ⇒ [<code>VerifyingKey</code>](#VerifyingKey)
Get the verifying key

**Kind**: instance method of [<code>KeyPair</code>](#KeyPair)
<a name="PrivateKey"></a>

## PrivateKey
**Kind**: global class

* [PrivateKey](#PrivateKey)
    * [new PrivateKey()](#new_PrivateKey_new)
    * _instance_
        * [.to_string()](#PrivateKey+to_string) ⇒ <code>string</code>
        * [.to_view_key()](#PrivateKey+to_view_key) ⇒ [<code>ViewKey</code>](#ViewKey)
        * [.to_address()](#PrivateKey+to_address) ⇒ [<code>Address</code>](#Address)
        * [.sign(message)](#PrivateKey+sign) ⇒ [<code>Signature</code>](#Signature)
        * [.toCiphertext(secret)](#PrivateKey+toCiphertext) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
    * _static_
        * [.from_seed_unchecked(seed)](#PrivateKey.from_seed_unchecked) ⇒ [<code>PrivateKey</code>](#PrivateKey)
        * [.from_string(private_key)](#PrivateKey.from_string) ⇒ [<code>PrivateKey</code>](#PrivateKey)
        * [.newEncrypted(secret)](#PrivateKey.newEncrypted) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
        * [.fromPrivateKeyCiphertext(ciphertext, secret)](#PrivateKey.fromPrivateKeyCiphertext) ⇒ [<code>PrivateKey</code>](#PrivateKey)

<a name="new_PrivateKey_new"></a>

### new PrivateKey()
Generate a new private key

<a name="PrivateKey+to_string"></a>

### privateKey.to\_string() ⇒ <code>string</code>
Get a string representation of the private key

This function should be used very carefully as it exposes the private key plaintext

**Kind**: instance method of [<code>PrivateKey</code>](#PrivateKey)
<a name="PrivateKey+to_view_key"></a>

### privateKey.to\_view\_key() ⇒ [<code>ViewKey</code>](#ViewKey)
Get the view key corresponding to the private key

**Kind**: instance method of [<code>PrivateKey</code>](#PrivateKey)
<a name="PrivateKey+to_address"></a>

### privateKey.to\_address() ⇒ [<code>Address</code>](#Address)
Get the address corresponding to the private key

**Kind**: instance method of [<code>PrivateKey</code>](#PrivateKey)
<a name="PrivateKey+sign"></a>

### privateKey.sign(message) ⇒ [<code>Signature</code>](#Signature)
Sign a message with the private key

**Kind**: instance method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| message | <code>Uint8Array</code> |

<a name="PrivateKey+toCiphertext"></a>

### privateKey.toCiphertext(secret) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
Encrypt the private key with a secret.

The secret is sensitive and will be needed to decrypt the private key later, so it should be stored securely

**Kind**: instance method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| secret | <code>string</code> |

<a name="PrivateKey.from_seed_unchecked"></a>

### PrivateKey.from\_seed\_unchecked(seed) ⇒ [<code>PrivateKey</code>](#PrivateKey)
Get a private key from a series of unchecked bytes

**Kind**: static method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| seed | <code>Uint8Array</code> |

<a name="PrivateKey.from_string"></a>

### PrivateKey.from\_string(private_key) ⇒ [<code>PrivateKey</code>](#PrivateKey)
Create a private key from a string representation

This function will fail if the text is not a valid private key

**Kind**: static method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| private_key | <code>string</code> |

<a name="PrivateKey.newEncrypted"></a>

### PrivateKey.newEncrypted(secret) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
Get a private key ciphertext using a secret.

The secret is sensitive and will be needed to decrypt the private key later, so it should be stored securely

**Kind**: static method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| secret | <code>string</code> |

<a name="PrivateKey.fromPrivateKeyCiphertext"></a>

### PrivateKey.fromPrivateKeyCiphertext(ciphertext, secret) ⇒ [<code>PrivateKey</code>](#PrivateKey)
Get private key from a private key ciphertext using a secret.

**Kind**: static method of [<code>PrivateKey</code>](#PrivateKey)

| Param | Type |
| --- | --- |
| ciphertext | [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext) |
| secret | <code>string</code> |

<a name="PrivateKeyCiphertext"></a>

## PrivateKeyCiphertext
Private Key in ciphertext form

**Kind**: global class

* [PrivateKeyCiphertext](#PrivateKeyCiphertext)
    * _instance_
        * [.decryptToPrivateKey(secret)](#PrivateKeyCiphertext+decryptToPrivateKey) ⇒ [<code>PrivateKey</code>](#PrivateKey)
        * [.toString()](#PrivateKeyCiphertext+toString) ⇒ <code>string</code>
    * _static_
        * [.encryptPrivateKey(private_key, secret)](#PrivateKeyCiphertext.encryptPrivateKey) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
        * [.fromString(ciphertext)](#PrivateKeyCiphertext.fromString) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)

<a name="PrivateKeyCiphertext+decryptToPrivateKey"></a>

### privateKeyCiphertext.decryptToPrivateKey(secret) ⇒ [<code>PrivateKey</code>](#PrivateKey)
Decrypts a private ciphertext using a secret string.

This must be the same secret used to encrypt the private key

**Kind**: instance method of [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)

| Param | Type |
| --- | --- |
| secret | <code>string</code> |

<a name="PrivateKeyCiphertext+toString"></a>

### privateKeyCiphertext.toString() ⇒ <code>string</code>
Returns the ciphertext string

**Kind**: instance method of [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
<a name="PrivateKeyCiphertext.encryptPrivateKey"></a>

### PrivateKeyCiphertext.encryptPrivateKey(private_key, secret) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
Encrypt a private key using a secret string.

The secret is sensitive and will be needed to decrypt the private key later, so it should be stored securely.

**Kind**: static method of [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)

| Param | Type |
| --- | --- |
| private_key | [<code>PrivateKey</code>](#PrivateKey) |
| secret | <code>string</code> |

<a name="PrivateKeyCiphertext.fromString"></a>

### PrivateKeyCiphertext.fromString(ciphertext) ⇒ [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)
Creates a PrivateKeyCiphertext from a string

**Kind**: static method of [<code>PrivateKeyCiphertext</code>](#PrivateKeyCiphertext)

| Param | Type |
| --- | --- |
| ciphertext | <code>string</code> |

<a name="Program"></a>

## Program
Webassembly Representation of an Aleo program

This object is required to create an Execution or Deployment transaction. It includes several
convenience methods for enumerating available functions and each functions' inputs in a
javascript object for usage in creation of web forms for input capture.

**Kind**: global class

* [Program](#Program)
    * _instance_
        * [.toString()](#Program+toString) ⇒ <code>string</code>
        * [.getFunctions()](#Program+getFunctions) ⇒ <code>Array.&lt;any&gt;</code>
        * [.getFunctionInputs(function_name)](#Program+getFunctionInputs) ⇒ <code>Array.&lt;any&gt;</code>
        * [.getMappings()](#Program+getMappings) ⇒ <code>Array</code> \| <code>Array.&lt;any&gt;</code>
        * [.getRecordMembers(record_name)](#Program+getRecordMembers) ⇒ <code>object</code>
        * [.getStructMembers(struct_name)](#Program+getStructMembers) ⇒ <code>Array.&lt;any&gt;</code>
        * [.id()](#Program+id) ⇒ <code>string</code>
        * [.isEqual(other)](#Program+isEqual) ⇒ <code>boolean</code>
        * [.getImports()](#Program+getImports) ⇒ <code>Array.&lt;any&gt;</code>
    * _static_
        * [.fromString(program)](#Program.fromString) ⇒ [<code>Program</code>](#Program)
        * [.getCreditsProgram()](#Program.getCreditsProgram) ⇒ [<code>Program</code>](#Program)

<a name="Program+toString"></a>

### program.toString() ⇒ <code>string</code>
Get a string representation of the program

**Kind**: instance method of [<code>Program</code>](#Program)
<a name="Program+getFunctions"></a>

### program.getFunctions() ⇒ <code>Array.&lt;any&gt;</code>
Get javascript array of functions names in the program

**Kind**: instance method of [<code>Program</code>](#Program)
<a name="Program+getFunctionInputs"></a>

### program.getFunctionInputs(function_name) ⇒ <code>Array.&lt;any&gt;</code>
Get a javascript object representation of the function inputs and types. This can be used
to generate a webform to capture user inputs for an execution of a function.

**Kind**: instance method of [<code>Program</code>](#Program)

| Param | Type |
| --- | --- |
| function_name | <code>string</code> |

<a name="Program+getMappings"></a>

### program.getMappings() ⇒ <code>Array</code> \| <code>Array.&lt;any&gt;</code>
Get a list of a program's mappings and the names/types of their keys and values.

**Kind**: instance method of [<code>Program</code>](#Program)
**Returns**: <code>Array</code> - - An array of objects representing the mappings in the program<code>Array.&lt;any&gt;</code>
**Example**
```js
const expected_mappings = [
   {
      name: "account",
      key_name: "owner",
      key_type: "address",
      value_name: "microcredits",
      value_type: "u64"
   }
]

const credits_program = aleo_wasm.Program.getCreditsProgram();
const credits_mappings = credits_program.getMappings();
console.log(credits_mappings === expected_mappings); // Output should be "true"
```
<a name="Program+getRecordMembers"></a>

### program.getRecordMembers(record_name) ⇒ <code>object</code>
Get a javascript object representation of a program record and its types

**Kind**: instance method of [<code>Program</code>](#Program)

| Param | Type |
| --- | --- |
| record_name | <code>string</code> |

<a name="Program+getStructMembers"></a>

### program.getStructMembers(struct_name) ⇒ <code>Array.&lt;any&gt;</code>
Get a javascript object representation of a program struct and its types

**Kind**: instance method of [<code>Program</code>](#Program)

| Param | Type |
| --- | --- |
| struct_name | <code>string</code> |

<a name="Program+id"></a>

### program.id() ⇒ <code>string</code>
Get the id of the program

**Kind**: instance method of [<code>Program</code>](#Program)
<a name="Program+isEqual"></a>

### program.isEqual(other) ⇒ <code>boolean</code>
Determine equality with another program

**Kind**: instance method of [<code>Program</code>](#Program)

| Param | Type |
| --- | --- |
| other | [<code>Program</code>](#Program) |

<a name="Program+getImports"></a>

### program.getImports() ⇒ <code>Array.&lt;any&gt;</code>
Get program_imports

**Kind**: instance method of [<code>Program</code>](#Program)
<a name="Program.fromString"></a>

### Program.fromString(program) ⇒ [<code>Program</code>](#Program)
Create a program from a program string

**Kind**: static method of [<code>Program</code>](#Program)

| Param | Type |
| --- | --- |
| program | <code>string</code> |

<a name="Program.getCreditsProgram"></a>

### Program.getCreditsProgram() ⇒ [<code>Program</code>](#Program)
Get the credits.aleo program

**Kind**: static method of [<code>Program</code>](#Program)
<a name="ProvingKey"></a>

## ProvingKey
**Kind**: global class

* [ProvingKey](#ProvingKey)
    * _instance_
        * [.toBytes()](#ProvingKey+toBytes) ⇒ <code>Uint8Array</code>
    * _static_
        * [.fromBytes(bytes)](#ProvingKey.fromBytes) ⇒ [<code>ProvingKey</code>](#ProvingKey)

<a name="ProvingKey+toBytes"></a>

### provingKey.toBytes() ⇒ <code>Uint8Array</code>
Create a byte array from a proving key

**Kind**: instance method of [<code>ProvingKey</code>](#ProvingKey)
<a name="ProvingKey.fromBytes"></a>

### ProvingKey.fromBytes(bytes) ⇒ [<code>ProvingKey</code>](#ProvingKey)
Construct a new proving key from a byte array

**Kind**: static method of [<code>ProvingKey</code>](#ProvingKey)

| Param | Type |
| --- | --- |
| bytes | <code>Uint8Array</code> |

<a name="RecordCiphertext"></a>

## RecordCiphertext
Encrypted Aleo record

**Kind**: global class

* [RecordCiphertext](#RecordCiphertext)
    * _instance_
        * [.toString()](#RecordCiphertext+toString) ⇒ <code>string</code>
        * [.decrypt(view_key)](#RecordCiphertext+decrypt) ⇒ [<code>RecordPlaintext</code>](#RecordPlaintext)
        * [.isOwner(view_key)](#RecordCiphertext+isOwner) ⇒ <code>boolean</code>
    * _static_
        * [.fromString(record)](#RecordCiphertext.fromString) ⇒ [<code>RecordCiphertext</code>](#RecordCiphertext)

<a name="RecordCiphertext+toString"></a>

### recordCiphertext.toString() ⇒ <code>string</code>
Return the record ciphertext string.

**Kind**: instance method of [<code>RecordCiphertext</code>](#RecordCiphertext)
<a name="RecordCiphertext+decrypt"></a>

### recordCiphertext.decrypt(view_key) ⇒ [<code>RecordPlaintext</code>](#RecordPlaintext)
Decrypt the record ciphertext into plaintext using the view key.

**Kind**: instance method of [<code>RecordCiphertext</code>](#RecordCiphertext)

| Param | Type |
| --- | --- |
| view_key | [<code>ViewKey</code>](#ViewKey) |

<a name="RecordCiphertext+isOwner"></a>

### recordCiphertext.isOwner(view_key) ⇒ <code>boolean</code>
Returns `true` if the view key can decrypt the record ciphertext.

**Kind**: instance method of [<code>RecordCiphertext</code>](#RecordCiphertext)

| Param | Type |
| --- | --- |
| view_key | [<code>ViewKey</code>](#ViewKey) |

<a name="RecordCiphertext.fromString"></a>

### RecordCiphertext.fromString(record) ⇒ [<code>RecordCiphertext</code>](#RecordCiphertext)
Return a record ciphertext from a string.

**Kind**: static method of [<code>RecordCiphertext</code>](#RecordCiphertext)

| Param | Type |
| --- | --- |
| record | <code>string</code> |

<a name="RecordPlaintext"></a>

## RecordPlaintext
Aleo record plaintext

**Kind**: global class

* [RecordPlaintext](#RecordPlaintext)
    * _instance_
        * [.toString()](#RecordPlaintext+toString) ⇒ <code>string</code>
        * [.microcredits()](#RecordPlaintext+microcredits) ⇒ <code>bigint</code>
        * [.serialNumberString(private_key, program_id, record_name)](#RecordPlaintext+serialNumberString) ⇒ <code>string</code>
    * _static_
        * [.fromString(record)](#RecordPlaintext.fromString) ⇒ [<code>RecordPlaintext</code>](#RecordPlaintext)

<a name="RecordPlaintext+toString"></a>

### recordPlaintext.toString() ⇒ <code>string</code>
Returns the record plaintext string

**Kind**: instance method of [<code>RecordPlaintext</code>](#RecordPlaintext)
<a name="RecordPlaintext+microcredits"></a>

### recordPlaintext.microcredits() ⇒ <code>bigint</code>
Returns the amount of microcredits in the record

**Kind**: instance method of [<code>RecordPlaintext</code>](#RecordPlaintext)
<a name="RecordPlaintext+serialNumberString"></a>

### recordPlaintext.serialNumberString(private_key, program_id, record_name) ⇒ <code>string</code>
Attempt to get the serial number of a record to determine whether or not it has been spent

**Kind**: instance method of [<code>RecordPlaintext</code>](#RecordPlaintext)

| Param | Type |
| --- | --- |
| private_key | [<code>PrivateKey</code>](#PrivateKey) |
| program_id | <code>string</code> |
| record_name | <code>string</code> |

<a name="RecordPlaintext.fromString"></a>

### RecordPlaintext.fromString(record) ⇒ [<code>RecordPlaintext</code>](#RecordPlaintext)
Return a record plaintext from a string.

**Kind**: static method of [<code>RecordPlaintext</code>](#RecordPlaintext)

| Param | Type |
| --- | --- |
| record | <code>string</code> |

<a name="Signature"></a>

## Signature
**Kind**: global class

* [Signature](#Signature)
    * _instance_
        * [.verify(address, message)](#Signature+verify) ⇒ <code>boolean</code>
        * [.to_string()](#Signature+to_string) ⇒ <code>string</code>
    * _static_
        * [.sign(private_key, message)](#Signature.sign) ⇒ [<code>Signature</code>](#Signature)
        * [.from_string(signature)](#Signature.from_string) ⇒ [<code>Signature</code>](#Signature)

<a name="Signature+verify"></a>

### signature.verify(address, message) ⇒ <code>boolean</code>
**Kind**: instance method of [<code>Signature</code>](#Signature)

| Param | Type |
| --- | --- |
| address | [<code>Address</code>](#Address) |
| message | <code>Uint8Array</code> |

<a name="Signature+to_string"></a>

### signature.to\_string() ⇒ <code>string</code>
**Kind**: instance method of [<code>Signature</code>](#Signature)
<a name="Signature.sign"></a>

### Signature.sign(private_key, message) ⇒ [<code>Signature</code>](#Signature)
**Kind**: static method of [<code>Signature</code>](#Signature)

| Param | Type |
| --- | --- |
| private_key | [<code>PrivateKey</code>](#PrivateKey) |
| message | <code>Uint8Array</code> |

<a name="Signature.from_string"></a>

### Signature.from\_string(signature) ⇒ [<code>Signature</code>](#Signature)
**Kind**: static method of [<code>Signature</code>](#Signature)

| Param | Type |
| --- | --- |
| signature | <code>string</code> |

<a name="Transaction"></a>

## Transaction
Webassembly Representation of an Aleo transaction

This object is created when generating an on-chain function deployment or execution and is the
object that should be submitted to the Aleo Network in order to deploy or execute a function.

**Kind**: global class

* [Transaction](#Transaction)
    * _instance_
        * [.toString()](#Transaction+toString) ⇒ <code>string</code>
        * [.transactionId()](#Transaction+transactionId) ⇒ <code>string</code>
        * [.transactionType()](#Transaction+transactionType) ⇒ <code>string</code>
    * _static_
        * [.fromString(transaction)](#Transaction.fromString) ⇒ [<code>Transaction</code>](#Transaction)

<a name="Transaction+toString"></a>

### transaction.toString() ⇒ <code>string</code>
Get the transaction as a string. If you want to submit this transaction to the Aleo Network
this function will create the string that should be submitted in the `POST` data.

**Kind**: instance method of [<code>Transaction</code>](#Transaction)
<a name="Transaction+transactionId"></a>

### transaction.transactionId() ⇒ <code>string</code>
Get the id of the transaction. This is the merkle root of the transaction's inclusion proof.

This value can be used to query the status of the transaction on the Aleo Network to see
if it was successful. If successful, the transaction will be included in a block and this
value can be used to lookup the transaction data on-chain.

**Kind**: instance method of [<code>Transaction</code>](#Transaction)
<a name="Transaction+transactionType"></a>

### transaction.transactionType() ⇒ <code>string</code>
Get the type of the transaction (will return "deploy" or "execute")

**Kind**: instance method of [<code>Transaction</code>](#Transaction)
<a name="Transaction.fromString"></a>

### Transaction.fromString(transaction) ⇒ [<code>Transaction</code>](#Transaction)
Create a transaction from a string

**Kind**: static method of [<code>Transaction</code>](#Transaction)

| Param | Type |
| --- | --- |
| transaction | <code>string</code> |

<a name="VerifyingKey"></a>

## VerifyingKey
**Kind**: global class

* [VerifyingKey](#VerifyingKey)
    * _instance_
        * [.toBytes()](#VerifyingKey+toBytes) ⇒ <code>Uint8Array</code>
    * _static_
        * [.fromBytes(bytes)](#VerifyingKey.fromBytes) ⇒ [<code>VerifyingKey</code>](#VerifyingKey)

<a name="VerifyingKey+toBytes"></a>

### verifyingKey.toBytes() ⇒ <code>Uint8Array</code>
Create a byte array from a verifying key

**Kind**: instance method of [<code>VerifyingKey</code>](#VerifyingKey)
<a name="VerifyingKey.fromBytes"></a>

### VerifyingKey.fromBytes(bytes) ⇒ [<code>VerifyingKey</code>](#VerifyingKey)
Construct a new verifying key from a byte array

**Kind**: static method of [<code>VerifyingKey</code>](#VerifyingKey)

| Param | Type |
| --- | --- |
| bytes | <code>Uint8Array</code> |

<a name="ViewKey"></a>

## ViewKey
**Kind**: global class

* [ViewKey](#ViewKey)
    * _instance_
        * [.to_string()](#ViewKey+to_string) ⇒ <code>string</code>
        * [.to_address()](#ViewKey+to_address) ⇒ [<code>Address</code>](#Address)
        * [.decrypt(ciphertext)](#ViewKey+decrypt) ⇒ <code>string</code>
    * _static_
        * [.from_private_key(private_key)](#ViewKey.from_private_key) ⇒ [<code>ViewKey</code>](#ViewKey)
        * [.from_string(view_key)](#ViewKey.from_string) ⇒ [<code>ViewKey</code>](#ViewKey)

<a name="ViewKey+to_string"></a>

### viewKey.to\_string() ⇒ <code>string</code>
**Kind**: instance method of [<code>ViewKey</code>](#ViewKey)
<a name="ViewKey+to_address"></a>

### viewKey.to\_address() ⇒ [<code>Address</code>](#Address)
**Kind**: instance method of [<code>ViewKey</code>](#ViewKey)
<a name="ViewKey+decrypt"></a>

### viewKey.decrypt(ciphertext) ⇒ <code>string</code>
**Kind**: instance method of [<code>ViewKey</code>](#ViewKey)

| Param | Type |
| --- | --- |
| ciphertext | <code>string</code> |

<a name="ViewKey.from_private_key"></a>

### ViewKey.from\_private\_key(private_key) ⇒ [<code>ViewKey</code>](#ViewKey)
**Kind**: static method of [<code>ViewKey</code>](#ViewKey)

| Param | Type |
| --- | --- |
| private_key | [<code>PrivateKey</code>](#PrivateKey) |

<a name="ViewKey.from_string"></a>

### ViewKey.from\_string(view_key) ⇒ [<code>ViewKey</code>](#ViewKey)
**Kind**: static method of [<code>ViewKey</code>](#ViewKey)

| Param | Type |
| --- | --- |
| view_key | <code>string</code> |
