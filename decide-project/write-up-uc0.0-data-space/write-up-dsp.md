---
description: Dataspace Protocol
---

# Write-up DSP

{% hint style="warning" %}
This page is under construction
{% endhint %}

{% hint style="info" %}
The project proposal does not mention the Dataspace Protocol by name. The team has interpreted DSP alongside DCAT as part of the Federation Layer commitment. This interpretation aligns with the DS4SSCC Reference Architecture, which positions DSP as the interoperability protocol layer sitting above the Federating Catalogue.
{% endhint %}

## Description UC/wanted deliverable

Sharing data between organizations across national borders requires not only a catalogue of what is available but also a standardized protocol for how participants formally request access, agree on usage terms, and initiate transfers. The DS4SSCC Reference Architecture identifies the Dataspace Protocol (DSP) as the standard mechanism for this layer.

DECIDe investigated whether DSP could fulfil this role within the project. The team conducted a thorough technical analysis of the DSP specification (version 2025-1) and began a partial implementation of the Catalog Protocol. Following that analysis and implementation experience, the decision was made not to adopt DSP in the DECIDe pilot. The specification, in its current form, does not sufficiently specify the lower bound of what participants must support to achieve actual interoperability –leaving too many critical implementation decisions open. This write-up documents the analysis, the conclusions that led to the decision, and the reasoning behind it.

The Dataspace Protocol is part of the same Federation Layer deliverable as the DCAT Federating Catalogue. Within the project proposal, this maps to the following deliverables and tasks:

| Deliverable                                                              | Activities                                                               |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **D2.6.2** Federation Layer available                                    | **T2.4** Define, develop and test open source semantic Federation Layer. |
| **D2.7.2** Federation Layer integrated in local dataspace of pilot sites | **T2.5** Integrate Federation Layer in local DS of all pilots            |

### Link to other deliverables

#### Federating Catalogue (DCAT)

DSP's Catalog Protocol was designed to sit on top of a DCAT catalogue, providing a standardized programmatic interface for discovering datasets and their access terms. Because DECIDe chose not to implement DSP, the DCAT Federating Catalogue remains the primary discovery mechanism –accessible directly via its SPARQL endpoint, LDES feed, and human-readable interface.

[write-up-dcat.md](write-up-dcat.md "mention")

#### Authorization Policies Store (ODRL)

DSP's Contract Negotiation Protocol is designed around ODRL Offers as the mechanism for communicating usage conditions and reaching binding Agreements. The ODRL write-up documents the policy approach DECIDe adopted and how policies are attached to datasets in the DCAT catalogue.

[write-up-odrl.md](write-up-odrl.md "mention")

#### Universal Trust Data Registry (VC)

Authentication and authorisation of dataspace participants are left unspecified by DSP. In DECIDe, the VC-based Universal Trust Data Registry handles this independently of DSP: credentials govern access to DCAT endpoints directly.

[write-up-verifiable-credentials.md](write-up-verifiable-credentials.md "mention")

## Glossary

