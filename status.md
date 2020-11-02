# PROPOSED Specifications

| name | URL | resources |
| - | - | - |
| ShEx 2.1 | http://shex.io/shex-semantics/ | [primer](http://shex.io/shex-primer/), [implementation reports](http://shexspec.github.io/shexTest/reports/2.1/) |
| ShEx 2.0 | http://shex.io/shex-semantics-20170713/ | [implementation reports](http://shexspec.github.io/shexTest/reports/2.0/) |
| ShapeMaps | https://shexspec.github.io/shape-map/ | |

# Feature Proposals

| name | status | champion | resources |
| - | - | - | - |
| extends | 2.ii | ericP | [primer text](https://rawgit.com/shexSpec/primer/extends/#inheritance), [notes](https://hackmd.io/IO78Rl6AQrmyFrAmyVm_Iw?both#ExtIsOrthogonal1), \[Try it\]([shex.js](https://rawgit.com/shexSpec/shex.js/extends/packages/shex-webapp/doc/shex-simple.html?manifestURL=../examples/inheritance/manifest.json))|
| discriminators | 1 | Harold | [notes](https://hackmd.io/1fpnYHxoSYOQhvYxHXddjA), [1st pass](https://gitter.im/shapeExpressions/discriminator) |
| warnings | 1 | -- | |
| unique | 1 | ericP | [wiki](https://github.com/shexSpec/shex/wiki/Unique) |
| named graphs | 1 | -- | [wiki](https://github.com/shexSpec/shex/wiki/NamedGraphs) |
| API | 2.i (ish) | -- | [spec](https://github.com/shexSpec/shex/wiki/API), [wiki](https://github.com/shexSpec/shex/wiki/API#shex-api) |

# Process

This PROPOSED process is analogous to the [WebAssembly](https://github.com/WebAssembly/meetings/blob/master/process/phases.md) and [Tc39 (Javascript)](https://tc39.es/process-document/) processes:

1. community endorsed crackpot idea - people think the crackpot idea is worth persuing.
2. defined
   1. specified - structure and semantics are adequate for implementation.
   2. testable - test suites provide good coverage of the crackpot idea.
3. tested - two implementations pass each test.
4. integrated - structure and semantics are polished in integrated into the specification.
5. published - a version of ShEx with this crackpot idea has been published.
