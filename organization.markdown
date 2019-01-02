---
layout: page
---

## Organization

There are many conceivable ways to organize the spec files. The structure is
based on the Ruby language as well as the major components of a Ruby
implementation.

The goal is to maintain locality by grouping related specs. Generally, there is
a single element (method, syntax element) per file, and the files are organized
into two main directories: language and core. Below is a partial graphic of the
directory tree.

    spec
    |-- core
    |     + -- array
    |     + -- bignum
    |     + -- binding
    |     + -- class
    |     + -- ...
    |     + -- time
    |     + -- true
    |     + -- unboundmethod
    |-- fixtures
    |-- language


### Syntax-sensitive Specs

There are three primary challenges in combining specs for multiple Ruby versions:

1. Syntax differences
1. Methods that behave differently
1. Libraries or classes that are not part of the version

All these are addressed by using <code>ruby_version_is</code> guards.

### Language

The `language` directory contains specs for the Ruby language proper. There
are numerous possible ways of categorizing the entities and concepts that
make up a programming language. Ruby has a fairly large number of reserved
words. These words significantly describe major elements of the language,
including flow control constructs like "for" and "while", conditional
execution like "if" and "unless", exceptional execution control like
"rescue", etc. There are also literals for the basic "types" like String,
Regexp, Array and Fixnum.

Behavioral specifications describe the behavior of concrete entities. Rather
than using concepts of computation to organize these spec files, we use
entities of the Ruby language. Consider looking at any syntactic element of a
Ruby program. With (almost) no ambiguity, one can identify it as a literal,
reserved word, variable, etc.

There is a spec file that corresponds to each literal construct and most
reserved words, with the exceptions noted below. There are also several files
that are more difficult to classify: all predefined variables, constants, and
objects (`predefined_spec.rb`), the precedence of all operators
(`precedence_spec.rb`), the behavior of assignment to variables
(`variables_spec.rb`), the behavior of subprocess execution
(`execution_spec.rb`), the behavior of the raise method as it impacts the
execution of a Ruby program (`raise_spec.rb`), and the block entities like
"begin", "do", "{ ... }" (`block_spec.rb`).

Several reserved words and other entities are combined with the primary
reserved word or entity to which they are related:

1. <code>predefined_spec.rb</code>: false, true, nil, self
1. <code>for_spec.rb</code>: in
1. <code>if_spec.rb</code>: then, elsif
1. <code>case_spec.rb</code>: when
1. <code>throw_spec.rb</code>: catch

### Core library

The `core` directory contains specs for the Ruby core library. These include
classes such as Array, String, Regexp, Range, Fixnum, Float, etc. The `core`
directory contains a subdirectory named after the classes in the core
library. For example, specs for the Array class are in the `array` directory.

Within each subdirectory of `core`, the specs for each method of that class
are placed in a separate file. For example, specs for Array#compact are
places in `spec/core/array/compact_spec.rb`. Method names with characters
like "?", "=", and "!" are in files named by stripping those characters. For
example, specs for Array#compact! are in the same file as specs for
Array#compact. All the spec files that are needed have already likely been
created. (See the documentation for [mkspec]({{ site.baseurl }}/mkspec/) for details.)
