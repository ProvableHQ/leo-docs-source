---
id: inclusion_proof
title: Inclusion Proofs
sidebar_label: Inclusion Proofs
---

Inclusion proofs are like a special handshake or password that gets you past verifier without revealing the actual element. In Aleo, inclusion proofs are like digital receipts that verify transactions without compromising users' privacy.

Inclusion proofs are a part of Merkle tree data structures, which are used in cryptographic systems to efficiently prove that a piece of data is part of a set of data. An inclusion proof, in this context, is a proof that a certain element is included in a set without revealing the entire set.

In SnarkVM function `inclusion_proving_key()` returns the proving key for inclusion circuit and `inclusion_verifying_key()` returns the verifying key for inclusion circuit. As the name implies, the inclusion proving key is used to prove that a certain element is included in a set, while the inclusion verifying key is used to verify that the proof is correct.

In Aleo, to execute a function, a user uses the synthesizer in a similar way as the deployment process. As such, the user will produce 𝑛+1 transition proofs if they wish to execute a function call that triggers 𝑛 nested calls. The synthesizer is run with the actual values (as opposed to random values) for the inputs as well as the different registers in the circuits.

In addition to producing these proofs, a user also produces 𝑛+1 “inclusion proofs”. These inclusion proofs are used to prove that any record being used as input in the transition or function call indeed exists. Inclusion proofs prove that records exist either in some previous block that has been included in the blockchain, or in one of the previous transition outputs.

An inclusion proof also publicly outputs the serial numbers (also called a nullifier in ZCash-like systems) that uniquely identify the records without leaking any information about them. This way, records cannot be consumed more than once. (In addition, the network enforces that no serial number is seen twice within the same transaction.)

**Note:** All of these different proofs are eventually aggregated together into a single proof using Varuna’s batching capabilities.
