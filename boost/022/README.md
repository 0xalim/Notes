# Day 22 - [20210729013339]

## Overview
* what is json
* what is yaml
* using yq and jq to parse, json and yaml data
* what is json-schema
* use json-schema to define domain model and schema
* little bit about json5


### what is json

[introduction](xyz.json)


1. format for data
1. easy to read
1. data types: strings, numbers(both int+float), boolean, null
1. formatting is really easy
1. used in lots of applications due to it's simplicity
1. fastest parser: json is for parsing not for people
1. use jq to parse json files


### what is yaml

[introduction](xyz.yaml)


* yaml is super set of json

possible to go from yaml -> json; not always the opposite

1. yaml for people: many variations, so slower in parsing but easy to read
1. main langauge for cloud native
1. use yq to parse yaml files


### what is a schema

* domain: some sort of grouping
* domain-model: give things *tags*
* schema: putting *rules* to refer to these tags

### json schema

[introduction](schema.json)

* json schema allows annotation + validation of json documents
* tries to make json human+machine readable
* essentially can give *headers* to objects to *define* what they are
