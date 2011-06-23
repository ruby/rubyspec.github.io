---
layout: page
---

## Style Guide

Generally, [RSpec](http://rspec.info) specs describe the expected behavior of
code. While RSpec is fairly young, there are some conventions for writing
specs. The RubySpecs cover a wide variety of components, so we have developed
some pragmatic conventions to handle the various situations. As noted below,
some conventions are more rigid than others.

These conventions apply to all specs. Existing specs that deviate from these
conventions need to be fixed. Consistency is the principle that will almost
always trump other conventions. Consistency aids understanding and
readability. There are many thousands of lines of code in the spec files, so
the value of consistency cannot be overstated.

The specs uniformly use `describe` not `context`. The use of `it` is
preferred over `specify` except in situations when the first word of the
string is not a verb. The word "should" is unnecessary noise in the spec
description strings and is not used. (The rationale is this: the spec string
describes the expected behavior unconditionally. The code examples, on the
other hand, set up an expectation that is tested with the call to the
_should_ method. The code examples can violate the expectation, but the spec
string does not. The value of the spec string is as clearly as possible
describing the behavior. Including "should" in that description adds no
value.)

Whenever possible, the spec strings should be written to conform to very
basic English sentence structure: _subject + predicate_. The spec strings
also uniformly use double-quotes, not single-quotes. The minimum number of
words should be used to describe the behavior. Only make distinctions when
they add significant value to understanding the behavior. This is explained
further below. The general rule across all the specs is to use the least
amount of detail to unambiguously describe behavior. Add to the detail
conservatively. This is conceptually consistent with doing the simplest thing
that could work.

Ruby is a beautifully expressive language with _optional_ parentheses. There
is a distinct preference for omitting parentheses in the specs whenever they
are not needed. In other words, parentheses should _not_ be used unless
necessary to make an expression syntactically or semantically correct.

### Basic Principles

#### Error messages

Do not check for specific error messages, just the exception type.
Implementations should be free to enhance the error message, offer
implementation-specific details, or even translate the error messages.  Code
raises are rescues the _class_ of an exception, not the error message, which
is provided for human consumption. The class of the exception is the interface
that the specs are testing.

{% highlight ruby linenos %}
# The following is correct.
lambda { 1/0 }.should raise_error(ZeroDivisionError)
{% endhighlight %}

{% highlight ruby linenos %}
# The following is NOT correct.
lambda { 1/0 }.should raise_error(ZeroDivisionError, "divided by 0")
{% endhighlight %}

#### One "should" per example

This is a guiding principle, not a hard and fast rule.

For expressing different aspects of a scenario, you can usually factor out
the scenario into a helper method, and then have different examples using the
helper method and asserting the specific different aspects. For example,
setting up a blocked thread takes a lot of fixture code, and it would be
tempting to check different aspects of a blocked thread in a single example.
Instead, this principle can be honored by keeping the fixture code in method
ThreadSpecs.status_of_blocked_thread in core/thread/fixtures/classes.rb, and
with code like this in core/thread/status_spec.rb:

{% highlight ruby linenos %}
describe "Thread#status" do
  it "describes a blocked thread" do
    ThreadSpecs.status_of_blocked_thread.status.should == 'sleep'
  end
end
{% endhighlight %}

and the following in core/thread/inspect_spec.rb:

{% highlight ruby linenos %}
describe "Thread#inspect" do
  it "describes a blocked thread" do
    ThreadSpecs.status_of_blocked_thread.inspect.should include('sleep')
  end
end
{% endhighlight %}

Cases where it is OK to break this rule is when the functionality is
expressable as a table for different values of the argument. For example, the
following in language/regexp_spec.rb. Each "should" expresses the same theme,
just different specific data-points. Breaking this up into individual
examples would obscure the larger picture, ie. the "table".

{% highlight ruby linenos %}
it 'supports escape characters' do
  /\t/.match("\t").to_a.should == ["\t"] # horizontal tab
  /\v/.match("\v").to_a.should == ["\v"] # vertical tab
  /\n/.match("\n").to_a.should == ["\n"] # newline
  //.match("").to_a.should == [""] # return
  /\f/.match("\f").to_a.should == ["\f"] # form feed
  /\a/.match("\a").to_a.should == ["\a"] # bell
  /\e/.match("\e").to_a.should == ["\e"] # escape
end
{% endhighlight %}

#### Detail level of tests

Each method can be viewed as a function with a domain and image. The domain
can typically be partitioned into equivalence classes. Specs should be
written for a representative element from each equivalence class and all
boundary conditions.

### 1. Core and Standard Library

The specs for the Ruby core and standard libraries use one `describe` block
per method. For particularly complex methods, such as Array#[], more than one
`describe` block may be used according to the nature of arguments the method
takes.

The `describe` string should be "Constant.method" for class methods and
"Constant#method" for instance methods. "Constant" is either a class or
module name. For subclasses or submodules, the "Constant" name should be
"Super::Sub". The `describe` string should not include arguments to the
methods unless absolutely necessary to describe the behavior of the method.
Keep in mind that in Ruby duck-typing is a deeply embedded concept. Many
methods will take any object that responds to a particular method or acts
like an instance of a particular class.

Nested `describe` blocks should be used only when absolutely necessary to
make the specs easier to understand. Various automated process scripts depend
on the `describe` string having the format explained above. Consequently,
when using nested `describe` blocks, ensure that the first block begins with
the method name.

{% highlight ruby linenos %}
# This is correct
describe "String#eql?" do
  it "returns true if other has the same length and content" do
    ...
  end
end

describe "Array#[]= with [index, count]" do
  it "returns non-array value if non-array value assigned" do
    ...
  end
end
{% endhighlight %}

Contrast the _good_ example above with the one below. The following example
deviates from the conventions for `describe` strings and uses "should" and
single-quotes for the descriptions.

{% highlight ruby linenos %}
# This is NOT correct
describe "String#eql?(string)" do
  it 'should return true if other has the same length and content' do
    ...
  end
end

describe 'Array#[]=(index, count)' do
  it 'returns non-array value if non-array value assigned' do
    ...
  end
end
{% endhighlight %}

The vast majority of the spec files for the core library have already been
created. To create template files for the standard library classes, refer to
the "mkspec":/wiki/mspec/Mkspec documentation.

#### 1.1 Utility Classes

Many spec code examples refer to a particular class. To prevent name clashes
with these different class definitions across all the specs, the classes
should be scoped to a module. The convention is as follows:

{% highlight ruby linenos %}
module ObjectSpecs
  class SomeClass
  end
end
{% endhighlight %}

The module is named after the class for which the specs are being written.
So, for the specs for _Object_, the module name is ObjectSpecs.

These utility classes are also referred to as _fixtures_. In the directory
for each class, there is also a `fixtures` directory. Refer to the existing
files for examples.

#### 1.2 Aliased or Identical Methods

Ruby has a significant number of aliased methods. True aliases are identical
methods, so the specs should be exactly the same for each aliased method. The
following illustrates the convention for specs for aliased methods (or just
otherwise identical interfaces.)

In `rubyspec/core/array/shared/collect.rb`

{% highlight ruby linenos %}
    describe :array_collect, :shared => true do
      it "returns a copy of array with each element replaced by the value returned by block" do
        a = ['a', 'b', 'c', 'd']
        b = a.send(@method) { |i| i + '!' }
        b.should == ["a!", "b!", "c!", "d!"]
        b.object_id.should_not == a.object_id
      end

      it "does not return subclass instance" do
        ArraySpecs::MyArray[1, 2, 3].send(@method) { |x| x + 1 }.class.should == Array
      end
    end
{% endhighlight %}

In `rubyspec/core/array/collect_spec.rb`

{% highlight ruby linenos %}
require File.dirname(__FILE__) + '/../../spec_helper'
require File.dirname(__FILE__) + '/shared/collect.rb'

describe "Array#collect" do
  it_behaves_like :array_collect, :collect
end
{% endhighlight %}

In `rubyspec/core/array/map_spec.rb`

{% highlight ruby linenos %}
require File.dirname(__FILE__) + '/../../spec_helper'
require File.dirname(__FILE__) + '/shared/collect.rb'

describe "Array#map" do
  it_behaves_like :array_collect, :map
end
{% endhighlight %}

#### 1.3 Floating Point Values

Writing specs that use floating point values poses a problem because two
values that look the same when rendered to a string may not actually be
bitwise equal. Also, floating point operations can result in a value that
differs based on the way the FPU carried out the operations.

Specs that compare floating point values should use `#should_be_close` with
the TOLERANCE constant. For floating point values that are exact, but larger
than the precision formatted with #to_s (e.g. 1093840198347109283720.00), use
the expanded float literal not the truncated precision format that #to_s
provides (e.g. don't use 1.09384019834711e+21).

#### 1.4 Private methods

Generally, no specs are written for private methods. A notable exeception are
the specs for `#initialize` on some classes. These specs are primarily written
to illustrate the behavior of `#initialize` for subclasses, where the subclass
`#initialize` behavior is contrasted with the superclass's. Another exeception
is `#initialize_copy`.

#### 1.5 Ruby Ducktyping Interface

Ruby method dispatch behavior calls `#method_missing` if an instance has no
method corresponding to a particular selector.  Ruby also defines a number of
methods, for example, `#to_ary`, `#to_int`, `#to_str`, that form an interface
to Ruby's ducktyping behavior. String methods, for instance, may call
`#to_str` when passed an argument that is not a String.

The point of the RubySpecs is to describe behavior in such a way that if two
different implementations pass a spec, Ruby code that relies on behavior
described by the spec will execute with the same result on either
implementation.

If a spec asserts that a method calls `#to_int` on an object, it is immaterial
to the final outcome whether an implementation calls `#to_int` and handles the
possibility that the method is missing in some way, or first calls
`#respond_to?(:to_int)` and then calls `#to_int`. There are only two
significant aspects to this from the perspective of user code (i.e. code using
the interface, not the Ruby implementation code): 1) `#to_int` is called and
performs some action; or 2) `#to_int` is not called.

