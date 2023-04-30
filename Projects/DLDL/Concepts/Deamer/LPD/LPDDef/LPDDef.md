# LPDDef (LPD Definition)

## Overview

DLDL supports various DSLs for generating and integrating LPDs for Deamer CC. One of the DSLs is LPDDef. LPDDef is used to forward declare LPDs and give the minimal information required for integrating the LPD in the Deamer CC ecosystem.

## Structure

```DLDL
name: Lexicon
type: main
description: <
	Language Property Definition of a lexicon, used to define a lexicon for language x.
	Designed by Thimo Böhmer
>
ldo: <
	Terminal
>
enum: <
	SpecialType
>
data: <
	Vector<Terminal> Terminals
>
function: <
	const Terminal GetTerminal(String terminalName) const
	{
		for (auto terminal : Terminals)
		{
			if (terminal.Name == terminalName)
			{
				return terminal;
			}
		}
	}
>
```

The above explains the LPD Forward Declaration. It tells us about:

- The LPD name
- Its LPD type
- Description of the LPD
- Associated LDOs
- Associated Enumerations
- The LPD datastructure
- LPD functionality

