---
layout: page
---

## mspec

The spec runner scripts are separated into two main parts. The first part is in `bin/mspec`. This script gathers all options needed to invoke the actual program that will execute the specs. The following options are available:

<pre>
$ bin/mspec -h
mspec [COMMAND] [options] (FILE|DIRECTORY|GLOB)+

  The mspec command sets up and invokes the sub-commands
  (see below) to enable, for instance, running the specs
  with different implementations such as ruby, jruby, rbx, etc.

   -B, --config FILE          Load FILE containing configuration options
   -t, --target TARGET        Implementation to run the specs, where:

     r or ruby     invokes ruby in PATH
     r19 or ruby19 invokes ruby19 in PATH
     x or rubinius invokes ./bin/rbx
     X or rbx      invokes rbx in PATH
     j or jruby    invokes jruby in PATH
     i or ironruby invokes ir in PATH

   -T, --target-opt OPT       Pass OPT as a flag to the target implementation
   -I, --include DIR          Pass DIR through as the -I option to the target
   -r, --require LIBRARY      Pass LIBRARY through as the -r option to the target
   -D, --gdb                  Run under gdb
   -A, --valgrind             Run under valgrind
       --warnings             Don't supress warnings
   -j, --multi                Run multiple (possibly parallel) subprocesses
   -v, --version              Show version
   -h, --help                 Show this message

  where COMMAND is one of:

    run - Run the specified specs (default)
    ci  - Run the known good specs
    tag - Add or remove tags

  mspec COMMAND -h for more options
</pre>


<code>-B, --config FILE</code>

Load FILE containing configuration options. Refer to the [configuration]({{ site.baseurl }}/configuration/) document.

<code>-t, --target TARGET</code>

Selects the Ruby implementation to be used to execute the spec files.

<code>-T, --targetopt OPT</code>

Passes OPT as a flag to the target implementation, For example, <code>bin/mspec -T "-d" -t r</code> would pass the debug flag to ruby.

<code>-I, --include DIR</code>

Passes this option directly to the selected target (see <code>-t</code>).

<code>-r, --require LIBRARY</code>

Passes this option directly to the selected target (see <code>-t</code>).

<code>-n, --name RUBY_NAME</code>

Overrides the name used to determine the Ruby implementation. RUBY_NAME defaults to <code>require 'rbconfig'; Config::CONFIG["RUBY_INSTALL_NAME"]</code> if this executes without exception.

<code>-X, --tags-dir DIR</code>

Overrides the directory prefix used to search for the tag files that can be associated with each spec file. The default directory is `spec/tags` in the current working directory.

<code>-D, --gdb</code>

Only useful with Rubinius. Passes the --gdb argument to shotgun to launch gdb.

<code>-A, --valgrind</code>

Only useful with Rubinius on a platform with Valgrind installed. Passes the --valgrind option to shotgun to run under Valgrind.

<code>-w, --warnings</code>

Don't suppress warnings from the target (see <code>-t</code>).

