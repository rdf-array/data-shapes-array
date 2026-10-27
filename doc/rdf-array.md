# data-shapes-array: the W3C SHACL test suites for rdf-array

This fork of [w3c/data-shapes](https://github.com/w3c/data-shapes) carries the SHACL 1.0 test suite
(`data-shapes-test-suite/`), the SHACL 1.2 test suite (`shacl12-test-suite/`) and shacl-shacl read as
[SHACL-array](https://rdf-array.github.io/rdf-array/docs/shacl-array.html): SHACL over
[rdf-array](https://github.com/rdf-array/rdf-array), RDF whose data model has sequences as terms. In
SHACL-array a SHACL list is a sequence term `( … )` and nothing else, an `rdf:first`/`rdf:rest` ladder
spelled out in triples is ordinary triples, `()` is the empty list and `rdf:nil` an ordinary IRI; SHACL
1.2's `sh:memberShape`, `sh:minListLength`, `sh:maxListLength` and `sh:uniqueMembers` constrain sequence
values. Branch `rdf-sequences`, cut from upstream a7f5f906 (2026-04-20, the commit rudof-array's
submodule pinned), published at <https://rdf-array.github.io/data-shapes-array/>. Tests that are new
under rdf-array live in [data-shapes-contrib](https://github.com/rdf-array/data-shapes-contrib).

## The rules

As in the other suite forks (rdf-tests-array, json-ld-api-array, shexTest-array):

1. **A test is never deleted.** A changed test keeps its name and gains an `rdfs:comment "rdf-array: …"`.
2. **Expectations change, inputs do not.** Shapes and data stay as written; where they spell a list
   with `( … )` they already read as sequences, and where they spell `rdf:first`/`rdf:rest`/`rdf:nil`
   explicitly those are ordinary triples and IRIs.
3. **Where a test has nothing left to test** under SHACL-array, it stays in its manifest with
   `mf:status sht:rejected` (the SHACL test vocabulary's status for a test not to be run) and the reason;
   its counterpart, when one makes sense, is in data-shapes-contrib.
4. **Every re-recorded expectation is derived twice**, by SHACL-array's reading by hand and by an
   implementation: jena-array's `jena-shacl` for all of them, and rudof-array where it has the feature
   (it has no SHACL 1.2 list constraints yet).

## Re-recorded (5)

| Suite | Test | Was | Now |
|---|---|---|---|
| 1.0 | `core/path/path-strange-001` | the path node is both a list `( ex:p ex:q )` and an inverse path; the list reading wins: violation at `ex:j` | the `rdf:first`/`rdf:rest` triples are not a list, so the path is `^ex:p`: violation at `ex:i`, value `ex:pi`, `sh:resultPath [ sh:inversePath ex:p ]` (jena-array and rudof-array agree) |
| 1.2 | `core/property/memberShape-001` | `_:b1`, a ladder, is a list whose member `_:bc` fails (`sh:detail`) | `_:b1` is not a list: the same violation, without `sh:detail` |
| 1.2 | `core/property/maxListLength-001` | `rdf:nil` is the empty list and conforms | `rdf:nil` is not a list: a new violation at `ex:person2`; `_:b1` (a ladder) violates as a non-list |
| 1.2 | `core/property/uniqueMembers-001` | `rdf:nil` conforms; `_:b1` has repeated members (`sh:detail` for each) | `rdf:nil` violates (new result at `ex:person2`); `_:b1` violates as a non-list, without details |
| 1.2 | `node-expr/shnex/list` `list-empty` | `()` is the constant `rdf:nil`: `mf:result ( rdf:nil )` | `()` is the empty sequence, a list expression with no members: `mf:result ()` |

## Rejected (7)

| Suite | Test | Why |
|---|---|---|
| 1.0 | `core/path/path-strange-002` | it chose the list reading of the path node over its invalid inverse path; without the list only `sh:inversePath ( ex:p )` is left, a one-member sequence path, which SHACL does not allow (jena-array reads it leniently as `ex:p`; rudof-array panics, [rdf-array#17](https://github.com/rdf-array/rdf-array/issues/17)) |
| 1.2 | `core/node/memberShape-001`, `minListLength-001`, `maxListLength-001`, `uniqueMembers-001` | the focus nodes are IRIs (and `rdf:nil`) spelled as ladders; an IRI is never a sequence, so every focus node violates and the tests no longer tell lists apart. Counterparts in data-shapes-contrib target sequence values |
| 1.2 | `core/node/in-003`, `core/node/xone-003` | `sh:in rdf:nil` / `sh:xone rdf:nil` are not lists, so the shapes graph is ill-formed. Counterparts with `sh:in ()` / `sh:xone ()` are in data-shapes-contrib. (Upstream, in-003 also does not parse: the `shsh:` prefix is undeclared, and its `sh:property` names `shsh:inSubjectsShapeXonePropertyShape`, which is not defined.) |

## shacl-shacl

`shacl/shacl-shacl.ttl`, `shacl12-vocabularies/shacl-shacl.ttl` and the copies the two suites validate
against themselves (`tests/core/complex/shacl-shacl-data-shapes.ttl`; the SHACL 1.2 suite's copy is the
1.0 shacl-shacl) are rewritten by `tools/rewrite-shacl-shacl.py` in rdf-array/rdf-array:

- `shsh:ListShape` is `sh:minListLength 0`, "any sequence"; `shsh:ListNodeShape` (the cells of a ladder) is gone;
- the member checks through `( p [ sh:zeroOrMorePath rdf:rest ] rdf:first )` are `sh:path p ; sh:memberShape [ … ]`
  (`sh:ignoredProperties`, `sh:languageIn`), and `shsh:ShapesListShape` uses `sh:memberShape shsh:ShapeShape`;
- a sequence path is a sequence term, not a blank node: `shsh:PathNodeShape`'s sequence and alternative
  branches check `sh:memberShape shsh:PathShape` and `sh:minListLength 2` (as SHACL 1.2's version already did
  for most of it), `shsh:PathListWithAtLeast2Members` is `sh:minListLength 2`, and `_:PathPath` no longer walks lists.

The `core/complex/shacl-shacl` tests (shacl-shacl validating itself) are unchanged in their expectation,
`sh:conforms true`, and carry a comment and `mf:requires ra:ShaclListConstraints`: a processor without SHACL 1.2's
list constraints ignores `sh:memberShape` and `sh:minListLength`, the sequence-path branch of `shsh:PathNodeShape`
then matches every path, and the `sh:xone` over path kinds fails (rudof-array, [rdf-array#17](https://github.com/rdf-array/rdf-array/issues/17)). Checked with jena-array: all four files conform to themselves, and
both versions report a ladder as `sh:in`, a non-list `sh:or`, a one-member sequence path and a non-string
`sh:languageIn` member, while a well-formed shape with nested sequence paths conforms.

## Unchanged on purpose

- `core/path/path-unused-001-shapes` (both suites): its `[ rdf:rest rdf:nil ]` is inside a path no shape uses.
- 1.2 `core/property/minListLength-001`: `rdf:nil` and `ex:notAList` violate, as before, though now as non-lists
  rather than as an empty list and a non-list.
- Every other test: their lists are written `( … )` and read as sequences, which is what SHACL-array means by a
  SHACL list. jena-array's `jena-shacl` passes the SHACL 1.0 core suite with only the changes above.

## Running

- jena-array's `jena-shacl` runs `data-shapes-test-suite/tests/core` (`TestShaclCoreWG`, in-tree copy in
  `jena-shacl/src/test/files/std`, refreshed by `std/update.sh`) and SHACL 1.2's eight list-constraint tests
  (`TestShacl12Lists`), skipping `mf:status sht:rejected` tests with their reason: all pass, 5 skipped.
- rudof-array's `shacl/tests/data-shapes` submodule points at this fork (branch `rdf-sequences`):
  path-strange-001 and node-expr `list` pass; shacl-shacl and path-strange-002 are ignored with the reasons
  above ([rdf-array#17](https://github.com/rdf-array/rdf-array/issues/17)).
