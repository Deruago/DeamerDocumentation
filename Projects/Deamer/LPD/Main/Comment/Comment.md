| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# Comment LPD

The comment LPD is used to model comments.

## LDO's

| LDO            | Purpose/Description                  |
| -------------- | ------------------------------------ |
| CommentValue   | Defines a relation between terminals and comments. |

## Use cases

The comment LPD provides a common base for tooling that depend on comment information.

Without this LPD, each tool needs to implement its own comment recognition retrieval system with assumptions. This results in worse support for comments.

## Notes

This is the most trivial LPD in Deamer CC, it may be used to learn about LPD construction.

## Examples

## DLDL

```DLDL
Comment SINGLE_COMMENT [
	Start "/"
]

Comment MULTI_COMMENT [
	Start "/*"
	End   "*/"
]
```

## C++ Front-End

### Header

```cpp
#ifndef CELESTE_COMMENT_H
#define CELESTE_COMMENT_H

#include "Deamer/Language/Generator/Definition/Property/User/Main/Comment.h"
#include "Deamer/Language/Type/Definition/Object/Special/Uninitialized.h"

namespace Celeste
{
	class Language;

	class Comment : public              ::deamer::language::generator::definition::property::user::Comment<
								::Celeste::Language>
	{
	public:
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::CommentValue> comment_SINGLE_COMMENT;
		::deamer::type::SafeReserve<::deamer::language::type::definition::object::main::CommentValue> comment_MULTI_COMMENT;


	private:
		

	public:
		Comment(Celeste::Language* language);
		
		void GenerateObjects() override;
	};
}

#endif // CELESTE_COMMENT_H
```

### Source

```cpp
#include "Celeste/Comment.h"
#include "Celeste/Language.h"

Celeste::Comment::Comment(Celeste::Language* language)
			:	::deamer::language::generator::definition::property::user::Comment<
					Celeste::Language>(language)
{
}

void Celeste::Comment::GenerateObjects()
{
	// Unknown References
	

	// Implement Comments
	comment_SINGLE_COMMENT
        .Set(::deamer::language::type::definition::object::main::CommentValue(
			Language->SINGLE_COMMENT.Pointer(),
			"/",
			"",
			""
		));
    comment_MULTI_COMMENT
        .Set(::deamer::language::type::definition::object::main::CommentValue(
			Language->MULTI_COMMENT.Pointer(),
			"/*",
			"",
			"*/"
		));


	// Unknown References
	

	// Add Comment
	AddObject(comment_SINGLE_COMMENT);
    AddObject(comment_MULTI_COMMENT);

}
```

