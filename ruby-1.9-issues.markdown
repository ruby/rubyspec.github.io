---
layout: page
---

## Ruby 1.9 Issues

There are two main problems running the specs on Ruby 1.9:

1. Ruby 1.9 changes language features and many core library methods.
1. Potential bugs in Ruby 1.9

The RubySpecs were initially written for Ruby 1.8. Many of the specs work fine with 1.9. However, many of the specs fail on 1.9 because the specs need to be updated. To facilitate the process of updating the specs for 1.9, tags for failures have been added to the RubySpec source.

MSpec provides a special runner script that will read the tags and NOT run specs that fail. You can also exclude tagged specs with the default runner script.

Specs are tagged with 'fails' and 'critical'. Specs tagged as critical are specs that either cause hangs or segfaults.

To run all the specs but exclude failing ones:

<pre>
$ cd rubyspec
$ mspec ci -tr19
</pre>

To run specs but exclude a particular tag:

<pre>
$ mspec -tr19 -G critical
$ mspec -tr19 -G fails
$ mspec -tr19 -G critical -G fails
</pre>

To run only the specs _with a particular tag_:

<pre>
$ mspec -tr19 -g critical
$ mspec -tr19 -g fails core/array
</pre>

### Listing tagged specs

To see the descriptions of all the specs tagged:

<pre>
$ mspec tag --list-all
$ mspec tag --list critical
$ mspec tag --list fails
</pre>

### Removing tags

Once the specs have been updated, or bugs fixed in Ruby 1.9, the tags can be removed.

<pre>
$ mspec tag --del fails core/array/append_spec.rb
</pre>

When removing tags, it is usually best to give the specific file to process.

### Stable version of 1.9

RubySpec does not attempt to spec any version of 1.9 prior to the 1.9.2p0
stable release. In `ruby_version_is` guards, use "1.9" if the behavior is 1.9
specific and has not changed in the stable release of 1.9.2. If 1.9.3 behavior
differs from 1.9.2 stable, then use the appropriate versions in the guards.

{% highlight ruby linenos %}
ruby_version_is "" ... "1.9" do
  it "does something on versions prior to 1.9" do
    # ...
  end
end

ruby_version_is "1.9" do
  it "does something on 1.9" do
    # ...
  end
end
{% endhighlight %}

### Using ruby_bug guards for 1.9

RubySpec only specifies the behavior of version 1.9 from the stable release at
1.9.2 patchlevel 0. The `ruby_bug` guard should not be used for any version of
1.9 prior to 1.9.2p0.

