# ShEx Telecons
## 

## Telecon 2020-04-15

Possible names for "Footprints"
| template | instance | verb | |
| --- | --- | --- | --- |
| footprint | footprint instance | stomp | [primer](http://uu3.org/checkouts/janeirodigital/footprintlib.js/footprints/primer?footprint,footprint%20instance,stomp) |
| blueprint | imprint | imprint | [primer](http://uu3.org/checkouts/janeirodigital/footprintlib.js/footprints/primer?blueprint,imprint,imprint) |
| constellation | constellation instance | instantiate | [primer](http://uu3.org/checkouts/janeirodigital/footprintlib.js/footprints/primer?constellation,constellation%20instance,instantiate) |
| seed | shrub | plant | [primer](http://uu3.org/checkouts/janeirodigital/footprintlib.js/footprints/primer?seed,shrub,plant) |

Andra: What is difference btw blueprint/footprint and Shape Expression?

Eric: "Here is the resource hierarchy I am expecting".

Discussing https://janeirodigital.github.io/footprints/primer?blueprint,imprint,stamp 

![](https://janeirodigital.github.io/footprints/blueprint.svg)

Andra: You are looking for a new name?

Eric: TimBL wants to connect elements in form to specific URL where that form should be located ("footprints").

Andra: DanBri: linking entity schema in Wikidata ("baskets") linking concepts to schemas.  Problem: With WD, more people speaking SPARQL, but not (yet?) ShEx.
https://schema-korf.wiki.opencura.com/wiki/Main_Page

Eric: Footprint is a declarative structure for a resource hierarchy. Footprints has implied hierarchy bound to different shapes. Useful when resource described is resource hierarchy that the footprint choreographs the construction of.  Qualifiers imply the construction of entities without actually making them.  Solution: nested structure. Footprints might. Blueprint - to imprint.

Andra: Footprints sounds more dynamic (depending on environment). Blueprint sounds more static.

Eric agrees: footprint is like a template. In Solid, defaults are hierarchical - look up in parent containers. Makes it easy to organize [medical records].  Artifact in Solid. People like to organize things hierarchically.

Andra: Knowledge Graph on top of a file system structure?

Eric: Everything can participate in that graph.

Eric: LD world is used to passing around graphs, but not rooted graphs. "Start at this node". Notion of "rooted graph" is not widely understood in RDF world.  Suppose the schema has a start. Waste of time to valid every node to look for matches. Will push on having rooted graphs in Health world.  XML world has problem with finding where to start.

Discussing https://www.biorxiv.org/content/10.1101/2020.04.05.026336v1.full.pdf+html

Eric: Most recent error: neglected to put the right https://github.com/shexSpec/spec/issues/35

PROPOSED: ericP to address spec#35 by making the obvious update to shex.io and spec.
ericP: +1
Andra: +1 
Tom: +1
Kat: +1
