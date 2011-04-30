---
layout: page
---

## Current Standard

The current "standard" versions of MRI for the specs are:

* 1.8.6p420
* 1.8.7p334
* 1.9.2p180

When adding new specs, developers should ensure that the specs pass on all of
these versions, or that the specs are guarded for the relevant version. Refer
to the documentation for [guards](/guards), and in particular the `ruby_bug`
guard for how to work with potential bugs discovered in the current standard
version.
