# Templates

## Overview

A template is used to define the overall structure of what you want to generate. In DST there are 2 parts:

- The main template, named with the convention: "template_name.dst"
- The setting template, named with the convention: "template_name.setting.dst"

The main template explains the main structure. The user can define various variables implicitly and how these variables are used.

The setting template is used to apply abstractions and set several settings for your template.

## Main template

When starting a DST project. You start with writing the main template. This template explains the main structure of the output you want to generate. For more advanced users, the main template defines the file.output variable value.

The main template looks like the following:

```DST
Hello {{value}}!
```

The above code shows a valid template. As you can see we used to variable "value". When we compile the template to C++ we will get a variable called "value" which we can use to set some value (such as "world").

One can also make more complex templates:

```DST
#ifndef {{project_name.Upper}}_{{class_name.Upper}}_H
#define {{project_name.Upper}}_{{class_name.Upper}}_H

class {{class_name}}
{
private:
	{{private_member.Variable_Field}}
public:
	{{public_member.Variable_Field}}

public:
	{{class_name}}() = default;
	~{{class_name}}() = default;
};

#endif // {{project_name.Upper}}_{{class_name.Upper}}_H
```

Notice that you can reuse variables multiple times and that we introduced specializations. Take a look at ``` {{private_member.Variable_Field}}``` this variable contains a specialization named ".Variable_Field". In DST we have several reserved specializations such as "Variable_Field". A Variable Field is equivalent to a dynamic array. It stores multiple variables and concatenates them.

There are multiple reserved keywords for variables and specializations. For a full list of the reserved functionality go to the bottom of this document. Each reserved keyword is listed with an attached document further explaining the keyword.

Alongside reserved specialization you can also define your own specializations. However, the support for custom specializations is currently limited.

## Setting template

Alongside the main template you can optionally utilize a setting template. The setting template is used to provide abstractions for the main template and to set specific settings.

Take the main template from earlier. To define a header guard we needed to combine 2 variables together and copy this 3 times. However, using the setting template we can make an abstraction variable called "header_guard":

```DST
{{header_guard}} = @<{{project_name.Upper}}_{{class_name.Upper}}_H>@
```

The above code is a valid setting template. It created a variable called "header_guard" which is defined as ```{{project_name.Upper}}_{{class_name.Upper}}_H"```.

We can now use this variable in the main template:

```DST
#ifndef {{header_guard}}
#define {{header_guard}}

class {{class_name}}
{
private:
	{{private_member.Variable_Field}}
public:
	{{public_member.Variable_Field}}

public:
	{{class_name}}() = default;
	~{{class_name}}() = default;
};

#endif // {{header_guard}}
```

Every variable can have a default value defined in the setting template. Alongside setting default values (abstractions), the setting template is used to set settings.

At the top it is recommended to set the following settings:

```DST
{{file.target_language}} = <C++>
{{file.file_name}} = <HeaderFile>
{{file.namespace}} = <deamer::templates::cmake::miscel::standard>
{{file.extension}} = <h>

{{header_guard}} = @<{{project_name.Upper}}_{{class_name.Upper}}_H>@
```

The file settings define important things in how the template will be compiled. The first setting defines that we target C++. The second one is used to specify the file name of the template. The namespace is used to define the namespace the template lives. And the last one specifies the file extension as the output is a header set this to 'h'.

## Alternative fields

In the setting template, you can define abstractions for variables. This can look like the following:

```DST
{{file.target_language}} = <C++>
{{file.file_name}} = <HeaderFile>
{{file.namespace}} = <deamer::templates::cmake::miscel::standard>
{{file.extension}} = <h>

{{header_guard}} = @<{{project_name.Upper}}_{{class_name.Upper}}_H>@
{{test1}} = <>
{{test2}} = @<>@
{{test3}} = #<>#
{{test4}} = @@<>@@
{{test5}} = &<>&
```

There are different ways of defining the field of a value. Look at the variables test1 and test2. The field of test2 starts with @< and ends with >@. Test1 starts with < and ends with >.

The reason you can define alternative fields is to free up character combinations. DST cannot understand if the following template contains 1 or 2 variables:

```DST
{{test}} = <
{{test2}} = <>>
```

