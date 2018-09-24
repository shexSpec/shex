# Shape Expressions (ShEx)

[![Join the chat at https://gitter.im/shapeExpressions](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/shapeExpressions?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This repository is for Shape Expressions management. 

It contains links to:
- [Gitter chat](https://gitter.im/shapeExpressions/Lobby)
- [issues list](https://github.com/shexSpec/shex/issues) ([2.0 release](https://github.com/shexSpec/shex/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0))
- [spec](https://shexspec.github.io/spec) ([repository](https://github.com/shexSpec/spec))
- [test suite](https://github.com/shexSpec/shexTest) ([implementation reports](http://shexspec.github.io/shexTest/reports/))
- [primer](https://shexspec.github.io/primer) ([repository](https://github.com/shexSpec/primer))
- [Public ShEx mailing list](mailto:public-shex@w3.org) ([archive](https://lists.w3.org/Archives/Public/public-shex/))
- [ShEx community group](https://www.w3.org/community/shex/)

## Branches

The following branches are in play (or …pending or ☠deletable) in shexSpec:

branch | repos | use
-- | -- | --
**master** | shexTest, shex.js, …spec | approved stuff for ShEx.next
├**new_spec** | shexTest | stratified negation violations
├┐**shapesMap** | …shexTest, …shex.js, …spec | exploit new JSON-LD 1.1 ability to have `shapes` be a map
│└**simpleRef** | shexTest, …shex.js, …spec | simplify grammar to allow simple references, e.g. `<S1> @<S2>`
├**tcParens** | ☠shexTest | allow ()s around TripleConstraint
├**LanguageTag-data-case** | shexTest | variations in data language-tag case
├**LanguageTag-schema-case** | shexTest | variations in schema language-tag case
├**langString-ATstar** | shexTest | empty language stem e.g. `[@~]` or `[@~ - @fr-be~]`
├**accessors** | shexTest, shex.js | language extension to compare against paths and test uniqueness constraints
├**negation** | shexTest, shex.js | language extension to test `!<p1> .` against graph (not neighborhood)
├──┐**backtick** | shex.js | look for entities with corresponding label
├─┐│**remoteQuery** | shex.js | execute validation queries via SPARQL
├┐││**worker** | shex.js | execute validation in Web Worker
│└└└**wikidata** | shex.js | combines 
├**2.0** | shexTest | state of tests for ShEx 2.0 release
└**ShEx1** | shexTest | archive of ShEx 1 tests
