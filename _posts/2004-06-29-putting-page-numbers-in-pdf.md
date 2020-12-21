---
id: 211
title: Putting Page Numbers in PDF
date: 2004-06-29T14:00:00+00:00
author: Steve Cassidy
layout: single
guid: https://stevecassidy.net/wordpress/?p=211
categories:
  - Uncategorized
---
One of the annoying things that needs doing when [organising a conference](http://www.assta.org/sst/2004/) is to produce the proceedings. These days that means generating a CDROM filled with PDF files and the main problem with that is adding proper identifiers to the PDF. In the old days of print, every paper had page numbers and consequently we expect to be able to cite page numbers in a conference proceedings. In Australia, the government expects us to submit page numbers for each conference paper as part of the annual audit of research activity. Hence the need to add page numbers to soft-copy PDF versions of papers.

Of course one can probably do this manually with various Adobe products but that doesn't help lower stress levels close to the conference when you have to insert a new paper and recalculate the page numbers. Hence the search for tools to do this automatically. This note is really just a placeholder for things I find on the road to building the SST2004 proceedings. Perhaps it will be useful to others too.

The general problem is known as PDF merge or PDF stamping, [Google](http://www.google.com/search?q=pdf+merge+stamp) will tell you who the big players are.

[acl02stamp](http://www.cs.jhu.edu/~jason/software/) promises to _&#8220;Stamp bibliographic citations (including automatically-determined page numbers) onto a sequence of academic papers in PDF format.&#8221;_. I've asked the author for a copy.

The [iText Java PDF library](http://www.lowagie.com/iText/) provides an API in Java for generating PDF and can do stamping by virtue of being able to read existing PDF files and add them to it's output.

**Update** Thanks to Jason Eisner, author of acl02stamp, I now know about the Perl [Text::PDF](http://search.cpan.org/src/MHOSKEN/Text-PDF-0.25/) module which provides an PDF generation API for Perl and includes a simple script pdfstamp.plx to achieve exactly what is needed. Jason's acl02 script wraps this to integrate it with the ACL conference tools for generating tables of contents etc. For reference, the Debian package [libtext-pdf-perl](http://packages.debian.org/unstable/perl/libtext-pdf-perl) is all that's needed.
