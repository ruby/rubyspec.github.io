---
layout: page
---

# Creating files with mkspec

 The [general organization](/wiki/rubyspec/Organization) of the specs is one directory per class/module, one subdirectory per sub-class/module, and one file per method name. For example, for the Kernel module, there is the `rubyspec/core/kernel` directory, and there is the `gsub_spec.rb` that contains specs for Kernel#gsub and Kernel.gsub (i.e. instance method and module method).

 The `mkspec` script can be used to create the skeleton files for all the methods of a class or module. The script will create the directory and file if they do not exist. If the file exists, it will be executed by `mspec-run` with the `--dry-run` option, essentially parsing the specs to check if there is a spec for the method. If not, a signpost spec like the following is inserted into the file.

<pre><code>
 describe "Float#to_s" do
   it "needs to be reviewed for spec completeness" do
   end
 end
</code></pre>

 The purpose of the signpost spec is to provide visibility of specs for methods that are partially, or completely, missing. Generally, these specs are then tagged with 'incomplete', which is excluded from `mspec-ci` runs. The signpost specs should be removed once reasonably complete specs have been written for the method.

 The `mkspec` script can be run repeatedly without affecting existing files. So, if you use it to generate files but do not fill in specs for them all. you can commit only the files that have specs and re-run the script later to re-create the files for methods not yet spec'd. In general, this is preferable to checking in numerous files that only have signpost specs in them. However, there are a number of previously empty files in the repository that have been updated using the new `mkspec` functionality.

<pre>
 $ bin/mkspec -h
 mkspec [options]

     -c, --constant CONSTANT          Class or Module to generate spec stubs for
     -b, --base DIR                   Directory to generate specs into
     -r, --require LIBRARY            A library to require


  How might this work in the real world?

    1. To create spec stubs for every class or module in Object

      $ mkspec

    2. To create spec stubs for Fixnum

      $ mkspec -c Fixnum

    3. To create spec stubs for Complex in 'superspec/complex'

      $ mkspec -c Complex -rcomplex -b superspec
</pre>

### Notes

 The `mkspec` command is intended to be run by MatzRuby. The help text above gives examples of invoking `mkspec` under typical circumstances.

 Unlike the runner scripts, `mkspec` does not have a `--target` option. This are essentially two reasons for this. Firstly, the specs are for ensuring compatibility with the "standard" reference implementation (commonly called MRI, or MatzRuby). So, it is the classes and modules of that implementation that are of general interest. Secondly, having the `--target` option requires re-exec'ing the script with a different implementation. That complexity isn't really justified.

 However, if you need to create specs for an implementation-specific class or module, it's still rather easy to do. For example, to create Rubinius specs for the Mailbox class, you could use the following:

<pre>
 shotgun/rubinius mspec/bin/mkspec -b spec/library/ -r mailbox -c Mailbox
</pre>


