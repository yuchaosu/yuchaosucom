---
title: Compiler
summary: Simple summary of compiler construction
date: 2024-08-19

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  

authors:
  - admin

tags:
  - Course Note
  - Academic

share: false
---

Suggested Textbook: Engineer a Compiler, Dragon book

{{< toc mobile_only=true is_open=true >}}

# Compiler
FORTRAN language has the word's first compiler. John Backus
FORTRAN(Formula Translation) is used for scientific computing.


Why are there so many programming languages?

Each language has its own distinct domain to use. Different domain has different needs, and most of time, these needs are conflicting.

  - Scientific computing: Float point, arrays, parallelism. FORTRAN
  - Business: persistance, report generation, data analysis SQL
  - System programming: control of resources, real time constraints C/C++

It is impossible to integrate all these things together.

Why are there new programming languages?

Programmer training is the dominant cost for a programming language.

1. Widely-used languages are slow to change. C/C++
2. Easy to start a language. Cna evolve quickly
Tension between these two. Productivity or training cost.
New domain may need new language. 

What is a good programming language?

There is no universally accepted metric for language design.

Tips: In Linux, if you want to execute the last command you type. Just try ![first character of your command](I have not try it yet)

Lexical Analysis
Simple speak, it is a divider in codes. 
It also classify the tokens.(Token class) Divides the codes into different tokens.(identifier, keywords, "(", ")", numbers...)
Official:
- Classify program substrings according to role(token class)
- Communicate tokens wo the parser

Left to right scan. So, sometimes lookahead is required.(Lookahead means look what are next)

## Regular Languages
Regular expressions specify regular languages
Regular expressions are syntax, while the regular languages are sets of strings. 
### Regular Expression
- Base cases
  * Single Character
    'c' = {"c"}
  * Epsilon
    The language consisting only for empty string
    $\epsilon$ = {""} $\epsilon$ is not empty set($\emptyset$)
- Compound Expressions
  These expressions can be built with other regular expressions.
  * Union
    $ A + B = \{a \vert a \in A\} \cap \{b \vert b \in B\}$   
  * Concatenation
    $ AB = \{ab \vert a \in A \wedge b \in B\}$
  * Iteration
    Kleene Closure 
    $A* = \cup _{i \geqq 0} A^i = A^0 \cup A^1 \cup A^2 \cup....$
    $A^0 = \epsilon$
- Other:
  * Positive Closure
    $A^+=\cup _{i \geqq 1} A^i = A^1 \cup A^2 \cup A^3 \cup....$
    $A^+= AA^*$
  * Option
    $A?=\{""\}\cap\{A\}$

Examples:
We have a alphabet $\Sigma$, which only have two elements 0 and 1
$\Sigma = \{0, 1\}$
In this case:
- $1^*$
  $1^* = \cup _{i \geqq 0} 1^i = "" \cup 1 \cup 11 \cup...$
  All strings of 1s
- $(1+0)1$
  $(1+0)1 = \{ab \vert a\in (1+0), b \in 1\} = \{11, 01\}$
- $0^*+1^*$
  $0^*+1^* = \{\cup _{i \geqq 0} 0^i + \cup _{i \geqq 0} 1^i\} = \{"", 0, 00, 000, 1, 11, 111,...\}$
- $(0+1)^*$
  $(0+1)^* = \cup _{i \geqq 0} (0+1)^i$ = \{"", 0+1, (0+1)(0+1), ...\}
  All strings of 0s and 1s. This set contains all possible string the alphabet $\Sigma$ can generate. We have a special symbol for it, $\Sigma^*$


## Formal Languages
Differences between regular language, formal language and regular expression
We have an alphabet $\Sigma$
The collection of all words over $\Sigma$ is denoted $\Sigma^*$
A **formal language** over the alphabet $\Sigma$ is a set of words over $\Sigma$.Equivalently, a formal language is a subset of $\Sigma^*$
A **regular language** over $\Sigma$ is a formal language over $\Sigma$ which is accepted by some DFA,
A **regular expression** is defined above.(Empty, single character, union, concatenation, kleene closure)

**Maximal Much: When we have two or more tokens have the same prefix, we will always choose the longer one.**
Example: "=" and "=="

**If one string is matched with two different kind of specifications, we use priority ordering.**
Example: $"if" \in Keywords, "if" \in Identifiers$, we will always parse the identifier first. It is hard for identifier to exclude 
all keywords. We can use priority to avoid the problem. When a higher priority specification is matched, the token is recognized.

