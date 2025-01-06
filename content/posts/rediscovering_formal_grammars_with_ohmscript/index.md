---
title: Formal Grammars With Ohmscript
date: 2025-01-07
summary: In "The Psychology of money", Morgan Housel presents a series of timeless lessons on wealth, greed and happiness. Knowing what one's supposed to do with their money it's easy, actually doing it is rather hard. 💸📖
tags: ["formal grammar", "rust", "computer science", "parser"]
draft: true
---

Last month I was reading [Crafting Interpreters by Robert Nystrom](https://craftinginterpreters.com/). If you're interested in programming language implementation or if you just like computer science I highly suggest giving it a go. Today I want to show you how computers *understand* the code you write.

To do that, we'll look at how we could evaluate mathematical expressions by writing an interpreter. This is the name we give to programs that do this because they act like translators between you and the computer. At the end we'll understand how a computer evaluates something like:

{{< katex >}}
\\( 5 + 6 * 3 \\)

## The Basics

Before looking at how it works, it's better to understand *what* exactly are we doing:

{{< mermaid >}}
flowchart LR
    A(Input String) --> B(Interpreter)
    B --> C(Number)
{{< /mermaid >}}

*Technically* we should also have to show an error if the users does something wrong like a typo but let's not worry about that for now.

In order to output a number we must be able to calculate it given the operators (`+` and `*`) and the numbers (`5`, `6` and `3`). That is the easy part. Slightly more involved is figuring out how to *understand* what the string of text *means*. When you see

{{< katex >}}
\\( 5 + 6 * 3 \\)

You actually think something like `5 added to the result of 6 times 3`. A computer literally sees just a bunch of meaningless characters.

We need to find a way to teach the computer how to interpret that string. Fortunately for us, this is a solved problem. Here's how it's typically done:

{{< mermaid >}}
flowchart LR
    A(Input String) --> |tokeniation| B(Tokens)
    B --> |parsing| C(Abstract Syntax Tree)
    C --> |interpretation| D(Number)
{{< /mermaid >}}

Again, we could have to return error instead of a number, we won't bother for now. Let's give that process a closer look.

### Tokenization

We take the input string, for example: `"5 + 6 * 3"` and we convert it into a list of **tokens**.
A token represents a single unit of the expression; the number `5` is a token, `+` is a token and so on. The above string gets tokenized into `Number(5), Operator(+), Number(6), Operator(*), Number(3)`.

### Parsing

This is the difficult bit. Here we do the *understanding* part of the process. Her we go from the list of tokens to what's called an **Abstract Syntax Tree**, AST for friends. Here's how the AST for our expression would look like:

{{< mermaid >}}
flowchart TD
    A('+') --> B(5)
    A --> C('*')
    C --> D(6)
    C --> E(3)
{{< /mermaid >}}

It basically a graph where each operator (`+` and `*`) is connect to its *operands* i.e. things that need to be added or multiplied. It's important to note that an *operand* can be a single number (like `5`) or a whole expression (like `6 * 3` in the case of the addition).

I can't show you today exactly of the algorithm works, maybe we'll do it in a future episode. For now you just have to know that we can do it, if we can tell the computer how to make an expression. This is where *formal grammars* come in.

### Interpreting

Once we have an AST, we just have to *traverse* it. For example, we know we want to add `5` and `6 * 3`. We know how to add a number, so the `5` is fine as-is. We have no idea how to add an expression! So we have to evaluate that first and the add the result to `5`. This is basically what it comes down to.

## Formal Grammars

Formal grammars are derived from generative grammars, a concept invented by Noam Chomsky in the 1950s. He's a linguist and was trying to explain what is it that someone knows when they can formulate phrases in a language. Formal grammars are directly correlated to generative grammars, they just don't explain natural languages.

### What is a Formal Grammar

To create a formal grammars, you'll need 3 things. Well 3 *sets* of things:

- A set of *terminals*.
- A set of *non terminals*.
- A set of *production rules*.

#### Terminals

Terminals are the *words* of your language. In English a terminal can be the word "muffin" or an exclamation point "!". Basically anything that you can write and cannot be divided. And yes, if you want to see it that way, every character can be considered a terminal.

In math expressions, terminal characters are things like "5", "5598374.23894763873", "+" etc etc.

#### Non Terminals

Non terminals represent zero, one or more characters. For example, in English *noun* would be a non terminal and it would represent any noun.
*Phrase* could also be a non terminal. This can go on and on.

In math expressions, non terminals are things like *expression* or *number*.

## Ohmscript

In the same period I was doing exercises for my exam [Fondamenti di Elettronica](https://www11.ceda.polimi.it/schedaincarico/schedaincarico/controller/scheda_pubblica/SchedaPublic.do?&evn_default=evento&c_classe=809818&__pj0=0&__pj1=b31231b20dc771d17bc84b4e6c48c848). I was getting frustrated while having to punch in the numbers for my resistance calculations. The formulas are easy, but they can get very long.

### Equivalent Resistance

If you have two resistors in series, the equivalent resistance of the two is:

{{< katex >}}
\\(R_{eq} = R_1 + R_2\\)

But if you have two resistors in parallel, the equivalent resistance becomes:

{{< katex >}}
\\(R_{eq} = \frac{1}{\frac{1}{R_1} + \frac{1}{R_2}}\\)

If you have more complex configurations, calculating the equivalent resistance can be very frustrating.

### The Language

I had an idea-creating a very simple scripting language that enabled me do this calculations more easily. I started with this idea:

- You can assign each resistor its resistance. For example: `R1 = 220`. The unit is omitted since it's always gong to be Ohms.
- If you have some resistors in series, you can calculate their equivalent resistance with the `->` function. For example: `->(R1, R2, R3)`.
- If you have some resistors in parallel, you can do similarly as before: `//(R1, R2, R3)`.
- Whatever you assign to `?` gets printed to the screen.

Let's see a larger example:

```ohmscript
R1 = 220
R2 = 10k
R3 = 1k

? = //(->(R1, //(R2, R1)), R3)
```

This program would print to the screen the result. If you had to do it by hand, it would mean calculating this:

{{< katex >}}
\\(R_{eq} = \frac{1}{\frac{1}{R_3} + \frac{1}{R_1 + \frac{1}{\frac{1}{R_2}+ \frac{1}{R_1}}}} = \\)
\\(\frac{1}{\frac{1}{1\cdot 10^3} + \frac{1}{220 + \frac{1}{\frac{1}{10 \cdot 10^3}+ \frac{1}{220}}}}\\)

The best thing is, you don't wouldn't need to rewrite the program every time you need to calculate the resistance of a similar configuration. This happens often in my exercises. For example you can just add:

```ohmscript
R1 = 220
R2 = 10k
R3 = 1k

? = //(->(R1, //(R2, R1)), R3)
? = -> (R1, //(R2, R3))
```

And this just works the way you expect it to. Isn't that nice? If you want to use it, or contribute to the project, you can find it here:

{{< github repo="TheCaptainCraken/ohmscript" >}}