DST will either set the value of test to either: ```{{test2}} = <>```  or ```{{test2}} = <```. Because it does not know if the '>' is used to close the first field. To fix this ambiguity alternative fields allows you to specify different start and stop sequences, allowing DST to deduce where the field stops.

To free most combinations, it is recommended to use the following field: ```@< some value >@```. If the sequence >@ is required, you can opt to use a different field instead. The alternative is using the reserved variables used to declare angle characters:

```DST
{{some_var_t}}       =  <template<{{right_angle_bracket}}>
{{this_is_the_same}} = @<template<>>@
{{and_this}}         = #<template<>#
```

## Reserved keywords

### Variables

#### Characters

| DST Variable        | Character | Documentation                                                |
| ------------------- | --------- | ------------------------------------------------------------ |
| left_angle_bracket  | <         | [Here](./ReservedVariables/left_angle_bracket.md) (./ReservedVariables/left_angle_bracket.md) |
| right_angle_bracket | >         | [Here](./ReservedVariables/right_angle_bracket.md) (./ReservedVariables/right_angle_bracket.md) |
| left_curly_bracket  | (         | [Here](./ReservedVariables/left_curly_bracket.md) (./ReservedVariables/left_curly_bracket.md) |
| right_curly_bracket | )         | [Here](./ReservedVariables/right_curly_bracket.md) (./ReservedVariables/right_curly_bracket.md) |
| left_bracket        | {         | [Here](./ReservedVariables/left_bracket.md) (./ReservedVariables/left_bracket.md) |
| right_bracket       | }         | [Here](./ReservedVariables/right_bracket.md) (./ReservedVariables/right_bracket.md) |

#### Template settings

| DST Variable | Usage                                               | Documentation                                                |
| ------------ | --------------------------------------------------- | ------------------------------------------------------------ |
| file         | Used to define how the template should be generated | [Here](./ReservedVariables/file.md) (./ReservedVariables/file.md) |

### Specialization

#### Formatting

| DST Specialization | Specialization name     | Usage                                               | Documentation                                                |
| ------------------ | ----------------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| Upper              | Upper case              | The variable value is converted to full upper case. | [Here](./VariableSpecializations/Upper.md) (./VariableSpecializations/Upper.md) |
| Lower              | Lower case              | The variable value is converted to full lower case  | [Here](./VariableSpecializations/Lower.md) (./VariableSpecializations/Lower.md) |
| Snake              | Snake case              | The variable value is converted to snake case.      | [Here](./VariableSpecializations/Snake.md) (./VariableSpecializations/Snake.md) |
| Colon              | Colon separation        | The variable value is separated into colons.        | [Here](./VariableSpecializations/Colon.md) (./VariableSpecializations/Colon.md) |
| DoubleColon        | Double Colon separation | The variable value is separated into double colons. | [Here](./VariableSpecializations/DoubleColon.md) (./VariableSpecializations/DoubleColon.md) |
| Slash              | Slash separation        | The variable value is separated into slash.         | [Here](./VariableSpecializations/Slash.md) (./VariableSpecializations/Slash.md) |
| BackSlash          | Back slash separation   | The variable value is separated into back slash.    | [Here](./VariableSpecializations/BackSlash.md) (./VariableSpecializations/BackSlash.md) |

#### Memory/Logic

| DST Specialization       | Specialization name      | Usage                                                        | Documentation                                                |
| ------------------------ | ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Variable_Field           | Variable Field           | Used to specify an dynamic array.                            | [Here](./VariableSpecializations/Variable_Field.md) (./VariableSpecializations/Variable_Field.md) |
| Variable_Field_Separator | Variable Field Separator | Used to specify how the next element should be concatenated. By default a newline character is inserted. | [Here](./VariableSpecializations/Variable_Field_Separator.md) (./VariableSpecializations/Variable_Field_Separator.md) |
| Function_Field           |                          | (Not implemented yet) Used to specify a function.            | [Here](./VariableSpecializations/Function_Field.md) (./VariableSpecializations/Function_Field.md) |
| Function_Field_Separator |                          | (Not implemented yet)                                        | [Here](./VariableSpecializations/Function_Field_Separator.md) (./VariableSpecializations/Function_Field_Separator.md) |

