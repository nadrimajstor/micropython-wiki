Note: MicroPython is work in progress and information here represents state of affairs at the beginning of 2014-01. As core functionality matures, it is expected that improvements and optimizations will follow. This page is in particular intended to identify areas to optimize.

MicroPython uses [string interning](http://en.wikipedia.org/wiki/String_interning). Currently all strings are interned, it's under consideration to support non-interned strings (like CPython does). File: qstr.c

MicroPython uses [Open addressing](http://en.wikipedia.org/wiki/Open_addressing) with [Linear probing](http://en.wikipedia.org/wiki/Linear_probing) of step = 1 for implementation of dictionaries. Rehashing happens when last free slot is used (on reaching loadfactor = 1.0).
