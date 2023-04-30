| Status: Incomplete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Value LPD

The Value LPD is used to model the value semantics of trivial value constructions.

## LDO's

| LDO            | Purpose/Description                  |
| -------------- | ------------------------------------ |
| ValueAbstraction     |  |
| ValueAbstractionType     |  |
| ValueObject     |  |
| ValueObjectType     |  |

## Use cases

Tooling that require trivial value semantics include:
- Tooling that depend on the Import LPD model

## Notes

This model targets trivial constructs, any value that is more complex than the concepts provided is not expressable with this model.

## Examples

## DLDL

```DLDL
Value StandardString(TEXT) [
	IsString(True) [	
		Start('\"')
		End('\"')
		EscapeSequence('\"')
	]
]

Value StandardInteger(NUMBER) [
	IsInteger(True)
]

Value StandardDecimal(DECIMAL) [
	IsDecimal(True)
]

Value StandardName(VARNAME) [
]
```

## C++ Front-End

### Header

```cpp
#ifndef CELESTE_VALUE_H
#define CELESTE_VALUE_H

#include "Deamer/Language/Generator/Definition/Property/User/Main/Value.h"
#include "Deamer/Language/Type/Definition/Object/Special/Uninitialized.h"

namespace Celeste
{
	class Language;

	class Value : public ::deamer::language::generator::definition::property::user::Value<
								::Celeste::Language>
	{
	public:
	// Value Objects (Rules)
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueObject> value_object__StandardString;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueObject> value_object__StandardInteger;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueObject> value_object__StandardDecimal;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueObject> value_object__StandardName;


	// Value Abstractions
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__0;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__1;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__2;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__3;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__4;
::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ValueAbstraction> value_abstraction__5;

	private:
		

	public:
		Value(Celeste::Language* language);
		
		void GenerateObjects() override;
	};
}

#endif // CELESTE_VALUE_H
```

### Source

```cpp
#include "Celeste/Value.h"
#include "Celeste/Language.h"

Celeste::Value::Value(Celeste::Language* language)
			:	::deamer::language::generator::definition::property::user::Value<
					Celeste::Language>(language)
{
}

void Celeste::Value::GenerateObjects()
{
	// Value Objects (Rules)
	
		value_object__StandardString.Set(::deamer::language::type::definition::object::main::ValueObject(
			::deamer::language::type::definition::object::main::ValueObjectType::Standard,
			"StandardString",
			{
				Language->TEXT.Pointer(),

			},
			{
				value_abstraction__0.Pointer(),

			}
		));

		value_object__StandardInteger.Set(::deamer::language::type::definition::object::main::ValueObject(
			::deamer::language::type::definition::object::main::ValueObjectType::Standard,
			"StandardInteger",
			{
				Language->NUMBER.Pointer(),

			},
			{
				value_abstraction__4.Pointer(),

			}
		));

		value_object__StandardDecimal.Set(::deamer::language::type::definition::object::main::ValueObject(
			::deamer::language::type::definition::object::main::ValueObjectType::Standard,
			"StandardDecimal",
			{
				Language->DECIMAL.Pointer(),

			},
			{
				value_abstraction__5.Pointer(),

			}
		));

		value_object__StandardName.Set(::deamer::language::type::definition::object::main::ValueObject(
			::deamer::language::type::definition::object::main::ValueObjectType::Standard,
			"StandardName",
			{
				Language->VARNAME.Pointer(),

			},
			{

			}
		));


	// Value Abstractions
	
		value_abstraction__0.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::IsString,
			"True",
			{
				value_abstraction__1.Pointer(),
				value_abstraction__2.Pointer(),
				value_abstraction__3.Pointer(),

			}
		));

		value_abstraction__1.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::StringStartPattern,
			"\"",
			{

			}
		));

		value_abstraction__2.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::StringEndPattern,
			"\"",
			{

			}
		));

		value_abstraction__3.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::StringEscapePattern,
			"\"",
			{

			}
		));

		value_abstraction__4.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::IsInteger,
			"True",
			{

			}
		));

		value_abstraction__5.Set(::deamer::language::type::definition::object::main::ValueAbstraction(
			::deamer::language::type::definition::object::main::ValueAbstractionType::IsInteger,
			"True",
			{

			}
		));


	// Unknown References
	

	// Add Value Objects (Rules)
	
	
	// Add Value Abstractions
	

	// Unknown References
	
}
```

