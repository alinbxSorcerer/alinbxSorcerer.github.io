   .. title: Fluent Python 
   .. slug: flunt-python 
   .. date: 2018-08-01 20:53:29 UTC+08:00
   .. tags: python 
   .. category: programming
   .. link:
   .. description:
   .. type: text


.. contents::

Preface
=======

   Here's the plan: when someone uses a feature you don't understand, simply shoot them. This is easier than learning something new, and before too long the only living coders will be writing in an easily understood, tiny subset of Python 0.9.6 — Tim Peters *Legendary core developer and author of The Zen of Python*

“Python is an easy to learn, powerful programming language.” Those are the first words of the `official Python Tutorial <https://docs.python.org/3/tutorial/>`__. That is true, but there is a catch: because the language is easy to learn and put to use, many practicing Python programmers leverage only a fraction of its powerful features.

An experienced programmer may start writing useful Python code in a matter of hours. As the first productive hours become weeks and months, a lot of developers go on writing Python code with a very strong accent carried from languages learned before. Even if Python is your first language, often in academia and in introductory books it is presented while carefully avoiding language-specific features.

As a teacher introducing Python to programmers experienced in other languages, I see another problem that this book tries to address: we only miss stuff we know about. Coming from another language, anyone may guess that Python supports regular expressions, and look that up in the docs. But if you've never seen tuple unpacking or descriptors before, you will probably not search for them, and may end up not using those features just because they are specific to Python.

This book is not an A-to-Z exhaustive reference of Python. Its emphasis is on the language features that are either unique to Python or not found in many other popular languages. This is also mostly a book about the core language and some of its libraries. I will rarely talk about packages that are not in the standard library, even though the Python package index now lists more than 60,000 libraries and many of them are incredibly useful.

Who This Book Is For
--------------------

This book was written for practicing Python programmers who want to become proficient in Python 3. If you know Python 2 but are willing to migrate to Python 3.4 or later, you should be fine. At the time of this writing, the majority of professional Python programmers are using Python 2, so I took special care to highlight Python 3 features that may be new to that audience.

However, *Fluent Python* is about making the most of Python 3.4, and I do not spell out the fixes needed to make the code work in earlier versions. Most examples should run in Python 2.7 with little or no changes, but in some cases, backporting would require significant rewriting.

Having said that, I believe this book may be useful even if you must stick with Python 2.7, because the core concepts are still the same. Python 3 is not a new language, and most differences can be learned in an afternoon. `What's New in Python 3.0 <https://docs.python.org/3.0/whatsnew/3.0.html>`__ is a good starting point. Of course, there have been changes since Python 3.0 was released in 2009, but none as important as those in 3.0.

If you are not sure whether you know enough Python to follow along, review the topics of the official `Python Tutorial <https://docs.python.org/3/tutorial/>`__. Topics covered in the tutorial will not be explained here, except for some features that are new in Python 3.

Who This Book Is Not For
------------------------

If you are just learning Python, this book is going to be hard to follow. Not only that, if you read it too early in your Python journey, it may give you the impression that every Python script should leverage special methods and metaprogramming tricks. Premature abstraction is as bad as premature optimization.

How This Book Is Organized
--------------------------

The core audience for this book should not have trouble jumping directly to any chapter in this book. However, each of the six parts forms a book within the book. I conceived the chapters within each part to be read in sequence.

I tried to emphasize using what is available before discussing how to build your own. For example, in `Part II <pt02.html>`__, `Chapter 2 <ch02.html>`__ covers sequence types that are ready to use, including some that don't get a lot of attention, like ``collections.deque``. Building user-defined sequences is only addressed in `Part IV <pt04.html>`__, where we also see how to leverage the abstract base classes (ABCs) from ``collections.abc``. Creating your own ABCs is discussed even later in `Part IV <pt04.html>`__, because I believe it's important to be comfortable using an ABC before writing your own.

This approach has a few advantages. First, knowing what is ready to use can save you from reinventing the wheel. We use existing collection classes more often than we implement our own, and we can give more attention to the advanced usage of available tools by deferring the discussion on how to create new ones. We are also more likely to inherit from existing ABCs than to create a new ABC from scratch. And finally, I believe it is easier to understand the abstractions after you've seen them in action.

The downside of this strategy are the forward references scattered throughout the chapters. I hope these will be easier to tolerate now that you know why I chose this path.

Here are the main topics in each part of the book:

