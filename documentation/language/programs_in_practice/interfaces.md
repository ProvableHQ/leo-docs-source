---
id: interfaces
title: Interfaces, Dynamic Calls & Dynamic Records
sidebar_label: Interfaces & Dynamic Dispatch
---

[general tags]: # "interface, dynamic_dispatch, dynamic_record, dyn_record, polymorphism, identifier"

Leo provides three related features for building composable, generic programs:

- **Interfaces** — declare a named contract that programs must fulfill. Available in Leo v4.
- **Dynamic Calls** — call into a program determined at runtime.
- **Dynamic Records** — pass and inspect records whose structure is unknown at compile time.

## Interfaces

An `interface` declaration specifies a set of functions, records, and mappings that a program must provide. Interfaces are declared outside the `program {}` block.

```leo
interface Transfer {
    record Token;
    fn transfer(input: Token, to: address, amount: u64) -> Token;
}
```

### Implementing an Interface

A program implements an interface by listing it after `:` in the program declaration. The compiler checks that the program provides everything the interface requires.

```leo
program my_token.aleo : Transfer {
    mapping balances: address => u64;

    record Token {
        owner: address,
        balance: u64,
    }

    fn transfer(input: Token, to: address, amount: u64) -> Token {
        return Token { owner: to, balance: input.balance - amount };
    }
}
```

### Record Requirements

An interface can require the existence of a record by name:

```leo
interface Foo {
    record Bar; // programs implementing Foo must declare a record called Bar
}
```

It can also require that the record has specific fields. Use `..` to indicate that implementors may declare additional fields beyond those required:

```leo
interface Foo {
    record Bar {
        owner: address, // all records must have an owner field
        baz: u64,       // Bar must also have a baz field of type u64
        ..              // implementors may add more fields
    }
}
```

### Inheritance and Composition

Interfaces can inherit from other interfaces using `:`:

```leo
interface Base {
    fn get_value() -> u64;
}

interface Extended : Base {
    fn set_value(v: u64) -> u64;
}
```

Multiple interfaces can be composed together using `+`:

```leo
interface Transfer {
    record Token;
    fn transfer(input: Token, to: address, amount: u64) -> Token;
}

interface Balances {
    mapping balances: address => u64;
}

// Token requires everything from both Transfer and Balances
interface Token : Transfer + Balances {}

program my_token.aleo : Token { /* ... */ }
```

## Dynamic Calls

Static calls require the callee program to be known at compile time:

```leo
// Static: the callee is fixed at compile time
fn route_transfer_static(to: address, amount: u64) {
    return token_a.aleo::transfer(to, amount);
}
```

Dynamic calls allow the callee to be determined at runtime. The caller still knows *what* it can call — expressed as an interface — but not *which* program it is calling:

```leo
// Dynamic: any program that implements TokenStandard can be called
fn route_transfer_dynamic(token_program: identifier, to: address, amount: u64) {
    return token_a.aleo::TokenStandard@(token_program)::transfer_public(to, amount);
}
```

The syntax is:
```
qualifier.aleo::Interface@(target)::method(args)
```
where:
- `qualifier.aleo` is the imported program that declares (or re-exports) the interface. This must be a static import.
- `Interface` is the interface name.
- `target` is an `identifier` value resolved at runtime — the name of the program to call into.
- `method` is the function to invoke.

### The `identifier` Type

The `identifier` type represents a program name resolved at runtime. An `identifier` literal uses single-quote syntax:

```leo
let target: identifier = 'my_program';
return token_a.aleo::TokenStandard@(target)::transfer_public(to, amount);
```

By default the target is looked up on the `aleo` network. To specify a different network explicitly, pass a second `identifier` as a second argument:

```leo
let target: identifier = 'my_program';
let network: identifier = 'aleo';
return token_a.aleo::TokenStandard@(target, network)::transfer_public(to, amount);
```

:::note
The only valid network identifier currently is `aleo`.
:::
## Dynamic Records

A `dyn record` is a record whose field structure is not known at compile time. It retains all the ownership and privacy properties of a regular record:

```leo
fn try_get_memo(rec: dyn record) -> u64 {
    let memo: u64 = rec.memo; // fails at runtime if `rec` does not have a field named `memo` of type `u64`.
    return 0u64;
}
```

`dyn record` complements dynamic calls: while dynamic calls allow a program to route logic to any compliant callee, `dyn record` allows that same program to accept, inspect, and forward records from programs it has never seen at compile time, without losing the safety guarantees of the type system.
