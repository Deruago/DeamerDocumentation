# DREGX: Regex

## Overview

Regexes are used to simplify the construction of DFAs. DREGX contains a parser for Regexes and is able to convert regexes to optimal DFAs.

## How to use Regexes?

Dregx offers various abstractions, one of those abstractions is the Regex class. This class can be used to easily work with regexes:

```C++
#include "Deamer/Dregx/Regex.h"

int main()
{
    auto regex = ::deamer::dregx::Regex("[a-zA-Z]+");
    regex.Match("a");
}
```

Abstractions can be found in the directory: "Deamer/Dregx/"

## How to use Regexes without abstractions?

If you want to use regexes without the abstractions, you need to do more boilerplate. However, it gives you far more control about how the DFA is constructed:

```C++
#include "Deamer/External/Cpp/Ast/Tree.h"
#include "dregx/Ast/Listener/User/TranslateToIr.h"
#include "dregx/Bison/Parser.h"
#include "dregx/Statemachine/ConvertRegexToDFA.h"

int main()
{
	using Tree = ::deamer::external::cpp::ast::Tree;
    auto parser = ::dregx::parser::Parser();
    auto tree = std::unique_ptr<Tree>(parser.Parse("[a-zA-Z]+"));
    auto translator = ::dregx::ast::listener::user::TranslateToIr();
    translator.Dispatch(tree_->GetStartNode());
    auto ir = translator.GetOutput();
    auto statemachine = ::dregx::statemachine::ConvertRegexToDFA::ConvertToStatemachine(ir.get());
    auto transitionTable = statemachine->ToTransitionTable();
    transitionTable.Match("a");
   
    return 0;
}
```

The above piece of code shows how we parse the regex, convert it to an IR, which in turn gets translated to a DFA, which outputs a transition table which is used to match some text.

Note that the above piece of code did not optimize the statemachine. Optimization is done by calling the "Minimize()" member function of the Statemachine class:



```C++
#include "Deamer/External/Cpp/Ast/Tree.h"
#include "dregx/Ast/Listener/User/TranslateToIr.h"
#include "dregx/Bison/Parser.h"
#include "dregx/Statemachine/ConvertRegexToDFA.h"

int main()
{
	using Tree = ::deamer::external::cpp::ast::Tree;
    auto parser = ::dregx::parser::Parser();
    auto tree = std::unique_ptr<Tree>(parser.Parse("[a-zA-Z]+"));
    auto translator = ::dregx::ast::listener::user::TranslateToIr();
    translator.Dispatch(tree_->GetStartNode());
    auto ir = translator.GetOutput();
    auto statemachine = ::dregx::statemachine::ConvertRegexToDFA::ConvertToStatemachine(ir.get());
    
    statemachine->Minimize(); // Optimize the DFA
    
    auto transitionTable = statemachine->ToTransitionTable();
    transitionTable.Match("a");
   
    return 0;
}
```

## Regex.Abstraction: Operators

The Regex abstraction offers 3 operators:

- Or (operator| , operator|=)
- Concatenate (operator+ , operator+=)
- Equal (operator==)

```C++
#include "Deamer/Dregx/Regex.h"

int main()
{
    auto regex = ::deamer::dregx::Regex("[a-zA-Z]+");
    regex.Match("a");
    
    regex += "[0-9]*";
    
    // Output: true
    std::cout << std::boolalpha;
    std::cout << "Output: " << (regex == "[a-zA-Z]+[0-9]*") << "\n";
}
```



## Regex.Direct: Operators

The Direct approach offers multiple operations:

- Or
- And
- Concatenate
- Equal

Each operation can be accessed as a member function call of the Statemachine class.

