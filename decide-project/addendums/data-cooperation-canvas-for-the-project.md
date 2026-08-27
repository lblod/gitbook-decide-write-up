# Data Cooperation Canvas for the project

We have used the [Data Cooperation Canvas](https://www.datacooperationcanvas.eu/canvas/intro) to get a grip on our use cases and business case.

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

## Organizational (purple)

### Key partners

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th width="140.88885498046875" valign="top">Partner</th><th width="414" valign="top">Role in the data exchange</th><th valign="top">Role(s)</th></tr></thead><tbody><tr><td valign="top">Agency for Home Affairs Flanders</td><td valign="top">Operates the central DECIDe data space infrastructure, develops and manages ingestion pipelines, AI enrichment services, validation tooling, federation services, catalog services, and data-space governance components. ABB acts as the primary technical operator of the shared data space.</td><td valign="top">Initiator, Coordinator, Developer, Contributor</td></tr><tr><td valign="top">City of Ghent</td><td valign="top"><p>Original provider of local decisions and legislation (LD&#x26;L). Publishes decisions through the LBLOD/OSLO ecosystem and contributes data to the data space. Also acts as pilot city for testing enrichment and citizen-facing services.</p><p>Actively contributes to the human validation of the AI generated data.</p><p>Is pilot city for UC1 and UC2.</p></td><td valign="top">Participant, Contributor, User</td></tr><tr><td valign="top">City of Freiburg</td><td valign="top"><p>Original provider of LD&#x26;L data through its OParl-based Council and Citizen Information System (RIS). Supplies structured decision data and participates in pilot implementations and validation activities.</p><p>Actively contributes to the human validation of the AI generated data.</p><p>Is pilot city for UC1</p></td><td valign="top">Participant, Contributor, User</td></tr><tr><td valign="top">City of Bamberg</td><td valign="top"><p>Original provider of LD&#x26;L data. Supplies decisions and council information that are transformed from PDFs and other local publication formats into the data space.</p><p>Actively contributes to the human validation of the AI generated data.</p><p>Is pilot city for UC2</p></td><td valign="top">Participant, Contributor, User</td></tr><tr><td valign="top">University of Jena</td><td valign="top">Supports the technical translation development to linked open data. Provides knowledge, research capacity and prework regarding the translation progress and development of knowledge graphs for AI development</td><td valign="top">Expert</td></tr><tr><td valign="top">University of Applied Science Kehl</td><td valign="top">Provides insight on legal decision process in Germany. Scientific expert on public administration processes in Germany. Conducts citizen survey on UC1 and UC2.</td><td valign="top">Expert</td></tr><tr><td valign="top">Urban Software Institute</td><td valign="top">Provides open urban data platform for demonstration of the projects deliverable, technical support of solution development and administrational support of project management on German side of the project</td><td valign="top">Coordinator, Developer</td></tr><tr><td valign="top">Domain validators</td><td valign="top">Human experts who review and validate AI-generated annotations using the Human Validation interfaces. Their validation increases trustworthiness and quality of the data space.</td><td valign="top">User</td></tr><tr><td valign="top">Data engineers and technical operators</td><td valign="top">Configure pipelines, monitor services, maintain infrastructure and ensure availability of the data space.</td><td valign="top">Developer</td></tr><tr><td valign="top">Data consumers</td><td valign="top">External organizations and individuals that discover and consume DECIDe datasets through the data space, DCAT catalog and APIs.</td><td valign="top">User</td></tr></tbody></table>

### Shared processes

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th width="108.111083984375" valign="top">Process step</th><th width="81.5555419921875" valign="top">Individual</th><th width="77.8887939453125" valign="top">Shared</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">Create</td><td valign="top">✅</td><td valign="top"></td><td valign="top">Decisions are created independently by each local authority through its own governance and decision-making procedures. Ghent, Freiburg and Bamberg remain fully responsible for producing LD&#x26;L.</td></tr><tr><td valign="top">Store (source data)</td><td valign="top">✅</td><td valign="top"></td><td valign="top">Cities maintain their own publication infrastructures (LBLOD/OSLO, OParl, PDFs). The original data remains under responsibility of the source city.</td></tr><tr><td valign="top">Transform</td><td valign="top"></td><td valign="top">✅</td><td valign="top"><p>DECIDe pipelines harvest and normalize decisions into a common ELI representation. This is a common data-space activity executed through shared infrastructure.</p><p>Shared Restricted mobility zone and SDG classification and impact assessment are executed centrally through the Codelist Mapping Tool.</p></td></tr><tr><td valign="top">Combine</td><td valign="top"></td><td valign="top">✅</td><td valign="top"><p>Data from Ghent, Freiburg and Bamberg are linked with shared semantic standards, annotations and policies.</p><p>Decisions from all pilot cities are combined with SDG and impact annotations.</p><p>Decisions from all pilot cities are combined with the Open Street Map information.</p></td></tr><tr><td valign="top">Interpret</td><td valign="top"></td><td valign="top">✅</td><td valign="top">AI enrichment pipelines perform semantic interpretation through classification, NER, NEL and other annotation processes. Human Validation complements this with expert review.</td></tr><tr><td valign="top">Visualize</td><td valign="top"></td><td valign="top">✅</td><td valign="top">Shared applications such as the Policy Impact Report, Smart Search and GIS-ready outputs present the common data in reusable ways.</td></tr><tr><td valign="top">Use</td><td valign="top">✅</td><td valign="top">✅</td><td valign="top">Data is reused both within the shared data space and by individual cities for local business needs and applications.</td></tr></tbody></table>

### Resources

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

#### Human Resources

<table><thead><tr><th width="154.22216796875" valign="top">Resource</th><th width="138.111083984375" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">Data engineers</td><td valign="top">✅ Available</td><td valign="top">ABB operates and maintains the ingestion pipelines, semantic infrastructure, AI enrichment services and data-space components.</td></tr><tr><td valign="top">Domain experts / validators</td><td valign="top">✅ Available</td><td valign="top">Human Validation is a central DECIDe component and relies on subject-matter experts to review AI-generated annotations.</td></tr><tr><td valign="top">Local government staff</td><td valign="top">✅ Available</td><td valign="top">Ghent, Freiburg and Bamberg provide operational, policy and domain expertise. + Expertise from other partners</td></tr><tr><td valign="top">Additional validator capacity</td><td valign="top">⚠️ Future need</td><td valign="top">Scaling the data space beyond the pilot phase may require larger validation communities.</td></tr><tr><td valign="top">Citizens</td><td valign="top">✅ Available</td><td valign="top"></td></tr></tbody></table>

#### Knowledge & Expertise

<table><thead><tr><th width="165.33331298828125" valign="top">Resource</th><th width="153.3333740234375" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">Semantic modelling expertise</td><td valign="top">✅ Available</td><td valign="top">DECIDe builds on ELI, RDF, SKOS, DCAT, SHACL, ODRL and related semantic standards.</td></tr><tr><td valign="top">AI and NLP expertise</td><td valign="top">✅ Available</td><td valign="top">Required for classification, NER, NEL, enrichment and search services.</td></tr><tr><td valign="top">Local government expertise</td><td valign="top">✅ Available</td><td valign="top">Necessary to correctly interpret municipal decision-making processes and outputs.</td></tr><tr><td valign="top">Additional AI quality monitoring expertise</td><td valign="top">⚠️ Future need</td><td valign="top">Several future work items mention explainability, monitoring and model drift management.</td></tr><tr><td valign="top">Legal expertise</td><td valign="top">✅ Available</td><td valign="top">F.i. GDPR, AI Act, ...</td></tr></tbody></table>

#### Data Resources

<table data-search="false"><thead><tr><th width="179.77783203125" valign="top">Resource</th><th width="158" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">LD&#x26;L decisions</td><td valign="top">✅ Available</td><td valign="top">Supplied by Ghent, Freiburg and Bamberg.</td></tr><tr><td valign="top">Linked Open Data from LBLOD</td><td valign="top">✅ Available</td><td valign="top">Used for Ghent data ingestion.</td></tr><tr><td valign="top">OParl council data</td><td valign="top">✅ Available</td><td valign="top">Used for Freiburg data ingestion.</td></tr><tr><td valign="top">PDF-based decision collections</td><td valign="top">✅ Available</td><td valign="top">Used for Bamberg ingestion.</td></tr><tr><td valign="top">Human validation data</td><td valign="top">✅ Available</td><td valign="top">Generated through Human Validation interfaces.</td></tr><tr><td valign="top">Larger validated datasets</td><td valign="top">⚠️ Future need</td><td valign="top">Needed to train production-grade supervised models.</td></tr><tr><td valign="top">SDG/RMZ codelist and annotations</td><td valign="top">✅ Available</td><td valign="top"></td></tr><tr><td valign="top">Geopgraphic registries</td><td valign="top">✅ Available</td><td valign="top"></td></tr><tr><td valign="top">Area-level geographic datasets</td><td valign="top">⚠️ Future need</td><td valign="top">Needed for neighborhood and parcel-based RMZ detection.</td></tr><tr><td valign="top">Extended mobility reference data</td><td valign="top">⚠️ Future need</td><td valign="top">Needed for mobility-mode-aware filtering.</td></tr><tr><td valign="top">Additional linked sources beyond LD&#x26;L</td><td valign="top">⚠️ Future need</td><td valign="top">Identified as future expansion for higher-quality RMZ detection.</td></tr></tbody></table>

#### Technical Resources

<table data-search="false"><thead><tr><th width="229.7777099609375" valign="top">Resource</th><th width="180.22216796875" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">Virtuoso triplestore</td><td valign="top">✅ Available</td><td valign="top">Core repository for all linked data assets.</td></tr><tr><td valign="top">semantic.works infrastructure</td><td valign="top">✅ Available</td><td valign="top">Shared platform for all DECIDe services.</td></tr><tr><td valign="top">AI enrichment pipelines (NER, NEL, RMZ/SDG annotations)</td><td valign="top">✅ Available</td><td valign="top">Used across all use cases.</td></tr><tr><td valign="top">Human Validation platform</td><td valign="top">✅ Available</td><td valign="top">Shared component for all use cases.</td></tr><tr><td valign="top">Federation components (DCAT, DSP, VC, ODRL)</td><td valign="top">✅ Available</td><td valign="top">Enable data-space participation and governance.</td></tr><tr><td valign="top">Production-scale deployment capacity</td><td valign="top">⚠️ Future need</td><td valign="top">Would become increasingly important when scaling beyond the pilot cities.</td></tr><tr><td valign="top">Policy Impact Report frontend</td><td valign="top">✅ Available</td><td valign="top">Visualize SDG linking</td></tr><tr><td valign="top">Codelist mapping tool</td><td valign="top">✅ Available</td><td valign="top">Link topic/theme to LD&#x26;L</td></tr><tr><td valign="top">GIS/GEO services / location services</td><td valign="top">✅ Available</td><td valign="top">Pinpoint RMZ to map</td></tr><tr><td valign="top">Smart search</td><td valign="top">✅ Available</td><td valign="top">Giving answers to questions</td></tr></tbody></table>

#### Organizational Resources

<table><thead><tr><th width="226" valign="top">Resource</th><th width="155.333251953125" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">Pilot city network</td><td valign="top">✅ Available</td><td valign="top">Ghent, Freiburg and Bamberg provide real-world implementation environments.</td></tr><tr><td valign="top">Shared governance structure</td><td valign="top">✅ Available</td><td valign="top">Achieved through data-space architecture, trust framework and access policies.</td></tr><tr><td valign="top">Reusable standards ecosystem</td><td valign="top">✅ Available</td><td valign="top">Based on European semantic and data-space standards.</td></tr><tr><td valign="top">Additional city participation</td><td valign="top">⚠️ Future opportunity</td><td valign="top">Explicitly identified as a scaling objective.</td></tr></tbody></table>

#### Financial Resources

<table><thead><tr><th width="173.4444580078125" valign="top">Resource</th><th width="129.7777099609375" valign="top">Available?</th><th valign="top">Motivation</th></tr></thead><tbody><tr><td valign="top">DECIDe project funding</td><td valign="top">✅ Available</td><td valign="top">The pilot infrastructure and use cases were developed within the project framework.</td></tr><tr><td valign="top">Long-term operational funding</td><td valign="top">⚠️ Future need</td><td valign="top">Not explicitly defined in the write-ups and would need to be addressed for large-scale deployment.</td></tr></tbody></table>

#### Cost Categories

<table data-search="false"><thead><tr><th valign="top">Cost Category</th><th valign="top">Description</th></tr></thead><tbody><tr><td valign="top">Cost Category</td><td valign="top">Description</td></tr><tr><td valign="top">Infrastructure cost</td><td valign="top">Triplestore, semantic.works platform, search components, federation services and deployment environments.</td></tr><tr><td valign="top">Development cost</td><td valign="top">Development of ingestion pipelines, AI services, Human Validation interfaces and use-case applications.</td></tr><tr><td valign="top">Data management cost</td><td valign="top">Data harvesting, standardization and maintenance.</td></tr><tr><td valign="top">AI cost</td><td valign="top">Model training, model inference, annotation generation and continuous improvement</td></tr><tr><td valign="top">Validation cost</td><td valign="top">Human review of AI-generated annotations.</td></tr><tr><td valign="top">Operational cost</td><td valign="top">Hosting, monitoring, support and maintenance of the data space.</td></tr></tbody></table>

### Business case

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

#### Business model

Focus of the data space for sure at this point in the life cycle of the data space is **collaboration** (data pooling, resource pooling, delegation data control). Additonaly cooperation (data altruism, citizen science, reciprocity: data for data) might become more important over time but will probably prosper from a more mature data space.

No direct revenue model is described nor desired at this point. The project itself is funded as a pilot initiative, by the pilot cities, the Flemish government and the DS4SSCC subsidy program. Participating partners contribute data, expertise, testing effort, and operational participation.

Focus has actively been on open standards; open-source components; publication of open data; reuse by cities and ecosystems; public-service objectives. The cooperation relies on mutual value creation rather than direct compensation.

Future revenues that could be taken into account might be: software licences; API monetisation; data sales; subscription plans; usage-based billing. Technically all of this is possible. We have proven this with the implementation of the technical building blocks in order to do so. It will depend on the scaling of the data space how this will evolve.

More specifically partners of the data space are considering the following:

* Premium policy-analysis services: Ghent explicitly considers policy-framework matching a potential future premium dataspace service.
* Data-space subscriptions: Third-party organizations may consume advanced datasets or services
* White-label deployments: Cities could deploy local instances of UC1 or UC2 components
* AI-assisted public services: Search, policy reporting and regulatory services can become reusable municipal products

Some of the current services implemented might also (theoretically) become paid services for additional municipalities or consumers. On the other hand third party organizations might also be considered to join the data space in order to foster extra software and/or service development on top of the data available in the data space. In the event that such parnters join the data space a profitable revenuemodel might become necessary. At this point this is not the goal. The main goal is to make the data space sustainable without monetization of the services.

#### Beneficiaries

The **primary beneficiaries** are clearly identified:

**Cities** gain reduced manual processing; reduced duplication of data collection and processing; automated decision enrichment; GIS-ready mobility data; policy reporting capabilities; citizen-facing search services; benchmarking policy outcomes

**Public servants** have faster access to information and improved decision support; obtain decision insights; policy monitoring; easier access to legislation and decisions; reduced administrative workload

**Citizens and businesses** gain easier access to local decisions; better transparency; natural-language search; better discoverability of relevant legislation and administrative decisions; improved understanding of rules and subsidies

**Data-space participants/consumers** can discover high-quality linked datasets; reuse linked data; integrate applications on top of the data space

**Potential future beneficiaries** include:

**Regional/national governments** monitoring alignment with policy objectives

**Software vendors** integrating or building on top of DECIDe services

**Ecosystem participants** or organizations (f.i. academic organizations) consuming curated legislative datasets; using policy-compliance and impact-analysis services

### Governance model

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

See Gitbook page on [Governance](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/legal-and-governance/governance-proposition-after-the-projectphase).

### Implementation roadmap

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th valign="top">Aspect</th><th width="93.5555419921875" valign="top">Exploratory stage</th><th width="91.3333740234375" valign="top">Preparatory stage</th><th width="103.5555419921875" valign="top">Implementation stage</th><th width="90.2222900390625" valign="top">Operational stage</th><th width="100.7950439453125" valign="top">Scaling stage</th></tr></thead><tbody><tr><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr><tr><td valign="top">Financial</td><td valign="top"></td><td valign="top">✅</td><td valign="top">✅</td><td valign="top"></td><td valign="top"></td></tr><tr><td valign="top">Participation</td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top">✅</td><td valign="top"></td></tr><tr><td valign="top">Use cases</td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top">✅</td><td valign="top"></td></tr><tr><td valign="top">Technology</td><td valign="top"></td><td valign="top"></td><td valign="top"></td><td valign="top">✅</td><td valign="top">✅</td></tr><tr><td valign="top">Organization</td><td valign="top"></td><td valign="top"></td><td valign="top">✅</td><td valign="top"></td><td valign="top"></td></tr><tr><td valign="top">Legal</td><td valign="top"></td><td valign="top">✅</td><td valign="top"></td><td valign="top"></td><td valign="top"></td></tr></tbody></table>

**Financial**: at this point the LD\&L data space within the DECIDE project does not have actual use case that could generate revenues. The technical building blocks to enable such use cases are in place, hence the checkmark with “Implementation stage”. The DECIDE consortium should, over time, explore use case options that could generate revenue.

**Participation**: 7 partners (1 is mostly services provider + 2 are mostly expertisepartners). The market master/consortium should explore possibilities for extra partners.

Opportunities for scaling participation:

* the Flemish Environmental Agency
* All other flemish local governments: long term plan
* All software suppliers/potential service providers in Flanders
* Athumi
* The city of Bamberg other administrative offices within our city

**Use cases**: 3 actual use cases at the moment (besides the basic set up of the data space):

* [Policy Impact report](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report)
* [Restricted Mobility Zones](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones)
* [Smart Search](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc2-smart-search) (f.i. for subsidy opportunities)

**Opportunities** for scaling participation:

* Case with the Flemish Environmental Agency
* Case with the Local Products and Services Catalogue in Flanders
* Case within the One-Stop-Shop Strategy of the Flemish government
* All other flemish local governments: long term plan
* All software suppliers/potential service providers in Flanders
* Athumi
* The city of Bamberg is planning to make the tools available to other administrative offices within our city, depending on how we can manage our safety regulations
* The city of Ghent: the local annotations on decisions concerning thematical information (realized in the Probe project, and on locations (realized in the DECIDe project) will be published as linked open data. In this way they will become available in the DECIDE dataspace.

**Technology**:

* All technology implemented by the Agency of Home Affairs is open source and available for implementation, reuse and additions.
* The DSP setup by \[ui!] is not open source, but is fully based on open standards and open interfaces. The interface documentation is openly available on the project gitbook.

Technologywise scaling should focus on implementing components with additional data space partners and extra components from extra/other use cases and/or partners.

Also see all future work sections in the write-ups on the DECIDE gitbook for more detailed possible steps to take.

**Organization**: see [governance propostion](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/legal-and-governance/governance-proposition-after-the-projectphase)

**Legal**: the data space consortium should invest in more structural legal basis. This goes a bit hand in hand with organizational growth and governance maturity.

### Current status

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

## Why? (red)

### Context

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Local decisions and legislation (LD\&L) are the backbone of every action in local government, yet they remain among the hardest types of public data to work with at scale. Decisions are scattered across multiple repositories, managed by different actors, encoded in different formats, and rarely linked to the broader policy context they relate to. The result is a constant cycle of re-collection and duplication across government levels, friction for businesses and citizens trying to understand what applies to them, and limited capacity for governments themselves to track how their decisions connect to the goals they have committed to.

### Added value

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

The main goal is to establish data space for local decisions and legislation that enables governments, citizens, researchers and businesses to discover, share, analyse and reuse decision-related information across administrative boundaries. And in that way, to support evidence-based policymaking, transparency, interoperability and public innovation by making local decisions and related legislative information available as machine-readable, federated and reusable data.

**Local decisions are among the most important information assets produced by public administrations.** Yet they remain fragmented across thousands of local governments and are often difficult to discover, analyse or compare.

DECIDE addresses this challenge by:

* transforming local decisions into interoperable semantic data;
* linking decisions and decision data with URI's, policies and thematic datasets;
* showcasing how to enable AI-supported enrichment and analysis;
* supporting transparency and democratic accountability;
* showcasing how to enable reuse by public, private and research actors.



In the **current situation** of the data space, all of the incentives below have been triggering the set up of the LD\&L data space and the services build upon them.

* Create value from combined data and Development of new data driven products/services: increased reuse of public-sector information.
* UC0.1, UC1 and UC2 show case the possibilities of reuse of combined data. F.i. improved policy monitoring.
* Resource and cost sharing: Foundation for future data-space services
* Knowledge exchange and enabling innovation/joint innovation providing vendor independence through open standards and open-source architecture
* Empowering citizens & communities, including greater common good through better policy transparency

It might be relevant and necessary, for the future, for the data space to have a more clear focus on one or a few of these incentives. This might be a challenge for the governance authority to tackle.

For each of the current use cases, the added value is described in the write ups:

* [Basic set up of the data space](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space)
* [Policy impact report](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report) on linking the sustainable development goals to local decision and legislation
* Finding [restricted mobility zone](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones) in LD\&L and linking them to machine readable location data
* A [question and answering service](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc2-smart-search) to give insight, amongst others, in environment friendly measures that home owners good benefit from

### Motivation & objectives

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

DECIDe's ambition is to show what becomes possible when LD\&L is treated as a core data space component: structured, machine-readable, and linked to thematic, geographic, and policy data from other sources. The proposal sets out three goals for this: raising the quality of policy decisions by connecting them to a broader context; enabling more efficient and proactive service delivery by allowing decisions to power downstream use cases; and supporting democratic transparency by making local legislative activity accessible and legible to citizens, businesses, and other stakeholders.

The wanted deliverable is a functioning LD\&L data space grounded in the DS4SSCC Reference Architecture, built across three pilot cities –Ghent (Belgium), Freiburg (Germany), and Bamberg (Germany). The data space provides a shared infrastructure layer covering data ingestion and normalization, AI-assisted semantic enrichment, human oversight and validation, federated discovery, access policy enforcement, and identity and trust management.

## Technical (green)

### Data & datasources

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

See [https://catalog.decide.lblod.info/dcat/datasets](https://catalog.decide.lblod.info/dcat/datasets)

<table data-search="false"><thead><tr><th width="95.22216796875" valign="top">Key partner</th><th width="220.7777099609375" valign="top">Data demand / use case</th><th width="84.6666259765625" valign="top">Priority</th><th valign="top">Data supply / availability</th><th valign="top">Current status</th></tr></thead><tbody><tr><td valign="top">Ghent</td><td valign="top">NER/NEL location annotations, district widgets, Smart Search, Policy Impact Report, RMZ detection</td><td valign="top">★★★</td><td valign="top">LBLOD/OSLO linked open data already available via SPARQL and ELI pipelines</td><td valign="top">🟢 Available. Data successfully ingested and enriched through DECIDe.</td></tr><tr><td valign="top">Freiburg</td><td valign="top">RMZ detection, GIS integration, Policy Impact Report</td><td valign="top">★★★</td><td valign="top">OParl API provides structured council and decision data</td><td valign="top">🟢 Available. Data successfully ingested and enriched through DECIDe.</td></tr><tr><td valign="top">Bamberg</td><td valign="top">Smart Search, Policy Impact Report, PDF/JSON decision processing</td><td valign="top">★★★</td><td valign="top">Council decisions available through PDFs and later JSON feeds</td><td valign="top">🟢 Available. Data successfully ingested and enriched through DECIDe.</td></tr><tr><td valign="top">ABB / DECIDe Core Team</td><td valign="top">Data space operation, AI enrichment, provenance, interoperability, Policy Impact Report</td><td valign="top">★★★</td><td valign="top">Operates ingestion, enrichment, validation, triplestore and shared infrastructure</td><td valign="top">🟢 Operational. Central provider of services and shared data-space components.</td></tr><tr><td valign="top">Pilot city domain experts</td><td valign="top">Human validation of AI results (SDG, RMZ, entities, Smart Search)</td><td valign="top">★★☆</td><td valign="top">Validation interfaces available through HVT and UC applications</td><td valign="top">🟢 Available. Validation layer deployed across all use cases.</td></tr><tr><td valign="top">External data-space consumers</td><td valign="top">Reuse of enriched decisions, GIS layers, Smart Search, datasets</td><td valign="top">★★☆</td><td valign="top">Access through DCAT, SPARQL, APIs and published datasets</td><td valign="top">🟡 Available where datasets are published; adoption depends on external uptake.</td></tr></tbody></table>

### Interoperability

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

Worldwide standard definitions available and used. See technical write ups on the public [Gitbook](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-data-quality-manager). Look for the section on “Data standards” with each use case for specificalities.

### Technical concepts/models

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

#### Technical concepts

See “Final architecture” parts in the different [write ups in Gitbook](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-data-quality-manager).

#### MIM implementation

**Accessing Data (MIM0)**

C1 Machine-readable data is retrievable through the web

* LD\&L data is available through 3 standard web-based mechanism:
  * SPARQL endpoint
  * JSON REST API
  * Datadump (Turtle format)
* Metadata (DCAT) is also available through LDES to support federation scenario
* Payloads are machine readable (Linked Data or JSON)

C2 Access is structured and queryable

* JSON REST API supports query parameters for basic filtering (mu-resources service)
* SPARQL endpoint supports advanced querying and filtering

C3 Changes in data can be subscribed to

* JSON API and SPARQL endpoint allow subscription to changes using Polling (as a basic fallback)
* New datadumps are notified through LDES allowing federated catalog to automatically synchronize using LDES client

C4 Access to information can be restricted

* actor identification (MIM3.C5.R0) is supported through Wallet application holding the users identity with DID standard
* Actor roles (MIM6.C1.R4) are defined in the Verifiable Credential (cfr Dataspace Membership role).
* Authorization happens in two steps. First, the Verifiable Credential is verified and results in a session of the user. Second, the session allows the database to authorize the user through ODRL rules (sparql parser component).
* data asset-level policies (MIM6.C1.R4) are supported through ODRL: instances of a target class are odrl:Asset and part of a named graph in the triple store (odrl:AssetCollection)

**Interlinking Data (MIM1)**

C1: Entities are identified using unique identifiers: We use HTTP URIs for every resource. Also, we add a UUID property, which is mainly used by the JSON API component.

C2: Entities can be typed: Entities are typed using RDF triples. Multiple types are used in practice. Types are HTTP URIs. They can be retrieved using HTTP Dereferencing.

Some ontologies, such as ELI, are imported in the triple store, so we display its semantics in the human validation frontend ([https://github.com/lblod/app-decide/tree/development#sources-for-rdfscomments](https://github.com/lblod/app-decide/tree/development#sources-for-rdfscomments)).

C3: Entities can be (de)referenced

* Relations on an entity can be dereferenced, as they are HTTP URIs
* Additional semantics can be applied to an existing ontology: all data is stored and queryable in a triple store, where knowledge can easily be extended using other ontologies or custom extensions

**Representing Data (MIM2)**

C1: All entities included in data sources are described using consistent data models to enable interoperability for applications and systems. Recognised standardised data models are reused as much as possible (ELI, Schema.org, PROV-O, AIRO, Web Annotation Data Model)

C2: Different data models for the same entity that are used within a common data sharing ecosystem should be easily transformable into a common data model: LD\&L of the three partner cities are all mapped to the ELI-EP data model as common data model (https://europarl.github.io/eli-ep/) to enable interoperability on an European level.

C3: Any data and its associated metadata can be shared in open, standardised, data transport formats so that data can be exchanged and interpreted consistently by different tools and implementations. SHACL is used as machine-readable representation of the data model. Each data set in the DCAT LDES is accompagnied with a generated SHACL shape supporting validation, and discovery.

C4: When useful, it should be possible to create "Application Profiles" for a data model, so that use case specific attributes can be added or specified without changing the underlying data model\*. Our extensions do not impact existing models. Existing semantics are respected (domain and range must be valid according to its definition). Use case specific attributes are published under the ext: (http://mu.semte.ch/vocabularies/ext/) namespace.

Documentation is provided by generating a landing page from the generated SHACL shape.

**Securing Data (MIM6)**

C1: Data is only accessible to users that should have access to it. OIDC4VCI / OIDC4VP, OpenID for Verifiable Credential Issuance and Presentations is used as mechanism for giving access to users that should have access to it.

C2: Data accessed by users has not been altered. The Transport Layer Security (TLS) Protocol is used to ensure data does not get altered when retrieving data.

C3: Data accessed by users originates from a verified source. HTTPS/TLS is used as secure protocol when fetching the data sources in the pipelines.

**Exchanging Data (MIM3)**

C1: Governance rules for the data sharing ecosystem can be defined

Governance rules are described in multiple documents:

* Link to governancepart on gitbook: https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/legal-and-governance
* Repeatable data plan: https://app.gitbook.com/o/-MP9Yduzf5xu7wIebqPG/s/PzeOtGh2pfnNKyqa7G5w/decide-project/write-up-uc0.0-data-space/write-up-repeatable-data-plan
* Ethical data sharing agreement: https://app.gitbook.com/o/-MP9Yduzf5xu7wIebqPG/s/PzeOtGh2pfnNKyqa7G5w/decide-project/legal-and-governance/ethical-data-sharing-agreement

In future work, one rule book should be created.

C2: Terms and conditions for data sharing can be defined: Each public dataset in the DCAT is linked with license “Open Data Commons Public Domain Dedication and License (PDDL) v1.0”. Private datasets use ODRL to define terms and conditions. See [https://app.gitbook.com/o/-MP9Yduzf5xu7wIebqPG/s/PzeOtGh2pfnNKyqa7G5w/decide-project/write-up-uc0.0-data-space/write-up-odrl#odrl-use-to-describe-dataset-usage-restrictions](../write-up-uc0.0-data-space/write-up-odrl.md#odrl-use-to-describe-dataset-usage-restrictions)

C3: Compliance with data sharing terms and conditions can be validated: 'dataset access restrictions', i.e. whether users can just read or write to the dataset, can be validated using ODRL.

C4: Available data assets can be discovered: LDES with DCAT information is available by each partner city. Also, a federated LDES is provided, combining the DCAT of all three partner cities. The federated catalog exposes a User Interface for human discovery.

C5: Ecosystem participants can be discovered:

* Data users are be able to identify data providers who share data in a data ecosystem before accessing or using that data. This is done by using the federated catalog UI or by consuming the LDES DCAT feed. Data users remain anonymous by default when accessing the public data.
* Data providers reliably identify data users who want to access their data assets and verify their metadata before granting access. This is done using OIDC4VCI / OIDC4VP and ODRL.
* Ecosystem participants are currently not able to discover each other. This can be tackled in future work.

C6: Data exchange can be agreed upon: A journey “'Buying' access to a data space” has been provided to demonstrate how we can agree on a data exchange: [https://app.gitbook.com/o/-MP9Yduzf5xu7wIebqPG/s/PzeOtGh2pfnNKyqa7G5w/decide-project/write-up-uc0.0-data-space/write-up-verifiable-credentials#buying-access-to-a-data-space](../write-up-uc0.0-data-space/write-up-verifiable-credentials.md#buying-access-to-a-data-space)

C7: New data currently not available can be requested: Data users can only express their interest in data assets currently unavailable in the data sharing ecosystem by sending a request to the Flanders helpdesk.

**Geospatial Data (MIM7)**

C1: Cities and communities can easily transfer geospatial data between internal and external (including IoT-related) IT systems: Geospatial data can be queried through GeoSPARQL as standards-based web service interface. The geos plugin is enabled in Virtuoso. Example Geo queries are defined: https://github.com/lblod/app-decide/blob/development/docs/sparql-example-gent.md#geo-queries

C2: Cities and communities can integrate 2D and 3D geospatial data coming from a variety of sources, for example geodata and building information models, and share that data within and between them in an interoperable way: In DECIDe, geodata originates from OpenStreetMap. However, a variety of sources can be integrated as long as standards are followed: Location Vocabulary (LOCN), GeoSPARQL, and Well-Known Text (WKT) representation.

C3: Cities and communities can integrate geospatial data with other data that can provide further information about the context: Geospatial queries can easily be extended for extra context using SPARQL.

C4: Cities and communities have a consistent and persistent way of describing individual instances of all features, things or entities included in the geospatial data sources: Detected locations in decisions first have a custom URI. A link is added using skos:exactMatch by the Entity Linking service to the persistent URI in the geospatial data source (OSM). This allows human validation on the relation.

C5: Coordinate Reference Systems (CRS) used in data sharing are easily transformable into a common CRS: Geo data is by default stored and queryable in EPSG:4326. Using bif:st\_transform(?geom, target\_srid), the geodata can be reprojected other common CRS.

**Local Digital Twins (MIM8)**

We are currently on the category “Visualisation” of LDT.

Aspects of category “Analytics” are also touched upon:

* ‘descriptive’ capabilities using Named Entity Recognition, Named Entity Linking, and Codelist mapping AI services.
* ‘diagnostic’ capabilities using the Data Quality Manager

In future work, simulation would be an interesting route: the DECIDe services can be used when drafting legislation to simulate the data that will be generated (detect entities), and the impact the legislation will have has on local, or high-level policies (cfr SDG codelist mapping with impact).

C1: Provide access to datasets used within the LDT, including static, real-time, and simulation data and calculation models: Datasets of the use cases are available on the DCAT catalog, with datadumps and links to data services. This encompasses the LD\&L data from the data sources in ELI format, enrichments generated by AI, human validations, and provenance of the pipelines and its components.

C2: Exchange data with external systems and other LDTs using interoperable interfaces: Yes, see Exchanging Data (MIM3)

C3: Reuse deterministic or AI models across different domains, communities, use cases, and/or LDTs: The common metadata standard AIRO is used for describing the AI models.

All AI models are callable through the triple store as interface. The job-task data model is used to instruct and monitor the models. The input and output data is linked at task level. For example, a Named Entity Recognition task links with an ELI decision as input, and links with multiple annotations as output.

Safeguarding the use of LLMs is done by creating separate (but linked) annotations that need to be reviewed by humans.

C4: Coordinate and manage data, models, and processing workflows within an LDT (intra-LDT) and across LDTs (inter-LDT)

Workflows (pipelines) can be defined that connect data sources, data transformations, model execution, and outputs. This is demonstrated in all pipelines of DECIDe (oslo-to-eli, extract-entities from ELI...) . Chaining the output of one step as input of the next step is implemented in the job controller service. A dashboard is provided to monitor execution status of the workflows. Provenance is recorded: input and output data container of each task is stored, AIRO for the precise configuration of each AI service.

C5: Provide multiple visualisations of data and results (e.g. 2D, 3D, dashboards) from common underlying data and models for interaction and comparison: Simulation is not in scope of DECIDe, so scenario- and variant-assumptions are not available. Spatial scope is currently limited on a city-level selection. Visualisation is done with several dashboards: human validation tool (HVT), Policy Impact Report (PIR), and Smart Search.

C6: Ensure that data exchanged (and simulation outputs) within and between LDTs can be interpreted consistently through shared or mapped semantics and clear provenance: See MIM2 (Representing Data) and MIM7 (Geospatial Data). Documentation of the semantic data model(s) is provided in the write ups for each use case.

### Infrastructure characteristics

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

See “Final architecture” parts in the different [write ups in Gitbook](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-data-quality-manager).

## Impact

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

### Data Spaces Governance

We are for sure at “**Data Cooperation Canvas agreed upon**” (level 3) and with that past the “Validate” phase. We are currently working towards “Define” phase and “Additional use cases defined”.

### Data Value Creation

We are for sure at “**Data infrastructure is able to connect new sources**” (level 4) and with that past the “Define” phase. We should be starting to work towards “Implement” and “Service Level Agreements (#of datasources covered by contracts). But we are not yet at such a level of maturity that is would be valuable to work in this. This will be looked at in the upcoming months after a period of just using the data in the data space and finetuning the rules of the data space.

### Technical infrastructure

We are for sure at “**Full architecture defined and tested**” (level 4)and with that past the “Define” phase. We should be starting to work towards “Implement” and “Architecture fully working as an operational system. This will be part of the expansion of the data space with new partners and new use cases.
