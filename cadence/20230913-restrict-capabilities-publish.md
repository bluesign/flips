---
status: draft 
flip: 196 
authors: Deniz Mert Edincik (deniz@edincik.com)
updated: 2023-09-13
---

# FLIP 196: Restrict capabilities.publish to account's own capabilities

## Objective

With current controller capabilities, it is possible to re-publish someone else's public capability; as this shift in behavior can cause problems, this FLIP strives to address this issue with minimal disruption.

## Motivation

Before the introduction of new capabilities API, public capabilities were guaranteed to point to storage at the same address. However, with the introduction of this API, it is now possible to obtain another account's public capability and republish it as one's own, while still maintaining the link to the other account's storage. 

A lot of scenarios such as voting and gating using the proof of Non-Fungible Token (NFT) ownership, usually involve checking if an account owns a certain balance or resource by verifying the public path capability. Now, the responsibility of protection falls on the developers, which is an extra burden and holds the potential of introducing errors.

The current suggested method of defence against this issue is always checking for the `address` of the capability or the `owner` of the resource after borrowing. Unfortunately, this is an error-prone approach that developers can easily forget.

## User Benefit

This proposal aims to add certain restrictions in order to decrease developer burden.

## Design Proposal

I suggest adding a restriction to the capability API, permitting only capabilities from the same account address to be published via `capabilities.publish`.

### Drawbacks

There may be existing capabilities that use the new API to access other accounts' storage, however I don't believe this will be a significant issue, given its relative newness.

### Performance Implications

I don't think there will be any performance implications.

### Engineering Impact

There is already a Draft PR implementation (https://github.com/onflow/cadence/pull/2782) 

### Compatibility

As this is an additional restriction, it is backwards compatible with the API. 


