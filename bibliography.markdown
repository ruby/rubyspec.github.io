---
layout: page
title: The Ruby Bibliography
---

# The Ruby Bibliography

Ruby hasn't historically been the subject of many research projects. A lot of
systems research has used languages like C, C++ and Java. Contemporary
programming language research often uses languages like Java, Scala and
Haskell. Modern research into VMs and JITs is often based on Java or recently
Python.

However there are now a growing number of research projects using Ruby. In this
page we list peer-reviewed papers and articles from reputable venues, and
theses from reputable schools. There are also many [high quality technical
books and articles about Ruby](https://www.ruby-lang.org/en/documentation/).

<!-- Note: Please list subject areas alphabetically, and publications newest
  first. Please format references as abbrv. To work around Markdown parsing
  author name lists with initials as a number-bulleted list, bold the name list.
-->

## Applications

### Bioinformatics

* **Smith, R., Williamson, R., Ventura, D., & Prince, J. T.** (2013). [Rubabel: wrapping open Babel with Ruby](http://www.biomedcentral.com/content/pdf/1758-2946-5-35.pdf). Journal of Cheminformatics, 5(1), 35.
* **N. Goto, P. Prins, M. Nakao, R. Bonnal, J. Aerts, and T. Katayama.** [Bioruby: bioinformatics software for the Ruby programming language](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2951089/pdf/btq475.pdf). Bioinformatics, 26(20):2617–9, Oct 2010.

### Modelling

* **J. S. Cuadrado, J. G. Molina, and M. M. Tortosa.** [RubyTL: A practical, extensible transformation language](http://link.springer.com/chapter/10.1007/11787044_13). Model Driven Architecture - Foundations and Applications, 4066, 2006.

## Parallelism and Concurrency

* **L. Lu, W. Ji, and M. L. Scott.** Dynamic enforcement of determinism in a parallel scripting language. In Proceedings of the 35th Conference on Programming Language Design and Implementation (PLDI), 2014. (to appear)
* **R. Odaira, J. G. Castanos, and H. Tomari.** [Eliminating global interpreter locks in Ruby through hardware transactional memory](http://researcher.watson.ibm.com/researcher/files/jp-ODAIRA/PPoPP2014_RubyGILHTM.pdf). In Proceedings of the 19th Symposium on Principles and Practice of Parallel Programming (PPoPP), 2014.
* **W. Ji, L. Lu, and M. L. Scott.** [TARDIS: Task-level access race detection by intersecting sets](http://wodet.cs.washington.edu/wp-content/uploads/2013/03/wodet2013-final9.pdf). In Proceedings of the 4th Workshop on Determinism and Correctness in Parallel Programming (WoDet), 2013.

## Tooling

* **C. Seaton, M. L. Van De Vanter, and M. Haupt.** [Debugging at full speed](http://www.lifl.fr/dyla14/papers/dyla14-3-Debugging_at_Full_Speed.pdf). In Proceedings of the 8th Workshop on Dynamic Languages and Applications (DYLA), 2014.

## Type Systems

* **B. M. Ren, J. Toman, T. S. Strickland, and J. S. Foster.** [The Ruby type checker](http://www.cs.umd.edu/~jfoster/papers/oops13.pdf). In Proceedings of the 28th Symposium on Applied Computing (SAC), 2013.
* **J. An, A. Chaudhuri, J. S. Foster, and M. Hicks.** [Dynamic inference of static types for Ruby](http://www.cs.umd.edu/~jfoster/papers/popl11.pdf). In Proceedings of the 38th ACM Symposium on Principles of Programming Languages (POPL), 2011.
* **M. J. Edgar.** [Static analysis for Ruby in the presence of gradual typing](http://www.cs.dartmouth.edu/reports/TR2011-686.pdf), 2011.
* **M. Madsen, P. Sørensen, and K. Kristensen.** [Ecstatic – type inference for Ruby using the cartesian product algorithm](http://projekter.aau.dk/projekter/files/61071016/1181807983.pdf). Master’s thesis, Aalborg University, 2007.
