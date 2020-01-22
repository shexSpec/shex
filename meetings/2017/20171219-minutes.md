## Telecon 2017-12-19 Minutes

 * Participants: Dimitris, Tom, Gregg, Kat
 * Regrets:  Lucas, Andra

Call in Jitsi # https://meet.jit.si/ShEx
 * Chair: Dimitris
 * Scribe:

last minutes: https://github.com/shexSpec/shex/blob/master/meetings/20171205-minutes.md

Agenda: https://github.com/shexSpec/shex/blob/master/meetings/20171219-agenda.md

## Pending actions


# Agenda

## Shape Extensions: Issue 50 https://github.com/shexSpec/shex/issues/50

 * Eric: This is more complicated than XML schema, where there is no notion of extensions and restrictions.  You can inherit something with a restriction, then you could extend it.  If the more nuanced use cases are not met, it would be good if this met the main use cases.
 
 * Dimitris: Gregg suggests we think more - discuss in next meeting?
 
 * Eric: Could spend some time geeking them out.  Happy to do now.

https://github.com/shexSpec/shex/blob/master/Inheritance.md#fhir-slicing-use-case
https://github.com/shexSpec/shex/blob/master/meetings/20171205-minutes.md#discussion-on-inheritance-issues

* Gregg: My concern: by promoting restrictions in flat way, can miss cases where branching: "if this, then that".

* Eric: I was looking for a way to make sure that I could restrict an observation to be a blood pressure, then extend bp to be postured bp, then restrict again to be reclined bp, then extend again a full bp

* Gregg: Linear- what about something like on medication - then means you can't have eaten anything in the last 10 hrs, and on another branch there is something that requires you to have eaten in the last 10 hours, then there would be a conflict.

* Eric: We need C++ people who do templating like no other. Were you looking for a diamond?

* Gregg: I think a diamond might show the problem with this.

* Eric: Foolhardy to assume these extension/restriction opperators- that both have the effect of allowing a derived class to stand in for a base class? Does that seem like these are the types of things I would want to align with the implicit OR? 

* Dimitris: You want to support only this case or also constraints from further up in the hierarchy.

* Eric: I was trying to do both at the same time.

* Dimitris: Is it different in the end?

* Eric: I don't think so. The only way would be 2 shapes that accept the same data, by virtue of inheritance it would have an implicit OR- 

*Dimitris: Then would it be an expansion mechanism?

* Eric: yes. 

* Eric: When this is in conjuction with IMPORT. 

* Eric: I'm trying to meet the FHIR use cases. I have the impression that they are a motivating set of use cases- and fairly complex. Feel free to insist on additional use cases. 

* Gregg: algorithm that turns something that uses inheritance into something that doesn't use inheritance.
