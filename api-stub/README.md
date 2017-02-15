# Mapping Discovery API to Standard Vocabularies

What would the [prototype API stub](https://github.com/ONSdigital/dp-dd-api-stub) look like if mapped to standard vocabularies, using JSON-LD
 
The data in this directory roughly corresponds to the sample data included in [the API stub project](https://github.com/ONSdigital/dp-dd-api-stub/tree/develop/stub/data)

The idea is to explore how JSON-LD and the standard vocabularies recommended in the draft metadata framework maps to the API response format. 

This would let ONS get [a 4/5 star rating](http://5stardata.info/en/) and conform to best practices used in EU and other standards, e.g. DCAT, DCAT-API, STAT-DCAT, etc.

Have not reviewed against current FE implementation, but going on the basis that if the data structure contains the same data, then should be possible to 
adapt if this is chosen as a useful step forward.

## Routes

* `/ons-context.json` -- (NEW) definitions of the properties and classes used in the responses. This defines the mapping of keys/values in the JSON to existing standards. Referenced from response docs
* `/datasets.json` -- paged view of collection of datasets
* ...

## General Comments Notes

Some notes on aligning this with JSON-LD.

* Use JSON-LD standard keys, e.g. `id` and `type`
* The `@id` values have been given as relative URLs, but in the actual API they'd be fully-qualified
* In a few cases some extra grouping keys have been removed, e.g. no longer a `metadata` key. All properties of a `Dataset`, etc are part of its metadata
* Use ISO 8601 for dates
* There aren't any existing standards for search results/collections, although [Hydra has support for describing views](http://www.hydra-cg.com/spec/latest/core/#collections). Have used 
that here. The main difference for list views is defining the response as a `Collection` with the paging parameters, etc as a `view` on that collection. Makes the semantics clearer and but keeps the essential items available. But (for now) have ignored some terms: `startIndex`, `itemsPerPage`, `page`, `totalPages`

## 
Context changes:
    
* Added `hydra`
* TODO: add extra custom terms, e.g. for paging and short-cuts?
* Added `associatedPublications` - might be better as dct:related than foaf:page?
* Added `methodology` - can be mapped to foaf:page for now?
* TODO: next release date
* TODO: national statistics
* TODO: XKOS terms for hierarchies http://www.ddialliance.org/Specification/RDF/XKOS
* TODO: source, temporal, spatial mappings
* TODO other short cuts
* TODO `hierarchical`: `false` ?
* TODO: dimension type: "standard", "classification", "time", perhaps use role?
* TODO: topConcept doesn't feel right
