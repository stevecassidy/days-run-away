---
id: 216
title: RDF Path Languages
date: 2003-10-16T14:00:00+00:00
author: Steve Cassidy
layout: single
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=216
categories:
  - Uncategorized
---
The world is moving quickly towards defining a path language for RDF and maybe for other more general directed graphs. Here's a few references:

  * [Pondering RDFPath](http://infomesh.net/2003/rdfpath/) which reviews a few proposals
  * [This thread](http://lists.w3.org/Archives/Public/www-rdf-rules/2003Sep/0001.html) on www-rdf-rules which is where the above reference comes from, contains a bunch of other references and some encouragement.
  *  	[A reference to my Extreme paper](http://jbetancourt.blogspot.com/2003_08_24_jbetancourt_archive.html) noting that [IsaViz](http://www.w3.org/2001/11/IsaViz/) (an RDF visualisation/authoring tool) is an interesting use case for a graph path language.
  * [Simon St Laurent](http://lists.xml.org/archives/xml-dev/200308/msg00159.html) talks about the need to work directly with graphs instead of forcing data into trees.
  * [Treehugger](http://rdfweb.org/people/damian/treehugger/index1.html) is a Saxon extension to allow XPath expressions to be evaluated over RDF data. Another example of pretending the RDF is a tree, as in Normal Walsh's [RDFTwig](http://rdftwig.sf.net/).
  * [RDFT](http://www.semanticplanet.com/2003/08/rdft/spec) and RDF Templating proposal that includes a NodePath expression language, eg: 
    <pre>resource()/resource('http://ex.example.com/name')/literal()</pre>
