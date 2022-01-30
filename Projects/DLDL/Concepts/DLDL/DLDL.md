# DLDL

This document covers DLDL (Deamer Language-Definition Language)

## Overview

DLDL is an alternative front end for Deamer. A front-end is how you utilize Deamer CC. Deamer CC is written in C++, using it in C++ is thus referred to as "using the C++ front-end".

As the C++ front-end is extremely verbose and explicit. It is not desirable to write in it. DLDL is used to generate the boilerplate code required for the C++ front-end. In essence, the output of DLDL is a full C++ project utilizing Deamer CC.



## How does one use DLDL?

DLDL needs to be installed first. The installation instructions are shared on the Deamer main repo and the DLDL repo:

- DLDL: https://github.com/Deruago/DLDL
- Deamer: https://github.com/Deruago/theDeamerProject

After you have installed it. You can generate a Deamer project. Deamer projects are the projects you make with the Deamer toolchain. Initializing a project is done as follows:

```bash
DLDL -init -language-name=QAZ
```

Where "QAZ" is your language name. There are multiple project types however, we will not discuss those here.

After you have initialized the project, you can write the various LPDs of you language in the definition folder. When you are done with defining your language. You can generate, compile, and run the C++ front-end created by DLDL:

```bash
DLDL -g -ac -ar
```

Perfect! DLDL is used to generate a compiler generation project. Which in turn is compiled and ran to create your new language.

## Definitions

Definitions in DLDL are DSLs representing LPDs in Deamer CC. Each definition is specialized for each LPD. This means that the definition is simple and easy to use. In DLDL there are various supported LPDs. Whenever more LPDs are created and used DLDL will be extended.

For a description for each LPD see: [./../LPD/LPD.md](./../LPD/LPD.md)

## Compiler Generator

The Compiler Generator directory contains the generated C++ front-end. This project can be compiled and ran. If the given definitions did not have any threat (issue), the generated code will generate the language and related generators.

## Multi Compilers

DLDL supports multi compilers. An example of a multi compiler is DLDL. DLDL exists of multiple DSLs, one DSL for each LPD. One can create multi compilers by creating a subdirectory in the main language definition folder. E.g.:

- Definition
    - QAZ
        - Lexicon.dldl
        - Grammar.dldl
            - SUBLANG1
                - Lexicon.dldl
                - Grammar.dldl
            - SUBLANG2
                - Lexicon.dldl
                - Grammar.dldl

When using Deamer v2.3.0 or later, one can initialize sub languages via command line.

## .deamer directory

Since Deamer v2.3.0 the .deamer directory is introduced. This is a hidden directory used to store DLDL settings and other meta data. Deamer and DLDL use this information to optimize and ease generation.

The primary function is to easily transfer project settings. Currently when you share your project, you need to give the correct generation instructions used for DLDL. The deamer changes this by storing this meta data.

## Deamer CC LPD generation

Since Deamer v2.3.0

DLDL is used to automatically generate and integrate LPDs in Deamer CC. Before Deamer v2.2.0 LPDs needed to be manually integrated with the system. The integration had a manual algorithm to do so however, this algorithm was automatable. This is done using DLDL.

Each LPD supported by Deamer CC requires the DLDL LPD definition.

## Deamer CC Tool generation

Since Deamer v2.3.0

Like LPD generation, tools are partly generatable. DLDL is used to automatically integrate tools in the Deamer CC infrastructure.

