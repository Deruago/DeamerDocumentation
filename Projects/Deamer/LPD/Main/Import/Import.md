| Status: Incomplete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Import LPD

The Import LPD is used to model import semantics in a language.

## LDO's

| LDO            | Purpose/Description                  |
| -------------- | ------------------------------------ |
| FileImport     |  |
| FileObjectImport     |  |
| ImportFileTarget     |  |
| ImportSpecification     |  |
| ImportStyleType     |  |
| ImportSyntacticRelation     |  |
| ImportType     |  |
| ObjectTarget     |  |

## Use cases

Tooling that want to provide:
- Project wide analysis
- Project/File dependency analysis
- Retrieve full project information automatically

Require import semantics to understand the relations between files. To provide these semantics the Import LPD is provided.

## Notes

The Import LPD takes inspiration of existing import semantics. The current styles are identified:
- Celeste
- Python
- C++

## Examples

## DLDL

```DLDL
Import File StandardFileImport(file_import) [
	FileTarget: TEXT
]
```

## C++ Front-End

### Header

```cpp
#ifndef CELESTE_IMPORT_H
#define CELESTE_IMPORT_H

#include "Deamer/Language/Generator/Definition/Property/User/Main/Import.h"
#include "Deamer/Language/Type/Definition/Object/Special/Uninitialized.h"

namespace Celeste
{
	class Language;

	class Import : public ::deamer::language::generator::definition::property::user::Import<
								::Celeste::Language>
	{
	public:
		// Import Specification
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ImportSpecification> import_specification__StandardFileImport;


		// File Import Rules
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::FileImport> import_file_import_rule__StandardFileImport;


		// File Object Import Rules
		

		// Import Syntactic Relation
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ImportSyntacticRelation> import_syntactic_relation__StandardFileImport_file_import;


		// Import File Target
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::ImportFileTarget> import_file_target__StandardFileImport;


		// Import Object Target
		

	private:
		

	public:
		Import(Celeste::Language* language);
		
		void GenerateObjects() override;
	};
}

#endif // CELESTE_IMPORT_H

```

### Source

```cpp
#include "Celeste/Import.h"
#include "Celeste/Value.h"
#include "Celeste/Language.h"

Celeste::Import::Import(Celeste::Language* language)
			:	::deamer::language::generator::definition::property::user::Import<
					Celeste::Language>(language)
{
}

void Celeste::Import::GenerateObjects()
{
	// Import Specification
	import_specification__StandardFileImport.Set(::deamer::language::type::definition::object::main::ImportSpecification(
	import_file_import_rule__StandardFileImport.Pointer(),
	::deamer::language::type::definition::object::main::ImportType::FileImport,
	::deamer::language::type::definition::object::main::ImportStyleType::Celeste
	));


	// File Import Rules
	import_file_import_rule__StandardFileImport.Set(::deamer::language::type::definition::object::main::FileImport(
		import_syntactic_relation__StandardFileImport_file_import.Pointer(),
		import_file_target__StandardFileImport.Pointer()
	));


	// File Object Import Rules


	// Import Syntactic Relation
	import_syntactic_relation__StandardFileImport_file_import.Set(::deamer::language::type::definition::object::main::ImportSyntacticRelation(
	Language->file_import.Pointer(),
	nullptr
	));


	// Import File Target
	import_file_target__StandardFileImport.Set(::deamer::language::type::definition::object::main::ImportFileTarget(
	Language->value_object__StandardString.Pointer()
	));


	// Import Object Target


	// Unknown References


	// Import Specification
	AddObject(import_specification__StandardFileImport);


	// File Import Rules


	// File Object Import Rules


	// Import Syntactic Relation
	AddObject(import_syntactic_relation__StandardFileImport_file_import);


	// Import File Target


	// Import Object Target


	// Unknown References

}
```

