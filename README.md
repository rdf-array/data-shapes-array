<img src=".github/rdf-array-logo.png" alt="[data-shapes]" width="96" align="right">

# data-shapes-array: data-shapes for rdf-array

**rdf-array** fork of [data-shapes](https://github.com/w3c/data-shapes): the W3C SHACL 1.0 and SHACL 1.2 test suites (`data-shapes-test-suite/`, `shacl12-test-suite/`) and shacl-shacl read as [SHACL-array](https://rdf-array.github.io/rdf-array/shacl-array.html), SHACL over rdf-array: a SHACL list is a sequence term `( … )`, an `rdf:first`/`rdf:rest` ladder is triples and not a list, `()` is the empty list and `rdf:nil` an IRI. The tests whose expectations depended on ladders or on `rdf:nil` are re-recorded or, where nothing is left to test, marked `mf:status sht:rejected`; each carries an `rdfs:comment "rdf-array: …"`. shacl-shacl checks lists with `sh:memberShape` and `sh:minListLength`. New tests live in [data-shapes-contrib](https://github.com/rdf-array/data-shapes-contrib). Branch `rdf-sequences`, published at <https://rdf-array.github.io/data-shapes-array/>; what changed and how it was checked is in [doc/rdf-array.md](doc/rdf-array.md). Part of the [rdf-array](https://github.com/rdf-array/rdf-array) organisation.

![](w3c-no-bars.svg)

# Data Shapes WG Repository

This repository is the working space of the [W3C's Data Shapes Working Group](https://www.w3.org/groups/wg/data-shapes/). 

The WG is currently working on updated and new [SHACL](https://www.w3.org/TR/shacl/) recommendations, as per the [WG's Charter](https://www.w3.org/2024/12/data-shapes.html).

## Deliverables:

#### Phase 1, prioritised until July 2025.

##### Public Working Drafts

* [SHACL 1.2 Core](https://w3c.github.io/data-shapes/shacl12-core/)
* [SHACL 1.2 SPARQL Extensions](https://w3c.github.io/data-shapes/shacl12-sparql/)

##### Editor's draft

* [SHACL 1.2 Node Expressions](https://w3c.github.io/data-shapes/shacl12-node-expr/)

#### Phase 2, now in progress since July 2025:

##### Editor's drafts

* [SHACL 1.2 Overview](https://w3c.github.io/data-shapes/shacl12-overview/)
* [SHACL 1.2 Rules](https://w3c.github.io/data-shapes/shacl12-rules/)
* [SHACL 1.2 UI](https://w3c.github.io/data-shapes/shacl12-ui/)
* [SHACL 1.2 Compact Syntax](https://w3c.github.io/data-shapes/shacl12-cs/)
* [SHACL 1.2 Profiling](https://w3c.github.io/data-shapes/shacl12-profiling/)

All of the above specifications are drafts that are automatically rebuilt on pushes to this repository.

#### Previous WG & Community Group Deliverables

##### Recommendations

* [SHACL](https://www.w3.org/TR/shacl/)

##### WG Notes

* [SHACL Use Cases and Requirements](https://www.w3.org/TR/shacl-ucr/)
* [SHACL Advanced Features](https://www.w3.org/TR/shacl-af/)
* [SHACL JavaScript Extensions](https://www.w3.org/TR/shacl-js/)

##### Test Suites

* [SHACL Test Suite and Implementation Report](https://w3c.github.io/data-shapes/data-shapes-test-suite/)

##### Community Drafts

* [SHACL Compact Syntax](https://w3c.github.io/shacl/shacl-compact-syntax/)

## Agendas and other admin

Please look to the [admin directory in the agenda fork](https://github.com/w3c/data-shapes/tree/agenda/admin).

## Contact & Involvement

To contact the working group, you can either:

**Contact the WG directly**  
Via the  [W3C's Data Shapes WG page](https://www.w3.org/groups/wg/data-shapes/).

**Lodge Issues against WG documents**    
Using [this repository's Issue Tracker](https://github.com/w3c/data-shapes/issues).