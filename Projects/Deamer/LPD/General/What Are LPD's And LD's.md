| Status: Complete | Part Of: Deamer CC |
| ---------------- | ------------------ |



# What Are LPD's And LD's

LPD's and LD's are fundamental parts of how Deamer functions. Deamer uses LD's to generate a lot of code, throughout the project.

## LD (Language Definition)

A LD or Language Definition, defines everything about your language, how it is used, to how we can implement it. Traditional language definitions only define the lexicon (lexer), and grammar (parser). We go beyond this allowing everything to be defined.

An great example are the various "LPD's" we offer (we taken a subset of LPD's):

| LPD          | Purpose / Description                                        |
| ------------ | ------------------------------------------------------------ |
| Lexicon      | Define the lexicon of our language, allows generation of Lexers. |
| Grammar      | Define the grammar of our language, allows generation of parsers. |
| Identity     | Define the "identity" of our language, e.g.: its name, associated file extensions, etc. |
| Generation   | Define how we can generate the language.                     |
| Threat       | Define which threats (problems) are in your language. This is often generated using a default generator. |
| Colorization | Define how we can color the language, allows generation of syntax highlighters. |

As you can see in the table above, we offer different definitions than you would be familiar with. Using these definitions we want to define how the language integrates with its potential ecosystem. Given enough definitions related to its ecosystem, Deamer can generate a big part of the ecosystem.

# LPD's (Language Property Definitions)

Language Property Definitions or LPD for short, are used to define properties of a language. LPD's can range from language specific things (such as: lexicon, grammar), to how languages are integrated (such as: colorization, generation).
