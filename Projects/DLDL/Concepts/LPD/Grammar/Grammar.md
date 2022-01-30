# DLDL LPD: Grammar

## Overview

After the Lexicon you have the Grammar. The Grammar is used to define what structural patterns are in your language. Using these patterns a CST or AST can be created.

## NonTerminals and ProductionRules

Grammars are often defined using a BNF like language. This means that you have nonterminals, productionrules and terminals. DLDL also has such a language, it is an extended variant of EBNF.

```DLDL
program
	stmts

stmts
	stmt stmts
	EMPTY

stmt
	INT VARNAME EQ NUMBER SEMICOLON
	
```

The above grammar specifies a grammar used to match with a series of integer variable declarations:

```C++
int abc = 10;
int abcdef = 12345;
```



One can specify different structures by using nonterminals and productionrules. Take the nonterminal ```stmt``` this NT specifies a production rule which matches with the pattern: ```INT VARNAME EQ NUMBER SEMICOLON```. When comparing the PR with the code you can see that each part of the PR is matched:

```C++
C++:  int abcdef  =  12345  ;
DLDL: INT VARNAME EQ NUMBER SEMICOLON
```



You can also refer to nonterminals in productionrules, take a look at ```stmts``` this NT makes sure we can match multiple ```stmt``` NTs:

```DLDL
stmts
	stmt stmts
	EMPTY
```

To understand the recursion, understand that the second PR matches with nothing. I.e. if there are no ```stmt```, the NT can still be matched with ```EMPTY```.

We thus know that ```stmts``` can be empty. The first PR becomes more clear, we want to match with a ```stmt``` followed by a ```stmts``` which is defined by a ```stmt``` followed by some ```stmts```, or nothing.

```
stmts
	stmt stmts
	      |
	      |- stmt stmts
	      |			|
	      |			|- stmt stmts
	      |			|
	      |			| EMPTY
	      |- EMPTY
	EMPTY
```

We can substitute ```stmts``` for any of its production rules. As it might be EMPTY it can thus map with 0 and 1 ```stmt```. However we can also insert the first PR this adds a new ```stmt``` opportunity allowing us to match with 2 ```stmt```. If there are only 2 ```stmt``` we can put the last ```stmts``` to ```EMPTY```. We can do this endlessly thus this construction matches with 0 or more ```stmt```.

## DLDL EBNF

As some constructions in BNF can be annoying to write such as the ```stmts``` example which matched with 0 or more ```stmt```. DLDL implements its own version of EBNF with some extensions making it much easier to write grammars.

### 0 or more (*)

Take the following grammar:

```DLDL
program
	stmts

stmts
	stmt stmts
	EMPTY
```

The goal is to matched with 0 or more ```stmt```. This can also be done using the * operator:

```DLDL
program
	stmt*
```

The * operator will make sure the NT is encountered 0 or more times.

### 1 or more (1)

Take the following grammar:

```DLDL
program
	stmts

stmts
	stmt stmts
	stmt
```

The goal is to matched with 1 or more ```stmt```. This can also be done using the + operator:

```DLDL
program
	stmt+
```

The * operator will make sure the NT is encountered 1 or more times.

### Grouping (())

Take the following grammar:

```DLDL
program
	A_B

A_B
	A B A_B
	EMPTY
As
	A As
	EMPTY

Bs
	B Bs
	B
```

The goal is to matched with the following sequence 0 or more times : 0 or more ```A``` followed by 1 or more ```B```.

This can be rewritten as follows:

```DLDL
program
	(A* B+)*
```

The grammar encapsulated the sequence: 0 or more ```A``` followed by 1 or more ```B```. And applied the * operator to make sure this sequence occurs 0 or more times.

### Optional ([], ()?)

Take the following grammar:

```DLDL
program
	optional_A

optional_A
	A
	EMPTY
```

The goal is to matched with ```A``` if it is possible otherwise nothing is matched.

This can be rewritten in 2 ways:

```DLDL
program
	[A]
```

Or we group the sequence and specify the optional operator:

```DLDL
program
	(A)?
```

It is also possible to the square brackets around a group:

```DLDL
program
	[(A)]
```

Take the one you like the most.

### A OR B (|)

Take the following grammar:

```DLDL
program
	A_OR_B

A_OR_B
	A
	B
```

The goal is to matched with either ```A``` or ```B```.

```DLDL
program
	(A | B)
```

You can use the ```|``` operator to specify a choice between left or right. This operator can be used multiple times in the same group:

```DLDL
program
	(A | B C | D | E F G | A)
```

The spots in between the ```|``` and in front and at the end or the choices. You can have the same option occur multiple times.

### Group extension (() -> ())

Suppose you want to match with function arguments. Using the current extensions this would look as follows:

```DLDL
program
	function_args

function_args
	[argument] (COMMA argument)*
```

An optional argument followed by 0 or more (COMMA argument). DLDL supports a cleaner way of specifying this relation using the arrow operator:

```DLDL
program
	function_args

function_args
	[argument] -> (COMMA argument)
```

The Arrow operator can be nested:

```DLDL
program
	[A] -> (B -> C)
```

This is the same as:

```DLDL
program
	A_B_C

A_B_C
	[A] (B C*)*
```

One might argue that the arrow operator is easily replaced. However, using the alternative makes your grammar generally harder to read. When using the arrow operator, you know whether a sequence is used to extend some part when reading left to right. When using the * operator, one can only know that something is used as extension when reading the full PR.

## Inserting actions

This LPD does not allow the user to insert actions. To do this a different LPD is required which is currently not implemented by DLDL and Deamer CC.

## Naming conventions

Each nonterminal should be written lower case.

Each terminal should be written upper case.