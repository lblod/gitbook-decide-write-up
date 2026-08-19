# Architecture mapping to DSSC/DS4SSCC and growth

ABB has developed an open source technology stack built on top of the [semantic.works](https://semantic.works/) microservice architecture. This architecture comprises of various reusable, containerized services that process and publish Linked Data. It was reused as the core architecture of the DECIDe project.

&#x20;In the context of the DECIDe project, many new containerized services were created and existing ones were extended. The images below give a graphical overview of this evolution. Note that only services that are used in the DECIDe project are shown, there are others.&#x20;

&#x20;The first image shows general services and frontend components:&#x20;

<figure><img src="../../.gitbook/assets/decide-ds4sscc-mapping-DSSC Part 1(1) (1).jpg" alt=""><figcaption></figcaption></figure>

The second image shows services that are more related to the pipelines in the DECIDe project:&#x20;

<figure><img src="../../.gitbook/assets/decide-ds4sscc-mapping-DSSC Part 2(1).jpg" alt=""><figcaption></figcaption></figure>

In both these images, services created in DECIDe were marked with a star, services extended in DECIDe with a triangle. Some services were simply used ‘as-is’ from the existing architecture, these don’t have a mark. The image also maps the various services to technical components from the [DSSC blueprint (v2)](https://archive.dssc.eu/space/BVE2/1071251457/Data+Spaces+Blueprint+v2.0+-+Home).&#x20;

Below is a mapping to the [DS4SSCC](https://www.ds4sscc.eu/) Participant Platform Architecture v2.0 as taken from the [3rd Technical Presentation in Ljubljana](https://static1.squarespace.com/static/63718ba2d90d0263d7fc1857/t/6a6ca51c8eba8c66874fbb55/1785505052108/Day+01+-+1c+-+Verifiable+Credentials+in+Data+Spaces+\(1\).pptx+\(1\).pdf). The DECIDe solution is marked up on the original drawing using red arrows pointing to the DS4SSCC component it implements.&#x20;

<figure><img src="../../.gitbook/assets/decide-ds4sscc-mapping-Participant Platform Architecture v2.0(2).jpg" alt=""><figcaption></figcaption></figure>

Briefly, this means:&#x20;

* The various harvesting Services harvest existing data sources&#x20;
* These are transformed into Linked data according to standard models and stored into a triplestore, which functions as a Data Broker&#x20;
* This data is then enriched by the various AI enrichment services (Data Processing)&#x20;
* Data Translation is unnecessary as at harvest time, data is already transformed into standardized Linked Data&#x20;
* There are various ways the data is made available (SPARQL, mu-resources, file download, LDES) in our Data API&#x20;
* Our publication is handled by our DCAT service&#x20;
* We have a DSP connector that deals with creating offerings and the dataspace protocol&#x20;
* An OID4VC service handles everything that is about credentials. This includes credential management, credential exchange, issuance, verification and a universal trust data registry&#x20;
* IDM depends on the partner, but an example case has been created with ACM/IDM on the ABB side&#x20;
* Authorization and Proxy/Gateway is different in the DECIDe dataspace. Authorization is not just enforced on a request level (access yes/no) in a Gateway, rather it is enforced on a SPARQL query level by a complex set of access rules expressed in ODRL and enforced by mu-authorization&#x20;
* Onboarding is a manual process at the moment that involves configuring the correct access rights in the data space by a technical team&#x20;
* The federation layer is provided by a federated DCAT catalog, fed by LDES feeds&#x20;
* Our vocabularies are standardized and we publish SHACL shapes as part of the DCAT datasets&#x20;
* Observability is tackled by adding provenance information to all of our AI and human review annotations. Observability of the inner workings stack is done through the semantic.works app-http-logger.&#x20;

More information about the services named in this section and their interplay can be found in the various architectural sections on this GitBook page.
