---
layout: page
---

## Guards

### Overview

The RubySpec project intends to provide a complete specification of the Ruby
language and its libraries. There is a single _standard_ implementation of
Ruby. The _standard_ includes the stable, released versions available from
<http://ruby-lang.org>. At present, the _standard_ is 1.8.6, 1.8.7, and 1.9.2.
Collectively, the _standard_ is often referred to as MatzRuby or MRI.

The challenge for RubySpec is to correctly spec the different behaviors
across versions, platforms, and implementations. To do this, the RubySpecs
depend on _guards_ provided by MSpec. Guards are methods that may or may not
take arguments and operate by yielding to a block if certain conditions are
true. If the conditions for the guard are not true, the guard method does not
yield to the block and the specs contained in the block are not run.

The guards serve two functions: 1) controlling which specs are run; 2)
documenting the specs. The documentation function of the guards is as
important for RubySpec as controlling which specs are run. Additionally, the
guard structure itself was chosen to be visually and conceptually similar to
the <code>describe/it</code> blocks in the specs.

The guard blocks should be placed around `describe` or `it` blocks. Guards
should _not_ be placed inside `it` blocks. The `it` description string should
concisely describe the facet of behavior illustrated by the example code. If
a guard is present, it necessarily means the guarded code behaves
differently. That difference should be described in the `it` string. In the
case of `before` or `after` actions, use judgment to keep the specs concise
but clear. If clarity would be enhanced by a few more lines of code, put the
guards around the `before` or `after` action itself. Clarity always trumps
counts of lines of code.

There are five categories of guards:

1. versions
1. platforms
1. bugs
1. implementations
1. environments

The specific guards in each of these categories are explained below.

### 1. Versions

Different versions of Ruby have some methods that behave differently. That
sounds circular, but it is the essence of software versions. To handle
different version behaviors, MSpec provides the <code>ruby_version_is</code>
guard.

The <code>ruby_version_is</code> guard has two forms. One form gives a single
version. Any implementation whose version number is greater than or equal to
the version in the guard will run the guarded specs. The second form takes a
range of versions. Any implementation whose version number is in the range
will run the guarded specs. See the examples below and note the range form of
the guard may include or exclude the ending version.

{% highlight ruby linenos %}
# Yields block if version >= 1.8.6-p114.
ruby_version_is "1.8.6.114" do
  it "returns true" do
  end
end

# Yields block if version is in the 1.8 series, except 1.8.7 and beyond.
ruby_version_is "1.8" .. "1.8.6" do
  it "returns nil" do
  end
end

# Yields block if version < 1.9.
ruby_version_is "" ... "1.9" do
  it "returns false" do
  end
end
{% endhighlight %}

The <code>ruby_version_is</code> guard takes one argument that may be a
string or a range of two strings. The format of the string is A.B.C.D, where:

* A is the major version
* B is the minor version
* C is the tiny (teeny) version
* D is the patchlevel

The string is converted to a number on which comparisons between versions can
be made so that, for instance, "1.8.6.37" is less than "1.8.6.112".

The range behaves as expected, respecting <code>#exclude_end?</code>. In the
example above, "" ... "1.9" means every version *before* 1.9.

### 2. Platforms

A single version of Ruby may have different behaviors depending on the
platform on which it runs. MSpec provides several guards for these
situations:

* big_endian
* little_endian
* platform_is
* platform_is_not

The `big_endian` guard yields to the block if the platform is big endian.
Likewise for the `little_endian` guard.

The <code>platform_is</code> and <code>platform_is_not</code> guards are more
complex. As their names suggest, they are inverses.

{% highlight ruby linenos %}
platform_is :linux, :bsd do
  it "opens the file" do
  end
end
{% endhighlight %}

The guard above will yield if RUBY_PLATFORM matches either "linux" OR "bsd".

{% highlight ruby linenos %}
platform_is :linux, :wordsize => 32 do
  it "opens the file" do
  end
end
{% endhighlight %}

The guard above will yield if RUBY_PLATFORM matches "linux" AND the processor
word size is 32-bit.

{% highlight ruby linenos %}
platform_is_not :windows, :wordsize => 32 do
  it "opens the file" do
  end
end
{% endhighlight %}

The guard above will yield if RUBY_PLATFORM does not matches "windows" AND
the processor word size is not 32-bit.

