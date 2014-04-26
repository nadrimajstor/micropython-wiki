This page is a proof-of-concept effort to document the differences between CPython3 (considered to be a reference implementation of the Python3 language) and MicroPython. It classifies differences into 3 categories, each category having a different status regarding the possibility that items belonging to it will change.

## Differences By Design
MicroPython is intended for constrained environments, in particular, microcontrollers, which have orders of magnitude less performance and memory than "desktop" systems on which CPython3 runs. This means that MicroPython must be designed with this constrained environment in mind, leaving out features which simply won't fit, or won't scale, with target systems. "By design" differences are unlikely to change.

1. MicroPython does not ship with an extensive standard library of modules. It's not possible and does not make sense, to provide the complete CPython3 library. Many modules are not usable or useful in the context of embedded systems, and there is not enough memory to deploy the entire library on small devices. So MicroPython takes the minimalist approach - only core datatypes (plus modules specific to particular hardware) are included with the interpreter, and all the rest is left as 3rd-party dependencies for particular user applications. The [micropython-lib](https://github.com/micropython/micropython-lib) project provides the non-monolithic standard library for MicroPython ([forum](http://forum.micropython.org/viewtopic.php?f=5&t=70))
1. Unlike CPython3, which uses reference-counting, MicroPython uses garbage collection as the primary means of memory management.
1. print() function does not check for recursive data structures, and if fed with such, may run in infinite loop. Note that if this is a concern to you, you can fix this at the Python application level. Do this by writing a function which keeps the history of each object visited, and override the builtin print() with your custom function. Such an implementation may of course use a lot of memory, which is why it's not implemented at the MicroPython level.

## Implementation Differences
Some features don't cater for constrained systems, and at the same time ar not easy to implement efficiently, or at all. Such features are known as "implementation differences" and some of them are potentially the subject for future development (after corresponding discussion and consideration). Note that many of them would affect the size and performance of any given MicroPython implementation, so sometimes not implementing a specific feature is identified by MicroPython's target usage.

1. No unicode support is actually implemented. Python3 calls for strict difference between ``str`` and ``bytes`` data types (unlike Python2, which has neutral unified data type for strings and binary data, and separates out unicode data type). MicroPython faithfully implements ``str``/``bytes`` separation, but currently, underlying ``str`` implementation is the same as ``bytes``. This means strings in MicroPython are not unicode, but 8-bit characters (fully binary-clean).
1. Object finalization (``__del__()`` method) is supported for builtin types, but not yet user classes. This is tracked by [#245](//github.com/micropython/micropython/issues/245).
1. It's not possible to subclass builtin types. [#401](//github.com/micropython/micropython/issues/401)

## Known Issues
Known issues are essentially bugs, misfeaturures, and omissions considered such, and scheduled to be fixed. So, ideally any entry here should be accompanied by bug ticket reference. But note that these known issues will have different priorities, especially within wider development process. So if you are actually affected by some issue, please add details of your case to the ticket (or open it if does not yet exist) to help planning. Submitting patches is even more productive. (Please note that the list of not implemented modules/classes include only those which are considered very important to implement; per the above, MicroPython does not provide full standard library in general.)

1. Some functions don't perform argument checking well enough, which potentially may lead to crash if wrong argument types are passed.
1. Some functions use ``assert()`` for argument checking, which will lead to crash if wrong argument types are passed/errornous conditions are faced. This should be replaced with Python exception raising.
1. It's not possible to override builtin functions in a general way. So far - no "builtins" module is implemented, so any overrides work within the current module only.
1. There's no support for persistent bytecode, and running bytecode (vs running Python source). [#222](//github.com/micropython/micropython/issues/222)
1. print() function doesn't use the Python stream infrastructure, it uses the underlying C printf() function directly. This means if you override sys.stdout, then print() won't be affected. [#209](//github.com/micropython/micropython/issues/209)
1. Some more advanced usages of package/module importing is not yet fully implemented. [#298](//github.com/micropython/micropython/issues/298)
1. <strike>Exception handling with generators is not fully implemented.</strike> (Some small details may need to be done yet.) [#243](//github.com/micropython/micropython/issues/243)
1. str.format() may miss few advanced/obscure features (but otherwise almost completely implemented). [#407](//github.com/micropython/micropython/issues/407)
1. <strike>Only the very basic implementation of % string formatting operator is available (%s and %r formatting codes).</strike> [#403](//github.com/micropython/micropython/issues/403).
1. ``struct`` module should have all functionality, but misses some syntactic sugar (like repetition specifiers in type strings).
1. No builtin``re`` module implementation. micropython-lib offers the initial PCRE FFI module for "unix" port [#13](//github.com/micropython/micropython/issues/13)
1. Only beginning of ``io`` module and class hierarchy exists so far.
1. ``collecions.deque`` class is not implemented.
1. ``memoryview`` object not implemented.
1. Container slice assignment/deletion not implemented. [#509](https://github.com/micropython/micropython/issues/509)
1. Keyword and keyword-only arguments need more work [#466](https://github.com/micropython/micropython/issues/466), [#524](https://github.com/micropython/micropython/issues/524).