# Write-up Provenance

This write-up describes the provenance and traceability pattern shared by the DECIDe data-space components. It is not a separate pipeline or a separate data store. It explains how source data, processing steps, AI output, human feedback and published data remain connected as Linked Data.

## Description UC/wanted deliverable

DECIDe collects local decisions and legislation (LD\&L) from heterogeneous sources, normalizes them to a common semantic model and enriches them with AI. Enrichments can influence search, policy reporting, geographic visualization and downstream reuse. A final RDF triple alone is not enough for a consumer to judge whether it is source data, a transformation result, an AI proposal or human feedback.

The wanted deliverable is an inspectable and queryable provenance chain for every important derived result. In practical terms, it should answer:

1. Where did a decision, document or assertion originate?
2. Which processing steps handled it and what was their result?
3. Which configured software component generated an AI enrichment?
4. Which source resource or text span supports that enrichment?
5. Has a person approved, rejected or corrected it?
6. Which retrieved decisions and prompt support a Smart Search answer?



For a machine-generated annotation, a consumer should be able to trace:

```
original source document or decision
  -> normalised ELI expression
  -> optional supporting text span
  -> processing task or PROV activity
  -> responsible configured software agent
  -> generated Web Annotation
  -> proposed assertion, entity or concept
  -> optional human review or correction
```

For a Smart Search answer, a consumer should be able to trace:

```
user question
  -> retrieved decision passages and retrieval scores
  -> complete prompt and model identifier
  -> generated answer
  -> optional reviews of the answer or its supporting passages
```

These trails make it possible to determine where a result came from, what evidence supported it, how it was produced and whether it was subsequently assessed by a person.

### Link to other deliverables

Provenance is not a separate pipeline, service or data store. It is interwoven throughout the DECIDe services and components that ingest source data, normalize decisions, generate AI enrichments, support human validation, answer Smart Search questions and publish derived results.

Each component contributes part of the overall provenance trail. Injestion pipelines retain relationships with the original input; processing services record tasks and activities; AI services connect annotations to evidence and configured software agents; the Human Validation Tool records assessments separately from machine output; and Smart Search retains the information needed to explain how an answer was generated.

