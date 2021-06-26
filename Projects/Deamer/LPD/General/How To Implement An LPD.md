| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# How To Implement An LPD

Implementing LPD's are important if you want to create a language, multiple LPD's form LD's. So it is important to know how to implement them.

Currently there are 2 ways:

- Using C++ front-end.
- Using DLDL

We will always recommend using DLDL over the C++ front-end, because the C++ front-end requires a lot of information, information that gets automated by DLDL. So it is quicker and more efficient to use DLDL.

There are cases when you want to consult the C++ front-end, this is mostly for LPD and extension developers, as new LPD's might not be supported by DLDL.

## DLDL Front-End

DLDL stands for "Deamer Language-Definition Language", it is a set of specialized DSL's used to generate the Language Definitions and Language Property Definitions. In general each LPD has its own specialized DSL, and each DSL is in the same style.

Examples on how to use DLDL can be found in the [example repo](https://github.com/Deruago/DeamerExamples).

To start using DLDL, you are required to initialize a project. This can be done using the following:

```bash
DLDL -init -language-name=your_language_name
```

It will generate a definition directory (and other directories). In this directory you can implement various LPD's. To implement an Lexicon, create or open the file "Lexicon.dldl". In this file you can define various terminals. An example:

```DLDL
Terminal: VARNAME [a-zA-Z]+[a-zA-Z0-9]*

Delete: ESCAPE_CHARS [\n\r\t ]
```

The lexicon above will map all variables, and delete all escape characters.

When you are done you can run:

```bash
DLDL -g -ac -ar
```

To generate your language.

## C++ Front-End

The C++ front-end is somewhat more complex to setup and implement. We assume you have everything correctly set up, and focus on LPD implementation.

If you want to implement an LPD in the C++ front-end, create a file Lexicon.h. In this file you need to create a lexicon class with as subclass a lexicon generator:

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

As you can see there is a clear pattern:

- Start with the declarations. These need to be public as some LPD's depend on other LPD's members, e.g. grammar needs information about the lexicon.
- Ctor, this initializes the generator.
- GenerateObjects() function. This function is used to initialize our declarations, and make them available using AddObject(). 

Each LPD follows the above pattern. It is also important to note, that we use the class "SafeReserve", this class is used to safely reserve a spot, this vastly simplifies the generation process so it is required to be used for every LDO.

Another note: If you want to access other LDO's from other LPD's, you can use the member "Language" that is available in the new LPD class.
