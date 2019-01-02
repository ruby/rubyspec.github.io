---
layout: page
---

## mspec-tag

There is a separate script for adding and removing tags for spec descriptions.
The <code>f, e, E, p, P, g, G, V, m</code> options are the same as for
`mspec-run` above.

Note: The runners make a distinction between specifying filters for which
specs will be executed (e.g. <code>f, e, E, p, P, g, G</code>) and which specs
will trigger actions. The `mspec-tag` script registers the TagAction to add or
delete tags based on the outcome of executing a spec (e.g. pass, fail, or
all). The TagAction is different than, for example DebugAction, in that
TagAction will match any spec by default. You can narrow the matches by using
the <code>K or S</code> options. In this way, you can run a whole set of
specs, but only tag a select few depending on what you pass with <code>K or
S</code>.

For example, use the following command to tag any Array#append specs that fail
with the tag 'fails'. Since `--add fails` is the default action and tag, the
following commands are equivalent:

<pre>
mspec tag spec/ruby/core/array/append_spec.rb
mspec tag --add fails spec/ruby/core/array/append_spec.rb
</pre>

To remove tags for specs that now pass, use the following command. Since `--pass` is the default outcome, the following commands are also equivalent:

<pre>
mspec tag --del fails spec/ruby/core/array/append_spec.rb
mspec tag --del fails --pass spec/ruby/core/array/append_spec.rb
</pre>

Please run `mspec tag -h` for the detailed help output.
