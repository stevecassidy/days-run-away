---
id: 221
title: 'Extreme Markup 2003: Day 3'
date: 2003-07-30T14:00:00+00:00
author: Steve Cassidy
layout: post
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=221
permalink: /2003/07/extreme-markup-2003-day-3/
categories:
  - Uncategorized
---
### Paolo Ciancarini

[Topic Maps and RDF](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Presutti01/EML2003Presutti01-toc.html) Talks about converting topic maps to RDF and back again, discusses some of the issues in doing the conversion both ways and presents an editor for doing the conversion and modifying/browsing the result. Has a java tool for doing this which might be made available on their website one day.

### James Mason

[Publication under topic map control](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Mason01/EML2003Mason01-toc.html) Deals with guidance documents for security classification which help people decide on the level of classification to assign to a given document. He's turned some of these guidance documents into into a topic map to be used for assisting in the classification task. 

### Lars Marius Garshol (Ontopia)

[tolog - topic map query](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Garshol01/EML2003Garshol01-toc.html) various proposals have been made but no standard, ISO cttee publilshed some requirements and recently a data model document. tolog, simple idea: topic map associations are like Prolog facts: queries are really like Prolog queries. Three implementations, one commercial from Ontopia (in the Omnigator) another layered on SQL and one in TM4J. 

tolog (after the fact) found to be very similalr to datalog (with negation, plus some SQL features). tolog queries match data and return tables (note: not composable?). Example:

<pre>born-in($PERSON: person, $PLACE: place),
located-in($PLACE: containee, italy: container) </pre>

Or use projection: 

<pre>select $PERSON  where 
born-in($PERSON: person, $PLACE: place),
located-in($PLACE: containee, italy: container) </pre>

Allow explicit disjunction (A | B) instead of using the Prolog way: to make it clearer for users. Defines some built in predicates, eg. <tt>instance-of</tt>, <tt>direct-instance-of</tt>. Has negation but require all vars to be bound when called (ie. not(composer($FOO)) won't generate all non-composers. Has full Prolog rules so complex programs are possible.

tolog 0.1 only queries associations with already known types. There are many other bits of the TM data model (ref?) that can't be queried. Eg. find all topics, find all occurences. Add new built-in predicates to do these, eg. topic-maps($TOPICS), BUT: what if there's an association called &#8216;topic-maps' (answer is that you need to use NS qualified predicate names, see below). 

Non binding closes &#8212; find all companies in Oslo and their home pages if they have them, use an empty or branch to indicate an optional clause:

<pre>located($COMPANY,oslo), (homepage($COMPANY,$HOMEPAGE) | )</pre>

There's an issue of referring to topics since they have fully qualified names (uris) and these depend on the file that the TM is stored in (and there are three different kinds). Queries become verbose and unmanageable. Answer is to use NS prefixes instead of full uri prefixes.

Defines a string module, but this introduces unsafe (defn: datalog) predicates such as string:length($STR,5) which shouldn't be evaluated unless $STR is bound. 

tolog can query anything: shows RDF, relational queries. TMTL: XSLT for topic maps (proposal). Works like XSLT but without xsl:template, eg: <tt><tmtl:foreach select="...tolog...">...</tt> 

### Liam Quin

[XML Query Update](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Quin01/EML2003Quin01-toc.html) A number of free implementations. XQengine (Howard Katz) is v. incomplete (see yesterday) but the only implementation which indexes files. Qexo is part of Kawa (scheme based) engine, compiles queries to java bytecode, can run in a servlet. IPSI-XQ (Java) does extensive query optimisation (and can show you what it did). Galax (written in OCAML) has schema support and static type checking, supports ML, C, C++ and Java APIs, second fastest in tests. Saxon is the fastest, now has XQuery as well as XSLT.

Since the standard isn't final yet, all of these implementations differ in which version they support. 

### Norm Walsh

[RDFTwig](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Walsh01/EML2003Walsh01-toc.html) There isn't a good way to work with RDF in XSLT because RDF isn't trees and XSLT/Xpath are optimised to work with trees. Can work with serialised RDF but ultimately you're working with the wrong data model: can be worked around but it's' fragile. There's no unique serialisation. RDFTwig lets you work with different serialisations within your stylesheet. Implemented in Java on top of the Jena RDF store. 

Different serialisations (breadth, depth and &#8216;breadth-first-deep' &#8212; duplicate nodes as long as you avoid cycles). RDFTwig defines a bunch of fns which return trees which can then be styled with XSLT.

Also supports Jena's RDQL &#8212; embed RDQL statements in <rt:rdql> 

Henry Thompson raises the option of serialising as a flat structure where everything is a reference &#8212; don't use the daughter relation for anything. This would make the xpath expressions more verbose (and perhaps impossible in XPath 1.0) but would avoid having to special case the places when references happen.

### Bob Lyons

[The schema conformance problem](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Lyons01/EML2003Lyons01-toc.html) is there an XML document that conforms to a schema. There can be schema for which you can't generate a conformant document, eg:

<pre>&lt;!ELEMENT section  (section+)&gt;   </pre>

Schema conformance is NP hard for DTD, RELAXNG (with XML Schema datatypes), NRL, undecideable (ie worse) for Schematron and XML Schema. Solvable in linear time for DTD with no ID/IDREF attributes and for RELAXNG with the base datatypes.
