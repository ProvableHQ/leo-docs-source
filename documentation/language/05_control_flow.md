---
id: control 
title: Control Flow
sidebar_label: Control Flow
---
[general tags]: # (loop, conditional, return)

### If Statements

If statements are declared as `if {condition} { ... } else if {condition} { ... } else { ... }`.
If statements can be nested.

```leo
    let a: u8 = 1u8;
    
    if a == 1u8 {
        a += 1u8;
    } else if a == 2u8 {
        a += 2u8;
    } else {
        a += 3u8;
    }
```

### Return Statements

Return statements are declared as `return {expression};`.

```leo
    let a: u8 = 1u8;
    
    if a == 1u8 {
        return a + 1u8;
    } else if a == 2u8 {
        return a + 2u8;
    } else {
        return a + 3u8;
    }
```


### For Loops

For loops are declared as `for {variable: type} in {lower bound}..{upper bound}`. The loop bounds must be integer constants of the same type. Furthermore, the lower bound must be
less than the upper bound. Nested loops are supported.


```leo
  let count: u32 = 0u32;

  for i: u32 in 0u32..5u32 {
    count += 1u32;
  }

  return count; // returns 5u32
```
