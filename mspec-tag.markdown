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
bin/mspec tag spec/ruby/1.8/core/array/append_spec.rb
bin/mspec tag --add fails spec/ruby/1.8/core/array/append_spec.rb
</pre>

To remove tags for specs that now pass, use the following command. Since `--pass` is the default outcome, the following commands are also equivalent:

<pre>
bin/mspec tag --del fails spec/ruby/1.8/core/array/append_spec.rb
bin/mspec tag --del fails --pass spec/ruby/1.8/core/array/append_spec.rb
</pre>

Detailed help output is available for the command.

<pre>
$ bin/mspec tag -h
ruby 1.8.6 (2008-03-03 patchlevel 114) [universal-darwin9.0]
mspec tag [options] (FILE|DIRECTORY|GLOB)+

 Ask yourself:
  1. What specs to run?
  2. How to modify the execution?
  3. How to display the output?
  4. What tag action to perform?
  5. When to perform it?

 What specs to run
   -e, --example STR          Run examples with descriptions matching STR
   -E, --exclude STR          Exclude examples with descriptions matching STR
   -p, --pattern PATTERN      Run examples with descriptions matching PATTERN
   -P, --excl-pattern PATTERN Exclude examples with descriptions matching PATTERN
   -g, --tag TAG              Run examples with descriptions matching ones tagged with TAG
   -G, --excl-tag TAG         Exclude examples with descriptions matching ones tagged with TAG
   -w, --profile FILE         Run examples for methods listed in the profile FILE
   -W, --excl-profile FILE    Exclude examples for methods listed in the profile FILE

 How to modify the execution
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

 What action to perform and when to perform it
   -N, --add TAG              Add TAG with format 'tag' or 'tag(comment)' (see -Q, -F, -L)
   -R, --del TAG              Delete TAG (see -Q, -F, -L)
   -Q, --pass                 Apply action to specs that pass (default for --del)
   -F, --fail                 Apply action to specs that fail (default for --add)
   -L, --all                  Apply action to all specs
       --list TAG             Display descriptions of any specs tagged with TAG
       --list-all             Display descriptions of any tagged specs

 Help!
   -v, --version              Show version
   -h, --help                 Show this message

 How might this work in the real world?

   1. To add the 'fails' tag to failing specs

     $ mspec tag path/to/the_file_spec.rb

   2. To remove the 'fails' tag from passing specs

     $ mspec tag --del fails path/to/the_file_spec.rb

   3. To display the descriptions for all specs tagged with 'fails'

     $ mspec tag --list fails path/to/the/specs
</pre>

<code>-K, --action-tag TAG</code>

Only consider specs tagged with TAG for adding or deleting a tag. If neither this option nor -S is given, consider any spec that is executed.

<code>-S, --action-string STR</code>

Only consider specs matching STR for adding or deleting a tag. If neither this option nor -K is given, consider any spec that is executed.

<code>-N, --add TAG</code>

Create a new tag named TAG. The format is 'tag' or 'tag(comment)', where comment is any collection of characters except these three <code>():</code>.

<code>-R, --del TAG</code>

Delete the tag TAG.

<code>-Q, --pass</code>

Only tag or delete tags for specs that pass.

<code>-F, --fail</code>

Only tag or delete tags for specs that fail. This is the default option. So, if you don't pass any of <code>Q, F, L</code> only the failing specs will be tagged.

<code>-L, --all</code>

Tag or delete tags for specs regardless of whether the spec passes or fails.

<code>--list TAG</code>

Display the descriptions for all specs tagged with TAG. You can provide multiple tags with <code>--list TAG1 --list TAG2</code>

Note that this command is not merely listing the tag files. Only the tagged specs that would actually be run are displayed. For example, specs that are excluded by guards are not display. None of the specs are actually executed.

<code>--list-all</code>

Display the descriptions for specs with any tags.

