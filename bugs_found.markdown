---
layout: page
---

## How to deal with bugs found in MRI while writing specs

It is often hard to know whether a particular behavior in MRI is a bug or just
surprising or inconsistent behavior. If the issue is a segfault, then it is
certainly a bug. If the issue is an infinite loop or threading deadlock, it is
also certainly a bug. Otherwise, it may or may not be a bug. That's up to Matz
to decide.

When encountering a bug in MRI, use the following steps:

1. Search the [Redmine MRI bug tracker](https://bugs.ruby-lang.org/projects/ruby-trunk/issues)
   for the same or similar behavior.
1. If there is an existing ticket for the issue, use the ticket number in a
   `ruby_bug` guard.
1. If there is not an existing ticket, file one and use the ticket number in
   the `ruby_bug` guard.
