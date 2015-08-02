---
layout: page
---

## Getting Started

To run the RubySpecs, you need to get MSpec and the RubySpec files.

### Getting MSpec

MSpec is the framework and scripts for running the RubySpecs. It is available
as a gem or directly from
[Github](http://github.com/rubyspec/mspec/tree/master).

<pre>
$ sudo gem install mspec
</pre>

OR

<pre>
$ git clone git://github.com/rubyspec/mspec.git
</pre>

If you are going to be working on the RubySpecs, you are _strongly_ encouraged
to always use the latest MSpec from Github.

### Getting the RubySpecs

The RubySpec source is available from
[Github](http://github.com/rubyspec/rubyspec/tree/master).

<pre>
$ git clone git://github.com/rubyspec/rubyspec.git
</pre>

If you are not working on the RubySpecs, you can checkout a particular version
with the following command:

<pre>
$ git checkout -f -b stable v0.7.3
</pre>

### Running the RubySpecs

If you cloned MSpec from Github, put the `mspec/bin` directory in your PATH.
The simplest way to run the specs is to change to the RubySpec directory and
invoke `mspec`.

<pre>
$ cd rubyspec
$ mspec
</pre>

This will run the specs with the `ruby` executable on PATH. To run the specs
with a different implementation, see the `-t` or `--target` option to mspec.
See `mspec -h` for more details. Read the [Runners]({{ site.baseurl }}/runners/) documentation
for complete details on all the runner scripts.

To run a single directory of specs, pass the path to the directory to `mspec`.
To run a single file, pass the name of the file.

<pre>
$ cd rubyspec
$ mspec core/array
$ mspec core/array/append_spec.rb
</pre>

The `ruby.1.8.mspec` and `ruby.1.9.mspec` config files also provide
pseudo-directories that will include all the appropriate files for the
version.

<pre>
$ mspec :core
$ mspec :language
</pre>

### Runner script help

To see more help for the particular runner scripts:

<pre>
$ mspec run -h
$ mspec ci -h
$ mspec tag -h
</pre>
