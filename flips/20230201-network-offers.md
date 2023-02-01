# Network Offers

| Status              | Proposed                                       |
|:--------------------|:-----------------------------------------------|
| **FLIP #**          |                                                |
| **Author(s)**       | Satyam Agrawal (satyam.agrawal@dapperlabs.com) |
| **Sponsor**         | Bjarte ?                                       |
| **Updated**         | 2023-02-01                                     |
| **Feedback period** | 28 days                                        |

## Objective

This FLIP proposes a Cadence contract standard for Network Offers which can be placed 
against NFTs listed for sale and are broadcast as events. Prospective buyers can enjoy 
the broadest possible reach for an offer to be matched with available NFT listings. It 
conversely gives sellers access to the widest breadth of offers being made on network. 
It also builds in utility and management functionality for cleanup and other maintenance.

## Motivation

The current marketplace ecosystem on Flow provides network wide reach for NFT sales
listings through the [NFT Storefront v2 standard](https://github.com/onflow/nft-storefront).
Sellers using the standard can make listings available for resale across multiple
marketplaces with no added effort. Marketplaces can consume listing events from across
the network and include them for sale if the commissions received meet the minimum
required. However, the equivalent ecosystem mechanics are not provided to prospective
buyers interested in NFTs for sale.

This proposal introduces a NFT offer creation mechanism used by prospective buyers,
and the means for sellers to consume offers. Prospective buyers can create offers and
specify filter criteria, which is notified to the network as an event. Interested 
marketplaces can consume `OfferAvailable` events and match filter criteria to listings 
they host. Marketplaces can also assert that offers meet their minimum required 
commission fee as a condition of accepting and matching an offer.

The introduction of Network Offers adds a key missing piece of the Flow Open Marketplace
Ecosystem and democratizes wide area discoverability and engagement for both sellers and
buyers. The standard is designed as a composable and secure foundation for other on-chain
functionality to be leveraged by other contracts.

## User Benefit

The buyer and seller benefits have already been clearly stated and the standard gives
buyers parity with sellers’ storefront listings. Marketplaces benefit by having a deeper
pool of offers they can provide to sellers, which in turn may result in increased 
sales volume.

This FLIP is provided to Flow community
members for additional input to ensure the best solution is released to the
network.

## Design Proposal
![](20230201-network-offers/offers-system-view.jpg)

Prospective buyers create new `Offer`s using the `OfferManager` resource’s 
`createOffer` function. The `Offer` resource represents an offer created by the 
purchaser which can be accepted by the seller using the `accept` function of the 
`Offer` resource. The `accept` function processes the full transaction, exchanging 
the NFT for the offered amount.

Prospective buyers can specify `offerFilters` at the time of Offer creation to match
specific traits of a NFT. This enables them to target specific collections, rarities,
collection specific traits, price and other filter dimensions. Marketplaces will 
match offer filters to sales listing they host.

We introduce the generic `OfferMatcher` contract interface that provides the 
`OfferMatcherPublic` resource interface, Whoever wants to introduce the filters 
for there offers they can create there custom resolver and follows the 
`OfferMatcherPublic` interface so `Offer` contract can consume the resolver 
whenever the offer get purchased.

During creation of the offer, the prospective buyer can define optional `offerCut`
to offer incentives to different stakeholders if desired.

Offer also respects the royalty of the NFT, If NFT follows the 
`MetadataViews.RoyaltyView` then during the purchase all the royalty receivers 
would be paid as per their desired sale cut.

![](20230201-network-offers/offers-component-flow.jpg)

The [PR](https://github.com/onflow/Offers/pull/1) with the implementation to date
is available for review and comment.

### Drawbacks

Buyers will not benefit if this work is not done, but moreover, network effects
will be stunted compared to the potential of a high participation network. 

### Alternatives Considered

This work is based on an earlier proprietary implementation developed by another
Dapper team. This version is better suited for composability and has already had 
some degree of community feedback in the PR. Find.xyz also have Offers 
functionality and have particpated in the PR

### Performance Implications

There are no unusual event emission characteristics, heavy computation, or large 
payloads/object graphs being stored that could impact performance when this contract 
is in use

* [TBD] There should be microbenchmarks. Are there?
* [TBD] There should be end-to-end tests and benchmarks. If there are not 
(since this is still a design), how will you track that these will be created?

### Dependencies

* Full dependency breakdown to be provided shortly


### Best Practices

 * Some new best practice guidance around Flow Openmarket Ecosystem will be provided

### Tutorials and Examples [TBD]

* If design changes existing API or creates new ones, the design owner should create 
end-to-end examples (ideally, a tutorial) which reflects how new feature will be used. 
Some things to consider related to the tutorial:
    - It should show the usage of the new feature in an end to end example 
    (i.e. from the browser to the execution node). 
    Many new features have unexpected effects in parts far away from the place of 
    change that can be found by running through an end-to-end example.
    - This should be written as if it is documentation of the new feature, 
    i.e., consumable by a user, not a Flow contributor. 
    - The code does not need to work (since the feature is not implemented yet) 
    but the expectation is that the code does work before the feature can be merged. 

### Compatibility 

* Fundamentally this should be providing compatibility.

### User Impact

* What are the user-facing changes? How will this feature be rolled out?
 [TBD]

## Related Issues

What related issues do you consider out of scope for this proposal, 
but could be addressed independently in the future?

## Prior Art

Does the proposed idea/feature exist in other systems and 
what experience has their community had?

This section is intended to encourage you as an author to think about the 
lessons learned from other projects and provide readers of the proposal 
with a fuller picture.

It's fine if there is no prior art; your ideas are interesting regardless of 
whether or not they are based on existing work.

## Questions and Discussion Topics

Seed this with open questions you require feedback on from the FLIP process. 
What parts of the design still need to be defined?
