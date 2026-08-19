# Pilot Data Space Functionality Assessment - Proof of Compliance (DECIDe)

This section provides concrete, hands-on evidence that the DECIDe pilot satisfies each requirement of the Data Space Functionality Assessment checklist. The examples below are taken directly from the running DECIDe infrastructure, which federates several administrations, including Ghent and ABB (Flanders, Belgium) and Bamberg and Freiburg (Germany), behind a shared, cross-border DCAT catalog. For each requirement we show the provider-side setup (Domain A) that makes a dataset available, followed by the consumer-side flow (Domain B) that discovers, authenticates against, and retrieves that data.

## Provider (Domain A)

To demonstrate provider readiness, we set up a dedicated example dataset that is deliberately access-restricted (as opposed to the fully public datasets normally published in DECIDe), so that the full publish → policy → catalog chain, including enforcement, can be shown end to end.

### Endpoint Publication

{% hint style="info" %}
A data source endpoint (API) is published or a file is exposed for download.
{% endhint %}

We set up a private SPARQL endpoint (`/api/private/sparql`) that expects a Bearer token as well as a header `dsp-role` holding the user's government URI. This endpoint mirrors the public SPARQL endpoint already used throughout DECIDe, but adds an authorization layer so that only the government(s) named in the accompanying ODRL policy (see the Policy Definition section below) can query it, and are only shown the data they are entitled to.

E.g., this could be a request someone from Ghent makes:

```shellscript
curl -X POST https://ds.decide.lblod.info/api/private/sparql \
  -H "Authorization: Bearer 123" \
  -H "dsp-role: http://data.lblod.info/id/bestuurseenheden/353234a365664e581db5c2f7cc07add2534b47b8e1ab87c821fc6e6365e6bef5:bought" \
  --data-urlencode "query=PREFIX dct: <http://purl.org/dc/terms/>
PREFIX eli: <http://data.europa.eu/eli/ontology#>
PREFIX epvoc: <https://data.europarl.europa.eu/def/epvoc#>
SELECT *
WHERE {
  ?work a eli:Work ;
    dct:identifier ?identifier ;
    dct:title ?workTitle ;
    eli:date_document ?dateDocument ;
    dct:creator ?creator ;
    eli:passed_by ?passedBy ;
    eli:is_realized_by ?expression .
  ?expression a eli:Expression ;
    eli:title ?expressionTitle ;
    eli:language ?language ;
    epvoc:expressionContent ?content .
}"
```

Which returns only Ghent data:

```json
{
    "head": {
        "link": [],
        "vars": [
            "work",
            "identifier",
            "workTitle",
            "dateDocument",
            "creator",
            "passedBy",
            "expression",
            "workType",
            "expressionTitle",
            "language",
            "content"
        ]
    },
    "results": {
        "distinct": false,
        "ordered": true,
        "bindings": [
            {
                "work": {
                    "type": "uri",
                    "value": "http://data.lblod.info/id/works/348bba93-d3eb-49b9-9d03-3e056b72cfda"
                },
                "identifier": {
                    "type": "literal",
                    "value": "TEST-GENT-001"
                },
                "workTitle": {
                    "type": "literal",
                    "xml:lang": "nl",
                    "value": "Test besluit Gent"
                },
                "dateDocument": {
                    "type": "typed-literal",
                    "datatype": "http://www.w3.org/2001/XMLSchema#date",
                    "value": "2026-06-09"
                },
                "creator": {
                    "type": "uri",
                    "value": "http://data.lblod.info/id/bestuurseenheden/353234a365664e581db5c2f7cc07add2534b47b8e1ab87c821fc6e6365e6bef5"
                },
                "passedBy": {
                    "type": "uri",
                    "value": "http://data.lblod.info/id/bestuurseenheden/353234a365664e581db5c2f7cc07add2534b47b8e1ab87c821fc6e6365e6bef5"
                },
                "expression": {
                    "type": "uri",
                    "value": "http://data.lblod.info/id/expressions/d06a2c07-07fa-4330-9e35-b1734a054824"
                },
                "expressionTitle": {
                    "type": "literal",
                    "xml:lang": "nl",
                    "value": "Test besluit Gent"
                },
                "language": {
                    "type": "uri",
                    "value": "http://publications.europa.eu/resource/authority/language/NLD"
                },
                "content": {
                    "type": "literal",
                    "xml:lang": "nl",
                    "value": "Lorem ipsum dolor sit amet. (Test - Gent)"
                }
            }
        ]
    }
}
```

