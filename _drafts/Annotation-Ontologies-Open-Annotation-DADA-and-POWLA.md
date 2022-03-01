---
id: 388
title: 'Annotation Ontologies: Open Annotation, DADA and POWLA'
date: 2014-03-29T13:36:16+00:00
author: Steve Cassidy
excerpt: |
layout: post
guid: https://stevecassidy.net/wordpress/?p=388
permalink: /?p=388
categories:
  - Language Resources
tags:
  - annotation
  - dada
  - rdf
  - web
---
I&#8217;ve recently been looking at the work coming out of the [Open Annotation](http://www.openannotation.org/) working group at the W3C which itself has brought together a few different threads of work on annotation of web resources.  It&#8217;s interesting to look at how this relates to the work we&#8217;ve done on using RDF to represent Linguistic Annotations since, on the surface at least, these seem to be very similar applications.  I thought I&#8217;d write up some notes on the similarities and differences between the two approaches.

Open Annotation aims to develop an RDF vocabulary to represent annotations of web based resources.  It has a fairly broad set of use cases in mind but as far as I know, hasn&#8217;t yet considered the case of Linguistic Annotation explicitly.   The work clearly shows it&#8217;s heritage as a development of the very early work on Annotea which provided a web based service for storing small RDF fragments describing notes (annotations) anchored to parts of web pages. The primary use-case for Open Annotation seems to be allowing people to make comments on web resources, perhaps pointing to a part of the resource that the comment references.  So, for example, I might make a comment on a few lines from an HTML page, associated a semantic tag with a region within an image, or link a web page to a short segment of a video file.

It&#8217;s interesting to contrast this with the requirements for Linguistic Annotation.  Our annotations tend to be very small in terms of content: a part-of-speech tag (Noun, Verb) rather than a full text comment; there might be more than one property attached to a particular annotation too.   Where annotations are attached to resources they are always associated with just part of the resource, not the resource as a whole &#8211; otherwise we call them meta-data.  While many annotations are associated with parts of a resource, some are not attached to the resource itself but to other annotations. For example, in a syntax tree, a higher level node like a Noun Phrase might be attached to subordinate words annotations rather than to the document itself.  Relations between annotations are important, these are used to record parent-child relations in a hierarchical description of sentence structures but also other relationships within a text, such as co-reference or dependencies within a sentence structure.

Perhaps the biggest difference though is that for a given resource, the number of Linguistic Annotations will tend to be very large, whereas in the Open Annotation use-cases, I imagine that only a small number of comments might be attached to a single resource.   For example, in a single document from the GCAusE corpus (part of the Australian National Corpus) which is manually annotated in the Conversation Analysis style, I count between 50 and 750 separate annotations.   In samples from the American National Corpus which has been annotated automatically, the count goes as high as 17,000 annotations per document.  This should be obvious when we realise that an annotation in this case is &#8216;this is a Noun&#8217; and that each word in the text will have a number of different annotations associated with it. I want to compare the structures that are used in OA with those that we have developed in the DADA system. I&#8217;ll touch later on an alternate RDF ontology for Linguistic Annotations, the [POWLA ontology](http://nachhalt.sfb632.uni-potsdam.de/powla/) developed by Christian Chiarcos. Here&#8217;s a picture of the structure of an OA annotation that references a part of a text resource &#8211; this is perhaps closest to what we&#8217;re doing with Linguistic Annotation.<figure style="width: 511px" class="wp-caption aligncenter">

