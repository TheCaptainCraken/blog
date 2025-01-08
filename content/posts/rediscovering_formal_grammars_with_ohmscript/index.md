---
title: Formal Grammars With Ohmscript
date: 2025-01-08
summary: Learn how computers understand code through tokenization, parsing, and interpretation, with a dive into formal grammars and a custom scripting language for resistance calculations.
tags: ["formal grammar", "rust", "computer science", "parser"]
---

Last month, I dove into [Crafting Interpreters by Robert Nystrom](https://craftinginterpreters.com/).
If programming languages or computer science make you tick, I highly recommend checking it out.
Today, I'm going to guide you through how computers actually understand the code you write.
(Spoiler: It's magic. Okay, not really. But it feels that way.)

To illustrate, we'll build a simple interpreter that evaluates mathematical expressions.
By the end, you'll understand how a computer goes from seeing this

```latex
5 + 6 * 3
```

To actually figuring out what it means. Buckle up!

## The Basics

Let's start with the big picture. Here's what we're trying to do:

{{< mermaid >}}
flowchart LR
    A(Input String) --> B(Interpreter)
    B --> C(Number)
{{< /mermaid >}}

Of course, we should also handle errors, like if the user types `5 + *`. But for now, let's keep it simple.

To output a number, the computer needs to calculate the result based on operators (like `+` and `*`) and numbers (like `5`, `6`, and `3`).
That part's straightforward. What's tricky is teaching the computer to understand what the input string *means*. When you see

{{< katex >}}
\\( 5 + 6 * 3 \\)

You actually think something like *5 added to the result of 6 times 3*. A computer literally sees just a bunch of meaningless characters.

We need to find a way to teach the computer how to interpret that string. Fortunately for us, this is a solved problem. Here's how it's typically done:

{{< mermaid >}}
flowchart LR
    A(Input String) --> |tokeniation| B(Tokens)
    B --> |parsing| C(Abstract Syntax Tree)
    C --> |interpretation| D(Number)
{{< /mermaid >}}

Let's unpack each step.

### Tokenization

First, we break the input string—"5 + 6 * 3"—into smaller pieces called tokens. A token is a single unit of meaning: 5 is a token, + is a token, and so on. The result looks like this:

```plain
Number(5), Operator(+), Number(6), Operator(*), Number(3)
```

### Parsing

Here's where things get interesting. Parsing takes the list of tokens and builds something called an Abstract Syntax Tree (AST for friends). This tree represents the structure of the expression:

{{< mermaid >}}
flowchart TD
A('+') --> B(5)
A --> C('*')
C --> D(6)
C --> E(3)
{{< /mermaid >}}

In this tree, operators (`+` and `*`) are connected to their operands (the things they operate on). For example, `+` connects to `5` and the  sub-expression `6 * 3`. Notice that operands can be either single numbers or entire expressions.

How do we build this tree? That's a topic for another day. For now it's important to know that the first step is to properly define how the language we're parsing is structured. This is were [formal grammars](#formal-grammars) come in.

### Interpreting

Once we have an AST, we traverse it to calculate the result. Here's the basic idea:

- Start at the top of the tree. The root tells us to add `5` and the result of `6 * 3`.
- Evaluate `6 * 3` (we can do it because we know how to multiply two numbers).
- Finally execute the add operation (using `5` and the result we just calculated).

Now let's dive a bit deeper into formal grammars. These are the basis for every parser.

## Formal Grammars

Formal grammars come from the idea of generative grammars, which were introduced by Noam Chomsky back in the 1950s. Chomsky, a linguist, wanted to figure out what it is we actually *"know"* when we can construct sentences in a language. Formal grammars take inspiration from this but focus on more structured systems—they aren't trying to explain natural languages like English or Spanish.

### What is a Formal Grammar?  

To create a formal grammar, you need three main things—or, more precisely, three sets of things:

1. A set of *terminals*
2. A set of *non-terminals*
3. A set of *production rules*
4. And one starting point: a start symbol (which is always a non-terminal)

Let's go through what these mean.

#### Terminals

Terminals are like the building blocks of your language—the "words" or symbols you work with. For example, in English, a terminal might be a word like “muffin” or a punctuation mark like “!”. Basically, a terminal is something you can write down, and it can't be broken into smaller parts.

If we switch to mathematical expressions, terminals could be numbers like `5` or `42.78`, or symbols like `+` and `*`.

#### Non-Terminals

Non-terminals, on the other hand, represent groups of things. Think of them as placeholders or categories. For example, in English, *noun* could be a non-terminal that stands for any noun, like "cat" or "book." Another example could be *phrase*, which stands for any valid phrase.

In math, non-terminals might be things like *expression* or *number*. They help define patterns or rules for combining terminals.

#### Production Rules

Production rules are the instructions for how to substitute one thing for another. They're like saying, “If you see A, replace it with B.” Both A and B can be any mix of terminals and non-terminals. For example:  

```pseudo
NOUN <- potato | apple
```

This rule means you can replace the non-terminal `NOUN` with either "potato" or "apple." You can have as many production rules as you need to define your grammar.

## A Formal Grammar for Math Expressions

Let's look at an example of how formal grammar can be used to define basic math expressions:

```pseudo
EXPRESSION <- number |
    EXPRESSION OPERATION EXPRESSION |
    ( EXPRESSION )

OPERATION <- + | - | * | /
```

Here's how it works: lowercase words like `number` are terminals, while uppercase words like `EXPRESSION` and `OPERATION` are non-terminals. We aren't going to define what a `number` is (that's a whole separate topic), but just assume it represents any valid number.

The start symbol for this grammar is `EXPRESSION`.

### Example Walkthrough

Let's break down how this grammar generates the expression `5 + 6 * 3`:

1. Start with the non-terminal `EXPRESSION`.
2. Use the rule `EXPRESSION <- EXPRESSION OPERATION EXPRESSION` to expand it. Now we have: `EXPRESSION OPERATION EXPRESSION`
3. Replace the first `EXPRESSION` with `number: 5`. Now it looks like: `5 OPERATION EXPRESSION`
4. Substitute the `OPERATION` with `+`. Now we have: `5 + EXPRESSION`
5. Expand the remaining `EXPRESSION` into `EXPRESSION OPERATION EXPRESSION`. Now it's: `5 + EXPRESSION OPERATION EXPRESSION`
6. Replace the first `EXPRESSION` with `number: 6` and the `OPERATION` with `*`. That gives us: `5 + 6 * EXPRESSION`
7. Finally, replace the last `EXPRESSION` with `number: 3`.

And there we have it: `5 + 6 * 3`. Done!

Here's the rewritten version with a friendlier and more relaxed tone:  

---

## Ohmscript  

A little while back, I was studying for my [Fondamenti di Elettronica exam](https://www11.ceda.polimi.it/schedaincarico/schedaincarico/controller/scheda_pubblica/SchedaPublic.do?&evn_default=evento&c_classe=809818&__pj0=0&__pj1=b31231b20dc771d17bc84b4e6c48c848) and got *seriously* fed up with punching numbers into my calculator for resistance calculations. The formulas aren't hard, but they can get really tedious when things get complicated.

### Equivalent Resistance

If you've got two resistors in series, the equivalent resistance is pretty simple:

{{< katex >}}
\\(R_{eq} = R_1 + R_2\\)

But if those resistors are in parallel, things get a little messier:

{{< katex >}}
\\(R_{eq} = \frac{1}{\frac{1}{R_1} + \frac{1}{R_2}}\\)

Now, if you're dealing with more complex resistor networks, figuring out the equivalent resistance can become a real headache.

### The Idea

That's when I had an idea: what if I created a simple scripting language to make these calculations easier? A kind of shortcut for resistor math!

Here's what I came up with:

- You can assign values to resistors. For example, `R1 = 220`. The unit (Ohms) is assumed, so you don't have to write it.
- For resistors in series, you can use the `->` function. For example, `->(R1, R2, R3)` calculates their equivalent resistance.
- For resistors in parallel, you use `//`. It works the same way: `//(R1, R2, R3)`.
- If you want to print the result of a calculation, just assign it to `?`.

Here's an example of how it works:

```ohmscript  
R1 = 220
R2 = 10k
R3 = 1k

? = //(->(R1, //(R2, R1)), R3)
```

This program would calculate the equivalent resistance of the circuit and print the result. If you did this by hand, it would mean calculating:

{{< katex >}}
\\(R_{eq} = \frac{1}{\frac{1}{R_3} + \frac{1}{R_1 + \frac{1}{\frac{1}{R_2}+ \frac{1}{R_1}}}}=\\)
\\(\frac{1}{\frac{1}{1\cdot 10^3} + \frac{1}{220 + \frac{1}{\frac{1}{10 \cdot 10^3}+ \frac{1}{220}}}}\\)

See how much cleaner that is?

The best part is that you don't need to rewrite the program every time you want to tweak the configuration. For example, let's say you need two different calculations:

```ohmscript
R1 = 220
R2 = 10k
R3 = 1k

? = //(->(R1, //(R2, R1)), R3)
? = ->(R1, //(R2, R3))
```

Just like that, both calculations run as expected. Neat, right?

If you're interested in trying it out—or contributing—you can find the project here:

{{< github repo="TheCaptainCraken/ohmscript" >}}

So, could we actually define a formal grammar for Ohmscript? Absolutely!
If you check out the repo, you'll find a file named `ohmscript.cgn`. That file contains my definition of the grammar.
But if you think you can come up with something cleaner or just want to chat about formal grammars, feel free to reach out—I'd love to hear your thoughts!

## The Elephant in the Room

I can practically hear the collective groans of computer scientists after [showing you the grammar for a math expression](#a-formal-grammar-for-math-expressions). And honestly, I get it. That grammar is **ambiguous**.

What does that mean? Well, if you're given a string, there's no clear way to figure out exactly *which rules* were applied—and in *what order*—to create it. This might not seem like a big deal at first, but things start falling apart when you try to write a parser. Why? Because ambiguous interpretations can lead to completely different results.

Here's the problem:

Did we do this?

```pseudo
EXPRESSION
EXPRESSION OPERATION EXPRESSION
5 + EXPRESSION
5 + 6 * 3
```

Or this?

```pseudo
EXPRESSION
EXPRESSION OPERATION EXPRESSION
EXPRESSION * 3
EXPRESSION OPERATION EXPRESSION * 3
5 + 6 * 3
```  

From just looking at the final string (`5 + 6 * 3`), there's no way to know! And that's a **huge** issue, especially when writing a parser. Parsing is all about starting with the string and working backward to figure out which rules were applied. If the grammar is ambiguous, this process becomes impossible.

In practical terms, it means our parser won't understand operator precedence. Does `5 + 6 * 3` mean `(5 + 6) * 3` or `5 + (6 * 3)`? We need the grammar to make that clear!

But don't worry—this isn't the end of the story. In a future article, I'll show you how to fix this kind of ambiguity. For now, feel free to experiment and see what you can come up with!
