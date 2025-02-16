---
id: programs
title: Programs
sidebar_label: Programs
---

A **program** is a fundamental artifact for representing application logic and application state.

Aleo introduces a new programming language called **Aleo instructions** that enables developers to write
private web applications. Aleo instructions is a statically-typed programming language for
writing privacy-preserving, secure programs on Aleo. By leveraging zero-knowledge proofs, Aleo instructions offers computational
integrity for real-world applications.

## Program Logic

Aleo instructions offers developers with an easy-to-use environment for writing programs. By designing an assembly language with
syntax familiar to developers and composable in features, Aleo instructions is well-suited to integrate with
existing developer frameworks to supercharge web apps with privacy and integrity.

```aleo showLineNumbers
program token.aleo;

record token:
    // The token owner.
    owner as address.private;
    // The token balance.
    amount as u64.private;

// The `mint` function initializes a new record with the
// specified number of tokens in `r1` for the receiver in `r0`.
function mint:
    input r0 as address.private;
    input r1 as u64.private;
    cast r0 r1 into r2 as token.record;
    output r2 as token.record;

// The `transfer` function sends the specified number of tokens
// to the receiver from the provided token record.
function transfer:
    // Input the sender's record.
    input r0 as token.record;
    // Input the token receiver.
    input r1 as address.private;
    // Input the token amount.
    input r2 as u64.private;

    // The `sub` operation is safe: it checks for underflows and 
    // halts if an underflow occurs. When an instruction like this 
    // halts, no zero-knowledge proof is generated. The output 
    // register `r3` holds the change amount to be returned to the sender.
    sub r0.amount r2 into r3;

    // Produces a token record for the specified receiver.
    cast r1 r2 into r4 as token.record;

    // Produces a token record with the change amount for the sender.
    cast r0.owner r3 into r5 as token.record;

    // Output the receiver's record.
    output r4 as token.record;
    // Output the sender's change record.
    output r5 as token.record;
```

## Program Data

### Program ID

Each program has a unique **program ID** that is stored in the program manifest `program.json`. This program ID is used to indicate the program that was run in the
consumption or production of [records](02_records.md).

### Program Input

To run a program, user-defined inputs are provided in the form of a **program input**. This input provided by the user
is fully private and not revealed to the public network, unless the user intends for it to be public.

### Program State

Each program is run with respect to user-provided **program state** on Aleo. In order to produce a valid state transition
on Aleo, the user satisfies a series of programs encoded in [records](02_records.md), which compose a [transaction](03_transactions.md).

### Program Output

Once a program is evaluated, its **program output** is produced, along with a zero-knowledge proof attesting to the
validity of the output.