This response confirms that the endpoint is reachable over HTTP, requires a valid identity to be presented via the `dsp-role` header, and returns real, non-trivial data (ELI-based legislative resources) scoped to the calling government, the concrete realization of the "publish an endpoint" requirement on the provider side.

### Policy Definition

{% hint style="info" %}
Access and/or usage policies for the data source are defined.
{% endhint %}

Publishing an endpoint is not enough on its own. Access to it must also be governed by an explicit access policy. For the private SPARQL endpoint introduced above, actual enforcement at request time is handled by the [DECIDe project's mu-auth authorization policy](https://github.com/lblod/app-decide/blob/development/config/authorization/config.ttl). This policy, expressed in ODRL and interpreted by [the mu-authorization service](https://github.com/mu-semtech/sparql-parser/tree/feature/odrl-configuration), defines party collections, asset collections (i.e. graphs), and permissions that determine which triples a given request may read or write. The service rewrites each incoming query to only target the graphs the requester is allowed to access.

Separately, the dataset's access rights are also described in machine-readable form via another ODRL policy, published alongside its DCAT metadata (see Metadata Creation below). This ODRL policy documents who is entitled to read the dataset, for discovery and transparency purposes towards other participants in the data space. There is no direct technical link between this descriptive policy and the mu-auth enforcement above: had the descriptive ODRL policy been missing, mu-auth would still enforce the same access restrictions.

### Metadata Creation

{% hint style="info" %}
Metadata, including a DCAT-based description, has been created and published.
{% endhint %}

Among the datasets published in the DECIDe project, there is an example private dataset. This dataset is added as a proof of concept for private datasets, as all other real datasets in the DECIDe project are public. The dataset with its ODRL policy is added as a migration in the DECIDe repository here: [https://github.com/lblod/app-decide/blob/development/config/migrations/20260519091850-add-private-dataset.ttl](https://github.com/lblod/app-decide/blob/development/config/migrations/20260519091850-add-private-dataset.ttl).

Here is a snippet of the most important triples added in the migration:

```turtle
private-ds-ex:dataset a dcat:Dataset ;  
    mu:uuid "60e19fdf-b549-41b9-9adf-420379285127" ;
    dct:title "Licensed Dataset" ;  
    dct:description "An example dataset available only to users who purchased a valid license." ;  
    dcat:distribution private-ds-ex:distribution .

private-ds-ex:distribution a dcat:Distribution ;  
    mu:uuid "341bc1ef-f4ab-4450-80af-b72d7c682e7f" ;
    dct:format "application/sparql-query" ;  
    dcat:accessURL <https://ds.decide.lblod.info/api/private/sparql> .

private-ds-ex:policy a odrl:Offer, ext:RestrictedPolicy ;  
    mu:uuid "1d8e1e34-ac8c-4fb8-845a-a9e8ca0e9f6b" ;
    dct:description "Only licensed users may access this content" ;
    odrl:conflict odrl:perm ;
    odrl:permission [  
        odrl:assigner <http://ds.decide.lblod.info> ;  
        odrl:target private-ds-ex:dataset ;  
        odrl:action odrl:read ;  
        odrl:assignee private-ds-ex:licensedUserCollection ;  
    ] ;
    odrl:prohibition [  
        odrl:assigner <http://ds.decide.lblod.info> ;  
        odrl:target private-ds-ex:dataset ;  
        odrl:action odrl:read, odrl:modify ;  
    ] .

```

This ODRL policy describes that only licensed users are allowed to read from the SPARQL endpoint, and that both writing and reading are forbidden by default. For more details, have a look at the ODRL writeup of the DECIDe project: [https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-odrl](https://abb-vlaanderen.gitbook.io/informatie-over-slimme-lokale-bronnen/decide-project/write-up-uc0.0-data-space/write-up-odrl).

This description is also published on the DCAT LDES feed, so other DCAT catalogs can easily pick up this content. You can find the first page of the DECIDe LDES feed at https://ds.decide.lblod.info/ldes/public/1. If you follow this feed to conclusion, you will have received the full description of all DCAT instances, including this example private dataset.

Together, this shows that the DCAT dataset and distribution metadata, along with its ODRL policy, were actually created and committed to the pilot's data, not just described in the abstract, and are kept in sync with the running triplestore via the LDES feed.

### Catalog Listing

{% hint style="info" %}
The resource is successfully listed in a DCAT-based catalog.
{% endhint %}

The above example DCAT dataset and its ODRL policy can also be accessed in our DCAT catalog. You can find its listing by viewing the catalog in a browser at `https://ds.decide.lblod.info/dcat/datasets` and searching for `licensed dataset`:

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

If you prefer, the dataset can also be accessed in a linked data file format by passing the correct `Accept` header and visiting the `https://ds.decide.lblod.info/dcat/catalogs` endpoint. For instance:

```bash
curl -H 'Accept: text/turtle' https://ds.decide.lblod.info/dcat/catalogs
```

Both the human-facing catalog UI and the machine-readable Turtle export confirm that the dataset is not just created in isolation, but is actually discoverable through the DCAT catalog, completing the provider-side chain of publish → policy → metadata → catalog.

The same dataset is also picked up by the federated catalog at `https://catalog.decide.lblod.info/dcat/datasets`, which aggregates listings across all DECIDe instances.

## Consumer (Domain B)

With the dataset published and access-controlled on the provider side, the following steps show a consumer discovering that dataset (and others) through the shared catalog, authenticating itself, and successfully retrieving data, including data spanning multiple, cross-border DECIDe instances.

### Discovery

{% hint style="info" %}
The resource was discovered via the DCAT catalog.
{% endhint %}

The DECIDe project offers a frontend for DCAT catalog visualisation and browsing. For example, catalog listings for ABB and Bamberg can be reached via [https://ds.decide.lblod.info/dcat/catalogs](https://ds.decide.lblod.info/dcat/catalogs) and [https://ds.decide.smartcitybamberg.de/dcat/catalogs](https://ds.decide.smartcitybamberg.de/dcat/catalogs) respectively. Dataset listings can likewise be reached via [https://ds.decide.lblod.info/dcat/datasets](https://ds.decide.lblod.info/dcat/datasets) and [https://ds.decide.smartcitybamberg.de/dcat/datasets](https://ds.decide.smartcitybamberg.de/dcat/datasets). A federated catalog aggregating all DECIDe instances, including ABB and Bamberg, is also available at [https://catalog.decide.lblod.info/dcat/catalogs](https://catalog.decide.lblod.info/dcat/catalogs).

The following image is a screenshot of the federated DCAT frontend:

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

A dataset's properties can be consulted by clicking "View dataset":

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

In this case, one distribution is available. Clicking it reveals the distribution's properties:

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

As can be seen in the screenshot, this distribution is a data dump in Turtle format. It can be accessed or downloaded from here, for example for further querying.

All queries sent to the triplestore in the DECIDe setup pass via the mu-authorization service. This service logs the queries coming in and the results coming out. For example, when accessing the datasets overview in the DCAT frontend, this is the first part of the equivalent mu-authorization logs:

{% code lineNumbers="true" %}
```
Requested:
BASE <http://mu.semte.ch/prefix/local/>
PREFIX vcard: <http://www.w3.org/2006/vcard/ns#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX ext: <http://mu.semte.ch/vocabularies/ext/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX dcat: <http://www.w3.org/ns/dcat#>
PREFIX rm: <http://mu.semte.ch/vocabularies/logical-delete/>
PREFIX typedLiterals: <http://mu.semte.ch/vocabularies/typed-literals/>
PREFIX mu: <http://mu.semte.ch/vocabularies/core/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX app: <http://mu.semte.ch/app/>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?uuid (MAX(?__modified94) AS ?__tmp95)
FROM <http://mu.semte.ch/graphs/ldes/ghent/public>
FROM <http://mu.semte.ch/graphs/ldes/freiburg/public>
FROM <http://mu.semte.ch/graphs/ldes/bamberg/public>
FROM <http://mu.semte.ch/graphs/ldes/abb/public>
WHERE {
  {
    ?s mu:uuid ?uuid ;
       a ?class .
    VALUES ?class { dcat:Dataset }
    OPTIONAL {
      ?s dcterms:modified ?__modified94 .
    }
  }
}
GROUP BY ?uuid
ORDER BY ASCMAX(?__modified94)
LIMIT 5

and received
{
  "head": {
    "link": [],
    "vars": [
      "uuid",
      "__tmp95"
    ]
  },
  "results": {
    "distinct": false,
    "ordered": true,
    "bindings": [
      {
        "uuid": {
          "type": "literal",
          "value": "60e19fdf-b549-41b9-9adf-420379285127"
        },
        "__tmp95": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#dateTime",
          "value": "2026-07-15T14:15:20.220454"
        }
      },
      {
        "uuid": {
          "type": "literal",
          "value": "1ae2da8e-9889-426b-a7e8-9360c15ba2cd"
        },
        "__tmp95": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#dateTime",
          "value": "2026-07-15T14:15:20.220454"
        }
      },
      {
        "uuid": {
          "type": "literal",
          "value": "5b338cc9-89fb-595d-82f2-e0015384dc90"
        },
        "__tmp95": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#dateTime",
          "value": "2026-08-07T11:25:26"
        }
      },
      {
        "uuid": {
          "type": "literal",
          "value": "34459d3e-ff3d-51e7-9431-db31a8ca5c83"
        },
        "__tmp95": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#dateTime",
          "value": "2026-08-07T12:04:35"
        }
      },
      {
        "uuid": {
          "type": "literal",
          "value": "a043b3de-2a51-54ec-9dd2-5ed9bf2d5980"
        },
        "__tmp95": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#dateTime",
          "value": "2026-08-07T12:06:36"
        }
      }
    ]
  }
}
```
{% endcode %}

The logs prove that the frontend queries for the first five available `dcat:Dataset` instances (it fetches their uuids) and gets the requested data back. This is only part of the logs generated to construct this specific frontend page, as more queries are also run, among others to fetch the selected datasets' properties, resulting in comparable logs.

### Authentication

{% hint style="info" %}
The consumer authenticated and presented required claims or credentials.
{% endhint %}

Having discovered a dataset via the catalog, the consumer must now authenticate before it can query the data. DECIDe implements this using OID4VC/OID4VP: instead of a classic username/password login, the consumer proves who they are and which organization/role they belong to by requesting or presenting a Verifiable Credential (VC).

When trying to access [https://yasgui.decide.lblod.info/sparql](https://yasgui.decide.lblod.info/sparql) the user is prompted to log in. Here the user can request a new VC or present an existing one. This is all handled by Decide's [oid4vc login service](https://github.com/lblod/oid4vc-login-service).

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

Once the user scans the QR code (or otherwise requests/presents a credential) with their wallet, the login service verifies it and establishes an authenticated session for that user. Proof of this verification is shown in the next section.

### Policy Satisfaction

{% hint style="info" %}
The consumer satisfied the access policy (authorization and policy evaluation).
{% endhint %}

The above-mentioned [oid4vc login service](https://github.com/lblod/oid4vc-login-service) handles authentication and authorization at the same time. After requesting or presenting credentials, the user is immediately redirected to the requested URL or denied access.

Upon **issuance** of a new VC, the following (redacted) logs indicate successful authentication/authorization:

```
info: Issuance succeeded -
    session: http://mu.semte.ch/sessions/78210c7c-8f15-11f1-a7ed-feab9a05b904,
    account: http://data.lblod.info/id/account/46a90284b5575f02aeea18127d4d0eda,
    group: http://data.lblod.info/id/bestuurseenheden/7d7122bd82f165e091a36c830d041b59aa48e4be9ea0aa06dc4c67ef381b7505,
    roles: Decide-gebruiker
```

Also, some triples are written to the triplestore for traceablility. When the issuance request comes in, the following are written to the `http://mu.semte.ch/graphs/verifiable-credentials/logs` graph:

```turtle
<http://data.lblod.info/flow-event/a1b2c3d4>
    a ext:CredentialIssuanceEvent ;
    mu:uuid "a1b2c3d4" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "issuance-started" ;
    dct:created "2026-06-11T10:00:00.000Z"^^xsd:dateTime .
```

When the issuance is successful, the following triples are written to the logs graph:

```turtle
<http://data.lblod.info/flow-event/e5f6g7h8>
    a ext:CredentialIssuanceEvent ;
    mu:uuid "e5f6g7h8" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "issuance-succeeded" ;
    dct:created "2026-06-11T10:00:05.000Z"^^xsd:dateTime ;
    ext:account <http://data.lblod.info/accounts/acc-uuid> ;
    ext:group <http://data.lblod.info/organizations/org-uuid> ;
    ext:roles "Decide-gebruiker" .
```

In case of failed issuance, these triples are inserted (the `ext:errorMessage` indicates what went wrong):

```turtle
<http://data.lblod.info/flow-event/a1b2c3d4>
    a ext:CredentialIssuanceEvent ;
    mu:uuid "a1b2c3d4" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "issuance-failed" ;
    dct:created "2026-06-11T10:00:03.000Z"^^xsd:dateTime ;
    ext:errorMessage "missing_proof" .
```

The service behaves similarly for VC **verification**. The following (redacted) log indicates successful verification:

```
info: Verification succeeded -
    session: http://mu.semte.ch/sessions/78210c7c-8f15-11f1-a7ed-feab9a05b904,
    account: http://data.lblod.info/id/account/46a90284b5575f02aeea18127d4d0eda,
    group: http://data.lblod.info/id/bestuurseenheden/7d7122bd82f165e091a36c830d041b59aa48e4be9ea0aa06dc4c67ef381b7505,
    roles: Decide-gebruiker
```

Once again, triples are written to the `http://mu.semte.ch/graphs/verifiable-credentials/logs` graph. These ones are inserted upon start of the verification process:

```turtle
<http://data.lblod.info/flow-event/i9j0k1l2>
    a ext:CredentialVerificationEvent ;
    mu:uuid "i9j0k1l2" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "verification-started" ;
    dct:created "2026-06-11T10:01:00.000Z"^^xsd:dateTime .
```

And these ones are inserted when verification was successful:

```turtle
<http://data.lblod.info/flow-event/m3n4o5p6>
    a ext:CredentialVerificationEvent ;
    mu:uuid "m3n4o5p6" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "verification-succeeded" ;
    dct:created "2026-06-11T10:01:04.000Z"^^xsd:dateTime ;
    ext:account <http://data.lblod.info/accounts/acc-uuid> ;
    ext:group <http://data.lblod.info/organizations/org-uuid> ;
    ext:roles "Decide-gebruiker" .
```

In case of failed verification, these triples are inserted (the `ext:errorMessage` indicates what went wrong):

```turtle
<http://data.lblod.info/flow-event/m3n4o5p6>
    a ext:CredentialVerificationEvent ;
    mu:uuid "m3n4o5p6" ;
    ext:session <http://mu.semte.ch/sessions/123> ;
    ext:eventType "verification-failed" ;
    dct:created "2026-06-11T10:01:02.000Z"^^xsd:dateTime ;
    ext:errorMessage "Credential issuer is not trusted" .
```

Together, these logs and triples provide a full, timestamped audit trail of the authorization decision (permit or deny, with a reason) for each request — the evidence needed to show that the access policy was actually evaluated and satisfied, not merely assumed.

### Successful Connection

{% hint style="info" %}
The consumer successfully connected to the endpoint or downloaded the file.
{% endhint %}

The final step of the consumer flow is the actual data transfer. After successful authorization/authentication, the user is automatically redirected to a page to perform SPARQL queries.

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

All queries go through the [mu-authorization service](https://github.com/mu-semtech/sparql-parser), which rewrites the query to target only the graph(s) the user has access to (based on their role(s) and organization) before running it against the triplestore. The service always runs the (rewritten) query, so it does not explicitly flag whether the request was authorized as intended; the returned resultset is what indicates whether access was successfully scoped.

The mu-authorization service logs the original query coming in, data about the requesting user, the rewritten query, and the data returned:

{% code lineNumbers="true" %}
```
Requested query as string:
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT * WHERE {
  ?sub ?pred ?obj .
} LIMIT 10
With access rights:MU-CALL-ID: 57300565487
MU-CALL-ID-TRAIL: NIL
MU-SESSION-ID: http://mu.semte.ch/sessions/123
MU-AUTH-SUDO: NIL
MU-AUTH-ALLOWED-GROUPS: [{"name":"public","variables":[]},{"name":"organization-member","variables":["<org-uuid>"]}]
MU-CALL-SCOPE: _
SOURCE-IP: 172.23.0.16
Requested:
<parsed-query>
and received
{
  "head": {
    "link": [],
    "vars": [
      "sub",
      "pred",
      "obj"
    ]
  },
  "results": {
    "distinct": false,
    "ordered": true,
    "bindings": [
      {
        "sub": {
          "type": "uri",
          "value": "http://www.w3.org/2001/XMLSchema#date"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/2000/01/rdf-schema#Datatype"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/15"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/14"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/13"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/12"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/11"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/10"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/9"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/8"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      },
      {
        "sub": {
          "type": "uri",
          "value": "http://mu.semte.ch/vocabularies/ext/embeddingVector/7d41910c-0da0-4817-9e7b-317a2ff50e1f/chunk/7"
        },
        "pred": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
        },
        "obj": {
          "type": "uri",
          "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#List"
        }
      }
    ]
  }
}
```
{% endcode %}

Together, the logged query, the access rights derived from the user's authenticated session, and the returned resultset show a complete, successful transfer: the consumer connected to the endpoint, was scoped to the data it is entitled to, and received a real resultset back, satisfying the final requirement of the KPI.

{% hint style="info" %}
To link catalog, authentication, policy and transfer steps, the HTTP calls sent between services in a semantic.works stack can be logged in an elastic search instance that connects all calls with the originating session-id. In the context of this pilot, however, this was not activated.
{% endhint %}
