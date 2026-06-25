---
description: Federation Layer
---

# Write-up DCAT

{% hint style="warning" %}
This page is under construction
{% endhint %}

## Description UC/wanted deliverable

Any data space needs a mechanism for discoverability: participating entities must have a reliable, standards-based way to publish what data they make available, so that both human users and automated agents can find and assess it. The goal is for a DCAT catalogue to sit at the highest-level entry point of the DECIDe data space, linking to the DCAT catalogues of all participating pilot partners. Each city hosts at least one catalogue describing its own datasets; an overarching Federation Catalogue then aggregates those local catalogues and signals which sources can be trusted within the data space.

The DCAT catalogue contains the essential information required for dataset discovery: access endpoints, licensing conditions, provenance, and metadata about content and structure. A human interface on top of the DCAT layer allows non-technical users to browse and understand what is available without needing to query a SPARQL endpoint or parse RDF directly.

Within the project proposal, this maps to the following deliverables and tasks:

| Deliverable                                                               | Activities                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **D2.6.2** Federation Layer available                                     | **T2.4** Define, develop and test open source semantic Federation Layer. |
| **D2.7.2** Federation Layer integrated in local data space of pilot sites | **T2.5** Integrate Federation Layer in local DS of all pilots            |

### Link to other deliverables

#### Authorization Policies Store (ODRL)

Each DCAT distribution carries machine-readable access rights expressed as ODRL policies, co-published alongside the distribution metadata. The two components are tightly coupled: DCAT describes what datasets exist and how to reach them; ODRL describes the rules that apply.

[write-up-odrl.md](write-up-odrl.md "mention")

#### Universal Trust Data Registry (VC)

The Universal Trust Data Registry is conceived as an augmentation of the Federation Catalogue: initially, inclusion in the DCAT catalogue serves as the proxy for source trustworthiness; the VC-based registry strengthens this, by adding W3C DID and W3C Verifiable Credential-based identity verification on top of the DCAT discovery layer.

[write-up-verifiable-credentials.md](write-up-verifiable-credentials.md "mention")

#### Data Space Protocol (DSP)

The Data Space Protocol governs how data exchange requests between participants are initiated and negotiated; it complements DCAT as the discovery layer by providing the protocol through which a consumer acts on what they find in the catalogue.

[write-up-dsp.md](write-up-dsp.md "mention")



## Glossary

