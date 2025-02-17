---
id: overview
title: Overview
sidebar_label: Overview
---


|                     |  Edwards BLS12  |     BLS12-377      |  
|:------------------- |:---------------:|:------------------:|
| Curve Type          | Twisted Edwards | Barreto-Lynn-Scott |
| Scalar Field Size   |    251 bits     |      253 bits      |
| Base Field Size     |    253 bits     |      377 bits      |
| G1 Compressed Size* |    32 bytes     |      48 bytes      |
| G2 Compressed Size* |       N/A       |      96 bytes      |

\* rounded to multiples of 8 bytes.

## Keccak
The sponge construction `Sponge[f, pad, r]` is a function that takes a variable-length input
and produces a fixed-length output (the hash value).
The permutation `f` is a function that takes a fixed-length input and produces a fixed-length output,
defined as `f = Keccak-f[b]`, where `b := 25 * 2^l` is the width of the permutation,
and `l` is the log width of the permutation.
For our case, `l = 6`, thus `b = 1600`.
The padding rule `pad` is a function that takes a variable-length input and produces a fixed-length output.
In Keccak, `pad` is a multi-rate padding, defined as `pad(M) = M || 0x01 || 0x00…0x00 || 0x80`,
where `M` is the input data, and `0x01 || 0x00…0x00 || 0x80` is the padding.
In SHA-3, `pad` is a SHAKE, defined as `pad(M) = M || 0x06 || 0x00…0x00 || 0x80`,
where `M` is the input data, and `0x06 || 0x00…0x00 || 0x80` is the padding.
The bitrate `r` is the number of bits that are absorbed into the sponge state in each iteration
of the absorbing phase.
In addition, the capacity is defined as `c := b - r`.
