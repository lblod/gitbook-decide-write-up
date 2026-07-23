---
description: Data Validation & Monitoring
---

# Write-up Data Quality Manager

## Description UC/wanted deliverable

The DECIDe project builds a data space for local decisions and legislation (LD\&L) across participating pilot cities. As AI enrichment pipelines and data ingestion processes populate the triplestore with ELI-structured data, it becomes essential to verify that the data conforms to the expected shapes and constraints. Without systematic validation, data quality issues –missing required properties, incorrect types, broken links– can accumulate silently and undermine the reliability of the data space.

The wanted deliverable is a **SHACL validation layer**: an automated service that runs SHACL and SPARQL-based shape constraints against all core entities in the triplestore, stores the results as a structured report, and exposes that report via a REST API. This allows the team and downstream consumers to monitor data quality continuously without manual inspection.

Within the project proposal, this maps to the following deliverables and tasks:

| Deliverable                                                                 | Tasks                                                                                                                           |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| <p><br><strong>D2.4</strong> (Adjusted) Data Quality Manager available</p>  | <p><strong>T2.6</strong> Adjust and test open source semantic Data Quality Manager,<br>implemented in Flanders (if needed).</p> |
| **D2.5** (Adjusted) Data Quality Manager integrated at relevant pilot sites | **T2.7** Integrate Data Quality Manager in local DS German pilots.                                                              |

### Link to other deliverables

#### UC0.0 Pipelines

The pipelines write-up documents the AI enrichment and ingestion pipelines whose outputs the SHACL validation layer assesses. Core ELI entities validated by the SHACL shapes are produced and structured by those pipelines.

[write-up-uc0.0-pipelines.md](write-up-uc0.0-pipelines.md "mention")

#### UC0.0 Human Validation (HV)

The Human Validation Tool provides a complementary quality layer: where SHACL validation checks structural and semantic conformance automatically, the HV tool enables domain experts to assess the correctness of AI-generated annotations. Together, they form the data quality monitoring approach for the DECIDe data space.

[write-up-uc0.0-human-validation-hv.md](write-up-uc0.0-human-validation-hv.md "mention")

## Glossary

