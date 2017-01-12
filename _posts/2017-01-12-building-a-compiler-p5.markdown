---
layout: post
title:  "DIKU - Walking through the parser"
date:   2017-01-12 10:30:41 +1100
categories: DIKU Compiler Course
---

I've just finished writing a recursive descent parser, and I'm really quite happy with it.  
You can view the implementation [here].

[here]: https://github.com/troydaniels/DIKU-Compiler-Course/blob/master/src/Parser.php

The parser's task is to take sequential tokens from the lexer, and determine if they fit the language grammar. If they do, it builds a data structure known as an *[abstract syntax tree]* (AST), which encodes the tokens, their relationships, and their precedence.  

[abstract syntax tree]: https://en.wikipedia.org/wiki/Abstract_syntax_tree

The parser's ```run()``` method iterates through the lines of an open filehandle, passed as an argument.  
Reflecting on the grammar, we can see that there are only three valid symbols for the first token in any line - ```PRINT```, ```WHILE``` or ```VARIABLE``` - which we can test 
for:

{% highlight php startinline=true %}
switch ( $this->currentToken['symbol'] ) {
  case "VARIABLE":
    $this->AST[] = @self::assignment();
    break;
  case "WHILE":
	$this->AST[] = @self::statement();
	break;
  case "PRINT":
	$this->AST[] = @self::printFunction();
  default:
	break;
{% endhighlight %}


The function for each ```case``` can then make assumptions about the tokens to follow.  

Each logical block in the program file is returned from these functions as an AST. 
You'll note I have implemented the ```Node``` class as abstract, and then extended the class several times to create, for example, a ```BinaryOperation``` class.  
This stops a ```Node``` object from being instantiated and removes a lot of duplicate code from ```Node.php```, whilst making the parser functions and resulting tree structure a little easier to follow.

Taking the ```assignment()``` function as an example:

{% highlight php startinline %}
// <assignment> => VARIABLE ASSIGN [ <exp_1> | QUOTED_STRING ] SEMICOLON
function assignment() {
    $variable = new Element( $this->currentToken['symbol'], $this->currentToken['value'], null, null );
    self::consume("VARIABLE");
    $assign = new BinaryOperation( $this->currentToken['symbol'], null, $variable, null );
    self::consume("ASSIGN");
    if( $this->currentToken['symbol'] === "QUOTED_STRING" ) {
        $string = new Element( $this->currentToken['symbol'], $this->currentToken['value'], null, null );
        $assign->right = $string;
        $this->consume("QUOTED_STRING");
    } else {
        $assign->right = self::exp_1();
    }
    self::consume("SEMICOLON");
    return $assign;
}
{% endhighlight %}
  
Following the ```assignment``` production, the above function attempts to build a valid AST.  
It uses a helper function, ```consume()```, to check the symbol of the current token, throw a (hopefully) helpful error if it does not match the assumed string, or otherwise update the current token with the next token from the lexer.

In this example, if the ```VARIABLE``` is not ```ASSIGN```-ed to a ```QUOTED_STRING```, it calls the function ```exp_1```. In a similar way, some of the grammar productions allow recursion, hence the name of this style of parser.

Adding these functions became easier as I became familiar with how they fit in with the grammar, with the exception of the ```statement()``` function, which can span several lines of input - thus leading to an edge case where the lexer would return a ```null``` if it encountered the end of a line.

All said and done, I'm quite happy. On to the next chapter!