{% hint style="info" %}
See the [UC0.0 Data space glossary](./#glossary) for definitions of DCAT, ELI, ODRL, and SHACL.
{% endhint %}

<table><thead><tr><th width="183.970703125">Term/Acronym</th><th>Explanation</th></tr></thead><tbody><tr><td><a href="https://semiceu.github.io/DCAT-AP/releases/3.0.0/">DCAT-AP (DCAT Application Profile)</a></td><td>A SEMIC recommendation that restricts and extends DCAT for use in European open data infrastructures. DECIDe's federation layer aligns with DCAT-AP v3.</td></tr><tr><td><a href="https://data.vlaanderen.be/doc/applicatieprofiel/DCAT-AP-VL/">DCAT-AP-VL</a></td><td>Flemish specialisation of DCAT-AP, based on DCAT v2. Used by Ghent and other Flemish open data publishers for local catalog publication.</td></tr><tr><td>DCAT feed</td><td>A downloadable RDF serialisation of a DCAT catalog (Turtle, RDF/XML, or N-Triples) published at a known URL, enabling automated consumption and federation.</td></tr><tr><td><code>dcat:Catalog</code></td><td>A curated collection of dataset and data service descriptions. Each pilot city hosts its own catalog; the Federation Catalogue aggregates these.</td></tr><tr><td><code>dcat:Dataset</code></td><td>A conceptual description of a collection of data published by a single agent. Described by title, description, publisher, contact point, access rights, and themes. Must have at least one distribution in DCAT-AP-VL.</td></tr><tr><td><code>dcat:DataService</code></td><td>A collection of operations accessible via an API, such as a SPARQL endpoint. Distinct from a distribution: a DataService is linked to a dataset via <code>dcat:servesDataset</code>.</td></tr><tr><td><code>dcat:Distribution</code></td><td>A specific physical representation of a dataset, available at an access URL. In DECIDe, distributions are typed as SPARQL endpoints, LDES feeds, or Resource APIs (JSON:API).</td></tr><tr><td>Federation</td><td>The mechanism by which a central catalog aggregates and replicates metadata from multiple member catalogs, enabling unified discovery without centralising the underlying data.</td></tr><tr><td><a href="https://w3id.org/ldes/specification">LDES (Linked Data Event Streams)</a></td><td>A specification for publishing RDF datasets as ordered, append-only event streams. DECIDe uses LDES to federate DCAT catalog metadata across pilot city catalogs to the Federation Catalogue.</td></tr></tbody></table>

## Business analysis + final feature passport (incl. functional analysis)

### Opportunity (problem, need, desire)

When a local source publishes data, it is important that interested third parties can actually find it. This is the core problem DCAT addresses: it provides a standardised way to describe datasets –their content, access endpoints, licensing conditions, and provenance– and to publish those descriptions somewhere they can be reliably discovered.

In DECIDe, each data source has a DCAT description for each of its datasets and distributions. An overarching Federation Catalogue aggregates those descriptions making all data space sources discoverable from a single entry point, and automatically picks up updates via LDES feeds. The DCAT catalogue itself is public; authentication is applied at the level of individual endpoints rather than at the catalogue level, keeping discovery open while access remains controlled. Being listed in the Federation Catalogue is an implicit trust signal: it means the source has been accepted as a trustworthy participant in the data space.

### Pilot partners

DCAT is relevant for all three pilot cities that participate in the data space: Ghent (Belgium), Freiburg and Bamberg (Germany). Each city has a DCAT catalog describing its sources within the data space, whether created locally or on the city's behalf as part of the DECIDe pipeline work.

#### Target audience / Personas

The DCAT Federation Catalogue serves two broad audiences: the data providers and technical maintainers who publish and manage catalog metadata, and the consumers who use the catalogue to discover and access data. Data engineers configure catalog entries and monitor federation; application developers and end users rely on the catalogue to find data and understand what they can do with it.

<table><thead><tr><th width="176.63671875">Persona</th><th>Journey</th></tr></thead><tbody><tr><td><strong>P1</strong> Original decision data provider</td><td>Publishes decision data from their local system; their datasets and distributions must be accurately described and registered in the DCAT catalogue so they are discoverable within the data space.</td></tr><tr><td><strong>P2</strong> Semantic framework owner</td><td>Defines the SHACL shapes and controlled vocabularies that describe dataset structure; ensures DCAT metadata accurately reflects these modelling choices and that dataset descriptions remain aligned with the semantic layer.</td></tr><tr><td><strong>P6</strong> Data engineer</td><td>Configures DCAT catalog entries for each dataset and distribution; monitors LDES feeds; ensures the Federation Catalogue stays in sync with member catalogs as data sources evolve.</td></tr><tr><td><strong>P7</strong> Data space consumer</td><td>Browses the Federation Catalogue to discover available datasets, inspect their distributions and access conditions, and retrieve data for use in an application or analysis.</td></tr></tbody></table>

### Functionality (requirements)

The DCAT component covers both the catalog publication standard and the federation mechanism. Each pilot city's data sources are described in a DCAT-AP v3 compliant catalog, capturing what endpoints are available, what they contain, who publishes them, and under what access conditions. The Federation Catalogue aggregates those local catalogs –replicating catalog metadata, not the underlying data– into a central triplestore, and exposes it through a human-readable interface, a SPARQL endpoint, and its own DCAT feed.

Three types of endpoints can be described in the catalog: SPARQL endpoints, LDES feeds, and Resource APIs (JSON:API). Alongside the endpoint descriptions, access conditions are co-published as ODRL policies and data structure expectations as SHACL shapes, keeping the full set of discovery metadata machine-readable and co-located in the catalog.

<table><thead><tr><th width="617.6279296875">Requirement</th><th>Priority</th></tr></thead><tbody><tr><td>DCAT-AP v3 compliant catalog per pilot city</td><td>Must-have</td></tr><tr><td>LDES feed per city catalog for federation</td><td>Must-have</td></tr><tr><td>Federation Catalogue aggregating all member catalogs via LDES</td><td>Must-have</td></tr><tr><td>SPARQL endpoint on the Federation Catalogue triplestore</td><td>Must-have</td></tr><tr><td>DCAT LDES feed exposed by the Federation Catalogue</td><td>Must-have</td></tr><tr><td>SPARQL endpoints modelled as <code>dcat:DataService</code> with <code>dcat:servesDataset</code></td><td>Must-have</td></tr><tr><td>LDES distributions modelled as <code>dcat:Distribution</code> + <code>ldes:EventSource</code> with <code>dct:conformsTo</code></td><td>Must-have</td></tr><tr><td>Resource API distributions modelled as <code>dcat:Distribution</code> with JSON:API media type</td><td>Must-have</td></tr><tr><td>ODRL policy co-published per dataset via <code>odrl:hasPolicy</code></td><td>Must-have</td></tr><tr><td>SHACL shapes co-published per dataset via <code>dct:conformsTo</code></td><td>Must-have</td></tr><tr><td>ODRL and SHACL metadata federated alongside DCAT catalog entries</td><td>Must-have</td></tr><tr><td>DCAT catalog publicly accessible without authentication</td><td>Must-have</td></tr><tr><td>Minimal human-readable DCAT catalog interface</td><td>Must-have</td></tr></tbody></table>

## Datasources, datasets and datastandards

### Data sources

| Data source | Type/category | Brief description |
| ----------- | ------------- | ----------------- |
|             |               |                   |

### Datasets available in the data space

| Dataset | IdP/Authentication service | Country of origin | Domain | Shared within the project | Reused within the project |
| ------- | -------------------------- | ----------------- | ------ | ------------------------- | ------------------------- |
|         |                            |                   |        |                           |                           |

### Data standards

| Standard | Link |
| -------- | ---- |
|          |      |

## Final architecture (and why)

The DECIDe DCAT architecture follows a federated model: each participating city or organisation maintains its own DCAT catalog, and the Federation Catalogue aggregates those member catalogs by following their LDES feeds.

At the city level, each pilot city has a DCAT catalog describing its datasets and distributions. The catalog is public within DECIDe scope –authentication is applied at the level of individual endpoints, not at the catalog level.

The Federation Catalogue operates by following the DCAT LDES feeds published by each member catalog. As member catalogs update, the Federation Catalogue replicates new or changed metadata entries into its own triplestore –not the underlying data, only the metadata pointing to access endpoints. The Federation Catalogue then exposes the aggregated metadata in three ways: a human-readable catalog UI for browsing by non-technical users, a SPARQL endpoint for querying the catalog metadata directly, and a DCAT LDES feed that allows other catalogs at regional, federal, or European level to follow and replicate the DECIDe catalog's contents.

ODRL policies and SHACL shapes are co-federated alongside DCAT catalog entries. When the Federation Catalogue follows a member catalog's LDES feed, it also picks up the ODRL access rules and SHACL structural descriptions attached to those catalog instances, making the full discovery metadata (what the data is, how to access it, what rules govern access, and what structure to expect) available from the Federation Catalogue.



<figure><img src="../../.gitbook/assets/dcat-federation.jpg" alt=""><figcaption></figcaption></figure>

A typical client flow is shown below. The client queries the well-known location of the Federation Catalogue to find available dataset distributions and their endpoints. From the DCAT descriptions they learn which endpoints require authentication and which are public. They authenticate where required –using Verifiable Credentials– and then access the endpoints relevant to them.

<figure><img src="../../.gitbook/assets/lokale-bron-architecture-DCAT.jpg" alt=""><figcaption></figcaption></figure>

### Final semantic components (and why) (if any)

The semantic foundation is DCAT-AP v3, the SEMIC European application profile of the W3C DCAT vocabulary. DCAT-AP v3 was chosen because it is a required standard in the DSSC Blueprint, it is structurally compatible with the existing Flemish DCAT-AP-VL publications (which are v2-based), and it adds useful features over v2 –versioning, dataset series, and inverse properties– without breaking backward compatibility.

Three distribution types are modelled:

* A SPARQL endpoint is modelled as a `dcat:DataService` with an `endpointURL`, linked to its dataset via `dcat:servesDataset`.&#x20;
* An LDES feed is modelled as a `dcat:Distribution` simultaneously typed as `ldes:EventSource`, with `dct:conformsTo` pointing to the LDES specification and supported media types listed explicitly.&#x20;
* A Resource API (JSON:API) is modelled as a `dcat:Distribution` with `dcat:mediaType` set to the JSON:API media type identifier. The Resource API option is the least interoperable of the three –the resources configuration that links its output to the semantic model is not public– and was included as a concession to project partners with limited linked data experience who prefer a JSON-based API over a SPARQL endpoint or LDES feed.

ODRL policies are co-published per dataset using `odrl:hasPolicy` on the `dcat:Dataset` description. SHACL shapes describing the expected content structure of datasets are co-published using `dct:conformsTo` at the dataset level. Both are surfaced by the LDES-based federation mechanism and replicated into the Federation Catalogue alongside the core DCAT metadata.

LDES was selected as the federation mechanism because it is event-driven: rather than periodically re-downloading entire catalog files, the Federation Catalogue receives incremental updates as dataset descriptions change. This is consistent with how LDES is used elsewhere in the LBLOD stack, and aligns with the direction SEMIC is taking for DCAT-AP feeds at the European level –a prototype for LDES-based DCAT-AP exchange was set up in Belgium and Sweden in 2024.

For the Ghent pilot, DCAT-AP-VL (v2) is used for the local publication to Metadata Vlaanderen in addition to the DCAT-AP v3 description used within the DECIDe data space. The two profiles are structurally compatible; the versioning gap means some v3 features are not expressible in the Flemish local publication, but this does not affect the DECIDe data space operation.

### Other explored semantic components (and why not)

Two alternative federation approaches were assessed before settling on the LDES-based architecture.

The **Data space Interoperability Framework PoC** (using the CKAN DCAT extension format) was suggested by DS4SSCC coaches in the context of DECIDe. It is a minimal federation solution –intentionally excluding identity, contracting, and consent– that works by having independent catalogs register with a federating instance, which then fetches their catalog information and exposes a distributed search endpoint. The core problem for DECIDe is the format requirement: the PoC requires the format used by the CKAN DCAT extension, which does not support SHACL descriptions of dataset contents or ODRL policy extensions. Adding these within the CKAN format would require building a custom adapter, at which point the ready-made solution loses most of its value. The federated search mechanism was also assessed as unlikely to scale: distributing a search query across many registered independent catalogs, with no clear pagination strategy, does not generalise to a large data space. This approach was ruled out on both grounds.

The **Eclipse EDC Federated Catalog** was also assessed. It works by crawling the DSP catalog endpoint of the catalogs it federates, making it architecturally closer to what DECIDe ultimately built than the CKAN PoC. DECIDe chose not to implement the Data Space Protocol at this stage –the DSP did not yet offer a comprehensive solution guaranteeing interoperability when examined– which already rules out the Eclipse approach as a dependency. In addition, the adopters documentation for the Eclipse Federated Catalog was still in a TBD state at the time of assessment, and it was unclear how the system handles federated catalogs using a different DCAT application profile than the federating catalog.

### Final AI components (and why) (if any)

n/a

### Other explored AI components (and why not)

n/a

## Final UI design (and why) (if any)

### Other explored UI design (and why not)

n/a

## Testing approach

### Risks & mitigations

## Possible future work

### Making the data space tamper-proof

When obtaining (meta) data from the data space, users want to be sure they obtain the correct, unmodified (meta) data.
Similarly, data space members and data providers want assurances that unauthorised changes are prevented when possible and can be detected otherwise.

Currently, the DECIDe application relies on access control to prevent unauthorised changes to stored data and the use of secure protocols, e.g. https, to protect data in transit.
But it does not provide users a way to explicitly verify the integrity of the (meta) data they receive.
Note, due to the federated setup of a data space, it will finally be the responsibility of each data space member to ensure the integrity of the data they offer.

A logical initial step, would be to usie DCAT's [checksum](https://www.w3.org/TR/vocab-dcat-3/#Property:distribution_checksum) property for distributions.
This property allows to link a DCAT `Distribution` resource to a `http://spdx.org/rdf/terms#Checksum` resource that specifies a checksum and algorithm used to compute it.
For single file distributions, e.g. a Turtle file or archive of files, the recipient can calculate the the checksum of the received file and compare this to the original one.
For this to be effective, this does require that the checksum can be [transmitted via a separate channel](https://www.w3.org/TR/vocab-dcat-3/#security_and_privacy) to ensure its integrity.
Providing such a channel would require extending the DECIDe application.
One possibility would be that data space members cryptographically sign the checksum resources for the distributions they produce.
The data space member's public key required to verify such signatures could be published using the member's DID (see [write-up-verifiable-credentials.md](write-up-verifiable-credentials.md)).
Alternatively, one could cryptographically sign a Distribution file as a whole, the signature would then be transmitted along with the actual distribution contents.
This would allow to leverage existing technologies such as [openpgp](https://www.openpgp.org/about/) to publish public keys and verify signatures.

For distributions that provide more dynamic access the data, e.g. SPARQL endpoints, the above approaches are not feasible because the data in these distributions changes often as new decisions come in and new annotations are generated.
For every such change, a new checksum would have to be generated and it would be almost impossible for users to know which checksum matches the current dataset.
Providing users the means to verify the integrity the responses they receive for their queries will require an analysis of existing approaches in this field.
One possibility would be to explore whether LDES can be leveraged to provide an auditable log of (parts of) the contents of the triplestore.
This would allow users to verify received responses against the data in the LDES feed.
This still poses significant challenges, such as how to guarantee the integrity of the feed itself.

Finally, ensuring the integrity of the DCAT data itself remains an open question.
For instance, an attacker should not be able to add a malicious distribution, e.g. one pointing to an download URL under the attacker's control, to a dataset.
One avenue to explore would be to cryptographically sign (parts of) DCAT resources.
But this still poses some open challenges.
For instance, any generated signature likely depends on the order of the signed triples, as different order will lead to different signatures, irrespective of any actual data changes.
A investigation of existing approaches is needed, as well as a more detailed analysis of how to approaches described above could be re-applied here.


## Relevant links

Link to github

Link to front-ends // Link to dev/test/prod (if any)

ACM/IDM link if relevant
