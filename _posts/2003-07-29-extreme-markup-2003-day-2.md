---
id: 222
title: 'Extreme Markup 2003: Day 2'
date: 2003-07-29T14:00:00+00:00
author: Steve Cassidy
layout: single
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=222
categories:
  - Uncategorized
---
### Thomas Passin

[Bookmark Management with Topic Maps](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Passin01/EML2003Passin01-toc.html) given large collections of browser bookmarks from different browsers, how do you manage them? Applies topic maps to the problem. Conventional bookmark managers are either flat (use search but how do you remember the keywords), of folder based (still hard to find related material).

Goals: navigation, collocation. Uses subject category names which are like folder paths claiming that this gives context for the end term of the path (eg. Software/Languages/Java vs Database/Interfaces/Java) while avoiding the need to organise things into a real hierarchy. Sourceforge project: [TM4Jscript](http://tm4jscript.sf.net).

### William Kent

Interesting keynote talk about identity...

### Matthew Fuchs

[XML Schema](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Fuchs01/EML2003Fuchs01-toc.html) The problem of validating schema which use RE like repitition operators, eg. (a,b){1,3} which are used in XML Schema. Obvious algorithm is to unroll the alternatives before processing the schema but this is clearly exponential. Is there a better way? Yes, but I don't understand enough about the problem enough to appreciate what's going on.

### Howard Katz

[XQuery from the bottom up](http://www.mulberrytech.com/Extreme/Proceedings/html/2003/Katz01/EML2003Katz01-toc.html) XQEngine (java OSS engine, available from fatdog.com) currently XPath only, preindexes documents to make query more efficient. Limited support at the moment (abbrev axes, element, attr and text nodes only). 

XPath eval starts at the end of the path, eg <tt>/book/title</tt> would find all the titles first &#8212; this is the maximal list of nodes that the result could contain, then walk back up the path expr discarding nodes.

Seems to have made some premature optimisations, eg. the node representations contain forward sibling pointers and parent pointers because he's only yet doing child/descendant processing.

Presents a demo on Shakespeare plays, indexes all plays on a fast machine in 2.5 seconds. It would be cool to evaluate XXPath against this &#8212; how bad are we?
