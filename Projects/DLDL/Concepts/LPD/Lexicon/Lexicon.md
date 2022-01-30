# DLDL LPD: Lexicon

## Overview

Most languages have some set of words they understand. This set of words range from keywords to variable sequences.

## Terminals

Terminals are used to define specific words in your language. Look at the following Lexicon:

```DLDL
Terminal: VARNAME  [a-zA-Z_]+
Terminal: NUMBER   [0-9]+
Terminal: DECIMALS [0-9]+[\.][0-9]+
```

You see that the language has 3 words. Namely variable names, numbers and decimals.

Each time you encounter a specific sequence. A lexer can recognize the sequence and create the correct terminal token. Thus if we encounter a number, it is recognized as a number.

When declaring a terminal DLDL follows the following structure (DLDL EBNF):

```DLDL
terminal
	[abstraction] VARNAME REGEX+
```

One starts with an abstraction followed by the terminal name. Which in turn is followed by a set of regexes.

This means that you optionally can use abstractions. It also means that you can concatenate multiple regexes as follows:

```DLDL
Terminal: TEST Hello World[!]
```

The above terminal TEST will only match with the sequence: ```Hello World!```. You can also remove the ```Terminal:``` abstraction, when DLDL does not understand the abstraction or has no abstraction it will default to a standard terminal.

## Abstractions

Abstractions are used to further specialize the Lexicon. Specializations can modify how the lexicon is integrated with other LPDs.

In DLDL we have the following abstractions:

| Abstraction           | Example                                         | Since Version | Description                                                  |
| --------------------- | ----------------------------------------------- | ------------- | ------------------------------------------------------------ |
| [Nothing]             | ```VARNAME [a-zA-Z]+```                         | Deamer 2.0.0  | Defaults to ```Terminal``` abstraction.                      |
| [Unknown abstraction] | ```NotExistingAbstraction: VARNAME [a-zA-Z]+``` | Deamer 2.0.0  | Defaults to ```Terminal``` abstraction.                      |
| Terminal              | ```Terminal: VARNAME [a-zA-Z]+```               | Deamer 2.0.0  | Used to specify standard Terminals.                          |
| Ignore                | ```Ignore: VARNAME [a-zA-Z]+```                 | Deamer 2.0.0  | Used to ignore the Terminal when constructing an AST.        |
| NoValue               | ```NoValue: VARNAME [a-zA-Z]+```                | Deamer 2.0.0  | Used to create a Terminal with no value. Useful for when the value of some keyword does not affect generation. |
| Delete                | ```Delete: VARNAME [a-zA-Z]+```                 | Deamer 2.0.0  | Used to delete the sequence. When matched the Lexer will not output a Terminal. It skips it. |
| Part                  | ```Part: VARNAME [a-zA-Z]+```                   | Deamer 2.3.0  | Used to specify a part of some other terminal. These terminals are not used in the lexicon. However, they can be used to construct terminals. |

## Precedence

The Lexicon precedence is often local to the generator used. However, most generators use the same rules.

The main thing you need to take in when creating Lexicons is whether two terminals are a subset of each other:

```DLDL
Terminal: VARNAME    [a-zA-Z_]+[a-zA-Z_0-9]*
Terminal: UNDERSCORE [_]
```

In this case the Value of the Terminal ```UNDERSCORE``` is a subset of the Value of the Terminal ```VARNAME```. Due to this, whenever the lexer matches an underscore it will output a VARNAME instead. The higher the terminal the more precedence it has. Some lexer generators set precedence to how specialized the regex is, but not all.

## Combined Terminals

Since Deamer v2.3.0 DLDL supports combining of Terminals.

Combining of terminals allows you to further specialize terminals and reuse terminal definitions.

```DLDL
Part: LETTER [a-zA-Z]
Part: DIGIT [0-9]

Terminal: VARNAME
	LETTER+ (LETTER | DIGIT)*
	[a-zA-Z]+[a-zA-Z0-9]*
```

The above Lexicon shows how we used the LETTER and DIGIT terminals in order to construct the VARNAME Terminal.

Combining of terminals has several benefits. Suppose we had the following code:

```C++
void get_integer();
void get_string();

void set_integer();
void set_string();
```

Creating a regex we can further specialize the VARNAME for functions:

```DLDL
NoValue: VOID void

Part: GET get
Part: SET set
Terminal: GET_OR_SET_VARNAME
	GET [_] VARNAME
	SET [_] VARNAME
Terminal: VARNAME [a-zA-Z_]+[a-zA-Z_0-9]*
```

The Lexicon specializes a get or set VARNAME by appending either GET or SET in front of it. As we used the terminal specialization mechanic, Deamer AST can generate allowing the user to easily access parts of a combined terminal.

This means that you can check if some function is a getter or setter by simple checking if the GET part is matched. Compared to the standard Terminal, in the standard variant you need to manually get the right subset and compare to "get".