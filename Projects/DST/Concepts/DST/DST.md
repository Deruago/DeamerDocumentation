# DST

## Overview

DST (Deamer String Template) is used to generate generation templates. Using a DSL one can easily define generation templates which can be utilized in C++.

## Templates

DST has different types of templates which can be implemented to generate code generation templates for C++.

An example of a standard template:

```DST
#ifndef {{header_guard}}
#define {{header_guard}}

// The definition for the type "{{class_name}}"
namespace deamer::ast {
	class {{class_name}}
	{
	private:
		{{private_member.Variable_Field}}
		
	public:
		{{constructor}}
		~{{class_name}}() = default;
	
		{{public_member.Variable_Field}}
	};
}

#endif // {{header_guard}}
```

The above template can be compiled to C++ which allows you to generate code according to this template.

See [Template.md](../Template/Template.md) (../Template/Template.md)

## Compiling templates to C++

Compiling templates to C++ is as simple as running the following command:

```bash
DST ./standard_template.dst ./template.setting.dst
```

This will output a C++ class. The interface of this class can be used to fill in variables, expand variable fields, and generate the output.

## Using C++ templates

When the template is compiled to a C++ header file. You can use the interface of the template type to generate code according to your template.

See [CPP_Frontend.md](../Template/CPP_Frontend.md) (../Template/CPP_Frontend.md)

## Runtime DST

DST is also available as library. DST can parse templates at runtime and allows an interface to interact with these runtime accessed templates.





