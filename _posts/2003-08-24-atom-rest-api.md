---
id: 219
title: Atom REST API
date: 2003-08-24T14:00:00+00:00
author: Steve Cassidy
layout: post
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=219
permalink: /2003/08/atom-rest-api/
categories:
  - Uncategorized
---
Mark Pilgrim describes his implementation of a  [REST API for Atom](http://diveintomark.org/archives/2003/08/18/atom_api_implementation), the RSS successor being developed by various folk. This appeals to my URL designer sensibilities and includes a nice example of how to use XML document responses containing URIs as part of an interface.

An interesting proposal in the document is a method of using a variation on HTTP digest authentification as part of the API. His method guards against replay attacks and avoids sending cleartext passwords over the net. It's a neat idea which could be usefully copied for [CANTCL](http://purl.org/tcl/cantcl/)
