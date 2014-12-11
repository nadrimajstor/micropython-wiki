# What to contribute

MicroPython is named "Micro" for a reason. We don't want to compete with CPython or replace it. We could spend year of development and do everything what CPython can do, just in 50% of size. It's not worthy an aim. If you want all CPython functionality, you can use CPython right now. What we try to achieve with MicroPython is being order(s) of magnitude smaller than CPython, to use Python (the language) where CPython could not be used at all. This necessitates being a CPython subset, making compromises for what can go into MicroPython core, and then stay orthodox about these compromises.

# Where to contribute

We write MicroPython in C not to keep writing code in C. We write it to let us and everyone write code in Python. This should be good rule of thumb - anything which can be written in Python, should be written in Python, unless there're *really* good reasons to write it in C. 

So, while we cannot achieve full CPython compatibility in MicroPython, we want to have as complete as possible, CPython-compatible standard library written in Python, there's a separate project for that: https://github.com/micropython/micropython-lib .

But note that even micropython-lib has its pretty well defined scope: it's for Python standard library modules, plus, as an exception, to modules we consider to be part of "standard MicroPython library". micropython-lib is not intended to become single destination (or outright dump) of all modules MicroPython. Instead, we would like to achieve the same model as used by Python in general: there's a community, with multitude of modules maintained by individual members of it.

Summing up, you have 3 choices where to contribute with MicroPython development:

* MicroPython core
* micropython-lib
* Maintain your own module or port, as part of general MicroPython community  

# How to contribute

Please make sure you read previous sections to understand MicroPython philosophy and approach. We expect contributors will be aligned with them on general matters, because otherwise it would be hard to understand why specific change is proposed at all.

If you're not sure if some change would go along with MicroPython approach, please discuss this change first. We have 2 channels of communication:

* Project tickets
* [Development forum](http://forum.micropython.org/viewforum.php?f=3)

You may not agree with us on interpretation of specific feature, please share your point of view, and be prepared to elaborate and argue it. But please keep in mind that we necessarily have to be conservative to achieve (and not lose on the way) project aims as stated in "What to contribute" section.

We accept changes using Github pull requests, as the most seamless way for both ourselves and contributors. When you make commits for a change, please follow format of existing commit messages (use "git log" to review them). For considerable changes, we expect detailed, yet formal and concise description in of the change in commit messages. If you make 2 or more considerable changes, they should go in separate commits. The same applies to unrelated changes - for example, don't put formatting changes and actual code changes in one commit. 

When you submit pull request, please make sure that it's understood why you propose this change: what problem it addresses, how it improves MicroPython (and not deteriorates, taking into account stringent constraints of "What to contribute" section). Good commit messages are good way to achieve this, but sometimes it's better to put informal points, colloquial arguments and big example in comments to pull request instead. 