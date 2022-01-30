# LD/LPD System

In Deamer languages are defined using the LD/LPD system. LD stands for "Language Definition" and LPD stands for "Language Property Definition".

## 1: Languages

### 1.1: What is a Language?

Different projects define languages differently. Often a language is seen as some syntax. Others see it as a combination of syntax and semantics.

In Deamer we define languages as follows:

- Some model which translates some input into some defined output
- Everything related to this model

The first point states that a language is some model which is used translate some input into some output. To illustrate, suppose we have some compiler called the "ABC compiler". The compiler has a C++ front-end and translates this into machine code. The language is the ABC compiler, Not the C++ front-end. The front-end is a subset of the ABC compiler model.

To further clarify, C++ is seen as a set of syntax and semantic rules. However C++ does not define the translation to machine code, it is simply syntax + semantics. As a language is defined as some model which takes in some input translating it into some output, C++ is clearly incomplete as no translation to machine code is given.

The second point states that everything related to the model belongs to the language. This means that the following things belong to languages:

- Front-end language syntax/semantics
- Back-end code generation
- Mid-end optimizations
- Issues with the language
- How the language should be highlighted
- How the language is generated
- Language documentation

Anything which slightly relates to the language, belongs to the language.

### 1.2: LD/LPD system

A question one might raise is, how do we define languages using the definition from 1.1?

Deamer CC has solved this by defining a specialized definition system called the "LD/LPD system". LD stands for Language Definition and LPD stands for Language Property Definition.

The idea of this system is that we have multiple sets of definitions which all define some property of the language. E.g. the syntax is a property of the language, but also its semantics, optimizations, documentation, issues, and more. These are all properties of the language. If one defines all properties of its language one can fully understand the language.

In Deamer we define the properties of the language using LPD's (Language Property Definition). One can combine multiple definitions to further define their theoretical language. Deamer understands the LPD's and is able to generate logic accordingly.