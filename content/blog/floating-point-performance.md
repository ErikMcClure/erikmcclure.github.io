+++
blogimport = true
categories = ["blog"]
date = "2010-01-30T01:53:00Z"
title = "Floating Point Performance"
updated = "2011-01-22T04:18:21.000+00:00"

[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Whenever I do intensive performance testing on delicate math operations, the results almost always surprise me. Today I have learned that if a program preforms a divide-by-zero on a floating point operation (which does not blow up the program), the resulting performance hit is almost equivalent to taking a square root.
