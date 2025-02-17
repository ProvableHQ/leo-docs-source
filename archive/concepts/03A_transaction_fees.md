---
id: transaction_fees
title: Transaction Fees
sidebar_label: Transaction Fees
---

# Transaction Fees

A **transaction fee** is a fee that is required in order to process a transaction on the Aleo network. Currently, there are two types of transaction fees that correspond to each transaction type: **deploy** and **execute**.

## Aleo Credits Denomination Table

|Denomination|Size of Transaction|Value|Example|
|:-:|:-:|:-:|:-:|
|`microcredit`|Byte|1|Smallest denomination|
|`millicredit`|Kilobyte (KB)|1000 microcredits|Most transactions average between 3 and 10 millicredits|
|`credit`|Megabyte (MB)|1000 millicredits|Largest denomination|

Note that transaction sizes are averages using [Big _O_ notation (on the order of)](https://en.wikipedia.org/wiki/Big_O_notation).

# Types of Transaction Fees

## Deploy Transaction Fee
A fee incurred when deploying an application on the Aleo blockchain

### Structure

|Part of Deploy Transaction|Size| Multiplier|Cost|
|:-:|:-:|:-:|:-:|
|`Transaction wrapper`|0 KB|1|0 millicredits|
|`Transition wrapper`|0 KB|1|0 millicredits|
|`Verifying key`|1 KB|2|2 millicredits|
|`Certificate`|1 MB|2|2 millicredits|
|`Program bytecode`|1 KB|1|1 millicredit|

|Total|Size|Multiplier|Total cost|
|:-:|:-:|:-:|:-:|
|`Transaction -> Transition`|5 KB|1000 *|5 credits|


#### Notes
* To prevent bad actors from spamming the network with superfluous transactions, a variable **Deploy Transaction Multiplier** will be added to all deploy transactions.

## Execute Transaction Fee
A fee incurred when executing an application on the Aleo blockchain

### Structure (minimum cost example)

|Part of Execute Transaction|Size| Multiplier|Cost|
|:-:|:-:|:-:|:-:|
|`Transaction wrapper`|0 KB|1|0 millicredits|
|`Transition wrapper`|0 KB|1|0 millicredits|
|`Input`|1 KB|2|2 millicredits|
|`Output`|1 KB|2|2 millicredits|
|`Proof`|1 KB|1|1 millicredit|

|Total|Size|Multiplier|Total cost|
|:-:|:-:|:-:|:-:|
|`Transaction -> Transition`|5 KB|1|5 millicredits|

### Structure (maximum cost example)

|Part of Execute Transaction|Size| Multiplier|Cost|
|:-:|:-:|:-:|:-:|
|`Transaction wrapper`|1 KB|65|65 millicredits|
|`Transition wrapper`|1 MB|32|32 credits|
|`Input`|1 KB|16 * 32|512 millicredits|
|`Output`|1 KB| 16 * 32|512 millicredits|
|`Proof`|1 KB|1|1 millicredit|

|Total|Size|Multiplier|Total cost|
|:-:|:-:|:-:|:-:|
|`Transaction -> Transition`|32 MB|1|32 credits|

Notes
* The minimum execute transaction fee may increase to prevent spamming; it is currently stable between 3 and 5 millicredits for a basic `credits.aleo` transfer



