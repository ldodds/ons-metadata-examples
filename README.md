# ONS Metadata Examples

This repository contains a number of examples demonstrating how metadata about ONS datasets can be described using standard formats and vocabularies.


* [ONS JSON LD Context](ons-context.jsonld) - a JSON-LD context that defines the namespaces and properties used in the examples. 
* [Armed Forces Dataset Example](armed-forces.jsonld) - example of the armed forces dataset [used in one of the prototypes](https://onsdigital.github.io/dp-ux-prototypes/dist/sprint-06-dataset/dataset.html)
* [Consumer Price Index](consumer-price-index.jsonld) - example of an ONS dataset described using JSON-LD. This uses a core set of mandatory properties
* [Consumer Price Index as Schema.org](consumer-price-index-schema.org.jsonld) - revised version of the above that uses only the Schema.org Dataset properties. Although it does use [contactPoint which is not yet supported](https://github.com/schemaorg/schemaorg/issues/1472)
* [Consumer Price Index Embedded JSONLD](consumer-price-index-schema.org.html) - embeds the above metadata into a dataset homepage. Also links ``canonical`` and ``alternate`` links to define dataset url and alternate views of the page. [Direct link to the relevant markup](https://github.com/ldodds/ons-metadata-examples/blob/master/consumer-price-index.html#L30).
* [Mapping API Stub to JSON-LD](api-stub) - illustrates mapping API stub responses to JSON-LD
