| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Threat LPD

The Threat LPD is a special LPD, used to define problems with a given language definition. You can see threats as the compiler errors and warnings of Deamer CC.

The main concepts of Deamer CC, shows us that problems with a language are also part of the language definition, that is why it is supported as LPD.

## LDO's

| LDO    | Purpose/Description                                     |
| ------ | ------------------------------------------------------- |
| Threat | Used to define a problem with the inputted definitions. |

Threat is a base class for multiple subclasses, each subclass is its own type of threat.

## Use cases

Modify generation of extensions. Extensions can use this information to optimize their outputs. If some nonterminal is not used, an extension might chose to ignore it.

Understand what problems are still left in your language.

## Notes

The Threat LPD should be generated instead of manually implemented. It is however still possible to manually implement the Threat LPD.

## Examples

## DLDL

Automatically generated when Deamer v2.1 or later is installed.

## C++ Front-End

```cpp
#ifndef LANGUAGE123_THREAT_H
#define LANGUAGE123_THREAT_H
#include "Deamer/Language/Generator/Definition/Property/Standard/Main/Threat.h"

class LANGUAGE123;

class Threat : public ::deamer::language::generator::definition::property::standard::Threat<
			  LANGUAGE123, ::deamer::language::type::definition::object::main::threat::
											analyzer::deamer::DeamerCore>
{
public:
    Threat(LANGUAGE123* language)
        :	::deamer::language::generator::definition::property::standard::Threat<
			  LANGUAGE123, ::deamer::language::type::definition::object::main::threat::
											analyzer::deamer::DeamerCore>(language)
    {
    }
};

#endif // LANGUAGE123_THREAT_H
```