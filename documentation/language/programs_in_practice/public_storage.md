---
id: public_state
title: Public State
sidebar_label: Public State
---
[general tags]: # (program, mapping, storage)


## Mappings

There are several functions available to query and modify mappings.  The examples below will reference the following mapping:
```leo
mapping balance: address => u64;
```

### Querying
To simply check if a value has been set for a particular `address` in `balance`:
```leo
balance.contains(addr)
Mapping::contains(balance, addr); // Alternate syntax
```
To query a value for a particular `address` in `balance`:
```leo
balance.get(addr)
Mapping::get(balance, addr); // Alternate syntax
```
Note that if value at `addr` does not exist above, then the program will fail to execute.  To query a value with a fallback for this case:
```leo
balance.get_or_use(addr,fallback_value)
Mapping::get_or_use(balance, addr, fallback_value); // Alternate syntax
```
A program can also query values from another program's mappings:
```leo
let balance1 = credits.aleo/account.get(addr);
let balance2 = credits.aleo/account.get_or_use(addr, 0u64);
```
Although values can be queried, a program cannot directly modify another program's mappings.


### Modifying
To set a value for a particular `address` in `balance`:
```leo
balance.set(addr,value)
Mapping::set(balance, addr, value); // Alternate syntax
```
To remove the value set at particular `address` in `balance`:
```leo
balance.remove(addr)
Mapping::remove(balance, addr); // Alternate syntax
```

### Usage
```leo showLineNumbers
program map.aleo {
    mapping balance: address => u64;

    async transition dubble() -> Future {
        return dubble_onchain(self.caller);
    }

    async function dubble_onchain(addr: address) {
        let current_value: u64 = balance.get_or_use(addr, 0u64);
        balance.set(addr, current_value + 1u64);

        let next_current_value = balance.get(addr);
        balance.set(addr, current_value + 1u64);
    }

}
```

:::info
Mapping operations are only allowed in an [async function](#async-function) or async block.
:::

## Storage Variables

Storage variables behave similar to option types.  There are several functions available to query and modify singleton storage variables.  The examples below will reference the following:
```leo
storage counter: u64;
```

### Querying
To query the value currrently stored at `counter`:
```leo
counter.unwrap();
```
Note that if `counter` has not been initialized, then the program will fail to execute.  To query the value with a fallback for this case:
```leo
counter.unwrap_or(fallback_value);
```
### Modifying
To set a value for `counter`:
```leo
counter = 5u64;
```
To unset the value at `counter`:
```leo
counter = none;
```

### Usage
```leo showLineNumbers
program storage_variable.aleo {
    storage counter: u64;

    async transition increment() -> Future {
        return increment_onchain();
    }

    async function increment_onchain() {
        let current_value: u64 = counter.unwrap_or(0u64);
        counter = current_value + 1u64;
    }

}
```
:::info
Storage variable operations are only allowed in an [async function](#async-function) or async block.
:::


## Storage Vectors

Storage vectors behave like dynamic arrays of values of a given types.  There are several functions available to query and modify storage vectors.  The examples below will reference the following:
```leo
storage id_numbers: [u64];
```

### Querying
To query the element currrently stored in `id_numbers` at index `idx`:
```leo
id_numbers.get(idx);
```
To get the current length of `id_numbers`:
```leo
id_numbers.len();
```
### Modifying
To set an element at index `idx` in `id_numbers`:
```leo
id_numbers.set(idx, value);
```
To push an element onto the end of `id_numbers`:
```leo
id_numbers.push(value);
```
To pop and return the last element of `id_numbers`:
```leo
id_numbers.pop();
```
To remove the element at index `idx`, return it, and replace it with the final element of `id_numbers`:
```leo
id_numbers.swap_remove(idx);
```
To clear the every element in `id_numbers`:
```leo
id_numbers.clear()
```
:::note
- `clear()` does not actually remove any values from the vector. It just sets the length to 0.
- Similarly `swap_remove()` and `pop()` do not actually remove values either. They just reduce the length by 1 to make sure the last element is no longer accessible.
:::

### Usage

```leo showLineNumbers
program storage_vector.aleo {
    storage id_numbers: [u64];

    async transition add_id(new_id: u64) -> Future {
        return add_id_onchain(new_id);
    }

    async function add_id_onchain(new_id: u64) {
        id_numbers.push(new_id);
    }


    async transition remove_id(idx: u32) -> Future {
        return remove_id_onchain(idx);
    }

    async function remove_id_onchain(idx: u32) {
        id_numbers.swap_remove(idx);
    }
}
```
:::info
Storage vector operations are only allowed in an [async function](#async-function) or async block.
:::

