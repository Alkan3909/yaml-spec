RFC-000
=======

Top-level block-nodes must start unindented


| Key | Value |
| --- | --- |
| Target | 1.3 |
| Status | 0 |
| Requires | |
| Related | |
| Discuss | [Issue 0](../../issues/0) |
| Tags | [block]() [indent]() [top]() |


## Problem

YAML technically allows top level block values (the 98% use case of YAML documents) to be start indented, but nobody does that.

It makes the language and parser strategies harder to reason about.


## Proposal

YAML top level block collections must be unindented.
Their content must start in the first column (aka "column 1").

## Explanation

There's a couple things going on here.

Languages (like Python) that use indentation based scoping are usually not coded with the top level indented.
In fact, Python does not allow it.

YAML does allow it but for no likely reason except consistency.
We've never seen YAML used in this way.

YAML 1.2 indentation is specced to start at an impossible level, -1.
That makes this be indented 0:
```
foo: 42
```

While a neat trick mathmatically it has its downfalls:
```
--- |2
    foo
  42
```
is hard to reason about.
It seems to mean `"  foo\n42\n" but technically means `"   foo\n 42\n"` because counting starts at -1.

A better idea is to just make top level stuff "unindented".
ie There's no concept of indentation at all, and everything starts in column 1, which is normal and expected.
Indented sub-scopes are always visually indented a certain number of spaces.

This makes the visual and technical levels of indentation the same, and reduces the ambiguity of discourse about a YAML node.