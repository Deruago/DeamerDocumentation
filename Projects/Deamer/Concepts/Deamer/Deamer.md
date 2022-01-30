# Deamer

Deamer is a infrastructure for compiler and ecosystem generation. Unlike parser generators, Deamer is capable of generating full compilers and ecosystems.

This document will briefly cover various important topics related to Deamer. A link to the full documentation of each topic is provided.

## LD/LPD system

The LD/LPD system is used to define compilers and ecosystems around compilers. It is deeply integrated in the Deamer infrastructure. Developers and users can define their own LPD's and integrate them automatically with the Deamer ecosystem.

See: [LD LPD system](./../Language/LD_LPD_system.md)

## Compiler generation

Given the Language Definition, Deamer can generate various projects such as compilers. Deamer is heavily oriented around ecosystem and compiler generation. Deamer has a default definition set which can be used to define full compilers and supported ecosystem generators.



## Ecosystem generation

Ecosystem generation is a relatively new concept in language generation. We started with parser generation, expanded to compilers, and Deamer adds to this the ability to generate ecosystems. Ecosystems determine whether you use or ignore a language.

In an ecosystem you have various components such as:

- Documentation
- Syntax highlighters / Semantic highlighters
- Code generators
- UML diagrams -> Language code (if language is OOP)
- Static analysers
- Language Servers (LSP)



## DLDL

One can utilize Deamer CC directly using the C++ front-end. However, this approach is not recommended for new comers. The C++ front-end is extremely verbose and explicit.

To automate the C++ programming DLDL is created. DLDL generates the full C++ implementation using specialized DSLs. The DSLs are simple and can be used to define any LPDs.

Alongside generating the C++ project, DLDL is also used to maintain Deamer projects.



## C++ Front-end

The C++ front-end is the same as directly using Deamer CC. As Deamer CC is written in C++, using the project in C++ is referred to as "using the C++ front-end". The DLDL front-end is an alternative way of using Deamer CC. DLDL generates a project utilizing the C++ front-end.

The C++ front-end is heavily designed with the concept "the user makes the choices". Deamer CC will not automate anything that it was not said to do so. An example of this is that you are required to manually link together multiple languages to organize multi compilers. A different example is that you are required to manually call the various generators. DLDL automates all this and more.

A reason of using the C++ front-end is when you need more control or when you are using highly experimental features not supported by DLDL. A different reason is when you develop for Deamer CC. In this case knowing how to use Deamer CC is extremely useful.

