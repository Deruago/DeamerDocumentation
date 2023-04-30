# LDODef (LPD Definition)

## Overview

DLDL supports various DSLs for generating and integrating LPDs for Deamer CC. One of the DSLs is LDODef. LDODef is used to define LDOs and Enumerations for Deamer CC.

## LDO Structure

```DLDL
name: Terminal
type: LDO
description: <
	Terminal symbol, used in lexicon definitions.
	Designed by Thimo Böhmer
>
data: <
	String Name
	String Regex
	SpecialType Special
>

Unique: <
	Name
>
```

The DSL allows you to give the following information:

- The LDO name
- The LDO type whether it is a LDO or enumeration
- Description
- The datastructure of the LDO

Note that you can specify which field groups are unique. DLDL can generate specific threat analyzers for checking if these field groups are unique. DLDL can also create various getter functions dependent on the unique members.

## Enumeration Structure

```DLDL
name: SpecialType
type: Enumeration
description: <
	Enumerates all special variants of Terminals, that are available.
>
members: <
	// A normal terminal
	Standard 0
	
	// Used to specify if we should remove this terminal
	// from the Terminal sequence
	Delete 1
	
	// Used to specify that its value should be ignored.
	// And discard the terminal after production rule deduction
	Ignore 2
	
	// Used to specify that its value should be ignored (has no value)
	NoValue 4
	
	// When encountering this terminal crash the program
	Crash 8
>
```

The difference is with the LDO variant is that the enumeration fills in the field members. Note that you can use comments in this field. The comments will be connected to the next enumeration in line.
