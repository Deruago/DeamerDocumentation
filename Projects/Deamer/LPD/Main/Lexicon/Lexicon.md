| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Lexicon LPD

The lexicon LPD is a simple lexicon definitions, supporting most of the use cases. It can recognize various terminals using regexes, and delete/skip certain sequences and more. If you have used lexer or parser generators before these concepts should be familiar with you.

## LDO's

| LDO      | Purpose/Description               |
| -------- | --------------------------------- |
| Terminal | Used to define a terminal symbol. |

## Abstractions

The Terminal LDO, allows various abstractions. Abstractions are concepts that modify the generation of other generators.

| Abstraction | Purpose/Description                                          |
| ----------- | ------------------------------------------------------------ |
| Standard    | Used to define a standard terminal.                          |
| Delete      | Used to delete or skip the terminal. E.g. if you don't want to match spaces, you can delete them. |
| Ignore      | The parser will use this terminal only to derive the production rule, it won't be visible in the AST. |
| NoValue     | The parser will not use the value retrieved from this terminal, but this terminal is visible in the AST. |
| Crash       | When encountered the lexer will crash. Some languages don't want the user to use e.g. tabs, they can prefer to stop compilation when encountering a tab. |

## Use cases

Generation of lexers, and source for extensions requiring knowledge about the lexicon.

## Notes

Extensions don't need to adhere to the abstractions given, but by default they often do.

## Examples

## DLDL

```DLDL
/ An VARNAME Terminal
Terminal: VARNAME [a-zA-Z]+[a-zA-Z0-9]*

Delete: ESCAPE_CHARS [\n\r\t ]
```

## C++ Front-End

```cpp
#ifndef LANGUAGE123_LEXICON_H
#define LANGUAGE123_LEXICON_H
#include "Deamer/Language/Generator/Definition/Property/User/Main/Lexicon.h"

class LANGUAGE123;

class Lexicon : public ::deamer::language::generator::definition::property::user::Lexicon<LANGUAGE123>
{
public:
	// Terminal declarations
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Terminal> VARNAME;
    
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Terminal> ESCAPE_CHARS;
public:
    Lexicon(LANGUAGE123* language)
        :	::deamer::language::generator::definition::property::user::Lexicon<LANGUAGE123>(language)
    {
    }
    
    void GenerateObjects() override
		{
			// Initialize Terminal LDO's
			VARNAME.Set(
                ::deamer::language::type::definition::object::main::Terminal("VARNAME", "[a-zA-Z]+[a-zA-Z0-9]*", ::deamer::language::type::definition::object::main::SpecialType::Standard));
        
			ESCAPE_CHARS.Set(
                ::deamer::language::type::definition::object::main::Terminal("ESCAPE_CHARS", "[\n\t\r ]", ::deamer::language::type::definition::object::main::SpecialType::Delete));

			// Add object calls
			AddObject(VARNAME);
			AddObject(ESCAPE_CHARS);
			
		}
};

#endif // LANGUAGE123_LEXICON_H
```
