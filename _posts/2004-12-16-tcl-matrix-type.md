---
id: 205
title: Tcl Matrix Type
date: 2004-12-16T13:00:00+00:00
author: Steve Cassidy
layout: post
guid: http://www.ics.mq.edu.au/~cassidy/wordpress/?p=205
permalink: /2004/12/tcl-matrix-type/
categories:
  - Uncategorized
---
I've just implemented a matrix object type for Tcl, the sources are available here ([matrix0.1-src.zip](/~cassidy/tcl/matrix0.1-src.zip)). The package implements a new object type for Tcl which looks like a nested list of doubles but is stored internally as a 2d matrix. Conversion to and from string values is only done as needed so that matrices can be passed by value efficiently in tcl scripts. Here's what you can do in Tcl with this package:

<pre>package require cmatrix 0.1
# make a big square matrix as a list of lists
set n 1000
for {set i 0} {$i &lt; $n} {incr i} {
    set row {}
    for {set j 0} {$j &lt; $n} {incr j} {
        lappend row [expr $j*$i]
    }
    lappend matrix $row
}

# add the matrix to itself, triggers conversion to matrix only once
puts sum:[time {set sum [cmatrix add $matrix $matrix]}]
# second time, no conversion needed
puts sum2:[time {set sum [cmatrix add $matrix $matrix]}]
# transpose this matrix
puts trans:[time {set trans [cmatrix transpose $sum]}]
# now get the first row of trans, triggers conversion from matrix
# back to list
puts lindex:[time {set row [lindex $trans 0]}]
 </pre>

Output from the above on my system is:

<pre>sum:1184447 microseconds per iteration
sum2:109284 microseconds per iteration
trans:191660 microseconds per iteration
lindex:3736227 microseconds per iteration
 </pre>

Of course there's probably more to do here. Currently I don't expose many matrix operations to the tcl level since originally this was meant as a C callable library. More can be added later. Comments are welcome.
