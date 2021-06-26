| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Grammar LPD

The grammar LPD defines context free grammars for languages. It supports nonterminals and production rules. It also adds some abstractions to the nonterminal.

## LDO's

| LDO            | Purpose/Description                  |
| -------------- | ------------------------------------ |
| NonTerminal    | Used to define a nonterminal symbol. |
| ProductionRule | Used to define a production rule.    |

## Abstractions

The Nonterminal LDO, allows various abstractions. Abstractions are concepts that modify the generation of other generators.

| Abstraction | Purpose/Description                                          |
| ----------- | ------------------------------------------------------------ |
| Standard    | Used to define a standard nonterminal.                       |
| Group       | Used to define if a nonterminal is a base class of direct underlying terminals/nonterminals. E.g. if a set of terminals are used in the same context (e.g. operators), then you can group them under a nonterminal "operator". This also works for nonterminals. |

## Use cases

Generation of parsers, and extensions requiring grammar information about the language.

## Notes

Extensions don't need to adhere to the abstractions given, but by default they often do.

## Examples

## DLDL

```DLDL
/ The grammar

program
	stmts

stmts
	stmt stmts
	EMPTY

stmt
	VARNAME
```

## C++ Front-End

### Header

```cpp
#ifndef LANGUAGE123_GRAMMAR_H
#define LANGUAGE123_GRAMMAR_H
#include "Deamer/Language/Generator/Definition/Property/User/Main/Grammar.h"

class LANGUAGE123;

class Grammar : public ::deamer::language::generator::definition::property::user::Grammar<LANGUAGE123>
{
public:
    // Non-Terminal declarations
    ::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::NonTerminal> program;
    ::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::NonTerminal> stmts;
    ::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::NonTerminal> stmt;
    
    // Production-Rule declarations
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ProductionRule> program_stmts;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ProductionRule> stmts_stmt_stmts;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ProductionRule> stmts_EMPTY;
    ::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ProductionRule> stmt_VARNAME;
public:
    Grammar(LANGUAGE123* language)
        :	::deamer::language::generator::definition::property::user::Grammar<LANGUAGE123>(language)
    {
    }
    
    void GenerateObjects() override;
};

#endif // LANGUAGE123_GRAMMAR_H
```

### Source

```cpp
#include "Grammar.h"
#include "LANGUAGE123.h"

void Grammar::GenerateObjects()
{
    // Non-Terminal initialization
	program.Set(::deamer::language::type::definition::object::main::NonTerminal("program", { program_stmts.Pointer() }));
	stmts.Set(::deamer::language::type::definition::object::main::NonTerminal("stmts", { stmts_stmt_stmts.Pointer(),stmts_EMPTY.Pointer() }));
	stmt.Set(::deamer::language::type::definition::object::main::NonTerminal("stmt", { stmt_VARNAME.Pointer() }));
    
    // Production-Rule initialization
	program_stmts.Set(::deamer::language::type::definition::object::main::ProductionRule({ Language->stmts.Pointer() }));
	stmts_stmt_stmts.Set(::deamer::language::type::definition::object::main::ProductionRule({ Language->stmt.Pointer(),Language->stmts.Pointer() }));
	stmts_EMPTY.Set(::deamer::language::type::definition::object::main::ProductionRule());
	stmt_VARNAME.Set(::deamer::language::type::definition::object::main::ProductionRule({ Language->VARNAME.Pointer() }));
    
    // Add objects
    AddObject(program);
	AddObject(stmts);
	AddObject(stmt);
    
	AddObject(program_stmts);
    AddObject(stmts_stmt_stmts);
    AddObject(stmts_EMPTY);
    AddObject(stmt_VARNAME);
}
```

