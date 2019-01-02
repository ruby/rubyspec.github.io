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
tagged as failing. The `mspec ci` command accepts a file, directory, or
shell glob to specify which spec files to execute. Invoking the command
without any files will cause the default set of files specified by the
configuration file to be run (See the
[configuration]({{ site.baseurl }}/configuration/) document).

Please run `mspec run -h` to list the available options.
