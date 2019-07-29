---
id: 223
title: 'Extreme Markup 2003: Day 1'
date: 2003-07-28T14:00:00+00:00
author: Steve Cassidy
layout: single
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=223
categories:
  - Uncategorized
---
Day 1 of the main conference saw an interesting range of papers from hard core modal logic applied to document markup to tips for making XSLT writing easier.

### B. Tommie Usdin

[It's the Markup, Stupid!](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Usdin01/EML2003Usdin01-toc.html) Why is XML popular? Don't believe the Hype. XML is just a syntax, data models came later and there are lots of them (at least 3). What XML is: Pointy brackets, unicode, constraint language (DTD), data models, trees, family of specs. These aren't related to the touted features &#8212; platform independance, internationalisation, future proofing, etc. So what's the story?

What is good/bad XML? All XML is well formed and could be valid &#8212; that's not the difference. BAD XML is stuff like MS Word's HTML export, but why? Good XML helps us achieve our business goals, but no specs tell us how to make Good XML. The secret rules of Good XML Markup are: Generic Markup and Indirection.

Seperating form and content is what enables businesses to achieve their goals, but the XML specs don't talk about this. (Last standard to talk about this was the GENCODE/SGML spec from 1983).

Indirection is using names to refer to things: notation declarations, entities, validation rules seperate from the document. Name it and point to it: allows me to change the pointer to enable portability. _Indirection Powers XML_

Key tool relating to Indirection: Oasis XML Catalogs. A way of mapping by logical names to entities outside the document &#8212; change the mapping to enable portability.

So, XML suceeds becauses it is a good way of doing Generic Markup and Indirection.

### Allen Renear

[Logic based approaches to documents](http://www.mulberrytech.com/Extreme/Proceedings/html/2002/CMSMcQ01/EML2002CMSMcQ01-toc.html). BECHEMEL Markup Semantics Project: SGML/XML markup makes assertions (this is a section, this is the title of this section). The document is given by the collection of licenced inferences, not the XML markup, not the data structure it serializes. Is an Existential-Conjunctive language (subset of FOL) good enough? Do you need to say more? Likely interesting extensions will be modal.

Can't expres negation, alternatives, conditionals, functions, uninversal quantification. Do we need to? Maybe: the author of A is German -> &#8216;the' implies universal quantifier (only one author). This implies that more than EC is needed.

_Alethic modal logic_ allows for some things to be always true while others can be true or false....and here I get lost in the details of modal logics for reasoning about documents.

### Jenni Tennison

_Type related changes in XSLT_, while XSLT2.0 is generally good this talk critiques the type related changes in the spec. Motivations for these are in alignment with XML Schema and to allow static typechecking of documents. Usual story is that type related changes can be ignored and XSLT1.0 stylesheets will still run. Is this true? Answer one, running existing stylesheets should be ok in \`compatability mode'. 

Type system is complicated by the fact that schema validation is optional (and probably won't be commonly available for a while) so the static type checking benifits won't be available. Type system is largely similar to XML Schema (but not the same) and using it (with some caveats) gives benifits like being able to sort on dates.

Casting makes XSLT2.0 more complicated since auto-casting isn't done. Explicit casting is needed in most cases.

### Simon St. Leurant

_What can you do with half a parser?_ an amusing talk illustrated by playmobile figures acting out the drama of XML's entry into the workplace. Core message is that we can disect the XML standard and build processing tools that allow us to ignore the parts that aren't of interest.

### Graham Moore

[Engineering the Semantic Web](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Moore01/EML2003Moore01-toc.html) We're in need of a communicatiton protocol to underly the semantic web, so that agents scan talk to metadata sources for example. We've got lots of tools for storing and serialising data, topic maps, rdf etc. Existing communication is in terms of HTTP and HTML, not as direct communication between, eg. data sources/data models. Shows image of the [semantic web layer cake](http://www.w3.org/2001/09/06-ecdl/slide17-0.html) but there's nowhere that the communication protocol can fit.

SW Use cases: web clients finding out metadata about a web resource being browsed. Client applications aggregating SW data from multiple sources. Possibly allow update of SW data as well as just query. 

Discusses some of the requirements for SW server. Can it be layered upon HTTP, deployment should be easy...

Identity resolution, RDF has a problem differentiating a &#8216;resource' and &#8216;knowledge about the reification of that resource' &#8212; both are the URI. Topic maps have standardised this.

Evaluate HTTP: don't want data stored on the server as RDF/XML or XTM &#8212; we might accept that these would be delivered but we don't want t o have to query the XML representations &#8212; need to work with the data store.

Evaluate URIQA (URI Query Agent, Patrick Stickler (Nokia)). HTTP extensioon that given a URI will return a concise bounded description (approx. all things known about the identifer). Easy to deploy and implement, has basic query support. No update, introspection support (ie. what QL do you support). Not a general mechanism for interacting with RDF models. [note though that this is really just thin layering on top of HTTP and so isn't really different to the previous para].

RDF Net API: define a protocol to enable the semantic web to allow querying and updatting RDF models. API supports: Query, GetStatements, InsertStatements, RemoveStatements PutStatements, UpdateStatetments, Options (of the server). Defined an HTTP and a SOAP binding for the protocol. Hopes that this is the _SAX_ enabler for the semantic web.

### [Kal Ahamed](http://www.techquila.com)

[Topic map design patterns](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Ahmed01/EML2003Ahmed01-toc.html). Subject **indicator** is a resource (document) which describes a subject to a human reader, might include machine oriented metadata for auto-consumption. Subject **identifier** defines an identity for a subject, can be compared to determine subject equality, should resolve to a subject indicator. Problems with these: what should be in an indic.? How published? How do I know when something is meant as an identifier?

Published Subject Indicator (PSI) defined by OASIS for defining subjects, required to be stable, accompanied by meta-data. Addresses 1.5 of the three objections (publishing as XHTML and PSI URIs are specially defined, but not enough about content of PSI documents. 

Missing link relates to prescriptive human readable content...

Design patterns, named generic solutions, having names makes them easier to talk about, compare, etc. Gives better solutions. Topic map design is similar (shared design problems, more than one solution, confusing for the novice) and different (about organisation, identity, semantics) than programming. So what's a topic map DP? KA say's pretty much all of it, including a PSI ref to an implementation of the DP. This fits in to the prescriptive human readable slot noted as missing above (I don't understand this yet). Diagram TM DPs as UML diagrams. Fill the role of prescriptive subject descriptions &#8212; describe how the data has been organised.

### Holman

[Literate XSLT](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Holman01/EML2003Holman01-toc.html) XSLT is trasformation by example. Try to address the challenge of writing stylesheets via Literate programming. &#8220;When writing stylesheets, focus on the result&#8221;. Distinguishes pull (xsl:for-each, xsl:value-of) vs push (xsl:apply-templates) of writing stylesheets. Push is modular, pull is monolithic, push is better because it's more reuseable.

Talks about doing XSLT design by annotating the result tree (for a push oriented stylesheet). Achieve this fairly easily by adding markup to mock result, running through <tt>literatexslt.xsl</tt> to produce the XSLT stylesheet. Advantage is that the mock markup is still viewable (since another namespace is used and, eg. HTML browser or FO processor will ignore). Among other things this allows some validation to be done on XPath expressions by using sample source data which has been generated to contain examples of all possible XPath expressions. Describes the process and the tools developed to follow this process. Sketches a GUI tool that might be used to carry out this kind of process involving interactive design of the mock result followed by drag and drop linking of the source to the mock result.