[<img class="   " title="Open Annotation example that uses text positioning" src="http://www.openannotation.org/spec/core/images/textposition.png" alt="Example RDF graph for an Open Annotation example" width="511" height="383" />](http://www.openannotation.org/spec/core/specific.html)<figcaption class="wp-caption-text">An example OA annotation that references part of a text resource using character offsets to indicate the region of interest.</figcaption></figure> 

The example shows that the main entity in an OA annotation is the Annotation object itself which has two mandatory properties denoting the Body and Target of the annotation. The Body is the content of the annotation which in the simplest case is just the URI of a web resource or a fragment identifier for part of a web resource.   Other body types are discussed, perhaps most relevant to the Linguistic Annotation use case is the use of [_tags _or _semantic tags_](http://www.openannotation.org/spec/core/core.html#Tagging) which would be properties attached to the body1 node in this example. The Target denotes the thing being annotated; in the simplest case this can be a web resource (a URL) but in this case we want to annotate just a small part of a text document.   OA introduces the Specific Resource class to do the work here, this references both a source document (source1) and a means of finding the relevant text in that document (selector1).   The selector in this case is a Text Position Selector which points to a start and end position within the text via character offsets. The motivation for this design is that the same selector might be used more than once &#8211; for example to select equivalent regions from many different resources.  There might also be different kinds of selectors &#8211; examples are given for using document context to define a region rather than start/end locators, or just using HTML fragment identifiers where they are available. Combining a couple of examples from the OA specification (Figure 2.1.3.3 and Figure 3.2.2.1 (reproduced above)), here is the RDF fragment that would represent an annotation on a region of text that has an attached semantic tag:

<pre>&lt;anno1&gt; a oa:Annotation ;
    oa:hasBody &lt;body1&gt; ;
    oa:hasTarget &lt;sptarget1&gt; .

  &lt;sptarget1&gt; a oa:SpecificResource ;
    oa:hasSource &lt;source1&gt; ;
    oa:hasSelector &lt;selector1&gt; .

  &lt;selector1&gt; a oa:TextPositionSelector ;
    oa:start 4 ;
    oa:end 7 .</pre>

<pre>&lt;body1&gt; a oa:SemanticTag ;
    foaf:page &lt;document1&gt; .</pre>

Let&#8217;s compare this with a similar example expressed with the DADA ontology we&#8217;ve used for Linguistic Annotations.  This is shown in the example below.<figure id="attachment_391" aria-describedby="caption-attachment-391" style="width: 460px" class="wp-caption aligncenter">

[<img class=" wp-image-391 " title="dada-annotation" src="http://localhost:8080/wp-content/uploads/2013/03/dada-annotation.png" alt="" width="460" height="426" srcset="http://localhost:8080/wp-content/uploads/2013/03/dada-annotation.png 575w, http://localhost:8080/wp-content/uploads/2013/03/dada-annotation-300x278.png 300w" sizes="(max-width: 460px) 100vw, 460px" />](http://localhost:8080/wp-content/uploads/2013/03/dada-annotation.png)<figcaption id="caption-attachment-391" class="wp-caption-text">An example of an annotation in the DADA ontology</figcaption></figure> 

The central entity anno1 is a dada:Annotation and is essentially equivalent to the oa:Annotation in the first example.  The relationship to the thing being annotated is represented differently however.   In this case, each annotation is part of an Annotation Collection (agroup1) which dada:annotates some resource (source1).    The dada:targets relation relates the annotation to a Locator, in this case a dada:UTF8Region with start and end defined by UTF8 character offsets; note that we are slightly more precise than OA here in the kind of text document that we&#8217;re indexing into. The first observation is that the two models have a similar structure at a macro level.  Both have the central annotation entity that has properties defining a region in the source document and a set of properties containing the annotation itself.  The details of how these properties are defined is different but semantically, oa:Annotation and dada:Annotation define equivalent entities. The way that  the target of the annotation is defined is perhaps the biggest difference between the two models.   In the OA model, the Specific Resource node contains all the information needed to dereference the target of the annotation.  This encapsulation ensures that this annotation could be serialised and  sent to a user agent who could display the annotation to the user.   OA annotations stand alone in this sense: even if there are many annotations on a single document, each one contains all the information needed to de-reference itself.  In contrast, DADA annotations are expected to exist in groups and hence the pointer to the source document is only recorded on the Annotation Collection.  The fact that region1 references source1 is only implicitly represented in the graph.  In part, this is motivated by a desire for a more compact representation of annotations: linking the source document to each region would add another triple for each annotation.  This may not be a significant issue for OA which seems to be targeting applications where the number of annotations per document is relatively small.  Linguistic annotation collections can be very large. For example, documents in the MASC segment of the Americal National Corpus can have up to 17,000 annotations associated with them (these annotations are automatically generated tags for things like part-of-speech and named entity references).  The OA representation adds three triples per annotation to the DADA representation because of the association with the source document via the sptarget1 Specific Resource node.  This would result in around 48k more triples for the document mentioned, a number which becomes significant in a collection of hundreds of documents.

## Annotation Provenance

OA has some useful features to keep track of [provenance](http://www.openannotation.org/spec/core/core.html#Provenance).