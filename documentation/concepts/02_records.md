---
id: records
title: Records
sidebar_label: Records
---


A **record** is a fundamental data structure for encoding user assets and application state.

Each account record contains information that specifies the record owner, its stored value, and its application state.
Records in Aleo are consumed and newly created from a [transition](04_transitions.md) function. A [transaction](03_transactions.md) consists of multiple transitions, If the `visibility` of the record is `private`, it is normally encrypted using the owner’s address secret key, to enable privacy in the application.

## Components of a Record

An Aleo record is serialized in the following format:

| Parameter |          Type          |                                                      Description                                                       |
|:---------:|:----------------------:|:----------------------------------------------------------------------------------------------------------------------:|
|  `owner`  |        address         |                               The address public key of the owner of the program record                                |
|  `data`   | Map<Identifier, Entry> | A data payload containing arbitrary application-dependent information. Each entry can either be `public` or `private`. |
|  `nonce`  |         group          |                                     The serial number nonce of the program record                                      |
An example record:
```bash
{
  owner: aleo13ssze66adjjkt795z9u5wpq8h6kn0y2657726h4h3e3wfnez4vqsm3008q.private,
  amount: 100u64.private,
  _nonce: 5861592911433819692697358191094794940442348980903696700646555355124091569429group.public
}
```

### Owner
`aleo13ssze66adjjkt795z9u5wpq8h6kn0y2657726h4h3e3wfnez4vqsm3008q`

The record owner is an account address, and specifies the party who is authorized to spend the record.


### Data
`100u64.private`

The record can encode arbitrary application information. The "amount" key is the data payload that the record carries.
An entry which has a `visibility` of `private` is encrypted and stored on the ledger.
This enables users to securely and privately transfer record data and values between one another over the public network.
Only the sender and receiver with their corresponding account view keys are able to decrypt the private entries.

### Nonce
`5861592911433819692697358191094794940442348980903696700646555355124091569429group`

The serial number nonce is used to create a unique identifier for each record, and is computed via a PRF evaluation of the address secret key ask of the owner and the record's serial number.
For a practical demonstration of a record in Aleo, see [here](https://youtu.be/JIgrKv_Q6Jo?feature=shared).


## Diving into the Concepts
To understand how to use records, we must understand the design principles behind Aleo.
Autonomous Ledger Execution Offchain (Aleo) is a layer-1 blockchain that combines general-purpose programmability with privacy by default.
The core idea behind Aleo is ZEXE or zero-knowledge execution initially written in this [research paper](https://eprint.iacr.org/2018/962.pdf) in 2018. It first introduced the record model which extends the UTXO model from Zcash and enables storing and encrypting arbitrary data (user assets and application states), rather than just values of specific assets or tokens.

### Privacy
There are generally four different types of privacy that relate to blockchains.

Aleo fulfils the 3 of them:
- [x] Private inputs (messages)
- [x] Private outputs (state changes)
- [x] Private user
- [ ] Private function

Initially, Aleo was aiming for function privacy as well (as detailed in the original ZEXE paper) but decided against it as it would have led to worse performance and longer proving times.


### Comparing state storage in blockchains
There are two main state models used in blockchains - UTXO (unspent transaction output) and the account model (introduced by Ethereum).

Aleo uses a variation of the UTXO model - the record model.

<p align="center" width="100%">
<img src={require("./images/account_vs_utxo.png").default} alt="Account vs UTXO"></img>
</p>

<p align="center" width="100%">
Source: <a href="https://www.galaxy.com/insights/research/mev-how-flashboys-became-flashbots/">galaxy.com</a>
</p>
<br></br>

### Account Model
In the account model as used in Ethereum, the application state can be found by referencing to a particular address.

As such, anyone would be able to view the activities of any account, simply with the knowledge of the address.

<p align="center" width="100%">
<img src={require("./images/ethereum_storage.png").default} alt="Ethereum Storage Diagram"></img>
</p>

<p align="center" width="100%">
Source: <a href="https://ethereum.org/en/develope.rs/docs/accounts/"> ethereum.org</a>
</p>
<br></br>


<p align="center" width="100%">
<img src={require("./images/ethereum_world_state.png").default} alt="Ethereum World State Diagram"></img>
</p>

<p align="center" width="100%">
Source: <a href="https://www.lucassaldanha.com/ethereum-yellow-paper-walkthrough-2/">Article by Lucas Saldanha</a>
</p>

<br></br>



### Record Model
In the record model, the application state, along with its owner are encrypted and stored on the blockchain.

<p align="center" width="300">
<img src={require("./images/record.png").default} alt="Aleo Records Diagram"></img>
</p>

<p align="center" width="100%">
Source: <a href="https://eprint.iacr.org/2018/962.pdf"> Zexe: Enabling Decentralized Private Computation</a>
</p>

<br></br>


<p align="center" width="100%">
<img src={require("./images/aleo_ledger.png").default} alt="Aleo World State Diagram"></img>
</p>


<p align="center" width="100%">
Source: <a href="https://eprint.iacr.org/2018/962.pdf"> Zexe: Enabling Decentralized Private Computation</a>
</p>
<br></br>



### Updating State
In the record model, applications update their state by consuming records containing the old state, and producing new records that contain the updated state. Records that have been used will be marked as spent and cannot be used again.

<p align="center" width="500">
<img src={require("./images/utxo.png").default} alt="UTXO diagram"></img>
</p>

<p align="center" width="100%">
Source: <a href="https://adapulse.io/the-extensive-guide-on-eutxo-utxo-and-the-accounts-based-model/"> adapulse.io</a>
</p>
<br></br>

The consumption and production of records is typically done in a transition function. A transaction in Aleo can contain up to 32 transitions, one of which is reserved for the transaction fee.


<p align="center" width="100%">
<img src={require("./images/transaction_in_aleo.png").default}  alt="Transaction in Aleo"></img>
</p>

### Why is the Record Model Useful?

In the account-based model, an application's data is stored in a persistent location tied to the application's account, and updates are made directly to this stored data. For a typical token transfer transaction using this model, user balances would be stored in a table mapping user account addresses to their respective balances. When User A transfers money to User B, A's balance in the table is reduced, and B's balance is increased by the same amount.
If we were to try making the transactions private (hiding the amount transferred and the identities of A and B), instead of storing actual balances, the application can store commitments to these balances. Transactions would then update these commitments rather than the actual balances. However, while this approach hides transaction values, it does not hide user identities. To also hide user identities, every transaction would need to update all commitments in the table, which becomes increasingly inefficient as the number of users grows.
Although the account model is more intuitive for developers, it uses account addresses to index global state. This means that while a private account model can achieve privacy for inputs and outputs, it still compromises user privacy since account addresses cannot be encrypted. Another issue with the private account model is the lack of concurrency, as only one user can access and update the entire program state at a time.
Aleo's record model uses program IDs to uniquely identify programs instead of account addresses. This improves privacy and enables programs to have internal states. This approach is more efficient and solves the concurrency issue.







