---
id: 231
title: Tim Bray on Good Web Citizenship
date: 2003-05-01T14:00:00+00:00
author: Steve Cassidy
layout: single
guid: https://stevecassidy.net/wordpress/?p=231
categories:
  - Uncategorized
---
[Tim Bray](http://www.tbray.org/ongoing/When/200x/2003/04/30/AppleWA) talks eloquently about what Apple could do to make their IMS service a good web citizen. Including:

  * Don't invent new URI schemes (Apple uses <tt>itms:</tt>) since the plumbing of the web doesn't understand them
  * Don't use text/xml as the mime type for XML. This is something I wasn't aware of. It seems that text/* is a license for a proxy to transcode the content, say from UTF-8 to US-ASCII. So the answer is to use application/xml instead avoiding the whole mess.

**Postscript** Apple also invented a new URI scheme (webcal:) for it's web calendar service, eg you can get the Australian holidays in ical format at <tt>webcal://ical.mac.com/ical/Australian32Holidays.ics</tt> &#8212; here again, plain http also works. So Apple is using URI schemes as mime-type indicators...