## Finite Automata(FA)
  - An input alphabet $\Sigma$
  - A set of states $S$
  - A state state $n$
  - A set of accepting states $F\subseteq S$
  - A set of transitions $state\rightarrow ^{input}state$
<img title="Finite Automata Symbols" alt="FA Symbols" src="Fsmnotation.gif">

## Deterministic Finite Automata(DFA)
  - One transition per input per state
  - No $\epsilon$-moves

## Nondeterministic Finite Automata(NFA)
  - Can have multiple transitions for one input in a given state(The result of transition is not determined)
  - Can have $\epsilon$-moves(The transition can move without input consumption)

Success: Arrive final State and Finish scanning.

## Convert FA to RE
a
## Convert RE to miniDFA

### Covert RE to NFA
<img title="Thompson's Construction" alt="Thompson's Construction" src="thompsoncons.png">

### Convert NFA to DFA
$\epsilon$-closure: The set of all states that can be reached from current state via one or more ε transitions.
  1. Begin with the start state, the $\epsilon$-closure of NFA start state will be the start state of DFA
  2. Select one output of the current state, find all $\epsilon$-closure of the output. It will be de destination of DFA on that output.
  3. Repeat 2 until traverse all possible transitions in the NFA.

## Implement FA
A DFA can ve implemented by a 2D table.
 - Each row represents one state
 - Each column represents input symbol
 - The cross of row and column is the destination of transition $S_i \rightarrow^{input} S_k$
To save memory, we can use a linked list to store repeated row.










<!-- ## Overview

Portable, Optimization, Convenient layer between abstract higher level language and machine(Productivity).
Syntax check. Link. One language to another language. and more.

Formal definition
- Preprocessing
- lexical analysis
- parsing
- semantic analysis
- IR conversion(Intermidiate Representation)
- code optimization
- code generation

Types:
- traditional: high to low
- source to source: rewriter
- just-in-time(JIT): during the program execution(maybe interpreter?)
- decompiler
- cross compiler
- binary recompiler: binary to binary

Motivation:
Enhance quality of program.
Metrics of quality: Speed, Reliability, resilience, extensibility, Security, Energy efficiency, readability, memory foot print, code size


Interpreter
And compiler

Answer
1. Block codes(Interpreter) Always compiler
2. Interpreter
3. Compiler, 
4. Interpreter(??)
5. Both
6. Compiler
7. Compiler 
8. Both
9. Interpreter
Two kind of compiler, before and in time

Java is different.
 Compiler use more memory than interpreter during compiling process.
 **ahead of time, just in time compiler?? Java runtime, Java compile process, JavaC**

## Lecture2

### Three pass compiler
(Can process multi-kind source code and compatible with many machines)
Front end(lexical semantic) O(0)
Middle End O(nlogn) CSC766
Back End NP Complete

#### Front End

Scanner and parser
(image)
Scanner(lexical analysis)
Map character stream into tokens
```
x = xx + 200; ---> <id, x> = <id,xx> + <number, 200> ;
```
Token: macro of syntax
Parser:(semantic analysis) DFS? The combination of leaves is the final sentence.
Recognize context-free syntax & report errors
Scanner define syntax and parser define the sentence structure.


Middle End
Analyze IR and rewrites IR.
**Preserve the meaning of codes**
What is the meaning of the codes?(IS there are possiblities that)
The Optimizer(Middle End)
A series of passes.
Redundant, Bad Code, 

for() {
  x = x + 5
  A[i] = A[i] - 5 * i
}

Back End
(Image)

Scanner
Finite Automaton
A five-tuple(S, A, T, s0, sF)
S: set of states
A: alphabet
T: a function that takes two arguments. T(s, c) return another state s'.
s0: Start State
sF: Final State

## Lecture 3
DFA

r(0,1,2,3,4,5,6,7,8,9)+

S S0 S1 S2
A r,0,1,2,3,4,5,6,7,8,9
T T(s0,r) = s1 T(s1,(0,1,2,3,4,5,6,7,8,9))=s2 T(s2,(0,1,2,3,4,5,6,7,8,9))=s2
S0
{S2} **Finish State should be a set**

Use a 2D form to store the transition table.
(Option using linked list to replace)


Alternation Union
Concatenation 
Closure Iteration
Priority: Closure >  Concatenation > Alternation

Letters (a-zA-Z)
Digit (0-9)
Identifier Letter(Letter|Digit)*

Integer (+|-|e)(0|(1-9)digit*)
Decimal Integer.digit*
Real (Integer|Decimal)E(+|-|e)digits*
Complex (Real, Real)

