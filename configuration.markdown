---
layout: page
---

## Configuration

MSpec provides a simple way to specify configuration options for the runner
scripts by putting them in a file rather than using command line switches.
The config files are simply Ruby files. There may be zero or more config
files. The runner scripts attempt to load several config files by default.
Additional config files can be specified with the <code>-B, --config
FILE</code> command line option.

The runner scripts attempt to load the following config files in order by
looking in '.' and 'spec' directories:

1. <code>default.mspec</code>
1. depending on RUBY_VERSION, either <code>ruby.1.8.mspec</code> or <code>ruby.1.9.mspec</code>
1. <code>~/.mspecrc</code>

Config files loaded later will override settings in config files loaded
earlier. All the default config files are loaded before command line options
are processed, so command line options will override config file options.
Config files loaded with the <code>-B</code> option can override any options
set before the file is loaded. Command line options are parsed left-to-right.

A config file has the following structure:

<pre><code>
class MSpecScript
  # An ordered list of the directories containing specs to run
  # as the CI process.
  set :ci_files, [
    'spec/ruby/core',
    'spec/ruby/language',
    'spec/core',
    'spec/compiler',
    '^spec/compiler/new_compiler_spec.rb',

    # These additional directories will be enabled as the
    # specs in them are updated for the C++ VM.
    # 'spec/debugger',
    # 'spec/subtend',
    # 'spec/parser',
  ]

  # The set of substitutions to transform a spec filename
  # into a tag filename.
  set :tags_patterns, [
                        [%r(spec/ruby/), 'spec/frozen/'],
                        [%r(spec/), 'spec/tags/'],
                        [/_spec.rb$/, '_tags.txt']
                      ]

  # The default implementation to run the specs.
  set :target, 'bin/rbx'
end
</code></pre>

The <code>MSpecScript</code> class defines the <code>set</code> class method.
The config file simply opens the <code>MSpecScript</code> class and sets
various options. Since the file is simply Ruby code, any valid Ruby code can
be used.

### Configuration Options

There are a variety of config options. These are listed below with their
function and default values.

#### Loading configuration files

<code>:path = [".", "spec"]</code>

The directories to search when loading config files.

<code>:config_ext = ".mspec"</code>

The extension to append to a config file name if the bare name is not found.
This enables specifying, for example, <code>-B full</code> for loading
<code>full.mspec</code>

#### Invoking the runner scripts

The <code>mspec</code> script does not run the specs directly. Instead, it
processes options and invokes a runner script. These config options provide
control over how the runner script is invoked.

<code>:target = ENV["RUBY"] || "ruby"</code>

The implementation to execute the runner script. Set with the <code>-t,
--target TARGET</code> option.

<code>:flags = []</code>

Options to pass to the implementation executing the runner script. Set with
the <code>-T, --target-opt OPT</code> option. (Compare with
<code>:options</code> below.)

<code>:command = "run"</code>

The runner script to invoke, where <code>run</code> runs the specs,
<code>ci</code> runs the CI set, and <code>tag</code> adds or removes tags.

<code>:options = []</code>

Options that are passed to the runner script itself. This is used internally
by the <code>mspec</code> front script.

<code>:requires = []</code>

Libraries for the implementation executing the runner script to require
before loading the runner script. Set with the <code>-r, --require
LIBRARY</code> option.

<code>:abort = true</code>

When <code>true</code>, registers a signal handler for SIGINT that exits immediately.

#### Selecting the tag set

<code>:tags_patterns = []</code>

A set of transformations applied in sequence to transform the name of a spec
file to the name of the corresponding tag file. (See the config file example
above.)

#### Selecting the set of specs to run by filters

<code>:includes = []</code>

The set of strings to match for specs that will be run. Each string is
converted into an _escaped_ Regexp. Set with the <code>-e, --example
STR</code> option.

<code>:excludes = []</code>

The set of strings to match for specs that will NOT be run. Set with the
<code>-E, --exclude STR</code> option.

<code>:patterns = []</code>

The set of patterns to match for specs that will be run. Each pattern is
converted into an _unescaped_ Regexp so valid Regexp characters are accepted.
Set with the <code>-p, --pattern PATTERN</code> option.

<code>:xpatterns = []</code>

The set of patterns to match for specs that will NOT be run. Set with the
<code>-P, --excl-pattern PATTERN</code> option.

<code>:tags = []</code>

The set of tags to match for specs that will be run. Set with the <code>-g,
--tag TAG</code> option.

<code>:xtags = []</code>

The set of tags to match for specs that will NOT be run. Set with the
<code>-G, --excl-tag TAG</code> option.

<code>:profiles = []</code>

Profiles are YAML files that specify sets of methods for which to run specs.

An example profile for the <code>Mutex</code> class follows:

<pre>
Mutex#:
- lock
- locked?
- synchronize
- unlock
Mutex_m.:
- define_aliases
- extend_object
Mutex_m#:
- mu_extended
- mu_initialize
</pre>

This config item is the set of profiles for specs that will be run. Set with
the <code>-w, --profile FILE</code> option.

<code>:xprofiles = []</code>

The set of profiles for specs that will NOT be run. Set with the <code>-W,
--excl-profile FILE</code> option.

#### Selecting the set of specs to run by explicit lists of files

<code>:files = []</code>

Specifies a list of files for <code>mspec-run</code> to process if no files
are given on the command line.

The entries in the list may be:

1. a simple file name
1. a directory name, in which case the directory is recursively searched for spec files (i.e. dir/**/*_spec.rb)
1. a glob pattern understood by <code>Dir[]</code>.

Entries can be omitted from the list by putting '^' as the first character.
The entries are processed in order, so an exclude entry (one with '^') will
only remove items from the list if they are already in the list. Note that
the exclusion facility works in the mspec runners for paths and files
specified on the command line as well.

<code>:ci_files = []</code>

Specifies a list of files for <code>mspec-ci</code> no process if no files
are given on the command line. Follows the same rules as <code>:files</code>
above.

#### Formatting runner results

<code>:formatter = nil</code>

The method of displaying the results of running the specs. When
<code>nil</code>, the DottedFormatter is used if the number of files to run
is less than 50. Otherwise, the FileFormatter is used. Set to
<code>false</code> to not use any formatter. This is used internally, for
example, by <code>mspec-tag</code> to list specs that have been tagged. Set
with the <code>-f, --format FORMAT</code> option.

#### Tagging specs

The <code>mspec-tag</code> adds or removes tags for specs.

<code>:tagger = :add</code>

The default action is to add tags. The action is set with the <code>-N, --add
TAG</code> and <code>-R, --del TAG</code> options.

<code>:tag = "fails"</code>

The default tag to add. Set with the <code>TAG</code> argument to the
<code>--add</code> or <code>--del</code> options.

<code>:outcome = :fail</code>

The result of running a spec that triggers the specified tag action. The
default is when the spec fails. Set with the <code>-Q, --pass</code>,
<code>-F, --fail</code>, and <code>-L, --all</code> options.

<code>:ltags = []</code>

The set of tags for which to list tagged specs. Set with the <code>--list TAG</code> option.

#### Triggering actions

While the specs are being run, certain actions can be invoked when a matching
spec is encountered. (See e.g. the <code>--spec-debug</code> and
<code>--spec-gdb</code> options for <code>mspec-run</code>.)

<code>:atags = []</code>

The set of tags for which to invoke the specified action. Set with the
<code>-K, --action-tag TAG</code> option.

<code>:astrings = []</code>

The set of strings for matching specs for which to invoke the specified
action. Set with the <code>-S, --action-string STR</code> option.

