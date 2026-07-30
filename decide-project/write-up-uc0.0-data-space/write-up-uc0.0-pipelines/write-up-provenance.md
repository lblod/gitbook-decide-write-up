# write-up Provenance

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

#### Jobs, tasks and data containers

#### PROV-based generation and responsibility

#### Configured agents and configuration snapshots

#### AI model registration with AIRO



### Final semantic components (and why) (if any)



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

### Possible future work DECIDe data space related





### Possible future work LBLOD related

## Relevant links

<table><thead><tr><th width="226.5830078125">Resource</th><th>Link</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>
