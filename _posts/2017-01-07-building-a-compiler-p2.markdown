---
layout: post
title:  "DIKU - Defining the language"
date:   2017-01-07 20:35:12 +1100
categories: DIKU Compiler Course
---

I've read through the second chapter on lexical analysis a couple of times now.  
Somehow I don't think (non-)deterministic finite automita featured heavily in my last compiler course (else perhaps my memory isn't quite what I'd thought).

But, not to worry. It all makes sense - we're going to tokenize and classify strings from whatever language we're compiling - with [N|D]FA giving a framework with which to talk about this process.  
And of course I've realised, I need to now make some really important decisions - the most important of which:

#### What am I trying to achieve?

I'm never going to be able to implement a production level compiler, especially not in the timeframe I have given myself - and impracticality aside, I think that would miss the point anyway.  
I'm looking to learn, not reinvent the wheel. So reimplementing gcc is out.

So what then? 

Well, I'd be happy with something that is [Turing complete]*, whilst being a little more readable than [Brainfuck].

[Turing complete]: https://en.wikipedia.org/wiki/Turing_completeness
[Brainfuck]: https://en.wikipedia.org/wiki/Brainfuck


So, in the interests of readability (and sanity), I have in mind a simple impertive language, with only a small subset of features:  
1. A ```while``` loop  
2. Varable names comprised of only ASCII letters  
3. Variables which can hold an ASCII string, float or integer  
4. A ```print``` function  
5. The usual mathematical operators ```*```, ```/```, ```+```, ```-``` and ```=```  
6. The usual delimeters ```{```, ```}```, ```(```, ```)``` and ```;```  
7. The usual comparison operators ```<```, ```>```, ```==```  

For example, the following simple program would be valid:  

    n = 0;  
    while( n < 10 ) {  
       print "This is itteration number ";  
       print n;
       print "\n";
       n = n + 1;
    }   

and result in the following output:

    This is itteration number 1
    This is itteration number 2
    This is itteration number 3
    This is itteration number 4
    This is itteration number 5
    This is itteration number 6
    This is itteration number 7
    This is itteration number 8
    This is itteration number 9
    This is itteration number 10

But that should be Turing complete.
<br>
And since my C is a little rusty, and I'd prefer to leverage a scripting language, I'm going to write the whole compiler in PHP.  
<br>

---

<br>   
_*The two caveats being that of course computers don't have infinite memory, and that I'd like to see my language in action, so I'll have a print function._


