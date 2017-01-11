---
layout: post
title:  "DIKU - Defining a grammar"
date:   2017-01-10 20:57:12 +1100
categories: DIKU Compiler Course
---

I've altered the ```Lexer``` class as I've realised it should have a ```next()``` method to return the *next* token from a string, rather than parse it in one go.  
In addition, I've removed the complexity from the ```NUMBER``` token regex, instead settling for an ```INTEGER```.

On reading through chapter 3 of the [course text], defining a grammar for the parser to use is the next step.

[course text]: http://www.diku.dk/~torbenm/Basics/basics_lulu2.pdf

For the simple language defined in [this post], I have constructed the following grammar (non-terminals in angled brackets)*:

[this post]: https://troydaniels.github.io/diku/compiler/course/2017/01/07/building-a-compiler-p2.html

Assignment:

    <assignment> =>
		VARIABLE ASSIGN [ <exp_1> | QUOTED_STRING ] SEMICOLON
    <exp_1> => 
		<exp_2> ( [ LESS_THAN | GREATER_THAN | EQUALITY ] <exp_2> )*
    <exp_2> =>
		<exp_3> ( [ PLUS | MINUS ] <exp_3> )*
    <exp_3> => 
		<exp_4> ( [ MULTIPLY | DIVIDE ] <exp_4> )*
    <exp_4> => 
		VARIABLE | ( PLUS | MINUS )? INTEGER | L_BRA <exp_1> R_BRA

Statement:

    <statement> =>
		WHILE L_BRA ( <exp_1> ) R_BRA L_PAR ( <block> )* R_PAR
    <block> =>
		( <print> )* | ( <assignment> ) *

Print:

    <print> =>
	PRINT [ VARIABLE | INTEGER | QUOTED_STRING ] SEMICOLON


I think this gives an unambiguous grammar (I'll spend some time thinking about that though), and my understanding is that the higher in the grammar a production appears, the lower its precedence, which means operators in this language should have the precedence they do in ordinary maths.

Next, to use this grammar to build the parser, wherein each non-terminal will have it's own method.


---  
<br>
**For readability's sake, I have left out the ````WHITESPACE```` token symbol*
