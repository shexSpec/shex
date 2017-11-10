= Introduction =

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
= Issue - AND vs. EachOf =