{% hint style="info" %}
See the [UC0.0 Data space glossary](./#glossary) for definitions of ELI, SHACL and Triplestore.
{% endhint %}

<table><thead><tr><th width="265.5673828125">Term/Acronym</th><th>Explanation</th></tr></thead><tbody><tr><td>loket-report-generation-service</td><td>The shared LBLOD microservice that provides the SHACL validation engine and report generation infrastructure. Extended within DECIDe to support batch processing, SPARQL-based shapes, and a REST API.</td></tr><tr><td>SHACL-SPARQL</td><td>An extension of SHACL that allows constraints to be expressed as SPARQL queries. Used in DECIDe for validation rules that are too complex to express in standard SHACL property shapes.</td></tr><tr><td>Shape</td><td>A SHACL construct that defines the expected structure of a class of RDF resources. Shapes are stored as Turtle (.ttl) and loaded by the validation service at runtime.</td></tr><tr><td>Validation report</td><td>The structured output produced by the periodic SHACL validation run. Stored in the triplestore and exposed via REST API. Contains one validation result per constraint violation found across all validated resources.</td></tr><tr><td>Validation result</td><td>A single entry in a SHACL validation report describing one constraint violation found during a validation run. Each result identifies the resource that failed validation (<code>sh:focusNode</code>), the specific shape constraint that was violated (<code>sh:sourceConstraintComponent</code>), and the property or value involved (<code>sh:resultPath</code>, <code>sh:value</code>). Results are stored in the triplestore as part of the validation report and exposed via the REST API.</td></tr></tbody></table>

## Business analysis + final feature passport (incl. functional analysis)

### Opportunity (problem, need, desire)

The DECIDe data space ingests local decisions and legislation from multiple cities and enriches them with AI-generated annotations. As the volume of data in the triplestore grows and pipelines evolve, there is no systematic mechanism to verify that data continues to conform to the expected ELI structure. Data quality problems – missing required predicates, resources of the wrong type, malformed literals – can go undetected until they cause unexpected results in downstream services or applications. For example, decisions that are not compliant with ELI result in less or none AI enrichments. The need is for an automated, recurring validation layer that checks all core entities against a defined set of SHACL constraints and makes the results visible to the team without requiring manual SPARQL queries.

### Pilot partners

All three pilot cities (Ghent, Bamberg, Freiburg) contribute LD\&L data to the DECIDe data space. The SHACL validation layer runs across data from all three cities without city-specific configuration; the SHACL shapes describe the shared ELI data model that applies to all pilot partners.

### Target audience / Personas

The primary audience for the SHACL validation output is the DECIDe development and data engineering team, who use the nightly reports to detect and resolve data quality issues before they affect pilot operations or downstream use cases. Secondary audiences include pilot city technical staff who may consume the REST API to monitor the quality of their city's data in the shared data space.

<table><thead><tr><th width="162.544921875">Persona</th><th>Journey</th></tr></thead><tbody><tr><td><br><strong>P2</strong> Semantic framework owner</td><td>Maintains the SHACL shapes used in the data space. Also, adds extensions for local requirements.</td></tr><tr><td><strong>P6</strong> Data engineer</td><td>Monitors the latest validation report via the REST API or triplestore queries to identify and resolve data conformance issues.</td></tr></tbody></table>

### Functionality (requirements)

The SHACL validation component is designed as a periodically automated process rather than an interactive interface. Because the triplestore may contain thousands of decisions and related entities, validation must operate in batches rather than loading everything into memory at once. The component must also be generically configurable.

<table><thead><tr><th width="611.919921875" valign="top">Requirement</th><th valign="top">Priority</th></tr></thead><tbody><tr><td valign="top">SHACL validation runs periodically via cron job</td><td valign="top">Must-have</td></tr><tr><td valign="top">Configurable using SHACL</td><td valign="top">Must-have</td></tr><tr><td valign="top">SHACL shapes cover ELI Work, ELI Expression, and Organization core entities</td><td valign="top">Must-have</td></tr><tr><td valign="top">Validation processes resources in batches to handle large datasets without memory issues</td><td valign="top">Must-have</td></tr><tr><td valign="top">Validation results stored as a report in the triplestore</td><td valign="top">Must-have</td></tr><tr><td valign="top">REST API endpoint exposes the latest validation report in JSON format</td><td valign="top">Nice-to-have</td></tr><tr><td valign="top">Extensions placed in the shared loket-report-generation-service for ecosystem reusability</td><td valign="top">Must-have</td></tr><tr><td valign="top">SPARQL-based SHACL shapes supported alongside standard SHACL shapes</td><td valign="top">Nice-to-have</td></tr><tr><td valign="top">HTTP 404 returned by the API when no report is found</td><td valign="top">Nice-to-have</td></tr><tr><td valign="top">Previous report deleted on each run to preserve only the latest report</td><td valign="top">Nice-to-have</td></tr><tr><td valign="top">A limited set (sample) of decisions can be validated</td><td valign="top">Nice-to-have</td></tr></tbody></table>

## Datasources, datasets and datastandards

The foundational data sources and ELI data model for DECIDe are documented in the [UC0.0 Pipelines write-up](write-up-uc0.0-pipelines.md). The SHACL validation layer does not introduce new external data sources; it operates entirely over data already present in the triplestore.

### Data sources

DECIDe triplestore (Virtuoso) Internal. The SHACL validation service reads ELI and Organization resources directly from the triplestore and writes validation results back to it.

| Data source                                      | Type/category                           | Brief description                                                                                        |
| ------------------------------------------------ | --------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| ELI-normalized LD\&L decisions                   | LD\&L as structured linked data         | The output of UC0.0 ingestion pipelines (decisions) is used as data source for the validation layer.     |
| Organizations (governing bodies, municipalities) | Organizations as structured linked data | The output of UC0.0 ingestion pipelines (organizations) is used as data source for the validation layer. |

### Datasets available in the data space

| Dataset                                                                          | IdP/Authentication service | Country of origin | Domain           | Shared within the project | Reused within the project |
| -------------------------------------------------------------------------------- | -------------------------- | ----------------- | ---------------- | ------------------------- | ------------------------- |
| Validation reports and results (`sh:ValidationReport` and `sh:ValidationResult`) | /                          | Belgium / Germany | Local governance | No, internally reused     | Yes                       |

### Data standards

<table><thead><tr><th width="454.4248046875">Standard</th><th>Link</th></tr></thead><tbody><tr><td>SHACL</td><td><a href="https://www.w3.org/TR/shacl/">https://www.w3.org/TR/shacl/</a></td></tr><tr><td>ELI (European Legislation Identifier)</td><td><a href="https://eur-lex.europa.eu/eli-register/about.html">https://eur-lex.europa.eu/eli-register/about.html</a></td></tr><tr><td>W3C Organization Ontology</td><td><a href="https://www.w3.org/TR/vocab-org/">https://www.w3.org/TR/vocab-org/</a></td></tr><tr><td>ELI-EP (ELI-EP is an application profile of the <a href="https://op.europa.eu/en/web/eu-vocabularies/eli">ELI</a> and <a href="https://joinup.ec.europa.eu/collection/eli-european-legislation-identifier/solution/eli-ontology-draft-legislation-eli-dl/">ELI-DL</a> ontologies, designed and used for data of the European Parliament.)</td><td><a href="https://europarl.github.io/eli-ep/">https://europarl.github.io/eli-ep/</a></td></tr><tr><td>ORG-EP (ORG-EP is an application profile of the W3C Organization Ontology, specifically designed to describe the organizational components of the European Parliament (MEPs, Parliamentary Groups, Committees, etc.).)</td><td><a href="https://europarl.github.io/org-ep">https://europarl.github.io/org-ep</a></td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

In this drawing, rdf classes are represented by rounded rectangles. Predicates are shown as arrows connecting their origin as the predicate subject to their target as a predicate object. The circles are concrete entities. In the drawing, one validation result is shown explaining that the decision `https://data.gent.be/id/besluiten/23.1214.2233.007` from the city of Ghent does not have a language property in the triplestore.

The prefixes used in this section are:

```
PREFIX sh: <http://www.w3.org/ns/shacl#>
PREFIX eli: <http://data.europa.eu/eli/ontology#>
```

## Final architecture (and why)

### Final semantic components (and why) (if any)

The semantic components related to the Data Quality Manager are depicted in the following drawing:

<figure><img src="../../.gitbook/assets/data_quality_manager.drawio (2).png" alt=""><figcaption></figcaption></figure>

In this drawing, services are depicted as rectangles, the virtuoso triplestore is shown as a cylinder and HTTP requests are shown as arrows pointing from the origin of the request to the receiver of the request. Core components, marked with a **C**, are described in the core semantic.works components section of the [UC0.0 Data space write-up](./#core-semantic.works-components).

#### loket-report-generation-service

The data quality manager is a SHACL validation layer and is implemented by extending the existing shared LBLOD microservice `loket-report-generation-service`, which already contains a SHACL engine and a scheduling mechanism for periodic script execution. Rather than building a standalone validation service from scratch, the DECIDe team extended this service with the capabilities needed for the project and contributed those extensions back to the shared service, making them available to the broader LBLOD ecosystem.

The validation script runs periodically via a cron job. It loads the target classes defined in the SHACL files, then iterates over all resources of each target class (Extension 1). Sampling can be enabled to iterate over a limited number of resources per target class (Extension 2). The resources are processed in batches, because the volume of decisions in the triplestore makes loading all resources into memory at once impractical. Processing in bounded batches keeps memory usage stable regardless of dataset size. For each batch, resources are loaded into a temporary in-memory store, validated against the relevant SHACL and SPARQL-based shapes, and the results are appended to the validation report in the triplestore.&#x20;

At the end of each run, the previous report(s) are deleted so only the latest report is retained. This deletion process also deletes the report and its results in batches.

Utility functions have been added to retrieve a set of triples for each resource (Extension 3).

SPARQL-based SHACL shapes are executed differently from standard shapes (Extension 4): rather than evaluating them in-memory, the service imports the batch data into a temporary graph in the triplestore and runs the SPARQL queries directly against the triplestore engine. This is significantly faster for complex graph traversals and allows the full query capabilities of Virtuoso to be used for validation.

A REST API (Extension 5) exposing the latest validation report is added to the service increasing the visibility of the validation results. A JSON response following the JSON API specification is returned. For example:

```json
{
	"data": [{
		"type": "validationresult",
		"id": "6e6d25d0-4eb5-11f1-b8a7-7d1643d2e013",
		"attributes": {
			"result": "http://data.lblod.info/id/validationresults/6e6d25d0-4eb5-11f1-b8a7-7d1643d2e013",
			"resultId": "6e6d25d0-4eb5-11f1-b8a7-7d1643d2e013",
			"focusNode": "https://data.gent.be/id/besluiten/23.1214.2233.0071",
			"focusNodeId": "cb1dacb9-a979-5d97-9a17-5057cb20fe8b",
			"resultSeverity": "http://www.w3.org/ns/shacl#Violation",
			"sourceConstraintComponent": "http://www.w3.org/ns/shacl#MinCountConstraintComponent",
			"resultMessage": "Less than 1 values",
			"resultPath": "http://data.europa.eu/eli/ontology#language",
			"targetClassOfFocusNode": "http://data.europa.eu/eli/ontology#Expression,http://data.europa.eu/eli/ontology#LegalExpression,http://data.vlaanderen.be/ns/besluit#Besluit"
		}
	}],
	"meta": {
		"total": "2565",
		"page": 1,
		"pageSize": 10,
		"totalPages": 257
	},
	"links": {
		"self": "/shacl-reports/latest/issues?page[number]=1&page[size]=10",
		"first": "/shacl-reports/latest/issues?page[number]=1&page[size]=10",
		"last": "/shacl-reports/latest/issues?page[number]=257&page[size]=10",
		"prev": null,
		"next": "/shacl-reports/latest/issues?page[number]=2&page[size]=10"
	}
}
```

Three standard SHACL shape files and one SPARQL-based shape file are included in the app-decide configuration (Extension 6). The ELI and Organization entities are essential for the correct processing of the enrichment pipelines (NER and NEL):

* Shapes for ELI Work entities: config/reports/shacl/work.ttl
* Shapes for ELI Expression entities: config/reports/shacl/expression.ttl
* Shapes for Organization entities: config/reports/shacl/organization.ttl
* SPARQL-based constraint demonstrating sh:SPARQLConstraint usage for ELI Expressions: config/reports/sparql/expression.sparql

### Other explored semantic components (and why not)

n/a

### Final AI components (and why) (if any)

n/a

### Other explored AI components (and why not)

n/a

## Final UI design (and why) (if any)

n/a

The SHACL validation layer has no end-user interface. Validation results are consumed programmatically via the REST API or by querying the triplestore directly. Visualization of validation results is noted as possible future work.

### Other explored UI design (and why not)

n/a

## Testing approach

The validation service is part of the guidelines of the cities of deploying their own local data space:

* Bamberg: [https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.bamberg.yml#L111](https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.bamberg.yml#L111)
* Freiburg: [https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.freiburg.yml#L110](https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.freiburg.yml#L110)
* Ghent: [https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.ghent.yml#L96](https://github.com/lblod/app-decide/blob/development/docs/docker-compose.override.ghent.yml#L96)

This way, the service is integrated and tested on the server of each city. By default, the application runs the service. No specific configuration is required by the city.

The service uses a cron mechanism to periodically run the validations ([https://github.com/lblod/loket-report-generation-service/tree/master#reports](https://github.com/lblod/loket-report-generation-service/tree/master#reports)). By default, the SHACL validations will run every day at 03:00 ([https://github.com/lblod/app-decide/blob/development/config/reports/shacl-report.js#L35](https://github.com/lblod/app-decide/blob/development/config/reports/shacl-report.js#L35)).&#x20;

During testing, we discovered that validating all decisions causes a high load on the triple store: a validation issue in one municipality typically recurs for every decision. Therefore, we added sampling  to validate a limited set of 100 decisions in each validation run by default.

Testing can be done by navigating to the provided REST API `/shacl-reports/latest/issues` .

During DECIDe, we will monitor the validation results and analyze what their root causes are.

### Risks & mitigations

#### Batch size tuning

The choice of batch size is a trade-off between memory usage and the overhead of repeated triplestore round trips. If the average resource has many linked properties, a given batch size may still be too large. This risk is accepted for the pilot; the batch size is configurable and can be adjusted based on observed memory behaviour.

## Possible future work

### Possible future work DECIDe data space related

#### No validation errors

In DECIDe, we focus on analyzing the validation results and fix them when they are critical for the use cases. Some validation errors still need to be resolved at the source or in the pipelines. However, some of the errors are more subtle. For example, the PDF pipeline generates basic ELI data, but not all required fields of ELI-EP can be extracted at this stage. The validation rules should therefore be adapated to take into account these edge cases.

#### Extended shape coverage

The current shapes cover ELI Work, ELI Expression, and Organization. Additional shapes could be defined for AI annotation objects (`oa:Annotation`), human validation votes, and other entity types used in the DECIDe use cases.

#### Validation dashboard

A lightweight UI that surfaces the latest validation report in a human-readable form –showing violation counts by entity type, severity, and constraint– would make data quality visible to non-technical stakeholders without requiring direct API or SPARQL access.

### Possible future work LBLOD related

## Relevant links

A [demo video of the data quality manager](https://www.youtube.com/watch?v=0R8QUt8Fp84\&list=PL4lITq-CVBnsEoKXRF9ZHrw56mkCm3App\&index=2). The video goes step-by-step through the validation process: what are shapes, how does it fetch data, what does the validation service output (logs and REST API). As described above, the service will run automatically using a cron mechnanism and will validate all the decisions in batches, but this can take a while. For demonstration purposes, we loaded a sample set to show end-to-end the results.

The validation results of the DECIDe server are available through this REST API: [http://ds.decide.lblod.info/shacl-reports/shacl-reports/latest/issues](http://ds.decide.lblod.info/shacl-reports/shacl-reports/latest/issues) <mark style="color:$warning;">Service will be back online after review here:</mark> [<mark style="color:$warning;">https://github.com/lblod/loket-report-generation-service/pull/17</mark>](https://github.com/lblod/loket-report-generation-service/pull/17)

{% embed url="https://github.com/lblod/app-decide/blob/development/config/reports/shacl/work.ttl" %}

{% embed url="https://github.com/lblod/app-decide/blob/development/config/reports/shacl/expression.ttl" %}

{% embed url="https://github.com/lblod/app-decide/blob/development/config/reports/shacl/organization.ttl" %}

{% embed url="https://github.com/lblod/app-decide/blob/development/config/reports/sparql/expression.sparql" %}

{% embed url="https://github.com/lblod/loket-report-generation-service/blob/feature/shacl-utils/app.js#L118" %}

{% embed url="https://github.com/lblod/loket-report-generation-service/tree/feature/shacl-utils?tab=readme-ov-file#example-shacl-report" %}

{% embed url="https://github.com/lblod/app-decide/pull/101" %}
