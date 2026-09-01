# Public trust and transparency assessment

## What is public trust?

Public trust refers to the confidence citizens place in government institutions and officials, based on the belief that they will act in the public's best interest. Underpinning the legitimacy of governmental authority and influencing public opinion. This trust is not incidental to governance — it actively shapes civic life: when citizens believe government is acting in good faith, they are more likely to vote, attend town halls, and engage in civic organizations, while its absence breeds apathy and weakens democratic participation.

## Score Description

Scoring will be based on the FAIR principles (findability, accessibility, interoperability and reusability. The FAIR principles are concerned with “\[enhancing] the reusability of \[..] data holdings”.

The parameters chosen here are a combination of two different frameworks: OECD\[2]and EIF\[3] which were chosen as they relate to the use of open data from a government standpoint as well as in a European context.

The mapping for the assessment will look as follows (see overview for the F1, … at the end of this document):

<table data-search="false"><thead><tr><th valign="top">Assessment Dimension</th><th valign="top">FAIR Principle</th><th valign="top">Scoring</th></tr></thead><tbody><tr><td valign="top">Discoverability</td><td valign="top">F1-F4</td><td valign="top">1-10</td></tr><tr><td valign="top">Metadata Quality</td><td valign="top">F2, R1</td><td valign="top">1-10</td></tr><tr><td valign="top">Provenance</td><td valign="top">R1.2</td><td valign="top">1-10</td></tr><tr><td valign="top">Accessibility</td><td valign="top">A1, A1.1, A1.2, A2</td><td valign="top">1-10</td></tr><tr><td valign="top">Interoperability</td><td valign="top">I1-I3</td><td valign="top">1-10</td></tr><tr><td valign="top">Reusability</td><td valign="top">R1, R1.1, R1.3</td><td valign="top">1-10</td></tr><tr><td valign="top">Governance Transparency</td><td valign="top">ALL</td><td valign="top">1-10</td></tr><tr><td valign="top">Data Quality Transparency</td><td valign="top">R1</td><td valign="top">1-10</td></tr><tr><td valign="top">Participation</td><td valign="top">Indirect Support</td><td valign="top">1-10</td></tr><tr><td valign="top">Long-term Sustainability</td><td valign="top">A2, F4</td><td valign="top">1-10</td></tr></tbody></table>

Each dimension is broken into five bands of two points each. A dimension is scored in a given band when its assessment description matches most, but not necessarily all, of that band’s criteria — use the lower of the two points in a band when only some criteria are met, the higher when nearly all are.

<table><thead><tr><th width="146.96295166015625" valign="top">Score band</th><th valign="top">Text</th></tr></thead><tbody><tr><td valign="top">1–2  Absent</td><td valign="top">There are no – almost no – measures to assess the parameter by.</td></tr><tr><td valign="top">3–4  Ad hoc</td><td valign="top">No standards, applied inconsistently, manually, no structure, …</td></tr><tr><td valign="top">5–6  Developing</td><td valign="top">Structured measures, but explicitly mentioned as not yet mature, applied to some but definitely not all use cases, not easy for the bigger public, …</td></tr><tr><td valign="top">7–8  Established</td><td valign="top">Standard based measures in place and functioning, applied to (nearly) all use cases if relevant, publicly available, only small gaps and specifically identifiable.</td></tr><tr><td valign="top">9–10  Advanced</td><td valign="top">Fully structured, standardized, machine-actionable, verifiable, … . Applied to all relevant use cases.</td></tr></tbody></table>

The evaluation is based on the [DECIDE project write up](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-data-quality-manager).

## Assessment of discoverability

### What is discoverability?

Dataset discoverability refers to the degree to which a dataset can be found by both humans and machines, often without any prior knowledge that the dataset exists.

It is the practical expression of "Findability," the first of the four FAIR principles (Findable, Accessible, Interoperable, Reusable) for scientific data management.

Discoverability is broader than reaching specialist researchers alone. Data should be findable not only by specialist researchers or researchers in general but also by other entities who may be interested in finding and reusing data in various capacities, and ensuring broad findability is intrinsically related to maximizing reuse of data — even reuse that occurs outside the research context in which the data were originally generated.

Finally, discoverability underpins downstream value. FAIR data speeds time to insight by ensuring datasets are easily discoverable, well-annotated, and machine-actionable, which is why it's treated as a foundational, not optional, property of well-managed data.

### Assessment of discoverability within the DECIDE project use cases

UC 0.0: Federated DCAT catalog fed by LDES; every decision gets a persistent ELI identifier (Work/Expression/Manifestation); explicit design goal of machine-readable, indexed discovery across all three cities. On top governing bodies are linked to persistent URI's if available. DCAT catalog also has a human readable interface for human browsing and exploring.

UC0.1 inherits the DCAT/ELI backbone, and the report's own outputs (SDG-linked annotations) are also described as separately catalogued or indexed.

UC1 built on the same catalogued, ELI-identified corpus as the rest of the data space and enriched by NEL service to linking to locations URI's.

UC2 explicitly designed as a search interface "over the full LD\&L corpus," directly increasing discoverability for non-technical end users, not just data professionals.

## Assessment of metadata quality

### What is metadata quality?

Metadata are information about e.g. author, creation date, data size, that describe data points or datasets.  Metadata quality is defined as the accuracy, completeness, consistency, and relevance of descriptive information about data, recording the who, what, when, where, and how — like creation date, source, and format — so users can trust and manage information over time. While data quality describes the accuracy of the actual data values, metadata quality is about the integrity and utility of said data. Just like for the whole assessment, the FAIR principles can be used to assess the quality of metadata.

### Assessment of metadata quality within the DECIDE project use cases

DCAT distributions are co-published with SHACL shapes and ODRL policies describing structure and access conditions — rich, standardized, machine-actionable metadata baseline.\
For UC0.1, UC1 same DCAT/SHACL foundation applies, use case specific metadata is added. For UC2 Same DCAT/SHACL foundation; data generated in Smart Search is not attributed with metadata.

## Assessment of provenance

### What is provenance?

Data provenance — sometimes called data lineage — is the documented history of a piece of data: where it came from, what happened to it, and who or what acted on it, from collection or generation through every transformation to its current state. It is a documented trail that accounts for the origin of a piece of data and where it has moved from to where it is presently.

### Assessment of provenance within the DECIDE project use cases

See https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines/write-up-provenance for all use cases.

## Assessment of accessibility

### What is accessibility?

Accessibility is a data quality dimension concerned with whether data is reachable and usable when it's needed. Broadly, it refers to the accessibility, usability, and integrity of data for various purposes, encompassing the practices, technologies, and policies that ensure data is readily available and can be understood, interpreted, and utilized.

At a more operational level, accessibility/availability is often defined narrowly as _uptime_: a measure of a table or data asset showing whether it was reachable, typically tracked through data observability tooling.

### Assessment of accessibility within the DECIDE project use cases

Multiple access modes (SPARQL, mu-resources, file download, LDES); access enforced at query level via mu-authorization/ODRL; VC-based identity/trust layer for authenticated access. Solid, though largely built for technical/data-space consumers. End users have a human readable and explorable interfase WCAG2.1 accessibility standards compliant. Specific Smart Search interface focussen on human understanding of the local decisions and legislation.

## Assessment of interoperability

### What is interoperability?

Data interoperability is the ability to accurately interpret data that is exchanged between different systems or organizations — ensuring the data has clear and unambiguous meaning, is correctly mapped, and is formatted in the required form. More concretely, it refers to the ways in which data is formatted so that diverse datasets can be merged or aggregated in meaningful ways. It is a foundational concept in open data and research data management, forming the "I" in the FAIR Data Principles.

The core requirement is semantic alignment, not just technical compatibility: for data to be interoperable, different datasets need to measure phenomena in the same way — each variable must ask the same question and format answers the same way. A dataset that records "date of birth" and one that records "age" may not be interoperable without significant cleaning, even though both describe roughly the same underlying fact.

### Assessment of interoperability within the DECIDE project use cases

All three heterogeneous source formats (LBLOD/OSLO, OParl, PDF) are normalized to one open EU standard (ELI/ELI-EP), reusing FOAF/ORG/ELI-DL and RDF/SHACL throughout — strong semantic interoperability, not just technical.

## Assessment of reusability

### What is reusability?

Data reusability describes how well-described and licensed data can enable broad reuse across contexts. More fully: reusability requires that data are in a domain-relevant data standard, that the conditions for usage are clear, and that the metadata provides sufficient attributes for meaningful reuse. Data should also be well-described so it can be replicated and/or combined in different settings.

It is, in a sense, the endpoint the other FAIR principles serve: the ultimate goal of FAIR is to optimize the reuse of data. Findability, accessibility, and interoperability get a user to the data and let them technically ingest it; reusability determines whether they can actually understand it well enough — and are legally and ethically permitted — to use it for a new purpose.

Reusability rests on a few pillars beyond raw documentation: it highlights the necessity for data to be stored and documented in a way that facilitates future retrieval and reuse, supported by comprehensive metadata, consideration of legal and ethical frameworks, and assessment of potential societal impacts.

Reusable data is also expected to preserve its original depth rather than being stripped down for a single use case: reusable data should maintain its initial richness — it should not be diminished for the purpose of explaining the findings in one particular publication. Optimal reuse requires levels of description sufficient to allow data to be replicated and/or combined in different settings, rich and accurate metadata, a clear usage licence, and detailed provenance information.

### Assessment of reusability within the DECIDE project use cases

Explicitly built as open-source, open-standards infrastructure with a dedicated "Repeatable Data Plan" component intended for reuse by other cities/data spaces.\
Little exception for DSP-components of Ui! Q\&A in Smart Search is not available for reuse due to potential unwanted personal data.

## Assessment of governance transparency

### What is governance transparency?

Governance transparency refers to the openness of the decision-making process behind data publication — who decides what gets released, under what rules, and how the public can scrutinize that process. It focuses on institutional behaviour rather than the data itself.

Transparency in governance refers to the availability of relevant information—about deliberations, decisions, rationale, and implementation—to affected citizens. Transparency is both intrinsically valued as a component of democratic legitimacy and instrumentally important as a precondition for accountability.

A key structural issue it addresses is freedom of Information legislation, while expanding in scope, places the burden of information access on the requesting citizen rather than on the producing institution — documents are private unless released, rather than public unless restricted. Governance transparency asks whether an institution defaults to openness or defaults to withholding.

### Assessment of governance transparency within the DECIDE project use cases

Legal base for making local decisions available for all pilot cities (national and regional legislation applies) + governance framework of the data space: https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/legal-and-governance/governance-proposition-after-the-projectphase (not yet on full maturity level).

## Assessment of data quality transparency

### What is data quality transparency?

Data quality transparency is the practice of making data, and information about how it was produced, open to scrutiny. Applied specifically to data _quality_, transparency means openly disclosing not just the data itself but the conditions under which it should be trusted.

The purpose is explicitly epistemic and verificatory: transparency is necessary so that users of public data products can understand what errors, limitations, or systematic biases the data might have — this understanding facilitates informed public discussion and makes it possible to quantify utility through stakeholder engagement. Transparency is also necessary so the public can verify that a data-producing mechanism was implemented correctly.

### Assessment of data quality transparency within the DECIDE project use cases

A dedicated Data Quality Manager component validates ELI entities against SHACL shapes and stores validation reports in the triplestore — a real transparency mechanism, though its outputs/reports aren't described as being surfaced to end users.

## Assessment of participation

### What is data participation?

The most cited working formulation comes from the Ada Lovelace Institute, a UK research institute focused on data and AI governance. They definedata stewardship as the responsible use, collection and management of data in a participatory and rights-preserving way. The participatory dimension specifically concerns involving stakeholders, communities, and individuals in the decision-making processes related to data that affect them, with the underlying goal of rebalancing the asymmetries of power between data collectors/institutions and the people the data describes.

### Assessment of participation within the DECIDE project use cases

Human Validation interfaces let domain experts (P4) vote on AI annotations — genuine participatory input, but restricted to internal/expert stakeholders, not the broader public the data space ultimately serves. Policy impact front-end and the Smart Search have the strongest potential for citizen-facing dimension directly serving the "democratic transparency" goal stated for the project.

## Assessment long-term sustainability

### What is data long-term sustainability?

The dominant recent formalization of the long-term sustainability of data is FAIR+S, an extension of the FAIR data principles that adds sustainability as an explicit fifth dimension. It calls for research artefacts to include information about their long-term life cycle sustainability, including expected maintenance, update frequency, and cumulative resource needs across development, deployment, and preservation. Critically, this reframes sustainability as a lifecycle property, not a point-in-time check. Sustainability must be evaluated across the full life cycle of digital research artefacts.

### Assessment of long-term sustainability within the DECIDE project use cases

For the established dataspace, the data are still accessible if the dataspace were no longer available, as the original data providers are obligated to keep the information accessible. For some of the data, the enriched version would no longer be available, but as each data provider was responsible to implement the DCAT standard, the data would still be registered and indexed in a searchable resource.\
Long term sustainability regarding the data space and the data services will depend on pilot evaluations after project phase.

## Final Scoring

After assessing all relevant points, the DECIDE project receives a score **of 73 percent**.

Even though the requirements were mostly satisfied, there is still room for improvement, which can be traced back to the limited time the team had relative to the project scope.

<table data-search="false"><thead><tr><th width="436.5926513671875" valign="top">Assessment dimension</th><th valign="top">Scoring</th></tr></thead><tbody><tr><td valign="top">Discoverability</td><td valign="top">9</td></tr><tr><td valign="top">Metadata quality</td><td valign="top">8</td></tr><tr><td valign="top">Provenance</td><td valign="top">8</td></tr><tr><td valign="top">Accessibility</td><td valign="top">8</td></tr><tr><td valign="top">Interoperability</td><td valign="top">9</td></tr><tr><td valign="top">Reusability</td><td valign="top">6</td></tr><tr><td valign="top">Governance transparency</td><td valign="top">6</td></tr><tr><td valign="top">Data quality transparency</td><td valign="top">7</td></tr><tr><td valign="top">Participation</td><td valign="top">6</td></tr><tr><td valign="top">Long-term sustainability</td><td valign="top">6</td></tr><tr><td valign="top">Score</td><td valign="top"><strong>73</strong></td></tr></tbody></table>

<br>



