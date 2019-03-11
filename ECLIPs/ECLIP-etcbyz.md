### ECLIP-?: Support for ETH Byzantium EVM and Protocol Upgrades

    ECLIP: undecided
    Title: Support for ETH Byzantium EVM and Protocol Upgrades
    Status: Draft
    Type: Standards Track
    Author: Isaac Ardis <isaac.a@etclabs.org>
    Created: 2019-02-11

### Abstract

Add support for a subset of protocol-impacting changes introduced in the Ethereum Foundation (ETH) network via the _Byzantium_ hardfork. The proposed changes include:

- Byzantium EVM opcodes and precompiled contracts, namely opcodes `REVERT` (EIP 206/140), `RETURNDATASIZE` (EIP 211), `RETURNDATACOPY` (EIP 211), and `STATICCALL` (EIP 214/116); and precompiled contracts for modular exponentiation, elliptic curve addition, scalar multiplication, and pairing (EIPs 198, 212/197, 213/196)
- Replacing the intermediate state root field in transaction receipts with the contract return status (EIP 658).

In addition to introducing the above Byzantium-derived changes, it is also proposed to simultaneously introduce support for EVM versioning, as specified in:

- [EIP1707: Use Version Byte Prefix for Contract Account Versioning](https://github.com/ethereum/EIPs/pull/1707)

This document proposes block `X,XXX,XXX` as the upcoming block height at which to implement these changes in the network, placing the expected date of protocol hardfork date on _XXXX-XX-XX_.

For more information on the opcodes and their respective EIPs and implementations, please see the __History__ section of this document.

### Motivation

To enhance the EVM's capabilities by adding 5 opcodes and 4 precompiled contracts, all of which have been in use on the ETH network since 2017-10-16. Adoption of the "receipt status" feature provides a helpful method for Dapp developers to access the successful or failed state of a contract. This would (re)establish a greater level of interoperability between Foundation and Classic Ethereum Virtual Machines ("EVM"s), and make a wider array of tooling available for the ETC network (eg. Solidity version, several contract debugging tools).

This protocol specification notably omits the scheduled features of the anticipated _Constantinople_ fork, which would be expected to include various further EVM upgrades. The reasoning for this omission hinges on a hedge toward battle-testing of those changes in light of multiple delays of that fork ([here](https://medium.com/ethereum-cat-herders/a-post-mortem-report-the-constantinople-ethereum-hard-fork-postponement-dd780d7ae63d), a postmortem of the latest delay)  due to security and implementation discrepencies.

### Specification

As per associated EIPs's specifications and implementations, with feature-readiness determined by interoperability (implementation) of smart contracts utilizing the introduced opcode and precompiled contracts (implementation before technical specification in case of discrepency). Technical specifications for each EIP can be found at those documents respectively.

### Rationale

__Interoperability__: establishing and maintaining interoperable behavior between Ethereum clients is important for developers and end-user adoption, yielding benefits for all participating chains (eg. ETH and ETC).

__On Immutability__: Introducing new opcodes in the VM has the potential to change behavior of existing contracts; in the case where previously an arbitrary invalid bytecode series (yielding _invalid opcode_) would now be assigned a meaning, and thus could generate or return a value other than _invalid_. 

Adding opcodes and precompiled contracts to the EVM increases its functionality at the potential cost of sacrificed ideals of "Immutability." The above referenced EIP1707 specifies an introduction of contract account versioning that would enable a guarantee that historical versions of an EVM would be always be available and configured per contracts, enabling eternally reproducable contract behavior.

### Implementation

Adoption of the content of this ECLIP requires a hard fork, and herein that adoption is proposed to be scheduled for block X,XXX,XXX roughly estimated to arrive _XXXX-XX-XX_.

The `ethereumproject/go-ethereum` client implemented an API-only (non-consensus impacting) partial adoption of EIP-658 receipt status code change via the [v5.5.0](https://github.com/ethereumproject/go-ethereum/releases/tag/v5.5.0) release, and this proposal would augment that change to extend through to the RLP encoding (and thus modify consensus protocol).

ETC Labs Core advocates and supports the adoption of [ethoxy/multi-geth](https://github.com/ethoxy/multi-geth) as a fully-featured and maintenance-scheduled client for this fork and beyond.

### History

##### Byzantium changes

These changes were introduced in the `ethereum/go-ethereum` client via the [1.7 "Megara" release](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.0).

These changes are catalogued via the EIP process in the following:

- EIP-658: Transaction receipts embedding status code instead of intermediate state root field - https://github.com/ethereum/EIPs/pull/658/files

- EIP-140: `REVERT` - https://github.com/ethereum/EIPs/pull/206/files

> The `REVERT` instruction provides a way to stop execution and revert state changes, without consuming all provided gas and with the ability to return a reason.


- EIP-211: `RETURNDATASIZE`, `RETURNDATACOPY` - https://github.com/ethereum/EIPs/pull/211/files

> A mechanism to allow returning arbitrary-length data inside the EVM has been requested for quite a while now. Existing proposals always had very intricate problems associated with charging gas. This proposal solves the same problem while at the same time, it has a very simple gas charging mechanism and requires minimal changes to the call opcodes. Its workings are very similar to the way calldata is handled already; after a call, return data is kept inside a virtual buffer from which the caller can copy it (or parts thereof) into memory. At the next call, the buffer is overwritten. This mechanism is 100% backwards compatible.


- EIP-214: `STATICCALL` - https://github.com/ethereum/EIPs/pull/214/files

> To increase smart contract security, this proposal adds a new opcode that can be used to call another contract (or itself) while disallowing any modifications to the state during the call (and its subcalls, if present).

- EIP-198: Precompiled contract for modular exponentiation - https://github.com/ethereum/EIPs/pull/198/files

- EIP-212: Precompiled contract for elliptic curve pairing - https://github.com/ethereum/EIPs/pull/212/files

- EIP-213: Precompiled contracts for elliptic curve addition and scalar multiplication - https://github.com/ethereum/EIPs/pull/213/files

- EIP-1707: Use Version Byte Prefix for Contract Account Versioning - https://github.com/ethereum/EIPs/pull/1707
