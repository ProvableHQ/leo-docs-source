---
id: glossary
title: Glossary
---

### Record Serial Number

A record serial number is the evaluation of a PRF on a spent record's serial number nonce and corresponding secret key. 
This means every record has a unique serial number, which will allow the Aleo network to prevent [double spends](02_records.md#double-spends) without revealing any information about the record contents.

### Record Commitment

The commitment that binds all the other attributes of a record without revealing the actual record data. 
This commitment is included in the transaction that created the record, which demonstrates the existence of the record without revealing its contents.

### Local Data Commitment

The commitment on the data being used to create the transaction - old records, new records, memo, and network id. This information is also used by the programs to reason about their application specific logic.

### Program

A program is user defined boolean function that reasons about a transactions local data and determines if it is valid. This allows for arbitrary applications to be run on Aleo. By representing state in records, programs can reason about their own application specific logic.

### Birth Program

A birth program is a program associated with the birth of a new record

### Death Program

A death program is a program associated with the death/spending of a record

### Ledger Membership Witness

The ledger membership witness is the merkle path proving that a record commitment exists in the global commitment Merkle tree.

### Randomized Signature

A randomized signature is a randomized Schnorr signature over the transaction contents that allows senders to delegate proof generation to third parties without revealing the private key authorized to spend the record.

### Variable Length Integer

A variable length integer is an encoding of 64-bit unsigned integers into variable length integers to save space when storing or transmitting numbers.
The algorithm is defined as:

|    Value    | Size (bytes) |               Format               |
|:-----------:|:------------:|:----------------------------------:|
|    < 253    |       1      |                 u8                 |
| <= u16::MAX |       3      | 0xFD followed by the length as u16 |
| <= u32::MAX |       5      | 0xFE followed by the length as u32 |
| <= u64::MAX |       9      | 0xFF followed by the length as u64 |
