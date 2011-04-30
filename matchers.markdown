---
layout: page
---

## Matchers

(TODO: Update these with all the current matchers and document them.)

* = = (expected)
** (1+2).should = = 3
** [1,2].size.should_not = = 0
* <(expected)
* <=(expected)
* >(expected)
* >=(expected)
* =~(expected)
* be_ancestor_of(expected)
* be_close(expected, tolerance)
* be_empty
* be_false
* be_kind_of(expected)
* be_nil
* be_true
* complain(complaint=nil)  #Regexp or String
* eql(expected)
* equal(expected)
* equal_element(element, attributes = nil, content = nil, options = {})
* equal_utf16(expected)
* include(*expected)       #Note: not 'include?' but 'include'
* match_yaml(expected)
* output(stdout=nil, stderr=nil)
* output_to_fd(what, where = STDOUT)
* raise_error(exception=Exception, message=nil, &block)

