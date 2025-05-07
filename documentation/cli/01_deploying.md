---
id: deploy
title: Deploy Your Programs 
sidebar_label: Deploy
---

The `leo deploy` command is used for deploying Leo program to a local devnet, Testnet, or Mainnet.

The following parameters need to be specified in either a `.env` file or as environment variables: the target network, the Private Key, and a node API endpoint.

An `.env` file should be formatted as follows:
```bash
 NETWORK=testnet
PRIVATE_KEY=APrivateKey1z...GPWH
ENDPOINT=https://api.explorer.provable.com/v1
```
From the root of the Leo program directory, run the following command:
```bash
leo deploy
```

Alternatively, the command syntax accomodates enviroment variables:
```bash
leo deploy --endpoint "{ENDPOINT} --private-key "{$PRIVATE_KEY}"
```

If a Leo program includes local dependencies, the `--recursive flag` will automatically deploy all dependency programs in order.
The following example program has one local dependency.

```bash
example_program
├── local_dependency
│   ├── src
│   │   └── main.leo
│   ├── .env
│   └── program.json
├── src
│   └── main.leo
├── .env
└── program.json
```
From the root of the `example_program` directory, run the following command:
```bash
leo deploy --recursive
```

This will generate a deploy prompt for `local_dependency.aleo`.

```bash
? Do you want to submit deployment of program `local_dependency.aleo` to network testnet via endpoint http://localhost:3030 using address aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px? (y/n) ›
```

After deploying the dependency program, a second deploy prompt will appear for the main program.

```bash
? Do you want to submit deployment of program `example_program.aleo` to network testnet via endpoint http://localhost:3030 using address aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px? (y/n) ›
```

When deploying to either the Testnet or Mainnet uisng public credits, there is a 12 second delay between programs deployments.  This ensures that programs are deployed in separate blocks and prevents the network from attempting to deploy multiple programs to the same blocks if sufficient funds are not available for the final deployment.


