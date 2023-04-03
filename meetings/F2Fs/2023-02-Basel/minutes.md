## Minutes
* New text written durring meeting for spec related to semantics of EXTENDS: http://shex.io/shex-next/
* https://github.com/hsolbrig/PyShEx/pull/85
* Group will set up some sessions with Harold to go into greater detail about steps to extend PyShEx
* git clone git@github.com:shexspec/grammar.git
* git clone git@github.com:shexspec/grammar-python-antlr.git

* download from https://www.antlr.org/download.html https://www.antlr.org/download/antlr-4.???-complete.jar
mv antlr-4.?????-complete.jar /usr/local/lib
CLASSPATH=.:/usr/local/lib/antlr-4.???complete.jar:
antlr4: aliased to java -Xmx500M -cp "$CLASSPATH" org.antlr.v4.Tool

* Note that the major and minor of antlr4 have to match pipenv / requirements.txt

* antlr4-python3-runtime = "~=4.11.4"

* git clone git@github.com:hsolbrig/pyjsg.git
(Not really necessary, but it is the spec)

* git clone git@github.com:hsolbrig/shexjsg.git
* git clone git@github.com:hsolbrig/pyshex.git

* Dependencies:

* grammar-python-antlr.git (aka PyShExC)

    shexTest (for testing) -- has to be in directory (could make into a submodule) q: is already a submodule in grammar?
    grammar (for g4 source)
    shexjsg (for testing)
    antlr4

* ShExJSG

    PyJSG
    pyshexc (for testing)

* PyJSG

    antlr4

* PyShEx

    ShExJSG
    PyShExC
    sparqlslurper
    sparqlwrapper (I think that this got imported to rdflib?)