| Component                                                                                                                                                                                                                         | Role in DECIDe                                             | Contribution to provenance                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Shared AI service base](https://github.com/semantic-ai/decide-ai-service-base)                                                                                                                                                   | Common AI provenance library                               | Creates configuration snapshots, configuration-specific agents, annotation activities and task/result-container patterns used by the AI enrichment services.                                             |
| [Geocoding / NER service](https://github.com/semantic-ai/decide-geocoding-service)                                                                                                                                                | Translation, segmentation and entity extraction            | Produces source- and span-based AI annotations using the shared provenance pattern. This connects extracted information to the relevant ELI expression or exact text span.                               |
| [Entity-linking backend](https://github.com/semantic-ai/entity-linking-backend)                                                                                                                                                   | Named Entity Linking                                       | Links detected entities to authoritative URIs and records the associated activity, configured agent and linking annotation.                                                                              |
| [Codelist labelling service](https://github.com/semantic-ai/codelist-labeling-service)                                                                                                                                            | Codelist and impact annotation                             | Produces whole-expression classification annotations and records the activity and configured software agent responsible for them.                                                                        |
| [Human Validation Tool](https://github.com/lblod/frontend-decide-human-validator) and [annotation-review service](https://github.com/lblod/annotation-review-service)                                                             | Inspection, review and correction of generated annotations | Present the source context and machine annotation to a reviewer and store approval, rejection or correction separately from the original machine output.                                                 |
| [Smart Search frontend](https://github.com/lblod/frontend-decide-question-answering) and [question-answering service](https://github.com/semantic-ai/decide-question-answering)                                                   | Smart Search and retrieval-augmented question answering    | Retain the question, complete prompt, model identifier, generated answer, citations, retrieved quotations and retrieval scores. Reviews of answers and quotations extend this trace with human feedback. |
| [OSLO-to-ELI](https://github.com/lblod/decide-harvester-transformation-service), [OParl-to-ELI](https://github.com/lblod/oparl-to-eli-service) and [PDF extraction](https://github.com/semantic-ai/decide-pdf-content-extraction) | Source ingestion and normalisation                         | Preserve source-specific relationships while converting heterogeneous input data into shared ELI resources used by downstream enrichment and search components.                                          |



## Glossary



<table><thead><tr><th width="254.966796875">Term / Acronym</th><th>Explanation</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>

## Business analysis + final feature passport (incl. functional analysis)

### Opportunity (problem, need, desire)

DECIDe makes decision data reusable by combining data from multiple cities and applying transformations and AI enrichment. This creates a trust problem: without an explicit trail, consumers cannot distinguish a publisher-provided fact from an inferred classification, an automatically translated expression, a linked entity or a human correction. They also cannot compare output from different service configurations or diagnose a failed pipeline run.

The provenance capability addresses this by retaining source, process, responsibility, evidence and review information as queryable Linked Data. It supports trustworthy reuse, explainability, debugging, reproducibility, human oversight, model comparison and auditable publication.

### Pilot partners

Provenance is implemented across the DECIDe services and therefore applies to data from all three participating partners: Ghent, Freiburg and Bamberg.

### Target audience / Personas

| Persona                    | Journey                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------- |
| **P3** Enrichment provider | Monitors jobs, tasks, configuration-specific agents and generated annotations.                          |
| **P4** Domain validator    | Inspects the source decision, evidence span, AI result and confidence before approving or rejecting it. |
| **P6** Data engineer       | Diagnoses failures, compares outputs across configurations and checks what a task consumed or produced. |
| **P7** Data space consumer | Filters or interprets data according to its source, producing component and human-review state.         |

### Functionality (requirements)



<table><thead><tr><th width="302.056640625">Area assessed</th><th>Conclusion</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>

## Datasources, datasets and datastandards

### Data sources

n/a

### Datasets available in the data space

n/a

### Data standards



## Final architecture (and why)

The architecture uses a combination of existing standards, with each one covering a distinct part of provenance and traceability.



#### ELI: source and legal-document model

ELI uses three levels, as explained in the [UC0.0 Pipelines](./#pdf-to-eli):

1. `eli:Work` identifies the abstract decision.
2. `eli:Expression` identifies a language or version and contains the decision text used by enrichment services.
3. `eli:Manifestation` identifies a concrete representation such as a PDF.

DECIDe supplements this structure with source-lineage information. The OSLO transformation preserves `prov:wasDerivedFrom` links from source decisions for the data. OParl also records source derivation with `prov:wasDerivedFrom` and uses ELI manifestations for concrete files. PDF conversion creates an `eli:Manifestation` and links it to the original PDF URL through `eli:is_exemplified_by`.

#### Annotation provenance

The [Web Annotation Model](./#web-annotation-model) was already briefly introduced. However, the AI services used within the DECIDe project go one step further by generating a `prov:Activity` alongside every `oa:Annotation`. As shown in the example blow, this allows for keeping track of timestamps, the service that generated the annotation, as well as the AI agent leading to the annotation.

```ttl
@prefix example: <http://www.example.org/> .
@prefix oa: <http://www.w3.org/ns/oa#> .
@prefix mu: <http://mu.semte.ch/vocabularies/core/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

example:myAIAnnotation a oa:Annotation, example:AIThemeEnrichment ;
  mu:uuid "0ab45594-9909-4aea-a7d9-63476ffe6e97" ;
  oa:hasBody example:InstallatieVergaderingThema ;
  nif:confidence 0.87 ;
  oa:hasTarget <https://data.arendonk.be/id/besluiten/24.1125.2636.6731>  .

example:myAIAnnotationRun a prov:Activity, example:AIThemeEnrichmentActivity ;
  mu:uuid "7dab6aa5-6a30-4ccc-86b7-dcd0c867a916" ;
  prov:startedAtTime "2025-06-20T08:09:51.032Z"^^xsd:dateTime ;
  prov:endedAtTime "2025-06-20T08:10:46.355Z"^^xsd:dateTime ;
  prov:generated example:myAIAnnotation ;
  prov:wasAssociatedWith example:myAIEnrichmentService ;
  prov:used <https://data.arendonk.be/id/besluiten/24.1125.2636.6731>, <https://data.arendonk.be/id/besluiten/24.1125.2636.6732>.

example:myAIEnrichmentService a prov:Agent, example:AIThemeEnricher ;
  mu:uuid "029f5390-531e-41f9-abcc-d75a416f9816" ;
  foaf:name "My Decision Theme Enricher" .
```

#### AI model registration with AIRO

The AI agent assocatied with an activity, can me modeled as a `prov:Agent`. This allows to keep track of very detailed information regarding the AI *component* being used. To model this all, we rely on [the AIRO ontology](https://delaramglp.github.io/airo/):

<figure><img src="../../.gitbook/assets/ai-model-registration-airo.jpg" alt=""></figure>

Most importantly, this allows to not only indicate which AI model was used, but also state the exact version of the model. This setup is very powerful in a system where models are being retrained with the help of e.g. user feedback, leading to new and improved model versions.

### Final semantic components (and why) (if any)

#### Configured agents and configuration snapshots

### Other explored semantic components (and why not)



### Final AI components (and why) (if any)

Provenance is not an AI component by itself. It records how the AI components used elsewhere in DECIDe produced their output.

### Other explored AI components (and why not)

n/a

## Final UI design (and why) (if any)

There is no standalone provenance user interface. The information is surfaced through the interfaces that need it:

* the pipeline/harvester interface exposes jobs, task status and input/result containers;
* the Human Validation Tool lets a reviewer inspect an annotation and its source context before voting;
* Smart Search displays answers and source quotations;
* SPARQL and exported RDF support technical inspection across source, activity, agent, annotation and review.

### Other explored UI design (and why not)

A dedicated provenance browser was not implemented during the pilot. A future browser could render a complete source-to-review chain and make configuration differences, evidence spans and export scope inspectable without SPARQL knowledge. The existing interfaces prioritize the context specific to their user journey.

## Testing approach



### Risks & mitigations



## Possible future work

As discussed, the AI components used within the DECIDe project generate a `prov:Activity` for every `oa:Annotation`, essentially storing provenance for AI-generated data. We could expand on this by also generating activities for annotations that are the result of user-related actions.

E.g. when a user validates AI-generated data, an *accept* annotation would be created, alongside an activity linking to the person as a `prov:Agent`. This new annotation would also link to the *original* AI annotation via `oa:hasTarget`. This way a chain of annotation could be created, very extensively showcasing the provenance of **all** (human- and AI-generated) affiliated data.

```ttl
@prefix example: <http://www.example.org/> .
@prefix oa: <http://www.w3.org/ns/oa#> .
@prefix mu: <http://mu.semte.ch/vocabularies/core/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix as: <https://www.w3.org/ns/activitystreams#> .

example:myHumanReviewAnnotation a oa:Annotation, as:Accept ;
  mu:uuid "161325e8-4302-4331-a7ba-67743e02ea8d" ;
  oa:hasTarget example:myAIAnnotation .

example:humanReview a prov:Activity, example:HumanReview ;
  mu:uuid "5f8779f1-5ad0-4403-a757-b43535afe73d" ;
  prov:startedAtTime "2025-06-20T08:45:06.853Z" ;
  prov:generated example:myHumanReviewAnnotation ;
  prov:wasAssociatedWith example:myReviewerHuman.

example:myReviewerHuman a prov:Agent, prov:Person ;
  mu:uuid "8694b5dd-5a98-4a2b-9de3-cb360d0a5ea1" ;
  foaf:name "Jos Test" .
```

## Relevant links

<table><thead><tr><th width="226.5830078125">Resource</th><th>Link</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>
