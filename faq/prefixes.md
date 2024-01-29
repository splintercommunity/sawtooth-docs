---
# Copyright (c) 2024 Bitwise IO, Inc.
# Copyright (c) 2018, Intel Corporation.
# Licensed under Creative Commons Attribution 4.0 International License
# <https://creativecommons.org/licenses/by/4.0/>
---

# FAQ Appendix: Transaction Family Prefixes

This is an unofficial list of some Transaction Family (TF) prefixes.
There is no central registry.

Sawtooth addresses are 70 hex characters. The prefix is either the first
6 characters of the SHA-512 hash of the namespace, or, for some base
namespaces, a \"hex word\". The Sawtooth Validator registry is an
outlier. It uses the SHA-256 hash (not SHA-512) and hashes
\"validator_registry\" (not \"sawtooth_validator_registry\"). The
remainder of the address is TF-specific and defined for each TF. Listing
of a TF does not imply endorsement.

All data payloads are encoded in base64 after serializing. Sawtooth
headers are serialized with Protobuf.

For base TF specifications, see [Transaction Family
Specifications]({% link docs/1.2/transaction_family_specifications/index.md%})

[//]: # (TODO: Apply styling to table)

|          TRANSACTION FAMILY NAME         | SERIAL- IZATION |           PREFIX          |                                                                                                 PREFIX ENCODING                                                                                                 |
|:----------------------------------------:|:---------------:|:-------------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| settings                                 | Protobuf        | 000000                    | Validator settings.  Only required TF                                                                                                                                                                           |
| identity                                 | Protobuf        | 00001d                    | Validator Identity for TP/Validator keys                                                                                                                                                                        |
| sawtooth _validator _registry            | Protobuf        | 6a4372                    | PoET Validator Registry. Used by PoET consensus to track other validators. See note above about hash prefix .                                                                                                   |
| blockinfo                                | Protobuf        | 00b10c                    | Validator Block Info.  Used for SETH   00b10c00 metadata namespace info about other namespaces   00b10c01 block info namespace historic block info   00b10c0100....00<block # in hex> info on block at block #  |
| sabre                                    | Protobuf        | 00ec00   00ec01   00ec02  | WebAssembly VM: NamespaceRegistry   Wasm: ContractRegistry   Wasm: Contracts                                                                                                                                    |                                                                   |
| SOME EXAMPLE TFs                         |                 |                           |                                                                                                                                                                                                                 |
| battleship                               | JSON            | 6e10df                    | Battleship example game                                                                                                                                                                                         |
| intkey                                   | CBOR            | 1cf126                    | Integer Key. Full production example                                                                                                                                                                            |
| smallbank                                | Protobuf        | 332514                    | Small Bank example app                                                                                                                                                                                          |
| xo                                       | CSV-UTF8        | 5b7349                    | Tic-tac-toe example game                                                                                                                                                                                        |                                                                 |                                                                                                                                                                                          |
