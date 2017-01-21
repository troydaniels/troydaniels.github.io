---
layout: post
title:  "DIKU - Targeting the JVM"
date:   2017-01-22 00:30:16 +1100
categories: DIKU Compiler Course
comments: true
---

After hours (and oh so much coffee) working my way through hard to pinpoint errors, thrown as a result of incorrectly formatted Java class files, the language now compiles down to Java bytecode.  
You can view the source [here].

[here]: https://github.com/troydaniels/DIKU-Compiler-Course/blob/master/src/Bytecode.php

![Targetting the JVM](https://raw.githubusercontent.com/troydaniels/troydaniels.github.io/master/_img/targeting-the-jvm.png)

This last step was by far the most frustrating, but also the most rewarding. I'm really glad it's finished, and working.  

There are a lot of magic numbers in the the new ```Bytecode``` class, but I feel in this instance, this makes the code slightly more readable. I've heavily commented throughout, with the structure of these comments being as close as practicable to those in bytecode generated in the [last post].

[last post]: https://troydaniels.github.io/diku/compiler/course/2017/01/18/building-a-compiler-p7.html

And as many of the basic class features are more or less unchanged for our purposes, there aren't too many non-trival features in this class, the most dificult task being having to keep track of 'index' locations in Java's *constant pool* data structure as new strings are added.  
All of the 'heavy lifting' is handled by the ```Compiler``` class, with the bytcode simply printing out a series of strings.

Internally, the contents of the output file is stored as a string - with php's ```pack()``` function used to format this string to hexidecimal, before being saved to a file, ```a.class```.

The class uses two helper functions, ```strToHex()``` and ```paddedHex()``` - the latter converting a decimal integer to hexidecimal, padded to the left by 0s, such that the returned string's length is a multiple of another integer passes as an argument.

---
<br>
This project could well be extended beyond its current features to create a richer, more capable language - and if you feel so inclined I encourage you to fork the [repository], and see what you can come up with.

[repository]:https://github.com/troydaniels/DIKU-Compiler-Course

For me though, it's time to move on to the next project!  