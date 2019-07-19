---
id: 269
title: Sparql Endpoint for Python WSGI
date: 2008-08-21T22:07:14+00:00
author: Steve Cassidy
layout: single
guid: http://www.ics.mq.edu.au/~cassidy/?p=269
permalink: /2008/08/sparql-endpoint-for-python-wsgi/
categories:
  - Uncategorized
tags:
  - python
  - rdf
  - software
---
As part of [DADA](http://www.clt.mq.edu.au/Research/Projects/dada/) (and yes, that page is a bit out of date) I wanted to provide a [Sparql endpoint](http://www.w3.org/TR/rdf-sparql-protocol/) to allow experimentation with querying the raw RDF annotation data. So far, we've built everything using Redland in Python but it seems there is no exsiting Sparql endpoint implementation for this combination. The Sparql protocol document is long but as far as I can tell the core of the protocol is a simple GET request with an encoded Sparql query, results are returned as raw XML in the special Sparql result format or as RDF/XML if the return type is a graph. This proves to be very easy to implement on top of Redland since it's query operator returns exactly those result types.

So, I present [SparqlEndpoint-0.1](http://localhost:8080/wp-content/uploads/2008/08/sparqlendpoint-01.zip), a python module that provides a WSGI conformant implementation of a Sparql Endpoint for Redland. It almost certainly doesn't implement all of the protocol standard and it can be improved no end, for example by making it independant of the RDF backend it queries (eg. using RDFlib).

I'm not putting up a demo endpoint just yet as I'm having severe performance issues with my development server in combination with Redland. The triple store is growing rapidly to the millions of triples and the result is a huge latency (tens of minutes) to perform some queries. Given some recent discussion on the Redland list I'm wondering whether a jump to one of the RDF specific stores is the thing to do. This would probably mean rewriting my code in Java but based on the [Berlin Sparql Benchmark](http://www4.wiwiss.fu-berlin.de/bizer/BerlinSPARQLBenchmark/) numbers, Sesame and Jena have the kind of performance I need (sub second query response times on 100M triples).

Well, enough of that. If you are interested in SparqlEndpoint please download and take a look. If there is interest I'm happy to share it and host development somewhere accessible.
