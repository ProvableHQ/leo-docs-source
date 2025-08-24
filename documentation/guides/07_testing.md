---
id: test 
title: Testing, Testing, 123
sidebar_label: Testing
---

Once deployed, an application lives on the ledger forever. Consequently, it's important to consider every edge case and rigorously test your code. There are number of tools and techniques you can use. 

- [**Unit and Integration Testing**](#test-framework) - Validate Leo program logic through test cases.

- [**Running a Devnet**](./03_devnet.md) - Deploy and execute on a local devnet.

- [**Deploying/Executing on Testnet**](./03_devnet.md) - Deploy and execute on the Aleo Testnet.

- [**Other Tools**](./04_other.md) - Tools and methodologies developed by the open-source Aleo community.


## Unit and Integration Testing
The Leo testing framework enables developers to validate their Leo program logic by writing unit and integration tests. Tests are written in Leo and are located in a `tests/` subdirectory of the main Leo project directory.

```bash
example_program
├── build
│   ├── imports
│   │   └── test_example_program.aleo
│   ├── main.aleo
│   └── program.json
├── outputs
├── src
│   └── main.leo
├── tests
│   └── test_example_program.leo
└── program.json
```
The test file is a Leo program that imports the program in `main.leo`.  The test functions will all be annotated with `@test` above the function declaration. 

This tutorial will use an example program which can be found in the [example's repository](https://github.com/ProvableHQ/leo-examples/tree/main/example_with_test).  

:::info
Developers can add multiple `leo` files to the test directory but must ensure that the name of the test file matches the program name within that test file.  For example, if the name of the test file is `test_example_program.leo`, the program name in that file must be `test_example_program.aleo`.
:::


### Testing `transition` Functions

The `example_program.leo` program contains a transition function which returns the sum of two `u32` inputs.

```Leo
transition simple_addition(public a: u32, b: u32) -> u32 {
    let c: u32 = a + b;
    return c;
}
```

The `test_example_program.leo` contains two tests to ensure that the transition logic returns a correct output and fails when the output does not match the sum of the input values.
```Leo
@test
transition test_simple_addition() {
    let result: u32 = example_program.aleo/simple_addition(2u32, 3u32);
    assert_eq(result, 5u32);
}
```

The `@should_fail` annotation should be added after the `@test` annotation for tests that are expected to fail.
```Leo
@test
@should_fail
transition test_simple_addition_fail() {
    let result: u32 = example_program.aleo/simple_addition(2u32, 3u32);
    assert_eq(result, 3u32);
}
```

### Testing Leo Types

Developers can test that record and struct fields match their expected values.  In `example_program.leo`, a record is minted transition function shown here:

```Leo
record Example {
    owner: address,
    x: field,
}

transition mint_record(x: field) -> Example {
    return Example {
        owner: self.signer,
        x,
    };
}
```

The corresponding test in `test_example_program.leo` checks that the Record field contains the correct value:

```Leo
@test
transition test_record_maker() {
    let r: example_program.aleo/Example = example_program.aleo/mint_record(0field);
    assert_eq(r.x, 0field);
}
```

:::info
Each test file is required to have at least one `transition` function.
:::


### Modeling Onchain State
While the testing framework cannot access on-chain state from either `testnet` or `mainnet`, developers can simulate on-chain state in `script`s. A script is interpreted Leo code in which developers are able to await `Future`s and update mappings using interpreted tests. When using interpreted tests, the `transition` or `function` keyword is replaced with the `script` keyword.

```Leo
@test
script test_async() {
    const VAL: field = 12field;
    let fut: Future = example_program.aleo/set_mapping(VAL);
    fut.await();
    assert_eq(Mapping::get(example_program.aleo/map, 0field), VAL);

    let rand_val: field = ChaCha::rand_field();
    Mapping::set(example_program.aleo/map, VAL, rand_val);
    let value: field = Mapping::get(example_program.aleo/map, VAL);
    assert_eq(value, rand_val);
}
```

:::info
External `transition`s -- `async` or not -- may be called from test `transition`s or scripts, but external `async function`s may only be called directly from scripts.
:::


### Running Tests
Invoking the `leo test` command will run all of the compiled and interpreted tests. Developer may optionally select an individual tests by supplying a a test function name or a string that is contained within a test function name.  For instance, to run the test for `test_async`, developers would use the following command:
```bash
leo test test_async
```
Either of the following commands will run both of the addition function tests:
```bash
leo test simple
```
or
```bash
leo test addition
```

See the `leo test` CLI documentation [here](./../03_cli.md#leo-test)


## Running a Devnet

A local devnet can be a heavyweight but reliable way to test your application on Aleo.

### Setup

The Leo CLI provides a helpful command to help startup a local devnet:
```bash
leo devnet --snarkos <SNARKOS>
```
The `<SNARKOS>` is the path to an installed binary of [**snarkOS**](https://github.com/ProvableHQ/snarkOS), the decentralized operating system that forms the backbone of the Aleo network.  

If you don't have snarkOS installed, you can pass the `--install` flag and the CLI will automatically download, compile, and store the binary at the path specified by `<SNARKOS>`
```bash
leo devnet --snarkos <SNARKOS> --install
```
:::info

Windows users will need to perform some additional steps in order for snarkOS to install properly:
1. Upon initially installing Rust, you should have been automatically been prompted to install Visual Studio with the MSVC 2022 C++ build tools. 
2. Open the Visual Studio Installer and install the C++ Clang Compiler for Windows and either the Windows 10 SDK or Windows 11 SDK (depending on your OS). Make a note of the installation path of the installed tools (should be of the form `{PATH}\Microsoft Visual Studio\2022\BuildTools`)
3. Within the aforementioned build tools directory, you should find the location of a file called `libclang.dll`.  For `x86`-based systems, this should be in the `VC\Tools\Llvm\bin` subdirectory.  For `x64`-based systems, this should be in the `VC\Tools\Llvm\x64\bin` subdirectory.
4. Once you have the full path of `libclang.dll`, create the `LIBCLANG_PATH` environment variable for your system and set it to this path.
5. snarkOS should now compile and run properly.

:::




The `tmux` command will allow you to toggle between nodes in your local devnet.  You can enable this by passing the `--tmux` flag upon startup:
```bash
leo devnet --snarkos <SNARKOS> --tmux
```
:::info
This feature is only available on Unix-based systems.
::

You'll need to install the `tmux` package first:

<Tabs defaultValue="cargo"
values={[
  { label: 'MacOS', value: 'macos' },
  { label: 'Ubuntu', value: 'ubuntu' },
]}>
<TabItem value="macos">

To install `tmux` on macOS, you can use the Homebrew package manager. If you haven't installed Homebrew yet, you can find instructions at their [website](https://brew.sh/). Once Homebrew is installed, run:
```bash
brew install tmux
```
</TabItem>
<TabItem value="prebuilt">
On Ubuntu and other Debian-based systems, you can use the apt package manager:

```bash
sudo apt update
sudo apt install tmux
```
</TabItem>
</Tabs>

Here are some useful (default) commands in `tmux`:
```bash
# To toggle to the next node in a local devnet
Ctrl+b n 
# To toggle to the previous node in a local devnet
Ctrl+b p 
# To scroll easily, press q to quit
Ctrl+b q
# To select a node in a local devnet
Ctrl+b w 
# To select a node manually in a local devnet
Ctrl+b :select-window -t {NODE_ID}
# To stop a local devnet
Ctrl+b :kill-session
```

See the full `leo devnet` CLI documentation [here](./../03_cli.md#leo-devnet)



### Usage

Each time you stop and restart the chain, the prior state and history will be saved.  You can clear any prior history by passing the `--clear-storage` flag:
```bash
leo devnet --snarkos <SNARKOS> --clear-storage
```
Clearing the ledger history may be useful if you wish to redeploy your program without changing the name.  However, this will erase all transaction history and start a new instance of the Aleo blockchain from genesis.



### Deploying and Executing

When deploying or executing programs on a local devnet, make sure that endpoint is set to `http://localhost:3030` rather than any external API endpoints.  You can do this either by manually setting the `ENDPOINT` environment variable, by passing the `--endpoint {ENDPOINT}` flag in the CLI, or by setting the `ENDPOINT` in a `.env` file within the root directory of your Leo project.

snarkOS automatically intializes four development accounts funded with Aleo credits that can be used for testing purposes.
```bash
# Account 0
APrivateKey1zkp8CZNn3yeCseEtxuVPbDCwSyhGW6yZKUYKfgXmcpoGPWH
AViewKey1mSnpFFC8Mj4fXbK5YiWgZ3mjiV8CxA79bYNa8ymUpTrw
aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px

# Account 1
APrivateKey1zkp2RWGDcde3efb89rjhME1VYA8QMxcxep5DShNBR6n8Yjh
AViewKey1pTzjTxeAYuDpACpz2k72xQoVXvfY4bJHrjeAQp6Ywe5g
aleo1s3ws5tra87fjycnjrwsjcrnw2qxr8jfqqdugnf0xzqqw29q9m5pqem2u4t

# Account 2
APrivateKey1zkp2GUmKbVsuc1NSj28pa1WTQuZaK5f1DQJAT6vPcHyWokG
AViewKey1u2X98p6HDbsv36ZQRL3RgxgiqYFr4dFzciMiZCB3MY7A
aleo1ashyu96tjwe63u0gtnnv8z5lhapdu4l5pjsl2kha7fv7hvz2eqxs5dz0rg

# Account 3
APrivateKey1zkpBjpEgLo4arVUkQmcLdKQMiAKGaHAQVVwmF8HQby8vdYs
AViewKey1iKKSsdnatHcm27goNC7SJxhqQrma1zkq91dfwBdxiADq
aleo12ux3gdauck0v60westgcpqj7v8rrcr3v346e4jtq04q7kkt22czsh808v2
```
Once your private key and endpoint have been correctly set, deploying and executing largely function the same as they would on Testnet or Mainnet.  For more details on either of those processes, check out the [**Deploying**](./03_deploying.md) and [**Executing**](./04_executing.md) guides.



### Querying Transaction Status

You can check your transactions by using the following API endpoint:

```bash
GET http://localhost:3030/testnet/transaction/{TRANSACTION_ID}
```

or by using `leo query` from the CLI:

```bash
leo query transaction {TRANSACTION_ID}
```

The transaction API endpoint is instructive in verifying whether a transaction succeeded or failed.  Since both successful and failed transaction execute a fee transaction, if only the fee transaction appears, that is a clear indication that the transaction has failed.  Note that on the Testnet and on Mainnet, failed transactions still require a fee since the network is performing a computation.

A full list of API endpoints is available [here](https://developer.aleo.org/references/apis/public_api/)

### Record Scanning

You can use the CLI from your installed snarkOS binary to view your records.  First, navigate to the directory where you installed the binary.  The use the following command syntax:
```bash
./snarkos developer scan --endpoint http://localhost:3030 --private-key {YOUR_PRIVATE_KEY} --start <block_number> --network 1
```

Setting `block_number` to `0` will list all of the records created starting from the genesis block, including your test credit records. 

```bash title="sample output:"
⚠️  Attention - Scanning the entire chain. This may take a while...

Scanning 3 blocks for records (100% complete)...   

[
  "{  owner: aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px.private,  microcredits: 23437500000000u64.private,  _nonce: 3666670146276262240199958044811329632452609778779651964870759629195088099828group.public}",
  "{  owner: aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px.private,  microcredits: 23437500000000u64.private,  _nonce: 4536868268814456227312360347031739423312689137706933033938812386306238998060group.public}",
  "{  owner: aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px.private,  microcredits: 23437500000000u64.private,  _nonce: 205967862164714901379497326815256981526025583494109091059194305832749867953group.public}",
  "{  owner: aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px.private,  microcredits: 23437500000000u64.private,  _nonce: 4424806931746512507605174575961455750579179367541686805196254590136284583805group.public}"
]
```

Setting `block_number` to `1` or higher will exclude the above credit records from the scan.



## Deploying/Executing on Testnet
To deploy and execute on Testnet, you'll need to set your endpoint back to one of the public facing options. Additionally, you'll need to obtain Testnet credits from one of the faucets below:

### Faucets

<!--TODO: Update this information once the new faucet becomes public.-->

At some point you'll need testnet credits. There are a few community-supported faucets to choose from:
- [**Puzzle**](https://dev.puzzle.online/faucet) - 15 credits / 4 hours

- [**Demox**](https://discord.com/channels/913160862670397510/1202322326230937640/1203135682873266207) - 10 credits / 12 hours

The faucets are periodically refreshed.


## Other Tools

The Aleo community has developed some neat tools to aid in testing.

- [**doko.js**](https://github.com/venture23-aleo/doko-js)

