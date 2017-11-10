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
and indirectly by `<Employee>`:
```
ABSTRACT <RepShape> { foaf:phone IRI+ }
<EmployeeShape> &<PersonShape> &<RepShape> { }
```
HS --> Why do you call this "indirect" -- I would have thought that indirect would have been:
```
<RepShape> &<PersonShape> { ex:representative @<EmployeeShape> }
<EmployeeShape> &<RepShape> {}
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
# Issue - `AND` vs. `EachOf`

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
HS -- the above example is a bit difficult to understand.  How about:
```
<LonelyBox> CLOSED {foaf:name xsd:string, foaf:mbox IRI}
<ExtendedLonelyBox> &<LonelyBox> {ex:shoesize xsd:decimal}
```
A ShEx interpreter worth its salt would be able to identify the above as an error -- the set of possible graphs that could satisfy 
<ExtendedLonelyBox> is empty.  Same thing as the previous example, however.
    
Likewise, a repeated property:
```
<BP> {
  :component { :code [ "systolic" ] ; :value . };
  :component { :code [ "diastolic" ] ; :value . };
}
```
could be extended with additional required triples:
```
<PosturedBP> &<BP> {
  :component { :code [ "posture" ] ; :value . };
}
```
An `AND` semantics would fail because the constraints on `:component` are incompatible between the two ANDed shapes.
Treating this as an inclusion bypasses this issue as it creates a single TE:
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
<s>
  :component [ :code "systolic" ; :value 110^^xsd:float };
  :component [ :code "diastolic" ; :value 80^^xsd:float ];
  :component [ :code "posture" ; :value "supone" ].
```
would not conform to a base shape `<BP>` which accepts only two `:components`.

HS -- This is desired behavior from my perspective, at least for one interpretation of "inherits".  A useful interpretation of:

```
<S2> &<S1> { ... }
```
Would be that every "instance" that satisfies S2 also satisfies S1 -- the set of graphs that satisfy S2 are a subset of
the set of graphs that satisfy S1.

In the above example, someone has gone to the trouble of asserting that <PosturedBP> has exactly two components.  As it is not closed, 
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


    



