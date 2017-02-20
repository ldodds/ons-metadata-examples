# Mapping Discovery API to Standard Vocabularies

An illustration of what the [prototype API stub](https://github.com/ONSdigital/dp-dd-api-stub) could look like if it was based on using 
[JSON-LD](https://www.w3.org/TR/json-ld/) and standard metadata vocabularies.

Implementing this approach would let ONS Digital achieve [a 4/5 star rating](http://5stardata.info/en/) for its data publication. It would 
also align with best practice being adopted by other data portals and efforts to improve publication of official statistics.

JSON-LD provides a means to publish data using JSON but with a well-defined mapping to standard vocabularies. Use of standard metadata vocabularies 
like DCAT, DCAT-AP, STAT-DCAT, etc will make it easier for others to use and harvest ONS data.

This means that developers looking to use the data as simple JSON documents can continue to do so. But there are additional semantics associated 
with the data for those that was want to use it.

Overall the goal here is to:

* See what the API could look like in JSON-LD
* Check whether the proposed mapping to standard vocabularies covers the data used in the prototype, or whether further work needed
* Provide concrete examples for further discussion and implementation
 
## Approach
 
The JSON files in this directory roughly correspond to the sample data included in 
[the API stub project](https://github.com/ONSdigital/dp-dd-api-stub/tree/develop/stub/data).

The original files have been updated to be valid JSON-LD documents. In practice this has involved:

* Declaring the [mappings](ons-context.json) to standard vocabularies by adding a `@context` key
* Adding unique URIs (`id`) to every resource 
* Changing to use standard property and type names
* Simplifying structure in a few places, e.g. removing of `metadata` objects
* Use ISO 8601 for dates

Some of the key differences have been spelt out below.

The goal has been to try to represent all of the existing test data using standard vocabularies. No attempt has been made to assess impact on, e.g. the 
current prototype front-end. The assumption is that if the data is still present then it should be relatively easy to adapted the front-end to 
use the new structure.

## Data model notes

* A Dataset is a DCAT Dataset
* The available downloads for a dataset are DCAT Distributions
* A Dataset will be structured into dimensions, attributes and measures. These are DataCube Components
* Dimensions and attributes will have values that are taken from a standard code list
* Code lists are SKOS Concept Schemes. They can be either a list of values or a hierarchy with broader/narrower relationships between entries
* Entries in a code list are SKOS Concepts. They will have a standard code and name
* All resources have a unique URI
* Datasets are organised into a Collection which can be presented as a series of partial views

## Important Changes

* The `id` values have been given as relative URLs, but in the actual API they'd be fully-qualified

* The structure for the `/dataset/X/dimensions` response is slightly different: rather than just an array of dimensions, it now identifies which dataset the dimensions are associated with. Previously this information was implicit in the URL structure, but not the data

* `/datasets.json` -- Reorganised on the basis that the response is a paged view over the collection of datasets. This approach is used in [Hydra](http://www.hydra-cg.com/spec/latest/core/#collections).

* `/code-lists` -- replaces `/hierarchies` with a more general name. Not all code lists used in dimensions are hierarchies, but they should all have a unique URI

* `/ons-context.json` -- This is the JSON-LD context document that declares mapping of JSON keys to the URIs for the standard properties and types used in the data. This is referenced from every API response to ensure that the mapping is declared. The context can be updated to add new vocabularies, rename keys, etc. So, for example, there is scope to use locally preferred names in the document. But the default approach has been to use standard naming

* `/terms.json` -- a new ONS vocabulary has been defined to define some custom terms which aren't covered by existing vocabularies. Every term has a unique URI. This document declares entirely new properties whereas the context document maps JSON object keys to URIs. For example `methodology` and `associatedPublications` have been mapped to existing terms (`foaf:page` and `dct:related` respectively). Whereas `nationalStatistic` is a new property which is first defined in `terms.json` and then mapped to a simple name in the context.

## Summary

* Relatively simple changes required to existing JSON to map to JSON-LD
* Existing URI structure provides good framework for a Linked Data view of the data
* JSON-LD context gives some flexibility to map preferred local names for properties/types to their standard URIs. So, for example can use meaningful names for still preserve semantics
* Overall relatively straight-forward to provide a 4-star approach

## Some issues to consider

* Some properties, e.g. `temporal` are given as simple strings, but might be better if they were URIs, e.g. links to relevant items in the administrative geography

* At the moment am using `dct:type` to capture what was originally the `type` for hierarchies, etc E.g. "geography", "classification", etc. Am not clear where these are used in the Front End, so have just kept them in place but used more appropriate key name. 

* The website taxonomy could be exposed as a SKOS Concept Scheme too, e.g. under `/code-lists` or elsewhere. In this case the `theme` properties of the dataset could use the relevant URIs from that list.

* There aren't any existing standards for search results/collections, although [Hydra has support for describing views](http://www.hydra-cg.com/spec/latest/core/#collections). Have used 
that here. The main difference for list views is defining the response as a `Collection` with the paging parameters, etc as a `view` on that collection. Makes the semantics clearer, but keeps the essential items available.

## Additional Notes

Context changes:
    
* Hydra keys
* TODO: add extra custom terms, e.g. for paging. New terms: `startIndex`, `itemsPerPage`, `page`, `totalPages`

* TODO: dimension type: "standard", "classification", "time", perhaps use role? or dct:type

* TODO `hierarchical`: `false` ?
* TODO: `levelType`
* TODO: hasTopConcept doesn't feel right, `xkos:belongsTo` is better
* TODO: depth can be handled by `xkos:ClassificationLevel` and `xkos:depth`? Although requires jumping through some hoops   
* TODO: XKOS terms for hierarchies http://www.ddialliance.org/Specification/RDF/XKOS

* RightsStatement

* Hydra + Catalog?