`Part I <pt01.html>`__
   A single chapter about the Python data model explaining how the special methods (e.g., ``__repr__``) are the key to the consistent behavior of objects of all types—in a language that is admired for its consistency. Understanding various facets of the data model is the subject of most of the rest of the book, but `Chapter 1 <ch01.html>`__ provides a high-level overview.
`Part II <pt02.html>`__
   The chapters in this part cover the use of collection types: sequences, mappings, and sets, as well as the ``str`` versus ``bytes`` split—the cause of much celebration among Python 3 users and much pain for Python 2 users who have not yet migrated their code bases. The main goals are to recall what is already available and to explain some behavior that is sometimes surprising, like the reordering of ``dict`` keys when we are not looking, or the caveats of locale-dependent Unicode string sorting. To achieve these goals, the coverage is sometimes high level and wide (e.g., when many variations of sequences and mappings are presented) and sometimes deep (e.g., when we dive into the hash tables underneath the ``dict`` and ``set`` types).
`Part III <pt03.html>`__
   Here we talk about functions as first-class objects in the language: what that means, how it affects some popular design patterns, and how to implement function decorators by leveraging closures. Also covered here is the general concept of callables in Python, function attributes, introspection, parameter annotations, and the new ``nonlocal`` declaration in Python 3.
`Part IV <pt04.html>`__
   Now the focus is on building classes. In `Part II <pt02.html>`__, the ``class`` declaration appears in few examples; `Part IV <pt04.html>`__ presents many classes. Like any object-oriented (OO) language, Python has its particular set of features that may or may not be present in the language in which you and I learned class-based programming. The chapters explain how references work, what mutability really means, the lifecycle of instances, how to build your own collections and ABCs, how to cope with multiple inheritance, and how to implement operator overloading—when that makes sense.
`Part V <pt05.html>`__
   Covered in this part are the language constructs and libraries that go beyond sequential control flow with conditionals, loops, and subroutines. We start with generators, then visit context managers and coroutines, including the challenging but powerful new ``yield from`` syntax. `Part V <pt05.html>`__ closes with a high-level introduction to modern concurrency in Python with ``collections.futures`` (using threads and processes under the covers with the help of futures) and doing event-oriented I/O with ``asyncio`` (leveraging futures on top of coroutines and ``yield from``).
`Part VI <pt06.html>`__
   This part starts with a review of techniques for building classes with attributes created dynamically to handle semi-structured data such as JSON datasets. Next, we cover the familiar properties mechanism, before diving into how object attribute access works at a lower level in Python using descriptors. The relationship between functions, methods, and descriptors is explained. Throughout `Part VI <pt06.html>`__, the step-by-step implementation of a field validation library uncovers subtle issues that lead to the use of the advanced tools of the final chapter: class decorators and metaclasses.

Hands-On Approach
-----------------

Often we'll use the interactive Python console to explore the language and libraries. I feel it is important to emphasize the power of this learning tool, particularly for those readers who've had more experience with static, compiled languages that don't provide a read-eval-print#loop (REPL).

One of the standard Python testing packages, ```doctest`` <https://docs.python.org/3/library/doctest.html>`__, works by simulating console sessions and verifying that the expressions evaluate to the responses shown. I used ``doctest`` to check most of the code in this book, including the console listings. You don't need to use or even know about ``doctest`` to follow along: the key feature of doctests is that they look like transcripts of interactive Python console sessions, so you can easily try out the demonstrations yourself.

Sometimes I will explain what we want to accomplish by showing a doctest before the code that makes it pass. Firmly establishing what is to be done before thinking about how to do it helps focus our coding effort. Writing tests first is the basis of test driven development (TDD) and I've also found it helpful when teaching. If you are unfamiliar with ``doctest``, take a look at its `documentation <https://docs.python.org/3/library/doctest.html>`__ and this book's `source code repository <https://github.com/fluentpython/example-code>`__. You'll find that you can verify the correctness of most of the code in the book by typing ``python3 -m doctest example_script.py`` in the command shell of your OS.

Hardware Used for Timings
-------------------------

The book has some simple benchmarks and timings. Those tests were performed on one or the other laptop I used to write the book: a 2011 MacBook Pro 13” with a 2.7 GHz Intel Core i7 CPU, 8GB of RAM, and a spinning hard disk, and a 2014 MacBook Air 13” with a 1.4 GHz Intel Core i5 CPU, 4GB of RAM, and a solid-state disk. The MacBook Air has a slower CPU and less RAM, but its RAM is faster (1600 versus 1333 MHz) and the SSD is much faster than the HD. In daily usage, I can't tell which machine is faster.

Soapbox: My Personal Perspective
--------------------------------

