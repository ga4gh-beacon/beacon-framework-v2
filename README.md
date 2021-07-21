# beacon-framework-v2
Beacon Framework version 2

## Introduction
The GA4GH Beacon specification is composed by two parts:

* the Beacon Framework
* the Beacon Model

The **Beacon Framework** (in *this* repo) is the part that describes the overall structure of the API requests, responses, parameters, teh common components, etc. It could also be referred in this document as simply the *Framework*.

A **Beacon Model** (in the [Models repo](https://github.com/ga4gh-beacon/beacon-v2-Models)) describes the set of concepts included in a Beacon version (e.g. Beacon v2), like *individual* or *biosample*. It could also be referred in this document as simply the *Model*.

The Framework could be considered the *syntax* and the Model as the *semantics*. 

Refer to the [Models repo](https://github.com/ga4gh-beacon/beacon-v2-Models) for further information about the Model and how to use it.

The Framework doesn't include anything related to specific entities but only the mechanisms for querying them and parsing the responses. 
The BF is, therefore, independent from/agnostic to any specific Model. It can be leveraged to describe models from other domains like proteomics, imaging, biobanking, etc.

A **Beacon instance** is just an implementation of a Beacon Model that follows the rules stated by the Beacon Framework.

If you are a Beacon implementer, then, you don't need to clone this (Framework) repo, you only need to **copy** (*or clone*) the Beacon Model and modify it to your specific case. You will find plenty of references to the Framework in the Model copy, and you will use the Json schemas here to validate that both the structure of your requests and responses are compliant with the Beacon Framework.

The Framework repo includes the elements that are common to all Beacons:

1. The configuration files
2. The Json schemas for the requests, the responses, and its respective sections
3. The files of every Beacon root
4. Examples of all the above (using a fake and simple Model)

## Folder structure of this repo
The above listed elements are organized in several folders (*in alphabetical order*):

* **common:** Json schemas and examples of the components used in other parts of the specification.
* **configuration:** Json schemas and examples for the configuration files that every Beacon MUST implement.
* **requests:** Json schemas and examples for the different sections of a request.
* **responses:** Json schemas and examples for the different types of responses and response sections.
* ***root* folder:** Only includes the definition of the Beacon root endpoints.

### The *root* folder and the Beacon root endpoints
The *root* folder only contains the endpoints.json document, an OpenAPI 3.0.2 description of the endpoints that every Beacon implementation MUST implement.
The endpoints are:
* the *root* (`/`) and `/info` that MUST return information (metadata) about the Beacon service and the organization supporting it.
* the `/service-info` endpoint that returns the Beacon metadata in the GA4GH Service Info schema.
* the `/configuration` endpoint that returns some configuration aspects and the definition of the entry types (e.g. *genomic variants*, *biosamples*, *cohorts*) implemented in that specific Beacon server or instance.
* the `/entry_types` endpoints that only return the section of the configuration that describes the entry types in that Beacon.
* the `/map` endpoint that returns a map (like a web *sitemap*) of the different endpoints implemented in that Beacon instance.
* the `/filtering_terms` endpoint that returns a list of the filtering terms accepted by that Beacon instance.

Most of these endpoints simply return the configuration files that are in the Beacon configuration folder.

### The Configuration 
Contains the files that describe the Beacon configuration, its contents are described in the section above, as they have almost a 1-to-1 relationship with such endpoints.

### The Requests 
Contains the following Json schemas:
* **beaconRequestBody.json:** Schema for the whole Beacon request. It is named `RequestBody` to keep the same nomenclature used by OpenAPI v3, but it actually contains the definition of the whole HTTP POST request payload.
* **beaconRequestMeta.json:** Meta section of the Beacon request. It includes request context details relevant for the Beacon server when processing the request, like the Beacon API version used to format the request or the schemas expected for the entry types in the response.
* **filteringTerms.json:** defines the schema for the filters included in the request.
* **requestParameters.json** defines the, very free, schema of the parameters included in the request.
* **examples-fullDocuments folder:** includes examples of "actual" requests. The example labelled with `MIN` in the name shows the minimal required attributes for the request to be compliant. The example labelled with `MAX` in the name includes a richer case with all the sections filled in.
* **examples-sections folder:** includes examples of "actual" sections of the requests. It is included to allow specification designers and Beacon implementers to check the compliance with a single section instead of having to implement a whole request. Such way, We aim to facilitate an "incremental" implementation of an instance.

#### Differences between FilterTerms and RequestParameters
The presence of two mechanism to refine the queries could sound confusing initially, but that separation is just taylored to facilitate the interpretation of the request.
Both, the filters and the parameters, are used to refine the query. An unrestricted query like `/datasets` should return the list of all datasets in a Beacon instance.

### The Responses
The Beacon concept includes several types of responses: some informative or informational and some with actual data payloads, and the error one. 

#### The Informational responses
A Beacon is able to return information, details, about itself. Many of the schema responses included in the `responses` folder have a 1-to-1 relationship with the corresponding configuration documents and their equivalent root endpoints, e.g. the `beaconEntryTypeResponse.json` is the schema of a response that wraps the `beaconConfiguration.json` document, and is then used as the payload of the `/entry_types` root endpoint. Schematically:
* *configuration/an_schema.json*: describes the schema of the configuration file itself.
* *responses/an_schema_response.json*: describes the format of the response that returns these configuration information.
* *root/endpoints.json*: describes the API endpoints to be called and parameters to be used to retrieve such responses.

The following schemas refer to informational responses: *beaconConfigurationResponse*, *beaconEntryTypeResponse*, *beaconFilteringTermsResponse*, ând *beaconMapResponse*.
 
### The results responses
A Beacon could return responses at different granularity levels:

* **boolean response:** only returns `exists: true` ('Yes') or `exists: false` ('No') to a given query.
* **count response:** returns `Yes`/`No` and the number of matching results.
* **resultset response:** returns `Yes`/`No`, the number of matching results and details of them per every collection (e.g. every dataset or cohort) and, if granted, details on every record that matches the query.

Each of these granularity levels has an equivalent response schema: 
* boolean > *beaconBooleanResponse*
* count > *beaconCountResponse*
* resultset (with or w/o record details) > *beaconResultSetsResponse*

An additional schema, *beaconCollectionsResponse*, describes such responses that returns details about the collections in a Beacon, but not the collection content themselves. Otherwise said, the response describes a dataset, but not returns the contents of any dataset.

### The common components
Some elements are transerval to the Framework and to any model, e.g. the schema for describing an ontology term or the reference to an external schema (like the reference to GA4GH Phenopackets or GA4GH Service Info schemas). 