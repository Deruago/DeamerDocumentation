| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Generation LPD

The generation LPD defines how Deamer CC needs to generate your language. As Deamer CC needs to integrate specific tools, it is required to have a proper generation LPD describing which tools to generate and which to integrate.

## LDO's

| LDO       | Purpose/Description                                          |
| --------- | ------------------------------------------------------------ |
| Generate  | Used to specify which tool to generate.                      |
| Integrate | Used to specify which source tool needs to be integrated with target tool. |
| Argument  | Used to specify different arguments for generators. If a generator allows different settings, you can interact using this LDO. |

## Use cases

Coordinate the generation and integration of generators in Deamer CC.

## Notes

By default the target language is C++.

DLDL always gives a full compiler generation LPD by default. This can however be overridden by manually implementing the GenerationLPD.

## Examples

## DLDL

```DLDL
Generate: Lexer Flex
Generate: Parser Bison
Generate: Ast CPP

Integrate: Flex Bison
Integrate: Flex DeamerAST
Integrate: Bison DeamerAST

Argument: Flex Debug
```

## C++ Front-End

### Header

```cpp
#ifndef LANGUAGE123_GENERATION_H
#define LANGUAGE123_GENERATION_H

#include "Deamer/Language/Generator/Definition/Property/User/Special/Generation.h"

class LANGUAGE123;

class Generation : public ::deamer::language::generator::definition::property::user::Generation<LANGUAGE123>
{
public:
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Generate>
		generate_Flex;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Generate>
		generate_Bison;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Generate>
		generate_DeamerAST;
    
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Integrate>
		integrate_FlexAndBison;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Integrate>
		integrate_FlexAndDeamerAST;
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::Integrate>
		integrate_BisonAndDeamerAST;
    
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::GenerateArgument>
		argument_Flex_Debug;
    
	::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::OSTarget>
		os_linux;

public:
	Generation(Language* language)
		: ::deamer::language::generator::definition::property::user::Generation<
			  LANGUAGE123>(language)
	{
	}

	void GenerateObjects() override
	{
		generate_Flex.Set(::deamer::language::type::definition::object::main::Generate(
			::deamer::tool::type::Tool::Flex));
		generate_Bison.Set(::deamer::language::type::definition::object::main::Generate(
			::deamer::tool::type::Tool::Bison));
		generate_DeamerAST.Set(::deamer::language::type::definition::object::main::Generate(
			::deamer::tool::type::Tool::DeamerAST));

        integrate_FlexAndBison.Set(
			::deamer::language::type::definition::object::main::Integrate(
				::deamer::tool::type::Tool::Flex, ::deamer::tool::type::Tool::Bison));
		integrate_FlexAndDeamerAST.Set(
			::deamer::language::type::definition::object::main::Integrate(
				::deamer::tool::type::Tool::Flex, ::deamer::tool::type::Tool::DeamerAST));
		integrate_BisonAndDeamerAST.Set(
			::deamer::language::type::definition::object::main::Integrate(
				::deamer::tool::type::Tool::Bison, ::deamer::tool::type::Tool::DeamerAST));

		argument_Flex_Debug.Set(
			::deamer::language::type::definition::object::main::GenerateArgument(
				::deamer::tool::type::Tool::Flex, "Debug"));

        os_linux.Set(::deamer::language::type::definition::object::main::OSTarget(
			::deamer::file::tool::OSType::os_linux));

		// Add object calls
		// AddObject(...)
		AddObject(generate_Flex);
		AddObject(generate_Bison);
		AddObject(generate_DeamerAST);

        AddObject(integrate_FlexAndBison);
		AddObject(integrate_FlexAndDeamerAST);
		AddObject(integrate_BisonAndDeamerAST);

		AddObject(argument_Flex_Debug);

		AddObject(os_linux);
	}
};

#endif // LANGUAGE123_GENERATION_H
```