Special functionality exists for matching :windows and :java as platforms.
For details, refer to the MSpec source.

{% highlight ruby linenos %}
platform_is :os => [:darwin, :bsd] do
  it "opens the file" do
  end
end
{% endhighlight %}

The guard above will yield if <code>Config::CONFIG['host_os']</code> matches
either "darwin" OR "BSD". If <code>Config::CONFIG['host_os']</code> is not
set in rbconfig, the :os parameters will be matched against RUBY_PLATFORM
instead.

### 3. Bugs

Sometimes a bug is discovered in the _standard_. In this case, we do two things:

1. File a ticket on the "bug tracker":http://bugs.ruby-lang.org/ to find
   out if the suspected behavior is actually considered a bug.
1. Add a `ruby_bug` guard that wraps the spec showing what is considered to be
   the _correct_ behavior.

{% highlight ruby linenos %}
ruby_bug "#5555", "1.8.6.114" do
  it "returns the sum" do
    (1 + 1).should == 2
  end
end
{% endhighlight %}

The above guard will NOT yield to the block on any version of the _standard_
less than or equal to 1.8.6 patchlevel 114.

The `ruby_bug` guard serves three purposes:

1. It documents that there is a bug in a particular version of the standard
   and refers to the ticket for that bug.
1. It provides the correct behavior description for all implementations
1. It prevents the spec for the correct behavior from running (and failing) on
   versions of the _standard_ implementation that have the bug and have
   already been released.

If it is determined that the behavior is not a bug but rather a version
difference, the `ruby_bug` guard should be replaced by an appropriate
<code>ruby_version_is</code> guard.

### 4. Implementations

Ideally, all Ruby implementations would have exactly the same behaviors. This
is not realistically possible given differences in the underlying technology,
for instance, whether the process `fork` facility is available.

It should be obvious, but it bears repeating, that in a specification for a
standard, there should be an absolute minimum of incompatible behavior. These
guards are not provided to make it easy to be inconsistent. Rather, they are
provided to make specifying a single standard as simple as possible,
recognizing that 100% conformity is an impossible ideal.

Recall that the purpose of the guards are _both_ to control which specs are
run AND document the specs. There are four distinct situations covered by the
five guards below. Each of the guards documents this difference.

* compliant_on
* not_compliant_on
* not_supported_on
* deviates_on
* extended_on

Keep in mind that the arguments to these guards are communal property (as are
all the specs) and respect them as you would want to be respected. There is
no concept of opt-in or opt-out here. Every implementation is responsible for
ensuring that their implementation's behavior is accurately represented in
one of these compliance scenarios. If an implementation has an excessive
number of non-compliant behaviors, this will be clearly visible in the specs.

#### 4.1 compliant_on / not_compliant_on

The `compliant_on` and <code>not_compliant_on</code> guards are inverses.
They document that the enclosed spec passes or does not pass on the listed
implementations or platforms.

{% highlight ruby linenos %}
compliant_on :jruby, :rubinius do
  it "returns true" do
  end
end
{% endhighlight %}

The spec above will run ONLY on the _standard_, JRuby, or Rubinius. The
`compliant_on` guard will not run on any other implementation or platform
except the ones listed.

Note that this guard could prevent the guarded spec from running on an
implementation that passes the spec. It is important to only use it in cases
where the behavior is clearly supported only on the listed implementations.
Generally, it is better to use <code>not_compliant_on</code> to explicitly
blacklist implementations.

There is no need to list :ruby for this guard. There is only one standard and
specs are _always_ expected to run correctly on the _standard_. If there is
version-specific behavior or a bug in the _standard_, use the
<code>ruby_version_is</code> or `ruby_bug` guard instead.

{% highlight ruby linenos %}
not_compliant_on :rubinius do
  it "returns false" do
  end
end
{% endhighlight %}

The spec above will run on EVERY implementation except Rubinius. The
<code>not_compliant_on</code> guard will always run on the _standard_.

It should be obvious that the `compliant_on` guard is more convenient when
the number of implementations that conform to the standard is small compared
to the total number of implementations. The <code>not_compliant_on</code>
guard is useful when the number of non-conforming implementations is small.
Another way to look at this, `compliant_on` essentially whitelists the
compliant implementations, while <code>not_compliant_on</code> blacklists the
non-comforming implementations.