It is conceivable that user code like the following exists:

{% highlight ruby linenos %}
class Silly
  def method_missing(sym, *args)
    return 1 if sym == :to_int
  end
end
{% endhighlight %}

In such case, the behavior of the following code would be different:

{% highlight ruby linenos %}
# The implementation calls #to_int without checking #respond_to?
[1, 2].at(silly) # => 2

# The implementation calls #respond_to? first
[1, 2].at(silly) # => TypeError
{% endhighlight %}

In the second case, the expected behavior is restored if the Silly class is
modified to implement a `#respond_to?(:to_int)`.

The point is that it really is not sensible to implement an object that
provides an interface but does not let the world know about it by either 1)
defining the method properly, or 2) defining `#respond_to?` to
indicate that the object provides the interface.

If real-world code exists that _depends_ on this silly implementation (i.e.
cannot be coded in a more realistic way), then we can revisit the utility of
specs that require `#respond_to?` to be called. Otherwise, these specs are too
tied to the implementation and impose an unrealistic burden on implementations
that may exhibit perfectly compatible behavior but not call `#respond_to?`.

### 2. Language

For the language specs, there is nothing as convenient or as concrete as a
particular method to spec. Review the discussion of the
[organization](/organization/) of the language specs. The general
conventions apply here: use simple English to describe the behavior of the
_language entities_ and only add detail as needed. Use a single `describe`
block initially and add distinguishing `describe` blocks as necessary. Use
`it` rather than `specify` whenever possible.

