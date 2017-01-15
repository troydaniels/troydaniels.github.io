---
layout: post
title:  "DIKU - Walking through the interpreter"
date:   2017-01-12 20:12:12 +1100
categories: DIKU Compiler Course
---

After several hours coding, I am happy to say that the language now has an interpreter!  
You can view the implementation [here].

[here]: https://github.com/troydaniels/DIKU-Compiler-Course/blob/master/src/Interpreter.php

In this post, I'll run through the new classes and logic that makes this possible.

As discussed in chapter 4 of [the text], a key requirement for this is a symbol table, to hold the value of each variable, within each lexical scope.

[the text]: http://www.diku.dk/~torbenm/Basics/basics_lulu2.pdf

The symbol table and its methods are absracted away into the ```SymbolTable]``` class.  
As the language only has a single scope, the symbol table is implemented as a simple string indexed array. To allow for multiple scopes, this implementation would have to be changed. 

This class has two methods, ```SymbolTable::bind()``` and ```SymbolTable::lookup()```, which allow for a variable name to be bound to some data (ie, ```n = 1```), and for the data associated with a variable to be retrieved.  
If a ```lookup``` is performed on a variable that has not been ```bound```, a warning message is printed - however execution will continue.

All filehandling logic has been moved into ```Interpreter.php```.
The ```Interpreter::run()``` method itterates through the AST array returned by ```Parser::run()```, and traverses the tree recursively in postorder:

```
...
// Recurse to bottom, and traverse child nodes left to right
  if( $left instanceof Node ) {
    self::postorderTraversal( $left );
  } else if ( $right instanceof Node ) {
            self::postorderTraversal( $right );
  }

  // Now, call relevant method for this node
  $this->previousCalc = $this->{"interpret_" . $symbol}( $node );
...
```

With the last line of the above snippet calling ```Interpreter::interpret_{$node['symbol']}```.

To take an explicit example, the operation ```n < 2``` would cause the following method to be called:

```
function interpret_LESS_THAN( $node ) {
  $left = $this->symbolTable->lookup($node->left);
  $right = $this->symbolTable->lookup($node->right);
  return ($left < $right);       
}
```

```Interpreter::interpret_WHILE()``` is the only real non-trivial method in this class, recursing on ```Interpreter::postorderTraversal()```, allowing for nested ```while``` loops, and for the ```while``` logical block to span several new lines.  
A variable named```nextInstruction``` was added to the ```Node``` class to accomplish this, as well as a check in ```Interpreter::postorderTraversal()```, actioning the execution any additional ```while``` block instruction.

In the same way, the ```ASTarray[]``` within the ```Parser``` class could be modified to a single AST representation, but for now I'm quite happy with the implementation.

For me, a key takeaway is that the logic in the ```Interpreter``` class is in some ways very similar to the ```Parser``` class, in the same way that the ```Parser``` class is to the ```Lexer``` class - with each dealing with higher and higher levels of abstraction.


Onward!