It is reasonable to assume that if there is a <code>not_compliant_on</code>
guard for a particular implementation, then there is also a `deviates_on` or
`extended_on` guarded spec for the same facet of behavior. This is not
enforced in any way by the system of guards. It makes sense that if a facet
of behavior is not consistent with the _standard_, then that can be
illustrated by another spec. This is not always the case. For instance, the
_standard_ may call a particular method on an instance that an alternative
implementation has no need to call. The alternative implementation may behave
the same in every aspect except calling the method. In such case, the
<code>not_compliant_on</code> guard on that one facet of behavior is
sufficient. It does not enhance the specs to add a `deviates_on` spec that
merely creates a mock <code>should_not_receive</code> expectation. This
obviously is in the gray line between implementation and interface.

#### 4.2 not_supported_on

The <code>not_supported_on</code> guard documents that there is no behavior
like that described in the guarded spec for the listed implementations.

{% highlight ruby linenos %}
not_supported_on :jruby do
  it "forks the process" do
  end
end
{% endhighlight %}

The above spec will run on EVERY implementation _except_ JRuby. In
particular, it will always run an the _standard_. Further, it will raise an
exception if passed :ruby.

The key difference between <code>not_supported_on</code> and the compliance
guards above is that <code>not_supported_on</code> offers no alternative to
the _standard_ behavior.

#### 4.3 deviates_on

If an implementation does not conform to the _standard_ behavior but instead
offers an alternative behavior, the spec illustrating that is wrapped in a
`deviates_on` guard.

{% highlight ruby linenos %}
deviates_on :rubinius do
  it "coerces to two Bignums" do
  end
end
{% endhighlight %}

The above spec will run ONLY on Rubinius. More than one implementation can be
listed. This guard will NEVER run on the _standard_ and will raise an
exception if passed :ruby.

#### 4.4 extended_on

If an implementation offers a behavior that does not exist at all in the
standard, the spec illustrating that behavior is wrapped in an `extended_on`
guard.

{% highlight ruby linenos %}
extended_on :rubinius do
  it "returns an immutable vector" do
  end
end
{% endhighlight %}

The above spec will run ONLY on Rubinius. More than one implementation can be
listed. This guard will NEVER run on the _standard_ and will raise an
exception if passed :ruby.

### 5. Environments

The following guards are broadly grouped by their relation to how the specs
are run. This is referred to as the spec _environment_. It includes guards
for which runner (e.g. RSpec or MSpec) is executing the specs, which classes
  are loaded when the specs run, and whether the process running the specs
  has superuser privileges.

* runner_is
* runner_is_not
* conflicts_with
* as_superuser
* quarantine!

#### 5.1 runner_is / runner_is_not

These two guards were initially added to protect specs whose behavior would
change in the presence of certain standard library classes like Rational.
This situation has been taken over by the `conflicts_with` guard below. These
guards should now only be used if the particular runner framework itself is
an issue.

{% highlight ruby linenos %}
runner_is :rspec do
  it "does something that only runs under RSpec" do
  end
end

runner_is :mspec do
  it "does something that only runs under MSpec" do
  end
end
{% endhighlight %}


#### 5.2 conflicts_with

This guard wraps specs for methods whose behavior may be changed incompatibly
by certain other classes.

{% highlight ruby linenos %}
conflicts_with :SomeClass do
  it "returns an Integer" do
  end
end
{% endhighlight %}

The above guard will NOT yield if SomeClass is defined.

#### 5.3 as_superuser

Some Ruby methods will only behave as expected if the process running the
code example has superuser privileges.

{% highlight ruby linenos %}
as_superuser do
  describe "File.chown" do
  end
end
{% endhighlight %}

The guard above will only yield if <code>Process.euid == 0</code>.

#### 5.4 quarantine!

The <code>quarantine!</code> guard will never yield to the block, so the
specs inside the guard will not run. This guard is only used to temporarily
disable a guard that is causing the _standard_ to fail severely (for example,
by causing a segfault) AND the correctness of the spec is suspect. The guard
allows the spec to be investigated but not cause any failures.

If a spec exposes a bug that is causing a segfault, the `ruby_bug` guard
should be used.

{% highlight ruby linenos %}
quarantine! do
  it "does something that causes a segfault" do
  end
end
{% endhighlight %}
