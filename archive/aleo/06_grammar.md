---
id: grammar
title: Aleo Instructions Grammar
sidebar_label: Grammar
---

This chapter contains a high-level grammar of Aleo instructions.
A more detailed ABNF grammar can be found [here](https://github.com/ProvableHQ/grammars).

```
program = *import
          "program" program-id ";"
          1*( mapping / struct / record / closure / function )
import = "import" program-id ";"
mapping = "mapping" identifier ":"
          mapping-key
          mapping-value
mapping-key = "key" identifier "as" finalize-type ";"
mapping-value = "value" identifier "as" finalize-type ";"
struct = "struct" identifier ":" 1*tuple
tuple = identifier "as" plaintext-type ";"
record = "record" identifier ":"
         "owner" "as" ( "address.public" / "address.private" ) ";"
         *entry
entry = identifier "as" entry-type ";"
closure = "closure" identifier ":"
          *closure-input
          1*instruction
          *closure-output
closure-input = "input" register "as" register-type ";"
closure-output = "output" operand "as" register-type ";"
function = "function" identifier ":"
           *function-input
           *instruction
           *function-output
           [ finalize-command finalize ]
function-input = "input" register "as" value-type ";"
function-output = "output" operand "as" value-type ";"
finalize = "finalize" identifier ":"
           *finalize-input
           1*command
           *finalize-output
finalize-input = "input" register "as" finalize-type ";"
finalize-output = "output" operand "as" finalize-type ";"
finalize-command = "finalize" *( operand ) ";"
command = contains
        / get
        / get-or-use
        / set
        / remove
        / random
        / position
        / branch
        / instruction
contains = "contains" identifier "[" operand "]" "into" register ";"
get = "get" identifier "[" operand "]" "into" register ";"
get-or-use = "get.or_use" identifier "[" operand "]" operand "into" register ";"
set = "set" operand "into" identifier "[" operand "]" ";"
remove = "remove" identifier "[" operand "]" ";"
random  = "rand.chacha" *2( operand ) "into" register "as" literal-type ";"
label = identifier
position = "position" label ";"
branch-op = "branch.eq" / "branch.neq"
branch = branch-op operand operand label ";"
instruction = ( unary
              / binary
              / ternary
              / is
              / assert
              / commit
              / hash
              / cast
              / call )
              ";"
unary = unary-op ( operand ) "into" register
unary-op = "abs" / "abs.w"
         / "double"
         / "inv"
         / "neg"
         / "not"
         / "square"
         / "sqrt"
binary = binary-op 2( operand ) "into" register
binary-op = "add" / "add.w"
          / "sub" / "sub.w"
          / "mul" / "mul.w"
          / "div" / "div.w"
          / "rem" / "rem.w"
          / "mod"
          / "pow" / "pow.w"
          / "shl" / "shl.w"
          / "shr" / "shr.w"
          / "and"
          / "or"
          / "xor"
          / "nand"
          / "nor"
          / "gt"
          / "gte"
          / "lt"
          / "lte"
ternary = ternary-op 3( operand ) "into" register
ternary-op = "ternary"
is = is-op operand operand "into" register
is-op = "is.eq" / "is.neq"
assert = assert-op operand operand
assert-op = "assert.eq" / "assert.neq"
commit = commit-op operand operand "into" register "as" ( address-type / field-type / group-type )
commit-op = "commit.bhp" ( "256" / "512" / "768" / "1024" )
          / "commit.ped" ( "64" / "128" )
hash = hash-op operand "into" register "as" ( arithmetic-type / address-type )
hash-op = "hash.bhp" ( "256" / "512" / "768" / "1024" )
        / "hash.ped" ( "64" / "128" )
        / "hash.psd" ( "2" / "4" / "8" )
        / "hash_many.psd" ( "2" / "4" / "8" )
cast = cast-op 1*( operand ) "into" register "as" cast-destination
cast-op = "cast"
cast-destination = register-type / "group.x" / "group.y"
call = "call" ( locator / identifier ) *( operand ) "into" 1*( register )
operand = literal
        / "group::GEN"
        / register-access
        / program-id
        / "self.caller"
        / "self.signer"
        / "block.height"
literal = arithmetic-literal
        / address-literal
        / boolean-literal
arithmetic-literal = integer-literal
                   / field-literal
                   / group-literal
                   / scalar-literal
integer-literal = signed-literal / unsigned-literal
signed-literal = [ "-" ] 1*( digit *"_" ) signed-type
unsigned-literal = [ "-" ] 1*( digit *"_" ) unsigned-type
field-literal = [ "-" ] 1*( digit *"_" ) field-type
group-literal = [ "-" ] 1*( digit *"_" ) group-type
scalar-literal = [ "-" ] 1*( digit *"_" ) scalar-type
address-literal = "aleo1" 1*( address-char *"_" )
address-char = "0" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9"
             / "a" / "c" / "d" / "e" / "f" / "g" / "h" / "j"
             / "k" / "l" / "m" / "n" / "p" / "q" / "r" / "s"
             / "t" / "u" / "v" / "w" / "x" / "y" / "z"
boolean-literal = "true" / "false"
register = "r" 1*digit
register-access = register *( "." identifier )
unsigned-type = "u8" / "u16" / "u32" / "u64" / "u128"
signed-type = "i8" / "i16" / "i32" / "i64" / "i128"
integer-type = unsigned-type / signed-type
field-type = "field"
group-type = "group"
scalar-type = "scalar"
arithmetic-type = integer-type / field-type / group-type / scalar-type
address-type = "address"
boolean-type = "boolean"
literal-type = arithmetic-type / address-type / boolean-type / string-type
plaintext-type = literal-type / identifier
value-type = plaintext-type ".constant"
           / plaintext-type ".public"
           / plaintext-type ".private"
           / identifier ".record"
           / locator ".record"
finalize-type = plaintext-type ".public"
              / identifier ".record"
              / locator ".record"
entry-type = plaintext-type ( ".constant" / ".public" / ".private" )
register-type = locator ".record"
              / identifier ".record"
              / plaintext-type
digit = "0"-"9"
uppercase-letter = "A"-"Z"
lowercase-letter = "a"-"z"
letter = uppercase-letter / lowercase-letter
identifier = letter *( letter / digit / "_" )
lowercase-identifier = lowercase-letter *( lowercase-letter / digit / "_" )
program-name = lowercase-identifier
program-domain = lowercase-identifier
program-id = program-name "." program-domain
locator = program-id "/" identifier
```