r(((0|1|2)(epsilon|digit))|(3(epsilon|0|1))|(4|5|6|7|8|9)) -->





<!-- 1. The Hugo Blox website builder for Hugo, along with its starter templates, is designed for professional creators, educators, and teams/organizations - although it can be used to create any kind of site
2. The template can be modified and customised to suit your needs. It's a good platform for anyone looking to take control of their data and online identity whilst having the convenience to start off with a **no-code solution (write in Markdown and customize with YAML parameters)** and having **flexibility to later add even deeper personalization with HTML and CSS**
3. You can work with all your favourite tools and apps with hundreds of plugins and integrations to speed up your workflows, interact with your readers, and much more

[//]: # ([![The template is mobile first with a responsive design to ensure that your site looks stunning on every device.]&#40;https://raw.githubusercontent.com/wowchemy/wowchemy-hugo-modules/main/starters/academic/preview.png&#41;]&#40;https://hugoblox.com&#41;)

### Get Started

- 👉 [**Create a new site**](https://hugoblox.com/templates/)
- 📚 [**Personalize your site**](https://docs.hugoblox.com/)
- 💬 [Chat with the **Hugo Blox community**](https://discord.gg/z8wNYzb) or [**Hugo community**](https://discourse.gohugo.io)
- 🐦 Twitter: [@GetResearchDev](https://twitter.com/GetResearchDev) [@GeorgeCushen](https://twitter.com/GeorgeCushen) #MadeWithHugoBlox
- 💡 [Request a **feature** or report a **bug** for _Hugo Blox_](https://github.com/HugoBlox/hugo-blox-builder/issues)
- ⬆️ **Updating Hugo Blox?** View the [Update Guide](https://docs.hugoblox.com/reference/update/) and [Release Notes](https://github.com/HugoBlox/hugo-blox-builder/releases)

## Crowd-funded open-source software

To help us develop this template and software sustainably under the MIT license, we ask all individuals and businesses that use it to help support its ongoing maintenance and development via sponsorship.

### [❤️ Click here to become a sponsor and help support Hugo Blox's future ❤️](https://hugoblox.com/sponsor/)

As a token of appreciation for sponsoring, you can **unlock [these](https://hugoblox.com/sponsor/) awesome rewards and extra features 🦄✨**

## Ecosystem

- **[Bibtex To Markdown](https://github.com/GetRD/academic-file-converter):** Automatically import publications from BibTeX

## Inspiration

[Learn what other **creators**](https://hugoblox.com/creators/) are building with this template.

## Features

- **Page builder** - Create _anything_ with no-code [**blocks**](https://hugoblox.com/blocks/) and [**elements**](https://docs.hugoblox.com/reference/markdown/)
- **Edit any type of content** - Blog posts, publications, talks, slides, projects, and more!
- **Create content** in [**Markdown**](https://docs.hugoblox.com/reference/markdown/), [**Jupyter**](https://docs.hugoblox.com/getting-started/cms/), or [**RStudio**](https://docs.hugoblox.com/getting-started/cms/)
- **Plugin System** - Fully customizable [**color** and **font themes**](https://docs.hugoblox.com/getting-started/customize/)
- **Display Code and Math** - Code syntax highlighting and LaTeX math supported
- **Integrations** - [Google Analytics](https://analytics.google.com), [Disqus commenting](https://disqus.com), Maps, Contact Forms, and more!
- **Beautiful Site** - Simple and refreshing one-page design
- **Industry-Leading SEO** - Help get your website found on search engines and social media
- **Media Galleries** - Display your images and videos with captions in a customizable gallery
- **Mobile Friendly** - Look amazing on every screen with a mobile friendly version of your site
- **Multi-language** - 35+ language packs including English, 中文, and Português
- **Multi-user** - Each author gets their own profile page
- **Privacy Pack** - Assists with GDPR
- **Stand Out** - Bring your site to life with animation, parallax backgrounds, and scroll effects
- **One-Click Deployment** - No servers. No databases. Only files.

## Themes

Hugo Blox and its templates come with **automatic day (light) and night (dark) mode** built-in. Visitors can choose their preferred mode by clicking the sun/moon icon in the header.

[Choose a stunning **theme** and **font**](https://docs.hugoblox.com/getting-started/customize/) for your site. Themes are fully customizable. -->

## License

Copyright 2024-present [Yuchao Su](https://yuchaosu.com).

Released under the [MIT](https://github.com/yuchaosu/yuchaosucom/blob/main/LICENSE.md) license.
