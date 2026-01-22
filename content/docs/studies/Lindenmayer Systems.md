---
title: "Lindenmayer Systems (L-Systems)"
weight: 1
---

{{< wip >}}
Updates soon.
{{< /wip >}}

# Parametric Lindenmayer Systems (L-Systems)

If you need directional growth with branching structures and do not require realistic physics, complex obstacle interaction, or continuous simulation, L-Systems are usually a suitable choice.

These algorithms were introduced in 1968 by Aristid Lindenmayer, a biologist who developed them to formally describe and model plant growth processes.

An L-System is not recursive in the classical sense. The rewriting process is parallel rather than sequential and does not rely on a call stack, function calls, or mutable state. Even so, the resulting structures often resemble fractals due to their self-similar expansion across iterations.

In simple terms, an L-System is a rule set that defines how a string evolves over time.

## How It Works

A string rewriting system, often referred to as a semi-Thue system, can be formally described as a tuple. L-System grammars are a restricted form of such systems and are commonly defined as a triple:

{{< katex >}} G = (V, \omega, P) {{< /katex >}}

{{< details "Tuple" >}}
A tuple is a finite ordered list of mathematical objects called elements. The order matters, and elements are typically written inside parentheses, separated by commas.
{{< /details >}}

This notation may appear meaningless if you are unfamiliar with formal grammars. That is reasonable. What follows is a more concrete interpretation.

An L-System can be understood as a synthetic grammar designed to represent growth. Instead of describing sentences or expressions or even directly representing it visually, it describes how a structure expands step by step through symbolic substitution.

In this grammar, V represents the set of symbols that exist, functioning as the vocabulary of the system. The symbol ω, called the axiom, represents the initial string from which all rewriting begins. P denotes the set of production rules that define how symbols are replaced at each iteration. Taken together, G defines everything required for an L-System to exist and operate, as a list.

It is important to note that the grammar alone does not produce geometry. To visualize the result, the generated string must be interpreted by a rendering mechanism, most commonly turtle graphics, which maps symbols to drawing commands such as movement, rotation, and state preservation.

