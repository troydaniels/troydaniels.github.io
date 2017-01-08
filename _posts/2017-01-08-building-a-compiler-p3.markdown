---
layout: post
title:  "DIKU - Walking through the lexer"
date:   2017-01-08 19:01:33 +1100
categories: DIKU Compiler Course
---

The lexer is finished, and it was surprisingly simple to implement.  
You can view the [implementation] and a simple [working example].  

[implementation]: https://github.com/troydaniels/DIKU-Compiler-Course/blob/master/src/Lexer.php
[working example]: https://github.com/troydaniels/DIKU-Compiler-Course/blob/master/src/implementation.php

As per the course notes, once we'd defined the language features (see the [last post]), we can look to match these features with some regex.  
Which means the key part of the ```Lexer``` class is the ```$tokenRegex``` array. 

To take an explicit example, the array entry ```"/^(\*)/" => "MULTIPLY"``` indexes the string ```"MULTIPLY"``` by a string, which when interpereted as regex, captures a literal '*' character appearing at the front of a string of text.

Anchoring all regex to the front of the string (ie, '^') allows us to move through it, and match substrings from left to right, to generate our list of tokens.

To accomplish this, at this stage I'm just passing ```run``` an open file-handle, and the method then steps line-by-line through the file, and checks the start of each string against each regex string in ```$tokenRegex```.  
If ```preg_match()``` returns true, we take ```$matches[1]``` as our token, as this is the first propper substring, and move forward ```strlen($matches[1])``` characters in our string.

**There are a couple of shortcomings with this implementation:**  
1. I'm relying on the ```$tokenRegex``` array to be in a rather specific order - that is, the captured string from an entry should not be a propper substring of a later entry in the array, for example the order of `==` and `=`.  
I _should_ instead really test for the largest captured substring in the ```run``` method.  
2. There is no lookahead, which is fine for this really simple language definition (ie, we don't allow for user defined functions), however if it were extended in any meaninful way, lookahead would likely be necessary.  

But there we go, a lexer that works for our simple language.  
Next, I'm on to Chapter 3 - Syntax Analysis.

[last post]: https://troydaniels.github.io/diku/compiler/course/2017/01/07/building-a-compiler-p2.html