I have been using, teaching, and debating Python since 1998, and I enjoy studying and comparing programming languages, their design, and the theory behind them. At the end of some chapters, I have added “Soapbox” sidebars with my own perspective about Python and other languages. Feel free to skip these if you are not into such discussions. Their content is completely optional.

Python Version Covered
----------------------

I tested all the code in the book using Python 3.4—that is, CPython 3.4, the most popular Python implementation written in C. There is only one exception: `The New @ Infix Operator in Python 3.5 <ch13.html#matmul_operator_sec>`__ shows the ``@`` operator, which is only supported by Python 3.5.

Almost all code in the book should work with any Python 3.x–compatible interpreter, including PyPy3 2.4.0, which is compatible with Python 3.2.5. The notable exceptions are the examples using ``yield from`` and ``asyncio``, which are only available in Python 3.3 or later.

Most code should also work with Python 2.7 with minor changes, except the Unicode-related examples in `Chapter 4 <ch04.html>`__, and the exceptions already noted for Python 3 versions earlier than 3.3.

Python Jargon
-------------

I wanted this to be a book not only about Python but also about the culture around it. Over more than 20 years of communications, the Python community developed its own particular lingo and acronyms. At the end of this book, `Python Jargon <go01.html>`__ contains a list of terms that have special meaning among Pythonistas.

Many terms here are not exclusive to Python, of course, but particularly in the definitions you may find meanings that are specific to the Python community.

Also see the official `Python glossary <https://docs.python.org/3/glossary.html>`__.

ABC (programming language)
   A programming language created by Leo Geurts, Lambert Meertens, and Steven Pemberton. Guido van Rossum, who developed Python, worked as a programmer implementing the ABC environment in the 1980s. Block structuring by indentation, built-in tuples and dictionaries, tuple unpacking, the semantics of the ``for`` loop, and uniform handling of all sequence types are some of the distinctive characteristics of Python that came from ABC.

Abstract base class (ABC)
   A class that cannot be instantiated, only subclassed. ABCs are how interfaces are formalized in Python. Instead of inheriting from an ABC, a class may also declare that it fulfills the interface by registering with the ABC to become a *virtual subclass*.

accessor
   A method implemented to provide access to a single data attribute. Some authors use *acessor* as a generic term encompassing getter and setter methods, others use it to refer only to getters, referring to setters as mutators.

