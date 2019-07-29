---
id: 207
title: Gnowsis and RDF Desktop Systems
date: 2004-11-04T13:00:00+00:00
author: Steve Cassidy
layout: single
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=207
categories:
  - Uncategorized
---
[Gnowsis](http://www.gnowsis.org/) is a _Semantic Web desktop System_ which means it aggregates various bits of personal data into an RDF store and provides a browser for the store. It is able to look at MP3-ID3 tags, email (in Outlook or Thunderbird), bookmarks in Firebird and at the file system. It will do full text indexing and provides a web server interface for browsing the store.

Which is weird because that's just how Giggle is evolving at the moment. I've begun to build in an RDF store which in the first instance will just contain the individual weblog posts and their metadata (date, title, etc). The web pages and RSS feeds will then be generated from this store rather than from the tangle of variables in the current implementation. The page generation will still be template based but now the templates include references to the rdf store rather than simple variable references. One outcome of this is less of a need to precompute lots of variables for the templates (eg. the time of the post in minutes), instead letting the templates compute things from the store if they want to. Currently my new templates are coded as procedures which generate text (eg. HTML) and I'm using [xmlgen](http://tclxml.sourceforge.net/xmlgen.html) to do the work internally.

Once I have an RDF store I can begin to put other bits of data in there. [CANTCL](http://purl.org/tcl/cantcl) already works on an RDF store populated from the package metadata. I could scan Tcl packages that I've written to generate something like my [Tcl page](/~cassidy/tcl/). Drop some [RDF annotated](http://norman.walsh.name/2004/06/07/flowers) jpeg images into a directory and I could have a photoblog; scan my MP3 library, read my [bookmarks](/~cassidy/bookmarks/), grok my [calendar](http://www.w3.org/2001/sw/Europe/reports/dev_workshop_report_2/). The key to making this easy to use, which derives from the original [blosxom](http://www.raelity.org/apps/blosxom/) idea, is to leave the data in it's original format (or invent a simple text file format) and leverage the file system to build structure into the weblog.
