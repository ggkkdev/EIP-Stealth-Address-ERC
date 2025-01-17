---
eip: TODO
title: Stealth Meta-Address Registry
description: A registry to map addresses to stealth meta-addresses
author: Toni Wahrstätter (@nerolation), Matt Solomon (@mds1), Ben DiFrancesco (@apbendi), Vitalik Buterin <vitalik.buterin@ethereum.org>
discussions-to: TODO
status: Draft
type: Standards Track
category: ERC
created: 2023-01-24
---

## Abstract

This specification defines a standardized way of storing and retrieving an entity's stealth meta-address, by extending [EIP-5564](./eip-5564.md).

## Motivation

The standardization of non-interactive stealth address generation holds the potential to greatly enhance the privacy capabilities of Ethereum by enabling the recipient of a transfer to remain anonymous when receiving an asset. Making stealth addresses easy to use by providing a shared, standardized registry for make it even easier to non-interactively send and receive assets with stealth addresses.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

This contract defines an `ERC5564Registry` that stores the stealth meta-address for entities. These entities may be identified by an address, ENS name, or other identifier. This MUST be a singleton contract, with one instance per chain.

The contract is specified below. A one byte integer is used to identify the stealth address scheme. This integer is used to differentiate between different stealth address schemes. A mapping from the scheme ID to it's specification is maintained in **{TODO where to maintain it?}**.

```solidity
pragma solidity ^0.8.17;

/// @notice Registry to map an address or other identifier to its stealth meta-address.
contract ERC5564Registry {
  /// @dev Emitted when a registrant updates their stealth meta-address.
  event StealthMetaAddressSet(
    bytes indexed registrant, uint256 indexed scheme, bytes stealthMetaAddress
  );

  /// @notice Maps a registrant's identifier to the scheme to the stealth meta-address.
  /// @dev Registrant may be a 160 bit address or other recipient identifier, such as an ENS name.
  /// @dev Scheme is an integer identifier for the stealth address scheme.
  /// @dev MUST return zero if a registrant has not registered keys for the given inputs.
  mapping(bytes => mapping(uint256 => bytes)) public stealthMetaAddressOf;

  /// @notice Sets the caller's stealth meta-address for the given stealth address scheme.
  /// @param scheme An integer identifier for the stealth address scheme.
  /// @param stealthMetaAddress The stealth meta-address to register.
  function registerKeys(uint256 scheme, bytes memory stealthMetaAddress) external {
    stealthMetaAddressOf[abi.encode(msg.sender)][scheme] = stealthMetaAddress;
  }

  /// @notice Sets the `registrant`s stealth meta-address for the given scheme.
  /// @param registrant Recipient identifier, such as an ENS name.
  /// @param scheme An integer identifier for the stealth address scheme.
  /// @param signature A signature from the `registrant` authorizing the registration.
  /// @param stealthMetaAddress The stealth meta-address to register.
  /// @dev MUST support both EOA signatures and EIP-1271 signatures.
  /// @dev MUST revert if the signature is invalid.
  function registerKeysOnBehalf(
    address registrant,
    uint256 scheme,
    bytes memory signature,
    bytes memory stealthMetaAddress
  ) external {
    // TODO If registrant has no code, spit signature into r, s, and v and call `ecrecover`.
    // TODO If registrant has code, call `isValidSignature` on the registrant.
  }

  /// @notice Sets the `registrant`s stealth meta-address for the given scheme.
  /// @param registrant Recipient identifier, such as an ENS name.
  /// @param scheme An integer identifier for the stealth address scheme.
  /// @param signature A signature from the `registrant` authorizing the registration.
  /// @param stealthMetaAddress The stealth meta-address to register.
  /// @dev MUST support both EOA signatures and EIP-1271 signatures.
  /// @dev MUST revert if the signature is invalid.
  function registerKeysOnBehalf(
    bytes memory registrant,
    uint256 scheme,
    bytes memory signature,
    bytes memory stealthMetaAddress
  ) external {
    // TODO How to best generically support any registrant identifier / name
    // system without e.g. hardcoding support just for ENS?
  }
}
```

Deployment is done using the keyless deployment method commonly known as Nick’s method, {TODO continue describing this and include transaction data, can base it off the format/description used in EIP-1820 and EIP-2470}.

## Rationale

TODO

## Backwards Compatibility

This EIP is fully backward compatible.

## Reference Implementation

You can find an implementation of this standard above.

## Security Considerations

TODO

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
