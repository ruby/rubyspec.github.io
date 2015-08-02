---
layout: page
---

## mspec-ci

As a Ruby implementation is undergoing development, it is not likely to pass
all the specs that have been written. However, when making changes to the
code, a way is needed to confirm that working features are not inadvertently
broken. Generally, some sort of continuous integration (CI) mechanism serves
this purpose.

To meet this need, MSpec has a special runner that excludes the specs that are
tagged as failing. The bin/mspec ci command accepts a file, directory, or
shell glob to specify which spec files to execute. Invoking the command
without any files will cause the default set of files specified by the
configuration file to be run (See the
[configuration]({{ site.baseurl }}/configuration/) document). The default formatter for
bin/mspec ci is dotted output (i.e. ... for passing specs, F for failing
specs, and E for errors).

<pre>
$ bin/mspec ci -h
ruby 1.8.6 (2008-03-03 patchlevel 114) [universal-darwin9.0]
mspec ci [options] (FILE|DIRECTORY|GLOB)+

 Ask yourself:
  1. How to run the specs?
  2. How to display the output?
  3. What action to perform?
  4. When to perform it?

 How to run the specs
   -B, --config FILE          Load FILE containing configuration options
   -n, --name RUBY_NAME       Set the value of RUBY_NAME (used to determine the implementation)
   -Z, --dry-run              Invoke formatters and other actions, but don't execute the specs
       --int-spec             Control-C interupts the current spec only

 How to display their output
   -f, --format FORMAT        Formatter for reporting, where FORMAT is one of:

       s, spec, specdoc         SpecdocFormatter
       h, html,                 HtmlFormatter
       d, dot, dotted           DottedFormatter
       f, file                  FileFormatter
       u, unit, unitdiff        UnitdiffFormatter
       m, summary               SummaryFormatter
       a, *, spin               SpinnerFormatter
       t, method                MethodFormatter
       y, yaml                  YamlFormatter

   -o, --output FILE          Write formatter output to FILE
   -V, --verbose              Output the name of each file processed
   -m, --marker MARKER        Output MARKER for each file processed

 What action to perform
       --spec-debug           Invoke the debugger when a spec description matches (see -K, -S)
       --spec-gdb             Invoke Gdb when a spec description matches (see -K, -S)

 When to perform it
   -K, --action-tag TAG       Spec descriptions marked with TAG will trigger the specified action
   -S, --action-string STR    Spec descriptions matching STR will trigger the specified action

 Help!
   -v, --version              Show version
   -h, --help                 Show this message

 How might this work in the real world?

   1. To simply run the known good specs

     $ mspec ci

   2. To run a subset of the known good specs

     $ mspec ci path/to/specs

   3. To start the debugger before the spec matching 'this crashes'

     $ mspec ci --spec-debug -S 'this crashes'
</pre>

<code>-f, --format FORMAT</code>

Selects the reporter to be used for showing the output from executing the specs.

<code>-V, --verbose</code>

Prints to STDERR each spec file that is being executed.

<code>-m, --marker MARKER</code>

Output MARKER for each file processed. Overrides -V

