---
id: 330
title: Notes on Conversion of GrAF to RDF
date: 2011-02-18T17:32:47+00:00
author: Steve Cassidy
layout: post
guid: http://web.science.mq.edu.au/~cassidy/wordpress/?p=330
permalink: /2011/02/notes-on-conversion-of-graf-to-rdf/
categories:
  - Language Resources
tags:
  - annotation
  - dada
  - rdf
---
The Graph Annotation Format ([GrAF](http://www.cs.vassar.edu/~ide/papers/LAW.pdf)) is the XML data exchange format developed for the model of linguistic annotation described in the ISO Linguistic Annotation Framework (LAF). LAF is the abstract model of annotations represented as a graph structure, GrAF is an XML serialisation of the model intended for moving data between different tools. Both were developed by Nancy Ide and Keith Suderman in Vasser with input from the community involved in the [ISO standardisation process](http://www.tc37sc4.org/WG1/wg1.htm) around linguistic data.<!--more-->

Like the other candidate universal annotation models (e.g. Annotation Graphs and the model embodied by our own DADA system), LAF is a directed graph model. In this case, the graph connects nodes which are associated with one or more annotations with edges representing relations between nodes, by default, the parent-child relation. This is almost exactly the same as the DADA model, but the minor differences have been tripping me up for a while as I've tried to understand LAF enough to write conversion filters to ingest data into the DADA system.

A visit to Vassar last week was the ideal opportunity to clear up my understanding.  As a step towards updating our GrAF to DADA ingestion process which is implemented as [an XSL stylesheet](https://bitbucket.org/stevecassidy/dada/src/938811302db9/xsl/graf2dada.xsl), I decided to write a stylesheet to convert GrAF into a fairly literal RDF model.  This allowed me to think about the interpretation of GrAF structures independent of their translation to the current DADA model.  I was concerned mainly with the structural elements of GrAF, rather than the annotation meta-data; this is equally important, but can be dealt with separately.

I armed myself with the latest version of the ISO LAF documentation and a copy of the manually annotated sub-corpus ([MASC](http://www.anc.org/MASC/Home.html)) of the American National Corpus. This is a nice sized data set where the automatically generated annotations have been manually checked and corrected.

GrAF is an XML format for standoff markup, meaning that the annotation is stored in a separate file to the source text rather than being embedded in the text as is normal in TEI for example.  A single text has a number of associated XML annotation files, each containing a different kind of annotation. In the MASC corpus these include Penn Treebank, part of speech and named entity annotations.   A single .anc file acts as a master reference and contains pointers to the raw text as well as the other XML files.

GrAF defines five main elements to represent annotation structures: nodes, annotations, edges, links and regions.  The graph structure is made up of nodes and edges while regions define the parts of the source document being annotated.  The link element relates a region to a node and the annotation element defines an annotation structure that can be attached to a node or an edge.

## Identifiers

One thing that's required for an RDF representation is that each entity is denoted by a unique identifier (a URI).  Most but not all of the GrAF elements have identifiers denoted by the xml:id attribute so we can re-use this in the RDF representation prefixed with a suitable base URI.  In choosing a base URI it makes sense to generate one that denotes the collection as a whole, something like http://www.anc.org/MASC/spoken/RindnerBonnie (not a working URI although DADA could make it so).  So the first node in the Penn Treebank annotation for this document which has the xml:id ptb-n00000 would have the RDF identifier http://www.anc.org/MASC/spoken/RindnerBonnie#ptb-n00000.

An implication of this is that all identifiers need to be unique within the collection of XML files.  The GrAF specification doesn't mandate this and the use of xml:id attributes will only ensure that the identifier is unique within the XML file.  As it happens, many of the identifiers in the MASC corpus are made unique by being prefixed by the annotation set name (ptb in the example). Some are not unique however and so to generate useful RDF we need to either generate our own unique identifiers or fix the original data.

One entity that doesn't have an identifier is the annotation element.  Annotations are connected to either a node or an edge and use a &#8216;ref' attribute to indicate what they are attached to. To represent these in RDF, we generate a unique identifier for each one.

## Types

We need RDF types for each of the entities being represented. The GrAF XML namespace URI can be repurposed to generate names for the types, e.g. http://www.xces.org/ns/GrAF/1.0/Node, abbreviated as graf:Node.  We use capitalised names for RDF types as per the convention.

A second a more tricky type issue is that of denoting the different kinds of annotation that are used in the corpus.  LAF avoids any reference to types because there is no consensus on what constitutes a type in this context. Instead it has the idea of an annotation set which gives a name to a group of annotations, for example Penn Treebank or Framenet.  Each name as an associated URI defined in an annotationSet definition in either the annotation XML file or the corpus header.  These aren't formal namespace URIs, just a URI that would provide some information about the kind of annotation being used.

An annotation has the following form in the XML file:

<pre>    &lt;a label="vchunk" ref="vc-n0" as="xces"&gt;
        &lt;fs&gt;
            &lt;f name="voice" value="active"/&gt;
            &lt;f name="tense" value="SimPre"/&gt;
            &lt;f name="type" value="FVG"/&gt;
        &lt;/fs&gt;
    &lt;/a&gt;</pre>

Something I've been a little confused about is the meaning of the &#8216;label' attribute.  Following some discussion, it seems that the label is a kind of annotation type and that we can think of it as being within a &#8216;namespace' defined by the annotation set label (&#8216;xces' in this case).  The three features listed in the feature set can also be thought of as being in the same namespace. Hence we can translate this to RDF as a resource of type graf:Annotation and introduce a property graf:type to denote the type of annotation:

<pre>&lt;id35803001&gt; a graf:Annotation;
    graf:type xces:vchunk;
    graf:annotates &lt;id35993621&gt;
    xces:voice "active";
    xces:tense "SimPre";
    xces:type  "FVG" .</pre>

Note that we don't translate the feature structure node into an RDF resource, feature structures map well directly to RDF properties and there is no sense in which the feature structure element has any status other than as a container for feature value pairs in the XML serialisation.

This all works well in most cases but there are a few instances in the MASC data that cause trouble.  In a small number of files there is no annotation set associated with some annotations (eg. in data/written/116CUL032-vc.xml). This means that there is no namespace to associate with the feature names. In [the GrAF schema](http://www.xces.org/ns/GrAF/0.99/graf-0.99.rng), the annotation set is marked as an optional attribute, so this is not an error.  However, some way of assigning a default namespace to bare features like this is needed to convert to RDF.   I'd argue that someone converting annotations to GrAF should be forced to make a decision and give a name (URI) to their annotation set; in this way, the ownership of annotations is clear and we won't get confused between two uses of the same feature name by different people.

A second complication comes when a feature name or annotation set label is not a valid QName (XML element name). This makes the conversion to XML/RDF difficult although in some cases the name may still be a valid RDF identifier (URI).  One example in the MASC data is a feature xmlns:xsi (eg. in ﻿﻿data/written/110CYL072-logical.xml), obviously translated literally from an XML instance.   In this case, one could argue that the feature isn't really an annotation on the source data and so shouldn't be included, but it raises the issue of what a valid identifier should be.  I think there's a strong case for requiring all identifiers to be qualified names in the sense described by the [XML Namespace standard](http://www.w3.org/TR/REC-xml-names/#dt-qualname), not just because I want to convert them easily to RDF, but because the concept of URI based names is so powerful in standards like this one.  We already have an emerging data category registry ([ISOCat](http://www.isocat.org/)) for names in the linguistic annotation space; this requirement would mesh well with the ISOCat facility to register names and would facilitate sharing of feature names and definitions.

In the style-sheet I'm writing now, I gloss over these two issues by generating a fake namespace URI where needed.

## Edges

In LAF, edges define relations between nodes and represent structural relations, mainly the parent-child relations needed to represent hierarchical structure.  Edges can also have annotations attached to them and the main use-case for this is the need for relationship types other than the default parent-child; a co-reference relationship between two nodes would be represented by an edge with an attached annotation containing the type name as a feature value.  Both of these cases are best represented in RDF by a regular relationship of an appropriate type.  In the MASC corpus, there aren't any examples of edges with attached annotations so all edges are converted to child relations by the stylesheet.  As an illustration, a resource of type graf:Edge is also created; an annotation could be attached to this in the same way as it is to a node.

## Regions

Regions are the means by which nodes in the graph are attached to the source media that is being annotated.  All regions in the MASC corpus are defined by two character offsets stored in the _anchors_ attribute.   The main issue with regions is not their representation in RDF but the choice of this kind of means of indicating location.   I'll leave that for another discussion as it doesn't impact on the choices made here to generate RDF from GrAF.

## Results

The most interesting result of this exercise is some insight into the design of GrAF and a better understanding on my part of the structures used in that format.  However, we can also apply the stylesheet to the data in the MASC corpus to get a set of RDF/XML files.  These can be fed into a triple store and queried with SPARQL.

To give an idea of the size of the data, the original XML files consists of 3505944 lines and 108M of text.  This translates to 3,935,634 triples.  I loaded this into a Sesame triple store and was able to browse the data easily using the workbench interface. Just as an illustration, a sample SPARQL query to find Penn Treebank annotations related by the child relation looks like:

<pre>PREFIX PTB:&lt;http://www.cis.upenn.edu/~treebank/&gt;
PREFIX graf:&lt;http://www.xces.org/ns/GrAF/1.0/&gt;
select ?parent ?plabel ?clabel
where {
        ?parent graf:child ?child .
        ?pann graf:annotates ?parent .
        ?pann graf:type PTB:tok .
        ?pann PTB:msd ?plabel .
        ?cann graf:annotates ?child .
        ?cann graf:type PTB:tok .
        ?cann PTB:msd ?clabel .
}</pre>

This runs reasonably quickly via the workbench web interface and returns a long list of results such as:

| Parent                                         | Plabel             | Clabel            |
| ---------------------------------------------- | ------------------ | ----------------- |
| <http://example.org/Article247_327/ptb-n00252> | &#8220;PRP$&#8221; | &#8220;NN&#8221;  |
| <http://example.org/Article247_327/ptb-n00806> | &#8220;PRP$&#8221; | &#8220;JJ&#8221;  |
| <http://example.org/Article247_327/ptb-n00973> | &#8220;PRP$&#8221; | &#8220;JJ&#8221;  |
| <http://example.org/Article247_327/ptb-n00973> | &#8220;PRP$&#8221; | &#8220;NNP&#8221; |
| <http://example.org/Article247_327/ptb-n00370> | &#8220;PRP$&#8221; | &#8220;JJ&#8221;  |
| <http://example.org/Article247_327/ptb-n00370> | &#8220;PRP$&#8221; | &#8220;JJ&#8221;  |

## Summary

This has been a useful exercise in understanding the structure of GrAF and hopefully illustrating some of the advantages of an RDF translation, in particular the usefulness of proper identifiers for each of the objects being described.  I'll take what I've learned here and modify the current GrAF ingestion scripts that are used to load annotations into the DADA triple store. Once that's done I should be able to publish a sample DADA linked data interface to the MASC corpus. Watch this space for a link.

The stylesheet can be found in the DADA source tree: [graf2rdf.xsl](https://bitbucket.org/stevecassidy/dada/raw/ed91bc01605f/xsl/graf2rdf.xsl) or check the [DADA project on Bitbucket](https://bitbucket.org/stevecassidy/dada/) for a more recent version.