{% hint style="info" %}
See the [UC0.0 Data space glossary](./#glossary) for definitions of ODRL and SPARQL.
{% endhint %}

<table><thead><tr><th width="254.966796875">Term / Acronym</th><th>Explanation</th></tr></thead><tbody><tr><td><code>Agreement</code> (ODRL)</td><td>An ODRL Agreement: an Offer accepted by both Consumer and Provider during a Contract Negotiation. Would serve as the basis for a Transfer Process request in a full DSP implementation.</td></tr><tr><td><code>callbackAddress</code></td><td>A URI provided by a message sender to which the receiving party must send follow-up messages.</td></tr><tr><td>Catalog Protocol</td><td>The DSP sub-protocol through which a Consumer queries a Provider for available DCAT Catalogues and Datasets, together with the ODRL Offers associated with them.</td></tr><tr><td>Consumer</td><td>A dataspace participant that requests data from a Provider.</td></tr><tr><td>Contract Negotiation Protocol</td><td>The DSP sub-protocol through which a Consumer and Provider exchange ODRL Offers and reach an Agreement.</td></tr><tr><td><a href="https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1">DSP (Dataspace Protocol)</a></td><td>A protocol specification (version 2025-1) by the Eclipse Dataspace Protocol working group, aiming to facilitate interoperable data sharing between dataspace participants.</td></tr><tr><td><a href="https://github.com/eclipse-dataspacetck/dsp-tck">DSP-TCK (Dataspace Protocol Technology Compatibility Kit)</a></td><td>An open-source test suite for verifying DSP compliance, recommended by DS4SSCC.</td></tr><tr><td><code>MessageOffer</code></td><td>A DSP-specific wrapper used within a Contract Request Message. Links to an existing ODRL <code>Offer</code> by id and carries the Consumer's proposed permission and obligation terms.</td></tr><tr><td><code>Offer</code> (ODRL)</td><td>An ODRL Policy that publicises data availability and usage conditions without granting rights. In DSP, Offers must reference DCAT Datasets or Distributions as their targets rather than ODRL Assets.</td></tr><tr><td>Provider</td><td>A dataspace participant that makes data available for others to discover, negotiate with, and retrieve.</td></tr><tr><td>Transfer Process Protocol</td><td>The DSP sub-protocol through which a Consumer and Provider coordinate the transfer of a dataset distribution.</td></tr></tbody></table>

## Business analysis + final feature passport (incl. functional analysis)

### Opportunity (problem, need, desire)

When datasets are published and discoverable via DCAT, a data consumer still needs a standardized way to formally request access, agree on usage terms, and retrieve data (without relying on custom integration agreements per partner). DSP was the obvious candidate for this protocol layer: it is the standard referenced in the DS4SSCC Reference Architecture, and it aims to provide exactly this kind of machine-readable interaction layer on top of a DCAT catalogue.

#### Analysis and decision

The team conducted a thorough technical analysis of DSP version 2025-1 covering all three sub-protocols: the Catalog Protocol, the Contract Negotiation Protocol, and the Transfer Process Protocol. A partial implementation of the Catalog Protocol was developed to test the specification in practice.

Following this analysis and implementation experience, the decision was made not to proceed with DSP in DECIDe. The core conclusion is that DSP, by itself, does not sufficiently guarantee interoperability between dataspace participants. The specification is deliberately technology-agnostic –giving implementers maximum flexibility– but in doing so leaves too many critical decisions open. Participants who both claim to support DSP may still be unable to exchange data without additional agreements, because the specification does not mandate a concrete lower bound on what must be supported.

Four specific gaps drove this conclusion.

* First, DSP provides no mechanism for a message sender to discover in advance which `callbackAddress` formats a recipient understands; the sender may receive an error but cannot know what format to use instead.
* Second, the Transfer Process Protocol is explicitly agnostic about wire protocols, meaning consumers have no standardized way to learn which transfer formats a provider supports –they must rely on provider documentation or trial and error.
* Third, the Contract Negotiation Protocol leaves the actual decision-making logic entirely out of scope: how a Provider evaluates an incoming offer, whether to accept or reject it, and crucially why. The specification makes reason-giving optional, so a terminated negotiation may leave the other party with no explanation, and left guessing what they can do differently.
* Fourth, authentication and authorisation methods are left almost entirely open: the specification advises use of the HTTP `Authorization` header but says nothing about which token types to use or how to obtain them.

The result is that for a new participant wishing to join an existing dataspace, DSP provides insufficient guidance. They would need to discover –through documentation, error messages, or experimentation– which specific technologies the existing participants have agreed to use. This is a significant barrier to open membership and undermines the interoperability promise.

#### What this means for DECIDe

Data sharing within DECIDe does not rely on DSP. Datasets are discoverable via the DCAT Federating Catalogue, access conditions are described via ODRL policies attached to DCAT entries, and access itself is controlled via the VC-based authorisation layer. Consumers access data directly through the endpoints described in DCAT, rather than through a DSP negotiation flow.

### Pilot partners

All three pilot cities –Ghent, Freiburg, and Bamberg– are affected by this decision in the same way: none of their datasets are exposed via a DSP endpoint. Their data remains accessible via the DCAT catalogue and its associated endpoints.

### Target audience / Personas

<table><thead><tr><th width="216.7880859375">Persona</th><th>Journey</th></tr></thead><tbody><tr><td><strong>P6</strong> Data engineer</td><td>Reviews the DSP analysis conclusions; understands why data sharing is handled via DCAT rather than DSP; monitors data access via DCAT endpoints.</td></tr><tr><td><strong>P7</strong> Dataspace consumer</td><td>Discovers and accesses DECIDe datasets via the DCAT catalogue and its endpoints, rather than through a DSP negotiation flow.</td></tr></tbody></table>

### Functionality (requirements)

No DSP functionality was deployed in DECIDe. The table below reflects what was assessed during the analysis and what the conclusion was regarding each area.

<table><thead><tr><th width="302.056640625">Area assessed</th><th>Conclusion</th></tr></thead><tbody><tr><td>Catalog Protocol – Provider endpoint</td><td>Implementation started but abandoned. Insufficient alone without the other sub-protocols.</td></tr><tr><td>Contract Negotiation Protocol</td><td>Analyzed in depth; not implemented. Specification leaves acceptance logic and reason-giving too open for reliable interoperability.</td></tr><tr><td>Transfer Process Protocol</td><td>Analyzed in depth; not implemented. Wire protocol and authentication left open by specification.</td></tr><tr><td>ODRL Offers compatible with DSP</td><td>Concept designed during analysis; informed ODRL implementation outside DSP context.</td></tr><tr><td>Authentication on DSP endpoints</td><td>Not implemented; DSP leaves this open and DECIDe uses VC-based auth independently.</td></tr></tbody></table>

## Datasources, datasets and datastandards

### Data sources

n/a

### Datasets available in the data space

n/a

### Data standards

<table><thead><tr><th width="285.30859375">Standard</th><th>Link</th></tr></thead><tbody><tr><td>Dataspace Protocol 2025-1</td><td><a href="https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1">https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1</a></td></tr><tr><td>ODRL Information Model 2.2</td><td><a href="https://www.w3.org/TR/odrl-model/">https://www.w3.org/TR/odrl-model/</a></td></tr><tr><td>DCAT v3</td><td><a href="https://www.w3.org/TR/vocab-dcat-3/">https://www.w3.org/TR/vocab-dcat-3/</a></td></tr><tr><td>DSP-TCK (test suite, for reference)</td><td><a href="https://github.com/eclipse-dataspacetck/dsp-tck">https://github.com/eclipse-dataspacetck/dsp-tck</a></td></tr></tbody></table>

## Final architecture (and why)

No DSP architecture was deployed. Following the analysis, the decision was made not to implement DSP in the DECIDe pilot. Data sharing is handled through the DCAT Federating Catalogue, ODRL policies, and the VC-based access control layer –each documented in their own write-ups.

A partial implementation of the Catalog Protocol was developed during the analysis phase but was not merged. This implementation demonstrated that the Catalog Protocol can be built on top of the DCAT layer –reusing existing Catalog, Dataset, and Distribution resources– but also confirmed that without the Contract Negotiation and Transfer Process protocols, the Catalog Protocol alone does not provide meaningful added value over direct DCAT access.

### Final semantic components (and why) (if any)

n/a

No DSP semantic components were deployed.

### Other explored semantic components (and why not)

The team conducted a detailed technical analysis of all three DSP sub-protocols, including the design of DSP-compatible ODRL Offers. The conclusions of that analysis are preserved below.

#### ODRL Offers for DECIDe

DSP relies on members using ODRL `Offer`s to exchange information on the datasets they offer and under which conditions. In ODRL, an `Offer` is a specific kind of `Policy` that does not itself grant any rights, but can be used to publicise which data an entity can provide and under which conditions. An `Offer` consists of three main parts:

* a description of the data it concerns (typically as `Asset`s or `Asset Collection`s),
* the conditions under which that data would be made available (defined as `Rule`s), and
* some identification of the `Party` that acts as assigning entity of the `Offer`.

For more information on the ODRL model and its concepts, please visit the [ODRL write-up](write-up-odrl.md#the-odrl-information-model).

DSP imposes two constraints on how `Offer`s must be defined that depart from standard ODRL.

1. First, the data an `Offer` concerns must be described as a DCAT `Dataset` or `Distribution` rather than an ODRL `Asset` or `Asset Collection`. In other words, an `Offer` will not explicitly describe the data itself but will refer to the appropriate DCAT resources as targets. For more background on DCAT and how it is used within DECIDe, consult the dedicated [DCAT write-up](write-up-dcat.md).
2. Second, the `Offer`s exchanged in DSP messages do not explicitly refer to the assigner entity, despite this being mandated by the ODRL Information Model. To remain compliant with both standards, the DECIDe application can be specified as assigning `Party` in the stored `Offer` definition, while this property is omitted when exchanging `Offer`s as part of DSP.

To make this concrete, let's say the DECIDe application contains a Dataset identified by `ext:exampleDcatDataset`. Imagine it's offered to other dataspace members under the following conditions:

* members can retrieve the dataset from DECIDe;
* when using the contained data, DECIDe must be attributed as its original source; and
* members are not allowed to sell the received data to other parties.

These conditions are respectively a `Permission`, a `Duty`, and a `Prohibition`. In ODRL Turtle format, such an `Offer` would be written as follows:

```turtle
@prefix odrl: <http://www.w3.org/ns/odrl/2/> .
@prefix ext: <http://mu.semte.ch/vocabularies/ext/> .
@prefix vcard: <http://www.w3.org/2006/vcard/ns#> .

ext:exampleOffer a odrl:Offer ;
  # The `target` and `assigner` will be shared for all contained rules, see <https://www.w3.org/TR/odrl-model/#composition-compact>
  odrl:target ext:exampleDcatDataset ; # Fictious dataset for example purposes
  odrl:assigner ext:decideParty ;
  # Rules
  odrl:permission ext:readPermission ;
  odrl:duty ext:attributeDuty ;
  odrl:prohibition ext:sellProhibition .

ext:readPermission a odrl:Permission ;
  odrl:action odrl:read .

ext:sellProhibition a odrl:Prohibition ;
  # Definition of `odrl:sell`: "To transfer the ownership of the Asset to a third party with compensation and while deleting the original asset." <https://www.w3.org/TR/odrl-vocab/#term-sell>
  odrl:action odrl:sell .

# Example Duty to illustrate how we could use that to require that DECIDe is attributed for the shared dataset.
ext:attributeDuty a odrl:Duty ;
  odrl:action odrl:attribute ;
  odrl:attributedParty ext:decideParty . # Alternative: link to an odrl:Asset that contains the attribution info

ext:decideParty a odrl:Party ;
  vcard:fn "DECIDe application" ;
  vcard:hasEmail <decide@lblod.info> ; # Fictitious email for example purposes
  vcard:hasURL <https://decide.lblod.info> . # Fictitious url for example purposes
```

#### Catalog Protocol

This sub-protocol defines an API supporting two requests with two possible responses that members of a dataspace should support. These requests allow external `Consumer`s –other members of the dataspace– to ask what DCAT `Catalogue`s and `Dataset`s a member can provide. The message contents reuse concepts from DCAT (`Catalogue`, `Dataset`, `DataService`, `Distribution`) and ODRL (`Offer`, `Permission`, `Constraint`). For example, a response listing available `Dataset`s uses ODRL `Offer`s to specify what rights are associated with the data.

The underlying data overlaps entirely with what is published via DCAT –specifically the `Catalog`, `Dataset`, and `Distribution`. DSP does assume that there is a single root catalogue, which is not a requirement in plain DCAT. The [`CatalogRequestMessage`](https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1/#catalog-request-message) allows optionally specifying implementation-specific filters. For a pilot application such as DECIDe, not supporting filters at all was considered acceptable, meaning any request containing a filter would receive an HTTP 400 (Bad Request) response as the specification instructs for unsupported filters.

The API primarily defines what DECIDe as `Provider` should support. To be fully compliant, DECIDe would also need to act as `Consumer` –sending requests for catalogues and datasets to other dataspace members.

#### Contract Negotiation Protocol

This sub-protocol defines a set of messages that a `Consumer` and `Provider` can exchange to come to an agreement on which dataset the `Consumer` can get from the `Provider` and how they may use it. Any exchange essentially starts with either the `Consumer` providing a `[Message]Offer` to the `Provider`, or vice versa. Multiple `Offer` proposals can be exchanged until both parties agree, at which point the `Offer` becomes an `Agreement` that must be verified and confirmed by both sides. The protocol specifies only the format and allowed order of messages –how a `Consumer` and `Provider` actually decide to accept or reject an `Offer` or `Agreement` is explicitly out of scope.

Several specific issues were identified. The `ContractNegotiationTerminationMessage` optionally allows the terminating party to specify a reason, but if they do not, the other party is left to guess why the negotiation was terminated and what they could do differently. The `ContractRequestMessage` and `ContractOfferMessage` similarly provide no field for the sender to explain their reasoning –a field that would, for instance, allow a `Consumer` to explain why they are making a new offer rather than accepting the one they received.

A further complication concerns the `MessageOffer` element. The "offer" provided as part of a `ContractRequestMessage` is not an actual ODRL `Offer` but a `MessageOffer`, which links to an actual `Offer`'s id and carries its own `offer.obligation` and `offer.permission` fields, which are described in the specification as signifying "the terms at which a Consumer would accept an Offer." (The `MessageOffer` type definition also includes `Prohibition`s, but these are not mentioned in the quoted description.) It is not clear how these obligations and permissions should be evaluated by the `Provider` during the negotiation. The practical interpretation settled on was that the `Provider` could simply error on any `ContractRequestMessage` that does not reference a pre-defined `Offer` id, and reject any `MessageOffer` whose permissions or obligations are broader than the pre-defined ones –though correctly implementing even this simplified logic was noted as non-trivial.

For state management, the team proposed persisting each negotiation as a `dspace:ContractNegotiation` in the triplestore, together with all messages exchanged as part of it (a JSON-LD schema defining all message types is available). This would allow the service to recover from crashes or restarts by reading ongoing negotiation state from the triplestore, and would provide long-term traceability of conducted negotiations.

#### Transfer Process Protocol

This sub-protocol defines the messages a `Consumer` and `Provider` can exchange to set up the actual transfer of a dataset distribution. The exchange starts with a request from the `Consumer` specifying an `Agreement` and a distribution `format`; if valid, the transfer is started. Subsequent messages can be sent by either side to mark the transfer as suspended, completed, or terminated. The protocol does not cover the actual data transfer itself –only how the transfer should be initiated and tracked.

What makes a `TransferRequestMessage` valid is not fully specified by the protocol, but the team identified four reasonable criteria:

* the specified `Agreement` must be known by the `Provider` and itself valid
* the `Agreement` must name the requesting `Consumer` as assignee
* the specified `format` must correspond to a valid `Distribution` for the targeted `Dataset`
* the `callbackAddress` must be a URI the service can understand.

To keep scope manageable, the team proposed limiting wire protocol support to HTTP, where distributions are exchanged either as inline or attached content (similar to what the existing [file-service](https://github.com/mu-semtech/file-service) supports). A pull transfer would be a GET from the `Consumer` to which the service replies with the dataset; a push transfer would be a POST from the service to the location the `Consumer` specifies in its `TransferRequestMessage`. The `Distribution`s already published via DCAT would be reused rather than defining a separate set.

#### Authentication

The HTTPS binding states that "_\[a]ll requests SHOULD use the `Authorization` header to include an authorization token_" but adds that "_The semantics of such tokens are not part of this specification._" Further implementation decisions, such as which kind of tokens to support as well as how to obtain them, are left to the dataspace members.

The `Version` message's `auth` property could be used to let connectors communicate which authorisation methods they support, but its vague wording, that it "_describes how a Dataspace Protocol endpoint is **secured**_"– leaves doubt about what exactly this covers and how it relates to the use of the HTTP `Authorisation` header. It is unclear whether, for instance, a rate-limiting policy to prevent DoS attacks would also count as "securing" an endpoint. The `auth` property also does not appear to support advertising multiple methods for a single endpoint version, for example, supporting both Basic authentication and OAuth simultaneously.

### Final AI components (and why) (if any)

n/a

### Other explored AI components (and why not)

n/a

## Final UI design (and why) (if any)

n/a

### Other explored UI design (and why not)

n/a

## Testing approach

No DSP implementation was deployed, so no testing was performed.

The DS4SSCC-recommended test suite for DSP compliance is the [DSP-TCK](https://github.com/eclipse-dataspacetck/dsp-tck). The team reviewed DSP-TCK in autumn/winter 2025. At the time, the catalogue portion was relatively lightweight –it checked that a response is non-empty and that datasets have the correct id. This by itself does not confirm full functional correctness of a DSP implementation, but this shortcoming may have been resolved with recent developments to DSP-TCK.

### Risks & mitigations

The primary risk of not implementing DSP is that DECIDe datasets are not accessible via the standard DS4SSCC protocol layer. Consumers who expect DSP-compliant endpoints will not find them. This risk is accepted on the grounds that DSP itself does not currently guarantee the interoperability it promises, and that within the DECIDe pilot the set of data consumers is small and known, i.e. limited to the pilot partners. This makes direct DCAT-based access a sufficient and more reliable alternative.

A secondary risk is that DSP matures after the project concludes and becomes the de facto standard, at which point DECIDe's lack of DSP support may be seen as a gap. This risk is accepted for the pilot phase and addressed in the future work section.

## Possible future work

### Possible future work DECIDe data space related

#### Revisiting DSP if interoperability gaps are resolved

Should DSP reach a maturity level where interoperability can be reliably achieved, a full three-protocol implementation –Catalog Protocol, Contract Negotiation Protocol, and Transfer Process Protocol– would be the natural next step. The analysis conducted in DECIDe provides a clear picture of what needs to be implemented and where the main complexity lies (in particular, the contract acceptance logic and the authentication integration).

### Possible future work LBLOD related

## Relevant links

<table><thead><tr><th width="226.5830078125">Resource</th><th>Link</th></tr></thead><tbody><tr><td>DSP specification 2025-1</td><td><a href="https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1">https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1</a></td></tr><tr><td>DSP-TCK test suite</td><td><a href="https://github.com/eclipse-dataspacetck/dsp-tck">https://github.com/eclipse-dataspacetck/dsp-tck</a></td></tr></tbody></table>
