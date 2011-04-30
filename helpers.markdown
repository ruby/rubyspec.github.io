---
layout: page
---

## MSpec Helpers

### language_version

Helper for syntax-sensitive specs. The specs should be placed in a file in
the @versions@ subdirectory. For example, suppose language/method_spec.rb
contains specs whose syntax depends on the Ruby version. In the
language/method_spec.rb use the helper as follows:

{% highlight ruby %}
  language_version __FILE__, "method"
{% endhighlight %}

Then add a file "language/versions/method_1.8.rb" for the specs that are
syntax-compatible with Ruby 1.8.x.

