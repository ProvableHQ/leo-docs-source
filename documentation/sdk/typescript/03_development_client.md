---
title: Development Client
sidebar_label: Development Client
---

<a name="DevelopmentClient"></a>

## Overview
**Kind**: global class

* [DevelopmentClient](#DevelopmentClient)
    * [new DevelopmentClient(baseURL)](#new_DevelopmentClient_new)
    * [.deployProgram(program, fee, privateKey, password, feeRecord)](#DevelopmentClient+deployProgram) ⇒ <code>string</code> \| <code>Error</code>
    * [.executeProgram(programId, programFunction, fee, inputs, privateKey, password, feeRecord)](#DevelopmentClient+executeProgram) ⇒ <code>string</code> \| <code>Error</code>
    * [.transfer(amount, fee, recipient, transfer_type, privateKey, password, feeRecord, amountRecord)](#DevelopmentClient+transfer) ⇒ <code>string</code> \| <code>Error</code>

<a name="new_DevelopmentClient_new"></a>

### new DevelopmentClient(baseURL)
<p>Creates a new DevelopmentClient to interact with an Aleo Development Server.</p>


| Param | Type | Description |
| --- | --- | --- |
| baseURL | <code>string</code> | <p>The URL of the Aleo Development Server</p> |

<a name="DevelopmentClient+deployProgram"></a>

### developmentClient.deployProgram(program, fee, privateKey, password, feeRecord) ⇒ <code>string</code> \| <code>Error</code>
<p>Deploys a program on the Aleo Network via an Aleo development server.
It requires an Aleo Development Server to be running remotely or locally.
If one is not running, this function will throw an error.</p>
<p>Information on how to run an Aleo Development Server can be found here:
https://github.com/ProvableHQ/sdk/rust/develop/README.md</p>

**Kind**: instance method of [<code>DevelopmentClient</code>](#DevelopmentClient)
**Returns**: <code>string</code> \| <code>Error</code> - <p>The transaction_id of the deployment transaction if successful</p>

| Param | Type | Description |
| --- | --- | --- |
| program | <code>string</code> | <p>Text representation of the program to be deployed</p> |
| fee | <code>number</code> | <p>Fee to be paid for the program deployment (REQUIRED)</p> |
| privateKey | <code>string</code> \| <code>undefined</code> | <p>Optional private key of the user who is deploying the program</p> |
| password | <code>string</code> \| <code>undefined</code> | <p>If the development server is started with an encrypted private key, the password is required</p> |
| feeRecord | <code>string</code> \| <code>undefined</code> | <p>Optional record in text format to be used for the fee. If not provided, the server will search the network for a suitable record to pay the fee.</p> |

**Example**
```js
const Program = 'program yourprogram.aleo;\n\nfunction hello:\n    input r0 as u32.public;\n    input r1 as u32.private;\n    add r0 r1 into r2;\n    output r2 as u32.private;\n';
const client = new DevelopmentClient("http://0.0.0.0:4040");
const transaction_id = await client.deployProgram(Program, 6000000, privateKeyString);
```
<a name="DevelopmentClient+executeProgram"></a>

### developmentClient.executeProgram(programId, programFunction, fee, inputs, privateKey, password, feeRecord) ⇒ <code>string</code> \| <code>Error</code>
<p>Executes a program on the Aleo Network via an Aleo development server.
It requires an Aleo Development Server to be running remotely or locally.
If one is not running, this function will throw an error.</p>
<p>Information on how to run an Aleo Development Server can be found here:
https://github.com/ProvableHQ/sdk/rust/develop/README.md</p>

**Kind**: instance method of [<code>DevelopmentClient</code>](#DevelopmentClient)
**Returns**: <code>string</code> \| <code>Error</code> - <p>The transaction_id of the execution transaction if successful</p>

| Param | Type | Description |
| --- | --- | --- |
| programId | <code>string</code> | <p>The program_id of the program to be executed (e.g. hello.aleo)</p> |
| programFunction | <code>string</code> | <p>The function to execute within the program (e.g. hello)</p> |
| fee | <code>number</code> | <p>Optional Fee to be paid for the execution transaction, specify 0 for no fee</p> |
| inputs | <code>Array.&lt;string&gt;</code> | <p>Array of inputs to be passed to the program</p> |
| privateKey | <code>string</code> \| <code>undefined</code> | <p>Optional private key of the user who is executing the program</p> |
| password | <code>string</code> \| <code>undefined</code> | <p>If the development server is started with an encrypted private key, the password is required</p> |
| feeRecord | <code>string</code> \| <code>undefined</code> | <p>Optional record in text format to be used for the fee. If not provided, the server will search the network for a suitable record to pay the fee.</p> |

**Example**
```js
const privateKey = "your private key";
const client = new DevelopmentClient("http://0.0.0.0:4040");
const transaction_id = await client.executeProgram("hello.aleo", "hello", 0, ["5u32", "5u32"], privateKeyString);
```
<a name="DevelopmentClient+transfer"></a>

### developmentClient.transfer(amount, fee, recipient, transfer_type, privateKey, password, feeRecord, amountRecord) ⇒ <code>string</code> \| <code>Error</code>
<p>Sends an amount in credits to a specified recipient on the Aleo Network
via an Aleo development server. It requires an Aleo Development Server
to be running remotely or locally. If one is not running, this function
will throw an error.</p>
<p>Information on how to run an Aleo Development Server can be found here:
https://github.com/ProvableHQ/sdk/rust/develop/README.md</p>

**Kind**: instance method of [<code>DevelopmentClient</code>](#DevelopmentClient)
**Returns**: <code>string</code> \| <code>Error</code> - <p>The transaction_id of the execution transaction if successful</p>

| Param | Type | Description |
| --- | --- | --- |
| amount | <code>string</code> | <p>The amount of credits to be sent (e.g. 1.5)</p> |
| fee | <code>number</code> | <p>Optional Fee to be paid for the transfer, specify 0 for no fee</p> |
| recipient | <code>string</code> | <p>The recipient of the transfer</p> |
| transfer_type | <code>string</code> | <p>The type of the transfer (possible values are &quot;private&quot;, &quot;public&quot;, &quot;private_to_public&quot;, &quot;public_to_private&quot;)</p> |
| privateKey | <code>string</code> \| <code>undefined</code> | <p>Optional private key of the user who is sending the transfer</p> |
| password | <code>string</code> \| <code>undefined</code> | <p>If the development server is started with an encrypted private key, the password is required</p> |
| feeRecord | <code>string</code> \| <code>undefined</code> | <p>Optional record in text format to be used for the fee. If not provided, the server will search the network for a suitable record to pay the fee.</p> |
| amountRecord | <code>string</code> \| <code>undefined</code> | <p>Optional record in text format to be used to fund the transfer. If not provided, the server will search the network for a suitable record to fund the amount.</p> |

**Example**
```js
const privateKey = "your private key";
const recipient = "recipient's address";
const client = new DevelopmentClient("http://0.0.0.0:4040");
const transaction_id = await client.transfer(1.5, 0, recipient, privateKey);
```
