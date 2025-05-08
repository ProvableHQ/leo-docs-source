---
id: execute
title: Run Your Programs
sidebar_label: Execute 
---


The `leo execute` command is used to call functions from deployed programs.  Under the hood, this commands downloads the program code from the specified network, synthesizes the program circuit, executes the function logic, generates proving verifying keys along with a zero-knowledge proof, and then submits a transaction object to the network.

The full syntax for the `leo execute` command is:
```bash
leo execute <method_name> [input_1] [input_2] ... [input_n] --program <program_name>.aleo --broadcast
```
As with the `leo deploy` command, you must specify the network, Private Key, and node endpoint parameters using either a `.env` file or environment variables.  

Developers have the option to inspect a transaction object before it is broadcast to the network using the `--dry-run` flag in lieu of the `--broadcast` flag.  Invoking this flag will print the transaction output to `stdout`.

```bash
leo execute <methbod_name> [input_1] [input_2] ... [input_n] --program <program_name>.aleo --dry-run
```

```bash
{"type":"execute","id":"at1lmse7pr4h0n3lv9mvn262up7v0nhyulm9ygn3kwpxym9008deygs4elkl9","execution":{"transitions":[{"id":"au1w7n6ahdjmvj2nz9nu6zhzq35v93nguwhv3tzqeywhntrxytmngpqfhg5ss","program":"example_program.aleo","function":"main","inputs":[{"type":"public","id":"692634202775738788892756541276479310312882753688899140142743446375832543859field","value":"2u32"},{"type":"private","id":"8385995819199163615771507865968584985436639267963028732797694416477829379079field","value":"ciphertext1qyqru0tr39h8kzquxjr7xfrud98m47mawmnvj5xlfnypzyvhepf7xzsawdtkl"}],"outputs":[{"type":"private","id":"3124422272714190443592630115423959519403736569564003413462129091659624867451field","value":"ciphertext1qyq08lr8g88k20xp6k9fr34l07fjaaddmdpnfpmdlxwz3mr6xrfjzqqf5nfna"}],"tpk":"4367604861311133041917107666803824204733856429271239279390409648472266500776group","tcm":"7471894725277672051255805412199677748935089735919789147124444004458893515904field","scm":"1788545946703859533314929059553927884362701746091184516349398904143684696559field"}],"global_state_root":"sr1pezvtfwkhq742lzngn8sc89t5s0ez4u64ux73dw2c7kvk5tjnqxqcz79km","proof":"proof1qyqsqqqqqqqqqqqpqqqqqqqqqqqfzm7qyhzn32nrg30d85qg7725jy8rnrhx5sp2xgkkx3jjzp3rttmg2lra09573rttj5pp0wmpx0cpq8xvl6s6zl3p6fn9hdqsyg5x2rz234l5g9knp0x6faex4dqxcpfs28rgy9rcl9qj8963g333f4egfqvqay0anlee9jwl9276cnh0kuhem3cafhacdyvpa74y28kkeuudxgufxg5qfd5h2sy520kjl8u8qsqh8e9rxlwp6v9059z8zzz26ytdcd7zdsl4l4xp2vn6aaanr0695knljv369h0xvphmxns7m5k7tpuqhptx8z6nvemvt7muchne4rx2rfmpv0pedxf8ms33pxte0hnlpfpdvtjnckcpejzwqjez8y70cyasppxggphh6fqehee9u58lklg3a750gxtjxxfq0z0g2cx0zmkf30tkwwqhlke7vljyswks4lynp46lqqudm45k79s27wgjluza8sc8vq8ld9evrqmj7mz4s9wm9qts4qzajl2f3q6t2f5mv55ctfp0svj3sqq8l6ujt79v93smpkcry9uxd87tdkypz8n2v0p35zrtkjjwh697gu6lxj5fpmu2x94m5gth8hnfy6qsvr9mn5adppml6wyzur60cuegjdnduhytqptpd2630aq5chht4h93flg0yzfq3har5dtnqcgxc0up09p9nt045c5umvlzuxkgzupsfdkarg0rzqdyy6n93fyeh25zru9nh96n04hm0aajjt0tmkspfgza7zmf8tfkk88gcf7wse3f346x5qeh4s2fjsf8zcj7zkcqnmq3nwevq68rkp8n3jae8d2r8qmp5c8dp6rzhsnkzx2lhxk4jhtwcnevcuqjv5l63ujac63p34s0lpqq7cxqplptw40z23nzaag0cz6yqdceguu352cc83spfds8esu92t2mukc2l3428sy8lt99fat8hmx7dej5kacuvt5rqz9y4hpjf72ll4ppyyqjvt8lya4vqqn48906ra7rtnh6yljc20dajs44c4m8z6flcjz8srxdpxce8szs7u2ty6gagcjfujzcdda43rk0k8jcta59jrcrxygtztps86cddvjg6rmmcasg30mmgx9d808clkrm0kqranpnr3djrv4qvdhqxn74w3050l7q4j3guv7cmgyt8gefg8zp58nc8zvj67vhqccdqvqqqqqqqqqqpj990s7fz7fpur5rs6wxupkx74kf9n83eslsqj5l0sv6cvj6q82gphgq8xgzh2gv50yzckgxzayxsyqfjllqh2jdg0y6dxxtpvw7dezaum7yscyahyxre33d50jput3jwz24zwpxfudrwn36zrqvy5xg2fcqqyrvaynt8rrcqzwt3lfk92xdln9yh3w9l3r7hnnrulpnvjtzau7sdpl6822ced9ee240am4dwq75vu8l29f8jky86m8epuqe4wge63ue79txmq4vhlzwk758d066xmz3qyqqj3xtvg"}}
```