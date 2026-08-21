# Write-up Repeatable Data Plan

Basic information on the DECIDE project can be found on the [website ](https://www.vlaanderen.be/lokaal-bestuur/digitale-transformatie/slimme-lokale-databronnen/over-decide)in Dutch, English and German. The webpage explains what the project acronym stands for, what the project is about, who the partners are, ... .

## Description UC/wanted deliverable

### Link to other deliverables

| Deliverable                                                                                                        | Activities                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **D1.9** Data Plan for the future                                                                                  | **T1.19** Work out repeatable data plan for future data space                                                                                                                                                                                                                     |
| **D4.1** Repeatable method for development and evaluation of use cases and overall governance of future data space | <p><strong>T4.1</strong> Work out repeatable method for evaluation, use case development and overall governance of future data space, based on both Data Cooperation Canvas as well as existing regional instruments, such as the Flemish Open City</p><p>Architecture Canvas</p> |

## Repeatable data plan for LD\&L data space (D1.9)

### Introduction

For every new partner in the data space or for every new dataset in the data space, some actions need to take part. If this is your first time reading about the local decision and legislation data space that was set up during the DECIDE project, please first read the information on our [website](https://www.vlaanderen.be/lokaal-bestuur/digitale-transformatie/slimme-lokale-databronnen/project-decide).

For the set up of the data space the data model canvas was used. This repeatable data plan gives a more practical and technical description of **the steps that need to be considered** to get a dataset data space ready.

To make a dataset accessible for the participants of the data space, the required data needs to be integrated from its data source into DECIDe's local source. This integration will involve multiple steps: which information is needed for the use case, what is the source of the data and how can it be retrieved, which data parts need enrichment by AI, which transformations will be needed, which taxonomies to choose, which AI model to choose, which ODRL rules to how, set to issue a specific VC for this dataset, which architectural components are specific for the dataset, etc.

Any (technical) alternative can be an option. In case of doubt on the interoperability, please [contact our technical team](mailto:digitaalABB@vlaanderen.be) to discuss.

Each step is briefly described below. For each use case and for each technical component we have **more extensive write-ups** on how we set them up on GitBook and GitHub.

{% hint style="info" %}
It is important that each new partner or new use case goes over these steps and **documents** how these steps are tackled. Documentation is required to give insight to other future new participants. And also so that the data space members keep on having an overview on lessons learned.
{% endhint %}

<mark style="color:red;">(Insert Figma)</mark>

### Step 1: Preparation

#### 1.1: Define the use case

Next to providing a description of the use case, also describe what value can be gained with the use case.

**Describe value creation plan**

For example:

* Use case suggests ecosystem-wide potential
* Use case benefits citizens, companies
* Contributes to AI-driven services which need larger, connected datasets
* Opportunities for service providers across data spaces
* Federation as a lever for broader inclusion

Overall business rationales for developing a data space:

* cost sharing
* joint innovation
* combined forces
* shared marketplace
* greater common good

In DECIDe, the four use cases were written out in the proposal and more in depth on GitBook: for who, what development is needed, why? For example, for UC1 of the DECIDe project it motivation is documented [here](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones#opportunity-problem-need-desire).

The value creation plan was part of the workshop in Ljubljana with the DS4SSCC team.

**Define competency questions**

For each use case, competency questions (CQs) should be defined to clarify the information need, hence the dataset / input data that is required.

For example, following CQs:

* CQ 1: _Which decisions are made?_
* CQ 2: _On which SDGs has the decision impact?_

In DECIDe, we wrote a "_Functionality (requirements)_" section for each use case. Here, the information need is described: which entities and properties are needed. For example, [UC1](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones#functionality-requirements) describes that location information and LD\&L decisions are needed.

**Mark which competency questions will be answered by AI**

Note per CQ whether the answer comes from a data source or will have to be generated by an AI service (Step 3).

* **You need a way to know whether the answer is right.** For every AI-answered CQ, define what a correct answer looks like and on which examples the result will be measured. See [Testing approach](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#testing-approach).
* **Decide whether AI-generated answers may be published without validation**, or whether they must pass human validation (Step 3.5) before they become part of the data space offering (Step 4.1).

#### 1.2: Locate and access the data source

The focus is on answering the following key questions:

* Where is the data? (location, endpoint, URL, etc.)
* How can be communicated with the data source? (concrete approach, API usage, frequency, etc.)
* How can this data source supply the information needed for the use case?

In DECIDe, we first had to locate the heterogeneous data sources of the partner cities (PDF, OParl API, etc.), which were then accessed using [pipelines](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines). Each data source was documented on Gitbook, so developers had a guideline how to access the data source. For example, the algorithm to access decision data in an OParl API:

{% file src="../../.gitbook/assets/datasource freiburg.pdf" %}

**Create a supply & demand matrix**

A data demand & supply matrix can be made to indicate how the data source can supply to the information need. Concrete examples can be documented, such as API URLs, SPARQL queries, data snippets.

| Demand & supply                                | Data source supply Ghent | Note                                                                                                                              |
| ---------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| CQ 1: _Which decisions are made?_              | High                     | Example SPARQL query to retrieve distinct decisions: [https://api.triplydb.com/s/dOPfZfFWC](https://api.triplydb.com/s/dOPfZfFWC) |
| CQ 2: _On which SDGs has the decision impact?_ | None                     | Needs to be generated with AI (in Step 3)                                                                                         |

In DECIDe, we wrote for each use case which data source can supply the information need of the use case. For example, UC1 needs unstructured decision text from the LD\&L decisions in the data space:

[https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones)

**Document what the AI enrichment needs as input**

For every row of the matrix that reads "needs to be generated with AI", the data source still has to supply the _input_ for that enrichment. Record:

* **Which text or fields** the enrichment needs (title, short description, full content), and whether the source actually provides them.
* **Which languages** occur in the source. Models are trained for a specific set of languages, and a language that was not in the training data is a known source of errors ([Testing generalization performance](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#testing-generalization-performance)). A multilingual set of sources therefore forces a route: either run the models on the source language directly, or translate to one pivot language first, run the models there, and project the annotations back onto the source-language text. DECIDe does the latter, translating to English, which buys one set of models for all languages, at the price of a translation model and a projection model.
* **Whether labeled or reference data exists** that can be reused to train or evaluate a model, for example an existing manual mapping of decisions to a codelist. This is a second kind of supply, next to the data itself, see [Supervised classification](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report#phase-2-supervised-classification-trained-classifier).

#### 1.3: Define data models and standards

This step considers how the information needs can be mapped onto (inter)national standards. This involves a separate process of selecting the appropriate data models (see "The process for selecting the appropriate data model per use case" lower on this page).

If the data model supports WKT and GeoSPARQL, you get GeoSPARQL querying functionality in the triplestore for free. Consumers can then retrieve and map location-bound decisions to GIS consumable formats

**Give AI-generated data a place in the data model**

Enriched data is not source data: it should remain distinguishable from the ingested data and must not alter it. Check that the chosen model provides for:

* the **body** (what was found) and the **target** (which expression, and which character positions within it),
* an optional **confidence** score, with a documented meaning,
* **provenance**: which service, model and model version produced the annotation, and whether a human validated it.

In DECIDe, we opted to model enriched data as web annotations on the source data, see [Web Annotation Model](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space#web-annotation-model).

### Step 2: Data services

This step is about integrating the required data sources (defined in Step 1.2) in the triplestore. This involves retrieving (step 2.1), transforming (step 2.2), and ingesting (step 2.3) the data in the triplestore. Various strategies can be implemented depending on the data source: implement all three steps in one microservice, or separate across different microservices.

List in the data plan which semantic components are needed for integrating the data source. For example, the [JSON to ELI](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#json-to-eli-pipeline) pipeline integrates the data source using one microservice, whereas the [PDF to ELI](write-up-uc0.0-pipelines/#pdf-to-eli-pipeline), [OParl to ELI](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#oparl-to-eli-pipeline) and [OSLO to ELI](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#oslo-to-eli-pipeline) pipelines use multiple microservices. Take into account that the data enrichment of Step 3 runs as a further pipeline after each of these, so a newly added source pipeline also has to be followed by the enrichment of the data it ingested, see [AI pipeline](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#ai-pipeline).

When integrating static Linked Data data sources, such as SKOS codelists, the [migrations service](https://github.com/mu-semtech/mu-migrations-service) which is part of the DECIDe application can be used. A Turtle or SPARQL INSERT file can be configured to import the static data sources as a one-time action, requiring no further modifications to the system.

### Step 3: Data enrichment

Some competency questions (Step 1.1) require data that may not be available in the data sources. Therefore, AI services can be used to enrich the ingested data. In the data plan, you should list the components that are needed for the AI enrichment. In DECIDe, we listed for each use case the components that are needed in the write ups. For example, in [UC1](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones#component-overview), we explained for each information need, why a certain component is needed. Below, we give a high-level overview of steps that are done by the AI services.

#### 3.1: Retrieve internal data

The first step is to fetch the data from the triplestore that needs enrichment. For example, the description of a decision.

AI services require interaction with the triplestore as any other microservice: load data to enrich, retrieve task information, save the results back. To prevent code repetition, we provide a [base image](https://github.com/semantic-ai/decide-ai-service-base) in Python with utility functions. One of these functions is the retrieval of a decision: its title, description, content, language, etc. This base image is then extended in the specialized AI services, described in the next steps.

**Choose an approach per information need**

When listing the components needed for the enrichment, be explicit about what kind of AI each information need actually calls for. Working down this list keeps cost, latency and failure modes under control:

| Approach                              | When it is the right choice                                                                            | Example in DECIDe                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **No AI**: a lookup, a rule, a parser | The answer is deterministic and the input is reasonably regular                                        | The date/period parser turns `"30/11/2024 until 05/12/2024"` into typed start and end dates without any model, see [Entity Recognition Task](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#entity-recognition-task)                                                                                                              |
| **A small, task-specific model**      | Examples exist or can be labeled, the task is span detection or classification, and the volume is high | Named entity recognition, entity refinement, location component parsing and cross-lingual entity projection all run on fine-tuned encoder models, see [Entity Recognition Task](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#entity-recognition-task)                                                                           |
| **An LLM**                            | The task needs language understanding, instruction following or tool use that no small model delivers  | Segmentation, codelist labeling, and the question answering of UC2, see [Segmentation Task](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#segmentation-task) and [UC2 Final AI components](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc2-smart-search#final-ai-components) |

An LLM (Generative Model) is the most expensive and the least predictable option, so a useful pattern is to use it once while building the solution rather than on every document: in UC0.1 an LLM generates labeled data, and a classifier trained on that data does the production work. The dependency then sits at build time, not at run time.

Two more things worth settling here:

* **What gets enriched.** Enriching everything is not free: every extra annotation type costs processing time and produces data that then has to be validated, governed and published.
* **How long documents are handled.** Models have a limited input size. Sending the full text to an LLM increases both cost and latency. Decide whether documents are chunked, how chunk results are aggregated back to document level, and whether the supporting chunk is kept as evidence for reviewers.

#### 3.2: Annotate unstructured text with Linked Open Data

This step involves the enrichment of text with structured data that is required for the use case. For example, detection of which geographical locations are mentioned in the unstructured text.

In DECIDe, text is fetched from ELI Work (title, short description) and Expression (content). With this text, a [Named Entity Recognition (NER) service](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#ai-pipeline) generates annotations linking parts in the text with Linked Data statements. By default, everything that can be mapped to an entity is saved in the triplestore. This way, information needs of other, potential use cases can already start using the enriched data.

#### 3.3: Link detected entities with registries

In your data plan, list which registries are important to link discovered entities with. The [Entity Linking](https://github.com/semantic-ai/entity-linking-backend) service can then be configured with the relevant registries. In DECIDe, we used an elastic search service in combination with nominatim as backends for the URIs of municipalities and geo-related look ups respectively, which is important for enriching decisions contained in PDFs. Knowing the municipality of a decision is a key filter for applications downstream.

Also decide what happens when nothing is found. If an entity is not in the registry, or the registry spells it differently, the correct outcome is possibly "not linked" and not a plausible-looking URI. DECIDe first tried an agentic LLM that wrote its own SPARQL queries for this, and moved away from it: the behavior was too non-deterministic and predictability was preferred over finding a few more links. If a model is used here after all, validate the proposed URI before storing it. See [Other explored AI components](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#other-explored-ai-components-and-why-not).

#### 3.4: Map decisions to codelists

Define which codelists you want to target. These codelists should be made available as linked data using [SKOS](https://www.w3.org/TR/skos-reference/) in the triplestore. In DECIDe we, for instance, map decisions to a codelist concerning [SDGs](https://github.com/lblod/app-decide/blob/development/config/migrations/add-sdg-codelist/20260310123608-add-simple-sdg-codelist.ttl).

The quality of the mapping can depend less on the model than on the codelist itself:

* **Write usable `skos:definition` descriptions.** In UC0.1 an LLM assigns labels based on these descriptions, and a classifier is then trained on those labels. Weak descriptions produce weak labels, and a classifier trained on weak labels stays weak, no matter how good the model is.
* **Expect class imbalance.** Rare concepts get almost no examples, so results will be poor for exactly those concepts.
* **Decide whether impact is recorded as well.** The annotation model allows a positive, negative, neutral or unknown impact next to the concept. Agents that cannot distinguish neutral from unknown should report unknown, see [UC0.1 data standards](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report#data-standards).

See [Cold start classification](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report#phase-1-cold-start-classification-zero-shot-llm).

#### 3.5: Validate enriched data

The data plan should describe whether the enriched data will be validated by domain experts, and how this data will be reused for better services. In DECIDe, we created a [Human Validation (HV)](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-human-validation-hv) tool to capture feedback on the AI annotations.

In addition to AI governance and assessing the performance of the AI services, having validation data can be used to improve the AI services. For instance in UC0.1, the feedback can be used to enrich the data used to train classification models for codelist mapping.

Whether or not there is human validation, the provenance and confidence foreseen in the data model (Step 1.3) have to be filled in for real, and have to survive a rerun. Without them, AI output cannot be re-evaluated after a model change and cannot be filtered by consumers in Step 4. In DECIDe the RAG service stores the question, the response, the prompt and the model used.

Plan the validation as recurring work rather than a one-off acceptance test. Validated annotations from the HVT are the natural evaluation set for the whole chain and the fastest way to see which step actually causes an error. Note that this is also where the enriched data meets the general validation setup of the application: SHACL shapes can check that annotations are structurally correct, but not that they are true, see [Write-up Data Quality Manager](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-data-quality-manager).

### Step 4: Data governance

This section describes different aspects to consider to share the data generated in steps 2 and 3 with other participants in the data space.

#### 4.1: Create and publish Data Space offering

The data plan must define the datasets that will be offered to the data space.

Provide metadata (title, publisher) in DCAT so this can be federated across the data space. Configure a SPARQL INSERT query in the [`publish dataset`](https://github.com/lblod/app-decide/tree/development/scripts/project/publish_dataset/queries) script to scope the data that will be offered. The script will automatically generate [basic DCAT metadata](https://github.com/lblod/app-decide/tree/development/scripts/project/publish_dataset/templates) for the dataset, but can be extended if needed.

The data plan must describe which API will be provided. Choose whether a public API can be used, or a private API. In DECIDe, public datasets are made available through a SPARQL endpoint, and with a datadump. For private datasets, a private SPARQL endpoint (`/api/private/sparql`) was set up, requiring a Bearer token plus a `dsp-role` header carrying the user's government URI, enabling scoped responses (e.g., a Ghent user receives only Ghent data).

The offerings (DCAT and ODRL) are automatically published in the data space using an LDES feed. No extra actions are required in the data plan. Note, any DCAT and ODRL data published on the LDES feed are also automatically consumed and republished by the [Federating Catalog](https://github.com/lblod/app-decide-federating-catalog). Only when data is published on a new LDES feed, extra actions are required to extend the Federating Catalog to consume this new feed.

**Be explicit about AI-generated content in the offering**

Decide in the data plan:

* **Whether AI-generated data is part of the offering at all**, and if so, whether it is offered as a separate dataset or distribution, so that consumers can take the source data without the inferences.
* **What the DCAT metadata states about it**: that the dataset contains AI-generated annotations, how they were produced, and whether they were validated by a domain expert. See [Write-up DCAT](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-dcat#data-standards) for the metadata model used, and [Annotation provenance](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines/write-up-provenance#annotation-provenance) for how the provenance of an annotation is recorded.
* **Whether unvalidated annotations are published**, and if so, whether consumers can filter on confidence or on validation status.

#### 4.2: Define policies

Describe in the data plan whether access or usage policies are applicable on the dataset. If this is the case, define an [ODRL Offer](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-dsp#odrl-offers-for-decide) for each dataset. Such Offers van be imported to the application as migrations. In DECIDe, we focus on access policies where a user or machine needs to be logged in using [Verifiable Credentials](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-verifiable-credentials) or tokens.

An ODRL Offer for a dataset (`odrl:Offer`, `ext:RestrictedPolicy`) contains following elements:

* **Action**: `odrl:read` (permitted), `odrl:read` + `odrl:modify` (prohibited by default)
* **Assignee**: `licensedUserCollection` — only users in this collection get read permission
* **Assigner**: `http://ds.decide.lblod.info`
* **Conflict handling**: `odrl:perm` (permissions override prohibitions where explicitly granted)

In order for your application to enforce the rules contained in the defined Offers, they should also be included in the [ODRL authorization policy](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-odrl#using-odrl-for-authorization-policies) of the [mu-authorization](https://github.com/mu-semtech/sparql-parser/tree/feature/odrl-configuration) service.

**Policies and AI: two directions to check**

* **Data going out to a model.** If an external model provider is used, the text sent in the prompt leaves your infrastructure. Check that the policies on the input data allow this, in particular for private data or data containing personal information. Once data has left, you no longer control where it is replicated or reused. This is one of the main reasons to make the local-versus-cloud choice per component in Step 5.1.
* **Output coming back from a model.** Check the terms of the model provider for restrictions on the generated output, for example on using it to train other models and on redistributing it as open data.

#### 4.3: Setup DID

If you want to share the dataset to users (not machines) with a Verifiable Credential, a DID needs to be setup. Choose which type of DID method you want to support in your data plan.

In DECIDe, we [mandate](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-verifiable-credentials#identity-methods) the use of `did:web` for the publisher of datasets.

#### 4.4: Define verifiable credentials

Define the type of Verifiable credentials that need to be issued and verified using the credential format ([VCT](https://ds.decide.lblod.info/.well-known/vct/vc-issuer)) as defined by the DECIDe issuer.

Describe which issuer service will be used and how this will be configured. For example, the [DECIDe open-source issuer service](https://github.com/lblod/oid4vc-login-service) can be reused and configured.

#### 4.5: Set up authorization process to receive verifiable credentials

Describe in the data plan how the credential(s) will be issued (e.g., marketplace, onboarding process). In DECIDE, any user logged in with ACM/IDM (but any OAuth2 solution works) can request to issue a "Data Space Membership credential".

### Step 5: Deployment

#### 5.1: Deploy software components

Provide an architecture of the needed technical components, which are specific for the input data / use case. Provide use case-specific Docker configurations.

In DECIDe, we provide a Docker configuration [per partner](https://github.com/lblod/app-decide/tree/development/docs#partner-configurations).

Provide a strategy how the application will be made accessible to the public. In DECIDe, we use [app-letsencrypt](https://github.com/redpencilio/app-letsencrypt) which is based on a default [Nginx-based service](https://github.com/nginx-proxy/acme-companion) that configures DNS with Letsencrypt for facilitating public access.

[Configure](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#job-controller) the (scheduled) job controller services to orchestrate the [data services](write-up-repeatable-data-plan.md#step-2-data-services) and [data enrichment](write-up-repeatable-data-plan.md#step-3-data-enrichment) pipelines.

**Deploying AI components**

AI services need more than a container image:

* **Model artifacts.** Pin an explicit model version instead of a floating tag, and decide whether the weights are baked into the image or downloaded on first run. A service that silently pulls the newest model at start-up changes the meaning of data you have already published, and makes a rollback impossible.
* **Hardware per component.** Weigh GPU against CPU per component instead of for the application as a whole. A locally hosted LLM realistically needs a GPU, while smaller models are faster on one but stay workable on CPU. If several services have to share the same GPU they end up queueing behind each other, so running the smaller models on CPU can reduce the overall waiting time.
* **Supporting infrastructure.** The approaches chosen in Step 3 can require components of their own: a search index, a vector store, a geocoding service. Each of those has to be deployed, sized and maintained like any other component of the application. In DECIDe, for example, the AI services depend on mu-search / Elasticsearch, and the embedding-based search needs a vector index kept in sync with the triplestore.

**Running models locally or in the cloud**

Record per AI component whether its model runs on your own infrastructure or is called as an external service. This is a deployment decision rather than an architectural one, and it is made per component rather than once for the whole application. DECIDe does both: the fine-tuned encoder models, the embedding model and the question answering of UC2 run locally (the [embedding service](https://github.com/semantic-ai/embedding-service) uses a local [ollama](https://ollama.com/) container, and UC2 answers from a locally hosted Mistral Nemo), while segmentation and the initial zero-shot codelist labeling call an external Mistral model, because smaller local models turned out not to be reliable enough for those tasks.

| Criterion               | Local / self-hosted                                                                                | External / cloud API                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Data sovereignty        | Input never leaves your infrastructure                                                             | Prompt content leaves your infrastructure; check the policies of Step 4.2 |
| Quality on hard tasks   | Limited: smaller models struggle with strict formatting, multi-step tool use and SPARQL generation | Usually clearly better and faster on exactly those tasks                  |
| Cost profile            | Up-front hardware; a 70B model needs serious hardware to answer within a reasonable time           | Pay per token, growing with volume and with document length               |
| Reproducibility         | You control the version and keep it as long as you want                                            | Providers deprecate and silently update models                            |
| Run-time dependency     | None                                                                                               | Availability, rate limits and pricing of a third party                    |
| Environmental footprint | Visible and yours to size                                                                          | Invisible, but real and part of the assessment                            |

Practical guidance:

* **Keep the provider swappable.** The translation task ships with two configurable backends, Mistral Nemo 12B running locally via Ollama or the European Commission's eTranslation API, and UC2 reaches its LLM through an abstraction precisely to avoid vendor lock-in. That keeps local-or-cloud a configuration choice instead of an architectural one.
* **Prefer the cloud at build time over the cloud at run time.** This is the UC0.1 pattern described in Step 3: an external LLM produces the labeled data, and a locally trained classifier does the production work.
* **Reduce what the model has to do before concluding that a big model is needed.** Deterministic search instead of open-ended query generation, and fewer tools per call, move work out of the model and bring smaller local models, or no model at all, within reach. This is the route DECIDe took for entity linking, where the agentic approach was replaced by deterministic search.
* **Fully open models are a real option for simple, structured tasks.** Apertus 8B and 70B were evaluated in DECIDe: usable for cleaning, formatting, extraction and single-step tool use, not reliable for strict output formats or multi-step reasoning.
* **Check what your organization allows before you design around a provider.** Next to the data-level policies of Step 4.2, many organizations restrict _which_ external providers may be used at all, and expect the choice to be motivated and approved. Find out early whether a list of permitted providers, internal AI guidelines or a reference architecture apply, because any of them can rule out an otherwise sensible design.

### 5.2: Maintenance

A data plan should mention the next actions that are required for a successful deployment.

For AI components, the recurring actions are:

* **Reprocessing.** A new model version, a changed prompt or a rewritten codelist description changes the output. Decide who judges that the existing backlog needs reprocessing, and how often that is expected to happen. The enrichment pipeline has to support it: a rerun should be triggerable per dataset or per decision without a full reingest, and it produces a new set of annotations rather than overwriting the earlier one. Keeping both is what makes it possible to compare model versions and it protects annotations a domain expert has already validated. What does need attention is that consumers can tell which set is the current one (Step 4.1).
* **Retraining.** Corrections captured in the human validation tool are the input for the next model version. Decide who analyzes the error patterns, how often, and what the trigger is for training a new version, see [Final AI components](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.1-policy-impact-report#final-ai-components-and-why-if-any).
* **New sources and languages.** A new partner possibly brings a language or a document layout the models have not seen.
* **External dependencies.** If an external model is used, track deprecations and version changes on the provider's side, and define what the pipeline does when the API is unavailable or rate limited: fail the task and retry later, or fall back to a local model.

#### 5.3: Monitoring

The data plan must mention how logs will be monitored. In DECIDe, we use the [app-http-logger](https://github.com/redpencilio/app-http-logger) service. Also, Prometheus [Node Exporter](https://github.com/prometheus/node_exporter) is used to monitor, for example, the memory and disk usage on the server on which the application is deployed.

Furthermore, the [DECIDe open-source issuer service](https://github.com/lblod/oid4vc-login-service) logs any attempts to issue or verify a verifiable credential to the triplestore, along with the outcome of the attempt.

Finally, to initiate and monitor the [data services](write-up-repeatable-data-plan.md#step-2-data-services) and [data enrichment](write-up-repeatable-data-plan.md#step-3-data-enrichment) pipelines, a configuration must be added to the [harvester frontend](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines#harvester-frontend).

**Monitoring AI services**

AI services can fail quietly: they keep producing output, but worse output. Per enrichment task, log at minimum which model and version ran, whether it failed, and the confidence of the annotations produced. A drop in average confidence, or a rise in decisions that no longer get mapped to any codelist concept, is usually the first visible sign that a source has changed. For external models, also monitor errors, rate limits and token usage, since that is where cost surprises appear.

#### 5.4: Backups

A data plan should describe the strategy taken to, ideally regularly, backup the data gathered during step 2 as well as the data added during the enrichment in step 3.

The enriched data of Step 3 has to be backed up as data in its own right, and not treated as something that can simply be recomputed when needed. Rerunning the enrichment produces a new version rather than a restore: LLM calls are not deterministic, and an external provider may have changed the model in the meantime, so the annotations that come back are not the ones that were lost. Even output from the trained models only returns identically for as long as that exact model version is still available.

For the same reason, back up what produced the data next to the data itself: the model artifacts (or a reference to a pinned version that is still obtainable), the training and evaluation datasets, and the prompts and service configuration. Give the human validation feedback the most attention, expert corrections cannot be recovered easily.

For the DECIDe application, we backup of the entire triplestore each night, along with additional files generated by the services as part of their functionality. We use [app-borgmatic](https://github.com/redpencilio/app-borgmatic) to automate this process. An instance of this application is deployed on the same server alongside the DECIDe application. Each night this applications (1) instructs the DECIDe triplestore to create datadump of its entire contents; and (2) transfers this datadump, along with additional files produced by the DECIDe application, to an external storage box.

#### 5.5: Support & documentation

A data plan must explicitly mention the conclusions that are made during analysis and discussions.

For the AI components, document at least a model inventory: which model performs which task, which version is deployed, where it comes from, and how it scored on which evaluation set. In DECIDe the first three are registered automatically in the triplestore: each AI service is recorded as an agent with a hash of its configuration and the model it calls, see [Agent and configuration registration](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-uc0.0-pipelines/write-up-provenance#agent-and-configuration-registration). Also decide how the use of AI is documented to end users of the service: annotations that look like facts but are inferred should be recognizable as such in the interface, not only in the data.

### The process for selecting the appropriate data model per use case

Each use case entails a certain information need. Before information can be stored in the local source, we need to align with semantic data models (or standards) so the data is interoperable with other stakeholders in the data space. This document describes the steps that need to be considered when aligning the information need of a use case with existing standards, for the following, simplified use case:

_As a local government, I want to share my decisions within the Legislative Data Space. (based on use case 0.0)_

The steps described below are inspired by the guide book for the analyst profile from Open Standards for Linked Organizations (OSLO): [https://informatievlaanderen.github.io/handreikingOslo/analist.html#subcategorie-sub1](https://informatievlaanderen.github.io/handreikingOslo/analist.html#subcategorie-sub1)

We only focus on the first step of developing a domain model, aligned with OSLO. The other step of creating an information model (application profile) is out of scope of this document.

#### Step 0 (should be tackled when defining the use case): Define competency questions for the use case

For each use case, competency questions (CQs) should be defined to clarify the information need. These questions are not an exhaustive list of what our data model should be able to tackle in the end. They only serve to scope the information need. More information can be found in the Ontology 101 paper: [https://protege.stanford.edu/publications/ontology\_development/ontology101.pdf](https://protege.stanford.edu/publications/ontology_development/ontology101.pdf)

For the example use case mentioned above, we see following CQs:

CQ 1: _Which decisions are made?_

_CQ 2: Who made the decision?_

_CQ 3: What is the content (as a text blob) of the decision?_

#### Step 1: Develop a domain model for the use case

Identify the top-level objects, properties and relationships that the use case will encounter, based on the competency questions.

A relationship has an object as range.

A property has a datatype as range (date time, integer, boolean, (language) string).

CQ 1: _Which decisions are made?_

* Decision (Object)

_CQ 2: Who made the decision?_

* Decision: taken by / decided by -> Organization (Relationship between Decision and an Organization)

_CQ 3: What is the content (as a text blob) of the decision?_

* Decision: content -> String (Property of Decision that expects a string)

#### Step 2: Be acquainted with current data standards

Have a look at all the standards that are listed on this Gitbook for each use case.&#x20;

#### Step 3: Identify and eliminate overlaps and differences between standards and domain model

The Simple Knowledge Organization System (SKOS) ontology can be used to assist with this. SKOS offers a set of relationships to indicate the degree to which two terms are semantically equivalent:

* Exact match: two terms are even more equivalent than in a close match and there is a transitive relationship.
* Related match: the terms are related but there is no hierarchical relationship.
* Broad match: two terms are related, but one term is more generic than the other. E.g. mammals skos:broader animals. “Broader” should in fact be read as “has a broader concept”.
* Narrow match: inverse of ‘broad match’, e.g. animals skos:narrower mammals.

'Close match' is stroked through, because they is not useful when selecting data models: the match cannot be used.When is a term suitable for reuse, and can this be done in a semantically correct manner? To check this, you can go through the following checklist:

1. Does the definition correspond to my own definition for this term?
2. Does the term have a specific domain restriction? This indicates in which domain a particular attribute may be used. If so, does this correspond to my domain model?
3. Does the term have a specific ‘range’ restriction? This indicates the nature of the values that this attribute is expected to have. If so, does this correspond to the values I expect for my term? E.g. a date, a string, a value from a specific code list.

If these questions show that this term corresponds in definition and use to that in your own domain model, they are semantically equivalent. The label and definition can be translated into Dutch for reuse. If no reusable term was found, it is important to document a label and definition yourself for exchange with third parties.

| Domain model                                     | Maps        | Data model                                                                                                                   |
| ------------------------------------------------ | ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Decision                                         | Exact match | LegalResource & LegalExpression (ELI)                                                                                        |
| Decision: taken by / decided by -> Organization  | Exact match | ​[http://data.europa.eu/eli/ontology#passed\_by](http://data.europa.eu/eli/ontology#passed_by)​                              |
| Decision: content (text blob) -> Language String | Exact match | ​[https://data.europarl.europa.eu/def/epvoc#expressionContent](https://data.europarl.europa.eu/def/epvoc#expressionContent)​ |

#### Step 4: Find a solution for unmapped terms <a href="#step-4-find-a-solution-for-unmapped-terms" id="step-4-find-a-solution-for-unmapped-terms"></a>

Unmapped terms must be created ourselves.At ABB (Flemish government), we do this by creating an ontology in Turtle format here: [https://github.com/lblod/vocabularies](https://github.com/lblod/vocabularies)​A similar approach is taken by the city of Ghent: [https://github.com/StadGent/Vocabularies](https://github.com/StadGent/Vocabularies) and University of Jena: [https://github.com/fusion-jena/GerPS-Process/tree/main/ontology](https://github.com/fusion-jena/GerPS-Process/tree/main/ontology)

### Testing

The repeatable data plan has been tested when adding a fourth data ingestion pipeline: [write-up-uc0.0-pipelines.md](write-up-uc0.0-data-space/write-up-uc0.0-pipelines.md#final-architecture-and-why "mention") from the city of Bamberg. This pipeline was introduced at the end of the project and thus a good candidate for evaluating the repeatable data plan whether all steps are mentioned to integrate a new dataset.

The Flanders Environment Agency (VMM) is interested in reusing the codelist mapping tooling for mapping policies of local government to their more high-level policies. In the future, VMM can try out the steps of the plan for their deployment.

## Repeatable method evaluation of use cases (D4.1)

### Introduction and Context

Adding to the more practical methode describe in the repeatable data plan above, a structured method for the evaluation and development of use cases and overall governance of a future dataspace is valuable too. During the DECIDE project, we used both the Data Cooperation Canvas and the Flemish VLOCA Canvas to do so. Hence our method is grounded in two complementary frameworks:

* The [VLOCA Canvas](https://www.vlaanderen.be/lokaal-bestuur/digitale-transformatie/vloca/het-vloca-canvas) (Vlaamse Open City Architectuur / Flemish Open City Architecture): the established instrument for digitalization projects of Flemish local authorities, maintained by the Flemish government (Agentschap Binnenlands Bestuur). The canvas consists of nine structured tiles that together describe a solution end-to-end.
* The [DCC](https://www.datacooperationcanvas.eu/canvas/intro) (Data Cooperation Canvas): a European framework designed to support organisations in setting up data cooperation, data sharing governance, and dataspace design.

The core principle is clear: within Flanders, the VLOCA Canvas remains the uniform reference language for all digitalization projects, ensuring consistency and alignment with existing Flemish instruments. While using the VLOCA Canvas, the DCC is used as a targeted complement, primarily within the 'Process, data & technology' and 'Governance' tiles, and adds value in areas where the VLOCA Canvas operates at a higher level of abstraction than dataspace-specific projects require.

This document concludes with a full application of the combined method to Use Case 1: Restricted Mobility Zones, developed in the context of the DECIDE project.

### The VLOCA Canvas

Since most information on the VLOCA Canvas is in Dutch, it might be relevant to give some more insight on that topic.

#### Purpose and Positioning of the VLOCA Canvas

The VLOCA Canvas is the cornerstone of the VLOCA methodology. It brings all components of a solution together in nine structured tiles and helps to capture insights, analyses and decisions in a structured way. Starting from a clearly defined need, it guides a project team step by step towards a solution for public service delivery that is applicable across local authorities in Flanders.

Critically, VLOCA does not start from a technology or application, but from a societal or organisational need. The methodology supports project teams to:

* Think strategically about the digitalisation of public services;
* Analyse a need and possible solution directions in a structured way;
* Develop a concept for a solution through co-creation;
* Collaborate between local authorities and supra-local government;
* Make conscious choices before implementation;
* Reflect on governance during and after realisation.

The VLOCA Canvas is not a brainstorming instrument. It is rather a documentation instrument and it helps project teams to reach their target. It is filled in progressively based on conversations with stakeholders, analyses and research, and joint reflection and decision-making. The canvas evolves throughout the full lifecycle of an initiative: at the start to define the need, during concept development to compare solution directions, during implementation to maintain direction, and after realisation as a reference document for knowledge sharing and reuse.

#### The Five Principles of the VLOCA Canvas

Every solution reviewed within VLOCA is assessed against five cross-cutting principles that run throughout all nine tiles:

* Scalability: solutions can grow and expand beyond one organisation or team;
* Sustainability: solutions continue to offer benefits over the long term;
* Interoperability: systems and processes work seamlessly together at legal, semantic, technical and organisational level;
* Security: attention to privacy of personal data and technological security;
* Inclusivity: user-friendliness and accessibility for all citizens and users.

#### The Nine Tiles of the VLOCA Canvas

The VLOCA Canvas describes a solution through nine interconnected tiles:

<figure><img src="../../.gitbook/assets/image (27).png" alt="Figure - VLOCA Canvas: 9-tile structure"><figcaption></figcaption></figure>

<table data-header-hidden data-search="false"><thead><tr><th width="201.7777099609375" valign="top">Tile</th><th valign="top">Description</th></tr></thead><tbody><tr><td valign="top">1. Challenges (Uitdagingen)</td><td valign="top">The starting point of every VLOCA trajectory. Describes what problem or opportunity is at stake, what societal or organisational context plays a role, and why it is relevant to address this challenge. Forms the anchor for all other tiles.</td></tr><tr><td valign="top">2. Value (Waarde)</td><td valign="top">Describes the value the solution creates or should create: better public services for citizens or organisations, more efficient internal operations, supported policy goals, or broader societal value. Makes clear why the solution is important.</td></tr><tr><td valign="top">3. Solution (Oplossing)</td><td valign="top">Defines the solution direction at a high level: the chosen approach, possible scenarios, the applications and building blocks that together form the solution, key risks and preconditions, and the guiding role of legislation. Sets the direction for the further development of processes, technology, governance and efforts.</td></tr><tr><td valign="top">4. Stakeholders (Belanghebbenden)</td><td valign="top">Maps all persons and organisations involved in or affected by the ultimate solution: those who use the service, those involved in delivering it, and those impacted by it. The goal is to make clear who is involved and whose needs and expectations must be taken into account.</td></tr><tr><td valign="top">5. Impact</td><td valign="top">Describes the concrete change the solution must realise and for whom that change is intended. Defines success factors and KPIs by looking at the current situation (baseline measurement), the desired result after implementation, and how the success of the solution can be tracked. Makes explicit what impact the solution must have and how it can be established that the solution genuinely delivers improvement.</td></tr><tr><td valign="top">6. Process, Data &#x26; Technology (Proces, data &#x26; technologie)</td><td valign="top">Describes how the solution is supported by processes, data and technology. Covers: the processes needed to deliver the service; the data used, shared or managed; the building blocks from which the solution is constructed; the building stones (generic, supra-local components) to which the solution connects; interactions between systems and processes; standards followed to support interoperability; and legal implications regarding privacy-first and the once-only principle. Stays at conceptual level (it is about what components are needed and how they connect, not about specific vendors or technical details).</td></tr><tr><td valign="top">7. Governance</td><td valign="top">Describes how the solution is organised, managed and evolved. Since solutions often come about in an inter-authority context, it is important to make clear agreements about cooperation, responsibilities and decision-making. Covers: how ownership of the solution is organised; roles and responsibilities of involved authorities and stakeholders; how decisions are made about the solution; how inter-authority cooperation is organised; and how management and further development of the solution are organised. In an inter-authority context, ownership of a solution preferably does not rest with a single authority, but is organised in a way that enables cooperation and shared responsibility.</td></tr><tr><td valign="top">8. Efforts (Inspanningen)</td><td valign="top">Describes what efforts are needed to realise and operate the solution. Covers: the high-level project planning for realisation; the most important work packages needed to realise the solution; the CAPEX (investment costs) for development and implementation; and the OPEX (operational costs) for management and operation of the solution.</td></tr><tr><td valign="top">9. Guidance (Handvaten)</td><td valign="top">Captures practical insights and recommendations that emerge from the trajectory: lessons learned, good practices, implementation recommendations for other authorities, and a communication plan to inform stakeholders and create support. The goal is to ensure that knowledge gained does not get lost but can be shared and reused by other authorities, contributing to faster implementation, better cooperation and less duplication of effort across Flanders.</td></tr></tbody></table>

#### Strengths of the VLOCA Canvas

* Established instrument with a uniform language across all Flemish digitalisation projects;
* Governance is an explicit, dedicated tile not an afterthought;
* Strong emphasis on co-creation and stakeholder engagement throughout the trajectory;
* Output-oriented: trajectory results in a concept document (reference document) with roadmap and architectural model;
* Designed for reuse: the Guidance tile explicitly captures knowledge for other authorities;
* Supported by the Flemish government with templates, facilitation guides and a knowledge hub.

#### Limitations for Dataspace-Specific Projects of the VLOCA Canvas

* The 'Process, Data & Technology' tile stays at conceptual level: deliberate by design, but insufficient for specifying dataspace-specific connector architectures, protocols and trust frameworks;
* Governance tile covers inter-authority cooperation well, but does not distinguish between data roles (provider, consumer, intermediary) relevant for data space design;
* No built-in maturity assessment or readiness scale for data sharing;
* No explicit mechanism for mapping data ownership, licensing conditions, or data sovereignty arrangements;
* No structured framework for cross-organisational data workflows (shared processes) beyond the authority collaboration context.

### The Data Cooperation Canvas (DCC)

#### Purpose and Positioning

In comes the [Data Cooperation Canvas](https://www.datacooperationcanvas.eu/canvas/nodes/page) as an answer to the limitations for the data space specific projects. The DCC is a European instrument that supports organizations in designing and governing data cooperation initiatives. Where the VLOCA Canvas provides a comprehensive project management framework for digitalization, the DCC zooms in specifically on the data-sharing dimension: who shares what data, with whom, under what conditions, via what technical mechanisms, and governed by what legal and organizational agreements.

The DCC is particularly relevant for projects involving multiple organizations exchanging data, especially in the context of European dataspaces and federated ecosystems where data sovereignty, interoperability, and trust are central concerns.

For more information on the DCC and how to use it, we refer to the website of the [Data Cooperation Canvas](https://www.datacooperationcanvas.eu/canvas/nodes/page).

### Complementary use of VLOCA and DCC

#### Core Principle for (Flemish) projects

The VLOCA Canvas and the DCC are not competing frameworks. Their combined use can be summarised as:

_**Use the VLOCA Canvas (9 tiles) as the primary framework for all (Flemish) digitalization projects; use the DCC to deepen the data-sharing, governance, and technical architecture dimensions where a dataspace is involved.**_

This principle ensures uniformity of methodology across Flanders, depth where dataspace specifics demand it, and European compatibility of outputs.

#### Complementarity Matrix — VLOCA Tiles and DCC Complements

<table data-header-hidden><thead><tr><th width="166.2222900390625" valign="top">VLOCA Tile</th><th valign="top">DCC Complement</th></tr></thead><tbody><tr><td valign="top">1. Challenges (Uitdagingen)</td><td valign="top">Context + Motivation &#x26; objectives together describe the business/organizational context and the shared motivation of key partners for data exchange</td></tr><tr><td valign="top">2. Value (Waarde)</td><td valign="top">Added value: describes what value the data cooperation creates and why it will succeed for all participants</td></tr><tr><td valign="top">3. Solution (Oplossing)</td><td valign="top">Technical concepts/models + Infrastructure characteristics deepen the solution with specific technical concepts (MIMs, APIs, data models) and infrastructure needs (cloud, connectivity, security)</td></tr><tr><td valign="top">4. Stakeholders (Belanghebbenden)</td><td valign="top">Key partners identifies all partners with their roles (Initiator, Coordinator, Participant, Contributor, User, Financier, Developer)</td></tr><tr><td valign="top">5. Impact</td><td valign="top">Added value: clarify what drives each partner to participate and what the financial sustainability model is</td></tr><tr><td valign="top">6. Process, Data &#x26; Technology (Proces, data &#x26; technologie)</td><td valign="top">Data &#x26; data sources + Shared processes + Interoperability: the DCC adds the demand &#x26; supply matrix for datasets, maps which process steps are shared vs individual, and specifies interoperability standards and effort required</td></tr><tr><td valign="top">7. Governance</td><td valign="top">Governance model provides 11 concrete governance archetypes (open data, data marketplace, data trust, etc.) and 4 power structures (Single, Hierarchical, Coordinated, Joint) to structure decision-making</td></tr><tr><td valign="top">8. Efforts (Inspanningen)</td><td valign="top">Resources + Business case + Implementation roadmap: identify organisational resources needed, establish the financial model, and define the phased scaling path across 6 axes (financial, participation, use cases, technology, organization, legal)</td></tr><tr><td valign="top">9. Guidance (Handvaten)</td><td valign="top">Current Status: provides the 5-stage maturity indicator (Exploratory → Preparatory → Implementation → Operational → Scaling) to assess where the cooperation stands and what is needed to progress</td></tr></tbody></table>

#### When to activate DCC Components

Not every VLOCA project requires the full DCC. Activate DCC components when:

* Multiple organisations need to agree on rules, norms and power structures for controlling the data exchange;
* The data flow has steps that need to be performed jointly, mapping individual vs. shared activities adds clarity;
* Specific MIMs, APIs or infrastructure choices need to be specified;
* A demand & supply matrix for datasets helps prioritise what data is available and what effort is needed;
* A maturity stage assessment (Exploratory through Scaling) is needed to guide the roadmap and communicate progress;
* Financial sustainability and organisational capacity need to be explicitly mapped for long-term viability.

### Application: [UC1 Restricted Mobility Zones](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc1-restricted-mobility-zones)

#### UC1 Applied to the VLOCA Canvas (9 Tiles)

<table data-header-hidden><thead><tr><th width="158.4443359375" valign="top">VLOCA Tile</th><th valign="top">UC1 — Restrictive Mobility Zones (Content)</th></tr></thead><tbody><tr><td valign="top">Challenges (Uitdagingen)</td><td valign="top">Flemish cities are legally mandated to implement Restrictive Mobility Zones to reduce air pollution and protect public health. However, boundary data, access rules and exemption registers are currently fragmented across 500+ organizations using different technologies and data formats. This makes consistent enforcement, data reuse and cross-municipal policy evaluation impossible. Municipalities work in isolation on the same challenge.</td></tr><tr><td valign="top">Value (Waarde)</td><td valign="top"><p>For citizens: cleaner air, clearer information about zone access, fewer erroneous fines</p><p>For municipalities: reduced cost of data publication; shared infrastructure replaces bilateral agreements</p><p>For enforcement services: reliable, real-time, harmonised zone data</p><p>For Flemish government: automated, comparable Sustainable Urban Mobility Indicators (SUMI) for policy evaluation</p><p>For research: open, reusable traffic and mobility data for modelling and impact analysis</p></td></tr><tr><td valign="top">Solution (Oplossing)</td><td valign="top"><p>Share and harmonise RMZ boundary data, access rules and exemption registers across municipalities via a data space.</p><p>The solution includes:</p><ul><li>A shared data publication infrastructure enabling municipalities to publish zone data once and make it reusable by multiple consumers</li><li>A cross-regional interlinking layer connecting with the European Mobility Data Space (EMDS) (out of scope for DECIDE project)</li><li>An interface enabling non-technical consumers to access and query zone data</li></ul></td></tr><tr><td valign="top">Stakeholders (Belanghebbenden)</td><td valign="top"><p>DECIDE project partners</p><p>Other (Flemish) municipalities</p><p>Flemish Department of Environment &#x26; Mobility as policy owner and evaluation stakeholder</p><p>Private enforcement services as data consumers for real-time zone enforcement</p><p>Journey planning and navigation services for data consumers</p><p>Research institutions: data consumers</p><p>Citizens and businesses as end beneficiaries</p></td></tr><tr><td valign="top">Impact</td><td valign="top"><p>Target:</p><p>By end of project, RMZ data can easily be visualized in GEO/GIS solutions of partners and possible other reusers.</p><ul><li>Data freshness: zone boundary updates reflected much quicker after policy change</li><li>Reduction in enforcement errors due to outdated zone data</li></ul></td></tr><tr><td valign="top">Process, Data &#x26; Technology</td><td valign="top"><p>Processes:</p><ol><li>Municipality publishes decision on RMZ</li><li>Data injection services of LD&#x26;L data space detects decision + links it to location if possible</li><li>Data publication in data space</li><li>Reuse of (updated) information</li></ol><p>Data:</p><p>RMZ decisions in local decisions and legislation + location information</p><p>Technology:</p><p>Use open source technology stack of the Flemish LBLOD ecosystem in line with technical data space building blocks</p></td></tr><tr><td valign="top">Governance</td><td valign="top"><p>Ownership:</p><p>Municipalities retain data sovereignty over their own RMZ data. Decision-making on data space level: to be determined.</p><p>Roles and responsibilities:</p><p>Municipalities (for data accuracy and timeliness); Agency for Home Affairs (infrastructure operation, standards maintenance); enforcement services — correct use of consumed data. Inter-authority cooperation: All municipalities. Long-term management: Data space standards reviewed periodically; onboarding process standardized for reuse by other municipalities and other dataspace use cases.</p></td></tr><tr><td valign="top">Efforts (Inspanningen)</td><td valign="top"><p>Project planning:</p><ul><li>By the end of DECIDE project: implemented in 2 pilot cities</li><li>2027: Full Flemish rollout + cross-border federation formalised</li><li>CAPEX: DECIDE project</li><li>OPEX: data space and AI infrastructure operation, standards maintenance, municipal onboarding support</li></ul></td></tr><tr><td valign="top">Guidance (Handvaten)</td><td valign="top"><p>Lessons learned from VLOCA trajectory:</p><ul><li>Governance design must precede technical implementation: participation agreements take longer to negotiate than connectors to deploy</li><li>Municipality capacity varies significantly: a supported onboarding process (facilitated if possible) is essential for scaling</li><li>Compliance is a prerequisite for European interoperability, non-compliant data requires costly harmonisation before it can be federated</li></ul><p>Good practices:</p><ul><li>Use the DSSC building blocks</li><li>Publish a clear data quality policy from the start; data consumers depend on consistent freshness and accuracy</li></ul><p>Communication plan:</p><ul><li>Regular updates to municipalities via Agency of Home Affairs channels</li><li>Publication of use case results on VLOCA Knowledge Hub for reuse by other authorities</li></ul></td></tr></tbody></table>

#### UC1 DCC Complement

The DCC components deepen and extend the VLOCA analysis for UC1, addressing the governance, data-sharing, and technical architecture dimensions that the VLOCA tiles cover at a higher level of abstraction:

<mark style="background-color:$warning;">See full data cooperation canvas for the DECIDE project (to be added).</mark>

### Key Insights from the UC1 Application

1. Governance precedes technology:

The VLOCA Governance tile correctly identifies the need for inter-authority cooperation and ownership arrangements. The DCC's Shared processes and Governance model components reveal the full operational complexity: participation agreements, data quality obligations, and liability arrangements all need to be in place before connectors can be deployed. Governance design is the critical path.

2. Maturity gap is larger than the canvas reveals:

The DCC's Current status assessment reveals Flanders is at the Preparatory stage (phase 2 of 5) on the DCC maturity scale for RMZ data cooperation. This was not fully visible from the VLOCA canvas alone and directly impacts the project roadmap; more time is needed for governance and standards work before technical scaling is feasible.

3. Data roles clarify the Governance tile:

The DCC's explicit data role framework resolves ambiguities in the VLOCA Stakeholders and Governance tiles. For UC1, municipalities are unambiguously data providers, enforcement services are consumers, and Agency for Home Affairs is the intermediary; each with distinct legal responsibilities and contractual obligations.

4. 'Process, Data & Technology' tile benefits most from DCC:

The VLOCA tile deliberately stays at conceptual level. The DCC's Data & data sources, Shared processes and Technical concepts/models components provide the implementation-ready detail needed for the data space (information essential for the project team but out of scope for a standard VLOCA trajectory).