aliasing
   Assigning two or more names to the same object. For example, in ``a = []; b = a`` the variables ``a`` and ``b`` are aliases for the same list object. Aliasing happens naturally all the time in any language where variables store references to objects. To avoid confusion, just forget the idea that variables are boxes that hold objects (an object can't be in two boxes at the same time). It's better to think of them as labels attached to objects (an object can have more than one label).

argument
   An expression passed to a function when it is called. In Pythonic parlance, *argument* and *parameter* are almost always synonyms. See *parameter* for more about the distinction and usage of these terms.

attribute
   Methods and data attributes (i.e., “fields” in Java terms) are all known as attributes in Python. A method is just an attribute that happens to be a callable object (usually a function, but not necessarily).

BDFL
   Benevolent Dictator For Life, alias for Guido van Rossum, creator of the Python language.

binary sequence
   Generic term for sequence types with byte elements. The built-in binary sequence types are ``byte``, ``bytearray``, and ``memoryview``.

BOM
   Byte Order Mark, a sequence of bytes that may be present at the start of a UTF-16 encoded file. A BOM is the character U+FEFF (``ZERO WIDTH NO-BREAK SPACE``) encoded to produce either ``b'\xfe\xff'`` on a big-endian CPU, or ``b'\xff\xfe'`` on a little-endian one. Because there is no U+FFFE characer in Unicode, the presence of these bytes unambiguously reveals the byte ordering used in the encoding. Although redundant, a BOM encoded as ``b'\xef\xbb\xbf'`` may be found in UTF-8 files.

bound method
   A method that is accessed through an instance becomes bound to that instance. Any method is actually a descriptor and when accessed, it returns itself wrapped in an object that binds the method to the instance. That object is the bound method. It can be invoked without passing the value of ``self``. For example, given the assignment ``my_method = my_obj.method``, the bound method can later be called as ``my_method()``. Contrast with *unbound method*.

built-in function (BIF)
   A function bundled with the Python interpreter, coded in the underlying implementation language (i.e., C for CPython; Java for Jython, and so on). The term often refers only to the functions that don't need to be imported, documented in `Chapter 2, “Built-in Functions,” <http://docs.python.org/library/functions.html>`__ of The Python Standard Library Reference. But built-in modules like ``sys``, ``math``, ``re``, etc. also contain built-in functions.

byte string
   An unfortunate name still used to refer to ``bytes`` or ``bytearray`` in Python 3. In Python 2, the ``str`` type was really a byte string, and the term made sense to distinguish ``str`` from ``unicode`` strings. In Python 3, it makes no sense to insist on this term, and I tried to use *byte sequence* whenever I needed to talk in general about…byte sequences.

bytes-like object
   A generic sequence of bytes. The most common bytes-like types are ``bytes``, ``bytearray``, and ``memoryview`` but other objects supporting the low-level CPython buffer protocol also qualify, if their elements are single bytes.

callable object
   An object that can be invoked with the call operator ``()``, to return a result or to perform some action. There are seven flavors of callable objects in Python: user-defined functions, built-in functions, built-in methods, instance methods, generator functions, classes, and instances of classes that implement the ``__call__`` special method.

CamelCase
   The convention of writing identifiers by joining words with uppercased initials (e.g., ``ConnectionRefusedError``). PEP-8 recommends class names should be written in CamelCase, but the advice is not followed by the Python standard library. See *snake\\\ case*.

Cheese Shop
   Original name of the `Python Package Index <https://pypi.python.org/pypi>`__ (PyPI), after the Monty Python skit about a cheese shop where nothing is available. As of this writing, the alias https://cheeseshop.python.org still works. See *PyPI*.

class
   A program construct defining a new type, with data attributes and methods specifying possible operations on them. See ``type``.

code point
   An integer in the range 0 to 0x10FFFF used to identify an entry in the Unicode character database. As of Unicode 7.0, less than 3% of all code points are assigned to characters. In the Python documentation, the term may be spelled as one or two words. For example, in `Chapter 2, “Built-in Functions,” <http://docs.python.org/library/functions.html>`__ of the *Python Library Reference*, the ``chr`` function is said to take an integer “codepoint,” while its inverse, ``ord``, is described as returning a “Unicode code point.”

code smell
   A coding pattern that suggests there may be something wrong with the design of a program. For example, excessive use of ``isinstance`` checks against concrete classes is a code smell, as it makes the program harder to extend to deal with new types in the future.

codec
   (encoder/decoder) A module with functions to encode and decode, usually from ``str`` to ``bytes`` and back, although Python has a few codecs that perform ``bytes`` to ``bytes`` and ``str`` to ``str`` transformations.

collection
   Generic term for data structures made of items that can be accessed individually. Some collections can contain objects of arbitrary types (see *container*) and others only objects of a single atomic type (see *flat sequence*). ``list`` and ``bytes`` are both collections, but ``list`` is a container, and ``bytes`` is a flat sequence.

considered harmful
   Edsger Dijkstra's letter titled “Go To Statement Considered Harmful” established a formula for titles of essays criticizing some computer science technique. Wikipedia's `“Considered harmful” article <http://en.wikipedia.org/wiki/Considered_harmful>`__ lists several examples, including `"Considered Harmful Essays Considered Harmful” <http://meyerweb.com/eric/comment/chech.html>`__ by Eric A. Meyer.

constructor
   Informally, the ``__init__`` instance method of a class is called its constructor, because its semantics is similar to that of a Java constructor. However, a fitting name for ``__init__`` is *initializer*, as it does not actually build the instance, but receives it as its ``self`` argument. The *constructor* term better describes the ``__new__`` class method, which Python calls before ``__init__``, and is responsible for actually creating an instance and returning it. See *initializer*.

container
   An object that holds references to other objects. Most collection types in Python are containers, but some are not. Contrast with *flat sequence*, which are collections but not containers.

context manager
   An object implementing both the ``__enter__`` and ``__exit__`` special methods, for use in a ``with`` block.

coroutine
   A generator used for concurrent programming by receiving values from a scheduler or an event loop via ``coro.send(value)``. The term may be used to describe the generator function or the generator object obtained by calling the generator function. See *generator*.

CPython
   The standard Python interpreter, implemented in C. This term is only used when discussing implementation-specific behavior, or when talking about the multiple Python interpreters available, such as *PyPy*.

CRUD
   Acronym for Create, Read, Update, and Delete, the four basic functions in any application that stores records.

decorator
   A callable object ``A`` that returns another callable object ``B`` and is invoked in code using the syntax ``@A`` right before the definition of a callable ``C``. When reading such code, the Python interpreter invokes ``A(C)`` and binds the resulting ``B`` to the variable previously assigned to ``C``, effectively replacing the definition of ``C`` with ``B``. If the target callable ``C`` is a function, then ``A`` is a function decorator; if ``C`` is a class, then ``A`` is a class decorator.

deep copy
   A copy of an object in which all the objects that are attributes of the object are themselves also copied. Contrast with *shallow copy*.

descriptor
   A class implementing one or more of the ``__get__``, ``__set__``, or ``__delete__`` special methods becomes a descriptor when one of its instances is used as a class attribute of another class, the *managed class*. Descriptors manage the access and deletion of *managed attributes* in the *managed class*, often storing data in the *managed instances*.

docstring
   Short for documentation string. When the first statement in a module, class, or function is a string literal, it is taken to be the *docstring* for the enclosing object, and the interpreter saves it as the ``__doc__`` attribute of that object. See also *doctest*.

doctest
   A module with functions to parse and run examples embedded in the docstrings of Python modules or in plain-text files. May also be used from the command line as:

   .. code:: screen

      python -m doctest
      module_with_tests.py

DRY
   Don't Repeat Yourself—a software engineering principle stating that “Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.” It first appeared in the book *The Pragmatic Programmer* by Andy Hunt and Dave Thomas (Addison-Wesley, 1999).

duck typing
   A form of polymorphism where functions operate on any object that implements the appropriate methods, regardless of their classes or explicit interface declarations.

dunder
   Shortcut to pronounce the names of *special methods* and attributes that are written with leading and trailing double-underscores (i.e., ``__len__`` is read as “dunder len”).

dunder method
   See *dunder* and *special methods*.

EAFP
   Acronym standing for the quote “It's easier to ask forgiveness than permission,” attributed to computer pioneer Grace Hopper, and quoted by Pythonistas referring to dynamic programming practices like accessing attributes without testing first if they exist, and then catching the exception when that is the case. The docstring for the ``hasattr`` function actually says that it works “by calling getattr(object, name) and catching AttributeError.”

eager
   An iterable object that builds all its items at once. In Python, a *list comprehension* is eager. Contrast with *lazy*.

fail-fast
   A systems design approach recommending that errors should be reported as early as possible. Python adheres to this principle more closely than most dynamic languages. For example, there is no “undefined” value: variables referenced before initialization generate an error, and ``my_dict[k]`` raises an exception if ``k`` is missing (in contrast with JavaScript). As another example, parallel assignment via tuple unpacking in Python only works if every item is explicitly handled, while Ruby silently deals with item count mismatches by ignoring unused items on the right side of the ``=``, or by assigning ``nil`` to extra variables on the left side.

falsy
   Any value ``x`` for which ``bool(x)`` returns ``False``; Python implicitly uses ``bool`` to evaluate objects in Boolean contexts, such as the expression controlling an ``if`` or ``while`` loop. The opposite of *truthy*.

file-like object
   Used informally in the official documentation to refer to objects implementing the file protocol, with methods such as ``read``, ``write``, ``close``, etc. Common variants are text files containing encoded strings with line-oriented reading and writing, ``StringIO`` instances which are in-memory text files, and binary files, containing unencoded bytes. The latter may be buffered or unbuffered. ABCs for the standard file types are defined in the ``io`` module since Python 2.6.

first-class function
   Any function that is a first-class object in the language (i.e., can be created at runtime, assigned to variables, passed as an argument, and returned as the result of another function). Python functions are first-class functions.

flat sequence
   A sequence type that physically stores the values of its items, and not references to other objects. The built-in types ``str``, ``bytes``, ``bytearray``, ``memoryview``, and ``array.array`` are flat sequences. Contrast with ``list``, ``tuple``, and ``collections.deque``, which are container sequences. See *container*.

function
   Strictly, an object resulting from evaluation of a ``def`` block or a ``lambda`` expression. Informally, the word *function* is used to describe any callable object, such as methods and even classes sometimes. The official `Built-in Functions <http://docs.python.org/library/functions.html>`__ list includes several built-in classes like ``dict``, ``range``, and ``str``. Also see *callable object*.

genexp
   Short for *generator expression*.

generator
   An iterator built with a generator function or a generator expression that may produce values without necessarily iterating over a collection; the canonical example is a generator to produce the Fibonacci series which, because it is infinite, would never fit in a collection. The term is sometimes used to describe a *generator function*, besides the object that results from calling it.

generator function
   A function that has the ``yield`` keyword in its body. When invoked, a generator function returns a *generator*.

generator expression
   An expression enclosed in parentheses using the same syntax of a *list comprehension*, but returning a generator instead of a list. A *generator expression* can be understood as a *lazy* version of a *list comprehension*. See *lazy*.

generic function
   A group of functions designed to implement the same operation in customized ways for different object types. As of Python 3.4, the ``functools.singledispatch`` decorator is the standard way to create generic functions. This is known as multimethods in other languages.

GoF book
   Alias for *Design Patterns: Elements of Reusable Object-Oriented Software* (Addison-Wesley, 1995), authored by the so-called Gang of Four (GoF): Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.

hashable
   An object is hashable if it has both ``__hash__`` and ``__eq__`` methods, with the constraints that the hash value must never change and if ``a =`` b= then ``hash(a) =`` hash(b)= must also be ``True``. Most immutable built-in types are hashable, but a tuple is only hashable if every one of its items is also hashable.

higher-order function
   A function that takes another function as argument, like ``sorted``, ``map``, and ``filter``, or a function that returns a function as result, as Python decorators do.

idiom
   “A manner of speaking that is natural to native speakers of a language,” according to the Princeton WordNet.

import time
   The moment of initial execution of a module when its code is loaded by the Python interpreter, evaluated from top to bottom, and compiled into bytecode. This is when classes and functions are defined and become live objects. This is also when decorators are executed.

initializer
   A better name for the ``__init__`` method (instead of *constructor*). Initializing the instance received as ``self`` is the task of ``__init__``. Actual instance construction is done by the ``__new__`` method. See *constructor*.

iterable
   Any object from which the ``iter`` built-in function can obtain an iterator. An iterable object works as the source of items in ``for`` loops, comprehensions, and tuple unpacking. Objects implementing an ``__iter__`` method returning an *iterator* are iterable. Sequences are always iterable; other objects implementing a ``__getitem__`` method may also be iterable.

iterable unpacking
   A modern, more precise synonym for *tuple unpacking*. See also *parallel assignment*.

iterator
   Any object that implements the ``__next__`` no-argument method, which returns the next item in a series, or raises ``StopIteration`` when there are no more items. Python iterators also implement the ``__iter__`` method so they are also *iterable*. Classic iterators, according to the original design pattern, return items from a collection. A *generator* is also an *iterator*, but it's more flexible. See *generator*.

KISS principle
   The acronym stands for “Keep It Simple, Stupid.” This calls for seeking the simplest possible solution, with the fewest moving parts. The phrase was coined by Kelly Johnson, a highly accomplished aerospace engineer who worked in the real Area 51 designing some of the most advanced aircraft of the 20th century.

lazy
   An iterable object that produces items on demand. In Python, generators are lazy. Contrast *eager*.

listcomp
   Short for *list comprehension*.

list comprehension
   An expression enclosed in brackets that uses the ``for`` and ``in`` keywords to build a list by processing and filtering the elements from one or more iterables. A list comprehension works eagerly. See *eager*.

liveness
   An asynchronous, threaded, or distributed system exhibits the liveness property when “something good eventually happens” (i.e., even if some expected computation is not happening right now, it will be completed eventually). If a system deadlocks, it has lost its liveness.

magic method
   Same as *special method*.

managed attribute
   A public attribute managed by a descriptor object. Although the *managed attribute* is defined in the *managed class*, it operates like an instance attribute (i.e., it usually has a value per instance, held in a *storage attribute*). See *descriptor*.

managed class
   A class that uses a descriptor object to manage one of its attributes. See *descriptor*.

managed instance
   An instance of a *managed class*. See *managed attribute* and *descriptor*.

metaclass
   A class whose instances are classes. By default, Python classes are instances of ``type``, for example, ``type(int)`` is the class ``type``, therefore ``type`` is a metaclass. User-defined metaclasses can be created by subclassing ``type``.

metaprogramming
   The practice of writing programs that use runtime information about themselves to change their behavior. For example, an *ORM* may introspect model class declarations to determine how to validate database record fields and convert database types to Python types.

monkey patching
   Dynamically changing a module, class, or function at runtime, usually to add features or fix bugs. Because it is done in memory and not by changing the source code, a monkey patch only affects the currently running instance of the program. Monkey patches break encapsulation and tend to be tightly coupled to the implementation details of the patched code units, so they are seen as temporary workarounds and not a recommended technique for code integration.

mixin class
   A class designed to be subclassed together with one or more additional classes in a multiple-inheritance class tree. A mixin class should never be instantiated, and a concrete subclass of a mixin class should also subclass another nonmixin class.

mixin method
   A concrete method implementation provided in an ABC or in a *mixin class*.

mutator
   See *accessor*.

name mangling
   The automatic renaming of private attributes from ``__x`` to ``_MyClass__x``, performed by the Python interpreter at runtime.

nonoverriding descriptor
   A *descriptor* that does not implement ``__set__`` and therefore does not interfere with setting of the *managed attribute* in the *managed instance*. Consequently, if a namesake attribute is set in the *managed instance*, it will shadow the descriptor in that instance. Also called nondata descriptor or shadowable descriptor. Contrast with *overriding descriptor*.

ORM
   Object-Relational Mapper—an API that provides access to database tables and records as Python classes and objects, providing method calls to perform database operations. SQLAlchemy is a popular standalone Python ORM; the Django and Web2py frameworks have their own bundled ORMs.

overriding descriptor
   A *descriptor* that implements ``__set__`` and therefore intercepts and overrides attempts at setting the *managed attribute* in the *managed instance*. Also called data descriptor or enforced descriptor. Contrast with *non-overriding descriptor*.

parallel assignment
   Assigning to several variables from items in an iterable, using syntax like ``a, b = [c, d]``—also known as destructuring assignment. This is a common application of *tuple unpacking*.

parameter
   Functions are declared with 0 or more “formal parameters,” which are unbound local variables. When the function is called, the *arguments* or “actual parameters” passed are bound to those variables. In this book, I tried to use *argument* to refer to an actual parameter passed to a function, and *parameter* for a formal parameter in the function declaration. However, that is not always feasible because the terms *parameter* and *argument* are used interchangeably all over the Python docs and API. See *argument*.

prime (verb)
   Calling ``next(coro)`` on a coroutine to advance it to its first ``yield`` expression so that it becomes ready to receive values in succeeding ``coro.send(value)`` calls.

PyPI
   The `Python Package Index <https://pypi.python.org>`__, where more than 60,000 packages are available, also known as the *Cheese shop* (see *Cheese shop*). PyPI is pronounced as “pie-P-eye” to avoid confusion with *PyPy*.

PyPy
   An alternative implementation of the Python programming language using a toolchain that compiles a subset of Python to machine code, so the interpreter source code is actually written in Python. PyPy also includes a JIT to generate machine code for user programs on the fly—like the Java VM does. As of November 2014, PyPy is 6.8 times faster than CPython on average, according to `published benchmarks <http://speed.pypy.org>`__. PyPy is pronounced as “pie-pie” to avoid confusion with *PyPI*.

Pythonic
   Used to praise idiomatic Python code, that makes good use of language features to be concise, readable, and often faster as well. Also said of APIs that enable coding in a way that seems natural to proficient Python programmers. See *idiom*.

refcount
   The reference counter that each CPython object keeps internally in order to determine when it can be destroyed by the garbage collector.

referent
   The object that is the target of a reference. This term is most often used to discuss *weak references*.

REPL
   Read-eval-print loop, an interactive console, like the standard ``python`` or alternatives like ``ipython``, ``bpython``, and Python Anywhere.

sequence
   Generic name for any iterable data structure with a known size (e.g., ``len(s)``) and allowing item access via 0-based integer indexes (e.g., ``s[0]``). The word *sequence* has been part of the Python jargon from the start, but only with Python 2.6 was it formalized as an abstract class in ``collections.abc.Sequence``.

serialization
   Converting an object from its in-memory structure to a binary or text-oriented format for storage or transmission, in a way that allows the future reconstruction of a clone of the object on the same system or on a different one. The ``pickle`` module supports serialization of arbitrary Python objects to a binary format.

shallow copy
   A copy of an object which shares references to all the objects that are attributes of the original object. Contrast with *deep copy*. Also see *aliasing*.

singleton
   An object that is the only existing instance of a class—usually not by accident but because the class is designed to prevent creation of more than one instance. There is also a design pattern named Singleton, which is a recipe for coding such classes. The ``None`` object is a singleton in Python.

slicing
   Producing a subset of a sequence by using the slice notation, e.g., ``my_sequence[2:6]``. Slicing usually copies data to produce a new object; in particular, ``my_sequence[:]`` creates a shallow copy of the entire sequence. But a ``memoryview`` object can be sliced to produce a new ``memoryview`` that shares data with the original object.

snake\\\ :sub:`case`
   The convention of writing identifiers by joining words with the underscore character (``_``)—for example, ``run_until_complete``. PEP-8 calls this style “lowercase with words separated by underscores” and recommends it for naming functions, methods, arguments, and variables. For packages, PEP-8 recommends concatenating words with no separators. The Python standard library has many examples of ``snake_case`` identifiers, but also many examples of identifiers with no separation between words (e.g., ``getattr``, ``classmethod``, ``isinstance``, ``str.endswith``, etc.). See *CamelCase*.

special method
   A method with a special name such as ``__getitem__``, spelled with leading and trailing double underscores. Almost all special methods recognized by Python are described in the `“Data model” chapter <http://bit.ly/1GsZwss>`__ of *The Python Language Reference*, but a few that are used only in specific contexts are documented in other parts of the documentation. For example, the ``__missing__`` method of mappings is mentioned in `“4.10. Mapping Types — ``dict``" <http://bit.ly/1QS9Ong>`__ in *The Python Standard Library*.

storage attribute
   An attribute in a *managed instance* used to store the value of an attribute managed by a *descriptor*. See also *managed attribute*.

strong reference
   A reference that keeps an object alive in Python. Contrast with *weak reference*.

tuple unpacking
   Assigning items from an iterable object to a tuple of variables (e.g., ``first, second, third =`` my\ :sub:`list`\ =). This is the usual term used by Pythonistas, but *iterable unpacking* is gaining traction.

truthy
   Any value ``x`` for which ``bool(x)`` returns ``True``; Python implicitly uses ``bool`` to evaluate objects in Boolean contexts, such as the expression controlling an ``if`` or ``while`` loop. The opposite of *falsy*.

type
   Each specific category of program data, defined by a set of possible values and operations on them. Some Python types are close to machine data types (e.g., ``float`` and ``bytes``) while others are extensions (e.g., ``int`` is not limited to CPU word size, ``str`` holds multibyte Unicode data points) and very high-level abstractions (e.g., ``dict``, ``deque``, etc.). Types may be user defined or built into the interpreter (a “built-in” type). Before the watershed type/class unification in Python 2.2, types and classes were different entities, and user-defined classes could not extend built-in types. Since then, built-in types and new-style classes became compatible, and a class is an instance of ``type``. In Python 3 all classes are new-style classes. See *class* and *metaclass*.

unbound method
   An instance method accessed directly on a class is not bound to an instance; therefore it's said to be an “unbound method.” To succeed, a call to an unbound method must explicitly pass an instance of the class as the first argument. That instance will be assigned to the ``self`` argument in the method. See *bound method*.

uniform access principle
   Bertrand Meyer, creator of the Eiffel Language, wrote: “All services offered by a module should be available through a uniform notation, which does not betray whether they are implemented through storage or through computation.” Properties and descriptors allow the implementation of the uniform access principle in Python. The lack of a ``new`` operator, making function calls and object instantiation look the same, is another form of this principle: the caller does not need to know whether the invoked object is a class, a function, or any other callable.

user-defined
   Almost always in the Python docs the word *user* refers to you and I—programmers who use the Python language—as opposed to the developers who implement a Python interpreter. So the term “user-defined class” means a class written in Python, as opposed to built-in classes written in C, like ``str``.

view
   Python 3 views are special data structures returned by the ``dict`` methods ``.keys()``, ``.values()``, and ``.items()``, providing a dynamic view into the ``dict`` keys and values without data duplication, which occurs in Python 2 where those methods return lists. All ``dict`` views are iterable and support the ``in`` operator. In addition, if the items referenced by the view are all hashable, then the view also implements the ``collections.abc.Set`` interface. This is the case for all views returned by the ``.keys()`` method, and for views returned by ``.items()`` when the values are also hashable.

virtual subclass
   A class that does not inherit from a superclass but is registered using ``TheSuperClass.register(TheSubClass)``. See documentation for ```abc.ABCMeta.register`` <http://bit.ly/1DeDbKf>`__.

wart
   A misfeature of the language. Andrew Kuchling's famous post “Python warts” has been acknowledged by the *BDFL* as influential in the decision to break backward-compatibility in the design of Python 3, as most of the failings could not be fixed otherwise. Many of Kuchling's issues were fixed in Python 3.

weak reference
   A special kind of object reference that does not increase the *referent* object reference count. Weak references are created with one of the functions and data structures in the ``weakref`` module.

YAGNI
   “You Ain't Gonna Need It,” a slogan to avoid implementing functionality that is not immediately necessary based on assumptions about future needs.

Zen of Python
   Type ``import this`` into any Python console since version 2.2.

Publish
-------

.. code:: ipython

   !pandoc --wrap=none 00.preface.org -o ~/Public/nikola_post/posts/fluent-python.rst


