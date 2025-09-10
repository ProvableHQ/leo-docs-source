---
id: cli_account
title: leo account
sidebar_label: Account
---
[general tags]: # (cli, leo_account, account, sign, verify, signature, private_key, view_key, address)

The `leo account` command is used to create and manage Aleo accounts, as well as sign and verify messages

:::warning
We urge you to exercise caution when managing your private keys. `leo account` cannot be used to recover lost keys.
:::


## Subcommands
* [`new`](#leo-account-new) - Generates a new Aleo account.
* [`import`](#leo-account-import) - Derive and Aleo account from a private key.
* [`sign`](#leo-account-sign) - Sign a message using your Aleo private key.
* [`verify`](#leo-account-verify) - Verify a message and signature from an Aleo address.

&nbsp;

---


## `leo account new`

[Back to Top](#subcommands)

Use this command to generate a private key, view key, and address for a new Aleo account.


The output should look like this:
```bash title="console output:"
  Private Key  APrivateKey1zkp...
     View Key  AViewKey1...
      Address  aleo1...
```


### Flags:
#### `--seed <SEED>`
#### `-s <SEED>`
Specifies a particular numeric value to use as the seed for the random number generator (RNG)



#### `--write `
#### `-w`
Writes the generated private key to a `.env` file in the current working directory (`./`)



#### `--discreet`
Print sensitive information (such as private key) discreetly to an alternate screen


&nbsp;

---



## `leo account import`

[Back to Top](#subcommands)

Use this command to derive the view key and address for an Aleo account from a private key.


To import an existing Aleo account, run the following command:
```bash
leo account import <PRIVATE_KEY>
```
where `<PRIVATE_KEY>` is your private key.

### Flags:
#### `--write `
#### `-w`
Writes the generated private key to a `.env` file in the current working directory (`./`)



#### `--discreet`
Print sensitive information (such as private key) discreetly to an alternate screen

&nbsp;

---

## `leo account sign`

[Back to Top](#subcommands)

Use this command to sign a message using your Aleo private key.

Assuming either the current working directory is a Leo project or the `$PRIVATE_KEY` environment variable has been set, you can sign a message using the following command:
```bash
leo account sign --message <MESSAGE>
```

### Flags:
#### `--message <MESSAGE>`
#### `-m <MESSAGE>`

:::info
This flag is required!
:::

Specifies the message to be signed.

---

#### `--private-key <PRIVATE_KEY>`
Explicity specifies the private key to sign the message with.  Overrides any private key in `.env` file or `$PRIVATE_KEY` environment variable.


#### `--private-key-file <PRIVATE_KEY_FILE>`
Alternative way to explicity specifies the private key by reading from a text file at path `<PRIVATE_KEY_FILE>` Overrides any private key in `.env` file or `$PRIVATE_KEY` environment variable.


#### `--raw `
#### `-r`
Parses the message as bytes instead of Aleo literals.

&nbsp;

---

## `leo account verify`

[Back to Top](#subcommands)

Use this command to verify a message and signature from an Aleo address.

To verify a message, run the following command
```bash
leo account verify --address <ADDRESS> --signature <SIGNATURE> --message <MESSAGE>
```
where `<MESSAGE>` is the message, `<SIGNATURE>` is the signature of that message, and `<ADDRESS>` is the address of the account that generated the signature.

### Flags:
#### `--address <ADDRESS>`
#### `-a <ADDRESS>`

:::info
This flag is required!
:::

Specifies the address of the account that generated the signature.


#### `--signature <SIGNATURE>`
#### `-s <SIGNATURE>`

:::info
This flag is required!
:::

Specifies the signature of the message.


#### `--message <MESSAGE>`
#### `-m <MESSAGE>`

:::info
This flag is required!
:::

Specifies the message that was signed.


#### `--raw `
#### `-r`
Parses the message as bytes instead of Aleo literals.

