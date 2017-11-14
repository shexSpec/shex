# Introduction

The [FancyShExDemo inheritance example](https://www.w3.org/2013/ShEx/FancyShExDemo.html?schemaURL=test/Issue-inheritance/schema.shex&dataURL=test/Issue-inheritance/pass-user-employee.ttl&colorize=true) has a base shape called `<Person>`:
```
ABSTRACT <PersonShape> {
    (  foaf:name xsd:string
     | foaf:givenName xsd:string+,
       foaf:familyName xsd:string),
    foaf:mbox IRI
}
```
which is extended by `<User>`:
```
<UserShape> &<PersonShape> { ex:representative @<EmployeeShape> }
```
and by `<Employee>` (which also extends a `<RepShape>`):
```
ABSTRACT <RepShape> { foaf:phone IRI+ }
<EmployeeShape> &<PersonShape> &<RepShape> { }
```
A node being tested against `@<PersonShape>`:
```
<User2>
    foaf:givenName "Bob" ;
    foaf:familyName "Smith" ;
    foaf:mbox <mail:bob@example.org> ;
    ex:representative <Thompson.J>
.
```
may be satisfied by any non-abstract derivative of `<PersonShape>`.
This entails two behaviors:
1. It builds a shape which includes the triple expressions from all included shapes.
2. It provides builds a set of possible shapes which can stand in for `<PersonShape>`.

The first can also be accomplished by creating a triple expression (TE):
```
<PersonTripleExpressionHolder> {

  $<PersonTE>
    (  foaf:name xsd:string
     | foaf:givenName xsd:string+,
       foaf:familyName xsd:string),
    foaf:mbox IRI
}
```
and including that TE in derived shapes:
```
<UserShape> { &<PersonTE>; ex:representative @<EmployeeShape> }
```
This inclusion has an additional `;` operator which explicitly creates and `EachOf` of the included TE and the remainder of the shape's TE:
```
<UserShape> {
  (
    (  foaf:name xsd:string
     | foaf:givenName xsd:string+,
       foaf:familyName xsd:string),
    foaf:mbox IRI
  );
  ex:representative @<EmployeeShape>
}
```
# Issue - `ShapeAnd` vs. `EachOf`
## `ShapeAnd`
One way to define inclusion would be to treat it as a `ShapeAnd`:

```
<UserShape> {
    (  foaf:name xsd:string
     | foaf:givenName xsd:string+,
       foaf:familyName xsd:string),
    foaf:mbox IRI
} AND {
  ex:representative @<EmployeeShape>
}
```
This works so long as the including shape is not CLOSED and there are no repeated properties shared between any included shape and the including shape.

A closed shape would fail because any candidate node would have more than e.g. `ex:representative`:
```
<UserShape> {
    (  foaf:name xsd:string
     | foaf:givenName xsd:string+,
       foaf:familyName xsd:string),
    foaf:mbox IRI
} AND CLOSED {
  ex:representative @<EmployeeShape>
}
```

![](https://placehold.it/350x40/fec/070?text=<HS>)

The above example is a bit difficult to understand.  How about:
```
<LonelyBox> CLOSED {foaf:name xsd:string, foaf:mbox IRI}
<ExtendedLonelyBox> &<LonelyBox> {ex:shoesize xsd:decimal}
```
A ShEx interpreter worth its salt would be able to identify the above as an error -- the set of possible graphs that could satisfy 
<ExtendedLonelyBox> is empty.  Same thing as the previous example, however.

![](https://placehold.it/350x40/fec/070?text=</HS>)

Likewise, a repeated property:
```
<BP> {
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . }
}
```
could be extended with additional required triples:
```
<PosturedBP> &<BP> {
  :component { :code [ "posture" ] ; :value . };
}
```
An `AND` semantics would fail because the constraints on `:component` are incompatible between the two ANDed shapes.

## `EachOf`

Treating this as an inclusion bypasses this issue as it creates an `EachOf` TE:
```
<PosturedBP> {
 (
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . }
 );
  :component { :code [ "posture" ] ; :value . };
}
```
This means that a node which conformed to a derived shape, e.g. `<PosturedBP>`:
```
<myPostureBP>
  :component [ :code "systolic" ; :value 110^^xsd:float };
  :component [ :code "diastolic" ; :value 80^^xsd:float ];
  :component [ :code "posture" ; :value "supine" ].
```
would not conform to a base shape `<BP>` which accepts only two `:components`.

## FHIR Slicing Use Case

The FHIR schema language calls repeatable properties `slicing`.
These can be open or closed, though they are commonly open.
For example, a `<BP>` would be open, allowing more "slices" on the `:component` property.
The closed one can be expressed as above in ShEx; the open one can be expressed by adding an `EXTRA :coomponent` to permit more slices on the property `:component`.
An open `<BP>`:
```
<BP> {
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . }
}
```
would accept the above instance of `<PostureBP>` (`<myPostureBP>`) as `:component` would be open.

## Two Inheritance Operators

XML Schema offers separate operators for [extension](https://www.w3.org/TR/xmlschema-1/#cos-ct-extends) and [restriction](https://www.w3.org/TR/xmlschema-1/#derivation-ok-restriction).
Taking `-` for restriction and `+` for extension, we could have a base shape define generic constraints on `:components`, a derived shape define their usage for a particular kind of observation, and a further derivation augment that usage:

```
<ObservationShape> {
  …
  :component {
    :code . ;
    :value .
  } *
}
<BP> -<ObservationShape> {
  …
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . }
}
<PosturedBP> +<BP> {
  :component { :code [ "posture" ] ; :value . };
}
```
Note that the `…`s here include many properties.


![](https://placehold.it/350x40/fec/070?text=<HS>)

This is desired behavior from my perspective, at least for one interpretation of "inherits".  A useful interpretation of:

```
<S2> &<S1> { ... }
```
Would be that every "instance" that satisfies S2 also satisfies S1 -- the set of graphs that satisfy S2 are a subset of
the set of graphs that satisfy S1.

In the above example, someone has gone to the trouble of asserting that \<PosturedBP> has exactly two components.  As it is not closed, 
it may have a bunch of other stuff as well, but only 2 components. If you wanted to open this up, you could say:
```
<BunchOfComponents> {
    :component {:code .; :value .}+;
}
<BP> &<BunchOfComponents> {
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . };
}
<PosturedBP> &<BP> {
  :component { :code [ "posture" ] ; :value . };
}
```
or
```
<BP> {
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . };
  :component { :code .; :value . }*;
}
```

## Discussion
The fundamental problem with `AND` semantics lies with repeats. A shape is extensible unless it is explicitly declared as closed. A
specific predicate, however, is "closed" unless it is declared as "open" through one of the approaches described above.  In any case,
we **can** get the desired behavior today -- CLOSED vs. default (i.e. not CLOSED) for new predicates and max-cardinality \< * for 
"closed" attributes that already exist vs. '+' or '\*' for 'open' attributes.

I don't object to an operation that violates the above rules -- something that says that instances of a "child" shape may *not* be valid
instances of the "parent", but this has to be crystal clear... we need to remember that ShEx was designed to be used as documentation, 
and, as such, (useful) extensions that have unexpected side effects such as re-opening a CLOSED shape or changing the cardinality or
or possible values for an attribute need to be crystal clear as to what they are doing.

Take:
```
<BP> CLOSED {
    :component { :code ["systolic", "diastolic"] ; value xsd:decimal }{1, 2};
}
<ViolatedBP> &<BP> {
    :component { :code ["systolic", "posture"]; value xsd:string } {3};
}
```
Being able to do the above might be handy for ShEx programmers, but anyone who reads that as all instances of \<ViolatedBP> 
are also instances of
\<BP> will be in for a rude surprise. I would expect:
```
<RestrictedBP> &<BP> {
    :component { :code ["systolic"] }{1};
}
```
To act as a constraint -- this may be a tall order, however.

![](https://placehold.it/350x40/fec/070?text=</HS>)





    



