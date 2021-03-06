IKOS version 1.3 release notes
==============================

Release date
------------

October 2017

List of changes
---------------

### IKOS Core changes

* Added an implementation of the Gauge domain.
* Combined the memory domain and the pointer domain, improving the precision for arrays of pointers and virtual tables.
* Improved the documentation

### Analyzer changes

* Added a report generator. The analyzer can now export the results in a gcc-style format, json or csv.
* Added an analysis of unaligned pointers.
* Improved the analysis performance by keeping previous fixpoints of called functions in memory.
* Added the intrinsic function `__ikos_debug()`, that dumps the current invariant.
* Added support for analyzing variable argument functions (`va_start`, `va_arg`, `va_end` and `va_copy`).
* Added support for analyzing structures in register (`insertelement` and `extractelement` in LLVM).
* Fixed an unsoundness in the exception analysis.
* Added a `settings` and `times` table in the result database, allowing a user to query the analysis options and timing results.
* Added special checks that warn about dangerous code patterns (such as casts from integers to pointers).
* Fixed and improved the analysis of bitwise and conversion operators.
* Moved all global variable initializations into the internal function `__ikos_init_globals`, also fixing the initialization of global objects in C++.

### ARBOS changes

* Added support for C variable-length arrays (VLA).

### LLVM frontend changes

* Upgraded to LLVM 4.0.x


IKOS version 1.2 release notes
==============================

Release date
------------

October 2016

List of changes
---------------

### IKOS Core changes

* Added an abstract domain to handle C++ exceptions.

### Analyzer changes

* Implemented a proper C++ exception propagation handling using abstract interpretation. IKOS correctly handles C++ code with exceptions.
* Improved the analysis of string-related functions (such as strlen, strcpy and strcat).
* Fixed the precision of the null dereference analysis.

### ARBOS changes

* Removed AR_Unwind as it is no longer generated by LLVM since LLVM 3.1
* Added APIs to set and retrieve the "resume" basic block in an AR_Code.


IKOS version 1.1.2 release notes
================================

Release date
------------

September 2016

List of changes
---------------

### IKOS Core changes

* Added interfaces for abstract domains, nullity domains, uninitialized variables domains, pointer domains and memory domains.
* Moved the pointer domain, the value domain and the summary domain under IKOS core.

### Analyzer changes

* Added a pass to unify exit nodes.

### ARBOS changes

* Added APIs to set and retrieve "unreachable" and "unwind" basic blocks in an AR_Code


IKOS version 1.1.1 release notes
================================

Release date
------------

August 2016

List of changes
---------------

### Analyzer changes

* Added a pass `branching-opt` to optimize all Control Flow Graphs of the AR, before the analysis. The pass detects unreachable paths in the graphs and removes them.

### IKOS Core changes

* Improved the precision of the interval domain on most binary operators.
* Added a wrapper for APRON.

### Other changes

* Improved the bootstrap script. It is now able to build and install zlib, ncurses, libedit, GMP, SQLite and Python.
* Added installation instructions for Archlinux, CentOS, Debian, Fedora, Mac OS X, Red Hat and Ubuntu.
* Added support for gcc 4.9.2 and boost 1.55.0.


IKOS version 1.1.0 release notes
================================

Release date
------------

June 2016

List of changes
---------------

### Analyzer changes

* Added the ability to demangle C++ function names.
* We now handle `calloc()` correctly. In IKOS 1.0.0, the function call was just ignored.
* Added the runtime options `--display-invariants` and `--display-checks`.
* Added a column called `column` to the result tables in the output database, containing the column number in the source code.
* Added a column called `stmt_uid` to the result tables in the output database, containing the UID of the checked statement.
* Performance improvement (the analysis is 70% faster in average).
* Bug fixes.

### LLVM Frontend changes

The LLVM frontend now supports both LLVM 3.7 and 3.8.

### ARBOS changes

ARBOS now gives the column number in addition to the line number in `AR_Source_Location`.


IKOS version 1.0.0 release notes
================================

Release date
------------

May 2016

List of changes
---------------

### LLVM Frontend changes

In this release we upgraded our LLVM frontend from version 2.9 to 3.7.

To have a C/C++ program fully represented in AR, there are several code transformation we had to handle with the frontend's intermediate representation. When using the LLVM framework as the frontend, we transform the following LLVM instructions so they can be expressed in AR:

* `phi` instruction
* `getelementptr` instruction
* `insertvalue` and `extractvalue` instructions
* `i/fcmp` and `select` instructions
* removed `br`
* `constantexpr` and its subclasses
* translation of intrinsic calls to AR-supported intrinsic calls. We currently support `memset`, `memcpy`, `memmove`, `vastart`, `vaend`, `va_arg`, and `vacopy`.

In the previous release v0.0.1 we handled all transformation in **ARBOS**. The previous tool chain architecture defined another intermediate representation, called **AIR**, that served as a direct translation from the LLVM bitcode to an S-expression format that was fed into **ARBOS**. **ARBOS** then parsed and transformed **AIR** to **AR** in memory.

This release removes **AIR** entirely. All transformation are done in **LLVMAR**, while **ABROS** only parses the AR and makes it available in memory for ARBOS analysis plugins to traverse and perform analysis on it.

### ARBOS API changes

* Each `AR_Global_Variable` may contain an initializer represented as `AR_Code`. In the previous release we dedicated an initializer function for each global variable. These initializer functions were individually invoked in `main()`. In this release, analyzers need to dive into the initializer of an `AR_Global_Variable` to analyze the initializer code.
* Differentiates between store vs. real sizes of an `AR_Type`.
* Supports `varags` intrinsic calls in the AR. This was not supported in the previous release.
* Function pointers are now available in the AR model; they were misinterpreted and were not available in the previous release.
