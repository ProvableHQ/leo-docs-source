---
id: accounts
title: Accounts
sidebar_label: Accounts
---

An **Aleo account** is composed of an [account private key](#account-private-key), [account view key](#account-view-key),
and an [account address](#account-address).

The account private key is used to authorize a transaction, which updates the global state of account records. The account
view key is used to decrypt account records, which are encrypted under the user's account address. Lastly, the account
address enables users to interact with one another, sending and receiving records that encode values and application data.

To protect user *assets* and *record data*, one should **never disclose their account private key** to any
third parties. For real-world applications on Aleo, users should derive a compute key from their account private key to
allow third parties to *trustlessly* run applications and generate transactions on a user's behalf.

Generate a new Aleo account [here](https://provable.tools).

## Account Private Key

An account private key is constructed from a randomly-sampled **account seed**. This account seed is used to generate:
- a **secret key** for the account signature scheme,
- a **pseudorandom function seed** for transaction serial numbers, and
- a **commitment randomness** for the account commitment scheme.

### Private Key Format

```
APrivateKey1zkp4X9ApjTb7Rv8EABfZRugXBhbPzCL245GyNtYJP5GYY2k
```

An account private key is formatted as a Base58 string, comprised of 59 characters.
The account private key is encoded with a [private key prefix](#account-prefixes) that reads `APrivateKey1`, indicating
that it is a private key and should not be shared with other users.

## Account View Key

An Aleo account view key is derived from an account private key and enables users to decrypt their
[records](02_records.md) from the global ledger.
As account view keys are able to access every record in a user's account, this key can be used by
third-party auditors to verify the complete history of an account.

The account view key is comprised of:
- a **secret key** for the account encryption scheme.

### View Key Format

```
AViewKey1nKB4qr9b5gK8wQvmM5sTPEuBwshtDdkCZB1SPWppAG9Y
```

An account view key is formatted as a Base58 string, comprised of 53 characters.
The account view key is encoded with a [view key prefix](#account-prefixes) that reads `AViewKey1`, indicating
that it is a view key and should only be shared with authorized parties.

## Account Address

An Aleo account address is a unique identifier that allows users to transfer value and record data to one another in transactions.

The account address is comprised of:
- a **public key** for the account encryption scheme.

### Address Format

```
aleo1dg722m22fzpz6xjdrvl9tzu5t68zmypj5p74khlqcac0gvednygqxaax0j
```

An account address is formatted as a Bech32 string, comprised of 63 characters.
The account address is encoded with an [address prefix](#account-prefixes) that reads `aleo1`.

## Advanced Topics

The descriptions of algorithms below make use of the following definitions of mathematical objects:

### Prime Fields

For a prime `r`, the prime field of order `r` is defined as the set of integers `{0, 1, ..., r - 1}` with addition and multiplication modulo `r`.
In this document, we will use two prime fields:
* <code>F<sub>scalar</sub></code>, of prime order `p = 2111115437357092606062206234695386632838870926408408195193685246394721360383`
* <code>F<sub>base</sub></code>, of prime order `q = 8444461749428370424248824938781546531375899335154063827935233455917409239041`

### Prime Order Elliptic Curve Groups

In this document, we will consider the order-`p` subgroup of points on an elliptic curve defined over the base field
<code>F<sub>base</sub></code>. Elements of this subgroup consist of a coordinate pair `(x, y)`. The group has two
associated operations: point addition, and point doubling. The group also has a distinguished point, the **generator** `G`,
which is a fixed point of the group.

### HashToField

For a finite field `F`, HashToField is a cryptographic hash function that takes as input either a sequence of bytes or 
a sequence of field elements and outputs a field element. The output is uniformly distributed over the field `F`.

### HashToScalar

An instantiation of HashToField that output elements in the scalar field <code>F<sub>scalar</sub></code>.
[source code](https://github.com/AleoNet/snarkVM/blob/mainnet/console/algorithms/src/poseidon/hash_to_scalar.rs#L24)

### EncodeToF

EncodeToF(x) is a function that encodes the Unicode string `x` into an element of <code>F<sub>base</sub></code>.

Details of the encoding:
- `x` is converted to its UTF-8 sequence of bytes `b`.
- `b` is turned into an unsigned integer `v` that represents the little endian value of `b`.
- `v` is reduced modulo the prime that defines the field <code>F<sub>base</sub></code>.

### Account Prefixes

|                         |  Type  | Human-Readable Prefix |                    Prefix Bytes                    |
|:-----------------------:|:------:|:---------------------:|:--------------------------------------------------:|
| **Account Private Key** | bytes  | `APrivateKey1`        | `[ 127, 134, 189, 116, 210, 221, 210, 137, 144 ]`  |
| **Account View Key**    | bytes  | `AViewKey1`           | `[ 14, 138, 223, 204, 247, 224, 122 ]`             |
| **Account Address**     | string | `aleo1`               | `aleo1`                                            |

### Offline Accounts

In many instances such as enterprise settings, it is advisable to handle sensitive keys and data on isolated, offline machines.
An Aleo account can be created on an offline machine and available for immediate use. In conjunction with account proving keys,
a user can ensure their private key remains offline even for creating transactions.

While no solution is perfect, it is advisable to create a new Aleo account on a disconnected device to minimize the risk of
leaking one's account private key to unintended parties.

### Account Commitment Outputs

The account commitment output is used to create an account view key, which is comprised of an encryption secret key.
This encryption secret key is a scalar field element derived from the account commitment output. To ensure the validity
of the account view key, the account commitment output should be representable in the scalar field.

### Create an Account

Given global instantiated Aleo parameters and subroutines.

#### Generate a Private Key

1. Sample a 32 byte `seed` from random

2. Construct private key components
    - `sk_sig` = HashToScalar(EncodeToF("AleoAccountSignatureSecretKey0") | `seed`))
    - `r_sig` = HashToScalar(EncodeToF("AleoAccountSignatureRandomizer0.0") | `seed`))

    where | denotes concatenation,
    and where `HashToScalar` denotes the [Poseidon hash function](https://eprint.iacr.org/2019/458.pdf).

3.`private_key` = (`seed`, (`sk_sig`, `r_sig`))

[source code](https://github.com/AleoNet/snarkVM/blob/mainnet/console/account/src/private_key/try_from.rs)

#### Generate a View Key
1. `(sk_sig, r_sig)` = `private_key`
2. `view_key` = `sk_sig` + `r_sig` + HashToScalar(`sk_sig` * `G` | `r_sig` * `G`)

where `G` is the generator of the base field.

#### Generate an Address

1. `address` = `view_key` * `G`
