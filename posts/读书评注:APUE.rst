   .. title: 读书评注: APUE
   .. slug: dushu-phngzhu-apue
   .. date: 2018-11-15 20:53:29 UTC+08:00
   .. tags: C,
   .. category: programming
   .. link:
   .. description:
   .. type: text

Preface to the First Edition
============================

Introduction
------------

This book describes the programming interface to the Unix system—the system call interface and many of the functions provided in the standard C library. It is intended for anyone writing programs that run under Unix.

Like most operating systems, Unix provides numerous services to the programs that are running—open a file, read a file, start a new program, allocate a region of memory, get the current time-of-day, and so on. This has been termed the *system call interface*. Additionally, the standard C library provides numerous functions that are used by almost every C program (format a variable's value for output, compare two strings, etc.).

The system call interface and the library routines have traditionally been described in Sections 2 and 3 of the *Unix Programmer's Manual*. This book is not a duplication of these sections. Examples and rationale are missing from the *Unix Programmer's Manual*, and that's what this book provides.

Unix Standards
--------------

The proliferation of different versions of Unix during the 1980s has been tempered by the various international standards that were started during the late 1980s. These include the ANSI standard for the C programming language, the IEEE POSIX family (still being developed), and the X/Open portability guide.

This book also describes these standards. But instead of just describing the standards by themselves, we describe them in relation to popular implementations of the standards—System V Release 4 and the forthcoming 4.4BSD. This provides a real-world description, which is often lacking from the standard itself and from books that describe only the standard.

Organization of the Book
------------------------

This book is divided into six parts:

#. An overview and introduction to basic Unix programming concepts and terminology (`Chapter 1 <part0013.xhtml#ch01>`__), with a discussion of the various Unix standardization efforts and different Unix implementations (`Chapter 2 <part0014.xhtml#ch02>`__).

#. I/O—unbuffered I/O (`Chapter 3 <part0015.xhtml#ch03>`__), properties of files and directories (`Chapter 4 <part0016.xhtml#ch04>`__), the standard I/O library (`Chapter 5 <part0017.xhtml#ch05>`__), and the standard system data files (`Chapter 6 <part0018.xhtml#ch06>`__).

#. Processes—the environment of a Unix process (`Chapter 7 <part0019.xhtml#ch07>`__), process control (`Chapter 8 <part0020.xhtml#ch08>`__), the relationships between different processes (`Chapter 9 <part0021.xhtml#ch09>`__), and signals (`Chapter 10 <part0022.xhtml#ch10>`__).

#. More I/O—terminal I/O (`Chapter 11 <part0023.xhtml#ch11>`__), advanced I/O (`Chapter 12 <part0024.xhtml#ch12>`__), and daemon processes (`Chapter 13 <part0025.xhtml#ch13>`__).

#. IPC—Interprocess communication (`Chapters 14 <part0026.xhtml#ch14>`__ and `15 <part0027.xhtml#ch15>`__).

#. Examples—a database library (`Chapter 16 <part0028.xhtml#ch16>`__), communicating with a PostScript printer (`Chapter 17 <part0029.xhtml#ch17>`__), a modem dialing program (`Chapter 18 <part0030.xhtml#ch18>`__), and using pseudo terminals (`Chapter 19 <part0031.xhtml#ch19>`__).

A reading familiarity with C would be beneficial as would some experience using Unix. No prior programming experience with Unix is assumed. This text is intended for programmers familiar with Unix and programmers familiar with some other operating system who wish to learn the details of the services provided by most Unix systems.

Examples in the Text
--------------------

This book contains many examples—approximately 10,000 lines of source code. All the examples are in the C programming language. Furthermore, these examples are in ANSI C. You should have a copy of the *Unix Programmer's Manual* for your system handy while reading this book, since reference is made to it for some of the more esoteric and implementation-dependent features.

Almost every function and system call is demonstrated with a small, complete program. This lets us see the arguments and return values and is often easier to comprehend than the use of the function in a much larger program. But since some of the small programs are contrived examples, a few bigger examples are also included (`Chapters 16 <part0028.xhtml#ch16>`__, `17 <part0029.xhtml#ch17>`__, `18 <part0030.xhtml#ch18>`__, and `19 <part0031.xhtml#ch19>`__). These larger examples demonstrate the programming techniques in larger, real-world examples.

All the examples have been included in the text directly from their source files. A machine-readable copy of all the examples is available via anonymous FTP from the Internet host ``ftp.uu.net`` in the file ``published/books/stevens.advprog.tar.Z``. Obtaining the source code allows you to modify the programs from this text and experiment with them on your system.

Systems Used to Test the Examples
---------------------------------

Unfortunately all operating systems are moving targets. Unix is no exception. The following diagram shows the recent evolution of the various versions of System V and 4.xBSD.

|image0|

4.xBSD are the various systems from the Computer Systems Research Group at the University of California at Berkeley. This group also distributes the BSD Net 1 and BSD Net 2 releases—publicly available source code from the 4.xBSD systems. SVRx refers to System V Release x from AT&T. XPG3 is the X/Open Portability Guide, Issue 3, and ANSI C is the ANSI standard for the C programming language. POSIX.1 is the IEEE and ISO standard for the interface to a Unix-like system. We'll have more to say about these different standards and the various versions of Unix in `Sections 2.2 <part0014.xhtml#ch02lev1sec2>`__ and `2.3 <part0014.xhtml#ch02lev1sec3>`__.

**In this text we use the term 4.3+BSD to refer to the Unix system from Berkeley that is somewhere between the BSD Net 2 release and 4.4BSD.**

At the time of this writing, 4.4BSD was not released, so the system could not be called 4.4BSD. Nevertheless a simple name was needed to refer to this system and *4.3+BSD* is used throughout the text.

Most of the examples in this text have been run on four different versions of Unix:

#. Unix System V/386 Release 4.0 Version 2.0 (“vanilla SVR4”) from U.H. Corp. (UHC), on an Intel 80386 processor.

#. 4.3+BSD at the Computer Systems Research Group, Computer Science Division, University of California at Berkeley, on a Hewlett Packard workstation.

#. BSD/386 (a derivative of the BSD Net 2 release) from Berkeley Software Design, Inc., on an Intel 80386 processor. This system is almost identical to what we call 4.3+BSD.

#. SunOS 4.1.1 and 4.1.2 (systems with a strong Berkeley heritage but many System V features) from Sun Microsystems, on a SPARCstation SLC.

Numerous timing tests are provided in the text and the systems used for the test are identified.

Acknowledgments
---------------

Once again I am indebted to my family for their love, support, and many lost weekends over the past year and a half. Writing a book is, in many ways, a family affair. Thank you Sally, Bill, Ellen, and David.

I am especially grateful to Brian Kernighan for his help in the book. His numerous thorough reviews of the entire manuscript and his gentle prodding for better prose hopefully show in the final result. Steve Rago was also a great resource, both in reviewing the entire manuscript and answering many questions about the details and history of System V. My thanks to the other technical reviewers used by Addison-Wesley, who provided valuable comments on various portions of the manuscript: Maury Bach, Mark Ellis, Jeff Gitlin, Peter Honeyman, John Linderman, Doug McIlroy, Evi Nemeth, Craig Partridge, Dave Presotto, Gary Wilson, and Gary Wright.

Keith Bostic and Kirk McKusick at the U.C. Berkeley CSRG provided an account that was used to test the examples on the latest BSD system. (Many thanks to Peter Salus too.) Sam Nataros and Joachim Sacksen at UHC provided the copy of SVR4 used to test the examples. Trent Hein helped obtain the alpha and beta copies of BSD/386.

Other friends have helped in many small, but significant ways over the past few years: Paul Lucchina, Joe Godsil, Jim Hogue, Ed Tankus, and Gary Wright. My editor at Addison-Wesley, John Wait, has been a great friend through it all. He never complained when the due date slipped and the page count kept increasing. A special thanks to the National Optical Astronomy Observatories (NOAO), especially Sidney Wolff, Richard Wolff, and Steve Grandi, for providing computer time.

*Real* Unix books are written using troff and this book follows that time-honored tradition. Camera-ready copy of the book was produced by the author using the groff package written by James Clark. Many thanks to James Clark for providing this excellent system and for his rapid response to bug fixes. Perhaps someday I will really understand troff footer traps.

I welcome electronic mail from any readers with comments, suggestions, or bug fixes.

*Tucson, Arizona*

*April 1992*

W. Richard Stevens

``rstevens@kohala.com``

``http://www.kohala.com/~rstevens``

Publish
-------

.. code:: ipython

   import glob
   import os
   import re
   import subprocess

   htmls = glob.glob("*.xhtml")
   # print(htmls)
   htmls.sort(key=lambda x: int(x[0:2]))

   # print(htmls)
   # for i, e in enumerate(htmls, start=1):
   #     os.rename(e, f"{i:02}.xhtml")

   for html in htmls:
       cmd = f"pandoc --wrap=none {html} -o {re.sub(r'xhtml$', 'org', html)}"
       assert cmd.endswith("org"), "not ends with org"
       subprocess.run(cmd, shell=True)

   print(os.listdir())

.. code:: ipython

   !ls 

.. code:: ipython

   !pandoc --wrap=none 00.Preface.org -o ~/Public/nikola_post/posts/读书评注:APUE.rst

Clean Chapters
--------------

.. code:: ipython

   import re
   import glob


   def clearup(filename):
       fp = open(filename, "r+")
       text = fp.read()
       text = re.sub(r"\\", "", text)
       text = re.sub(r"<<.+>>", "", text)
       text = re.sub(r".*:PROPERTIES:.*\n.*:CUSTOM_ID:.*\n.*:END:.*", "",
               text)

       text = re.sub(r"\*(\d\.\d)\*", "\g<1>", text)
       text = re.sub(r"\[\[.*Click here to view code image.*\]",   "", text)
       text = re.sub(r"\*\[\[.*\]\[([0-9]\.[0-9]*)\]\]\*", "\g<1>", text)
       # print(text[:100])
       fp.seek(0)
       fp.write(text)
       fp.close()

   clearup("07.org")

   # orgs = glob.glob("*.org")

.. code:: ipython

   import glob


   orgs = glob.glob("*.org")
   orgs = filter(lambda x: x[0].isdigit(), orgs)
   orgs = sorted(orgs, key=lambda x: x[0:2])[10:]
   # orgs.sort(key=lambda x: int(x[0:2]))

   for org in orgs:
       clearup(org)

Add chapter names
-----------------

.. code:: ipython

   print(cs)
   import glob
   import os

   orgs = glob.glob("*.org")
   orgs.sort(key=lambda x: x[:2])
   orgs = orgs[1:23]


   for old, new in td:
       os.rename(old, new)

   print(orgs)

.. code:: ipython

   # 忘记加后缀名
   # ! rm *.xhtml
   fs = os.listdir()
   fs = filter(lambda x: not x.endswith("org"), fs)
   print(list(fs))
   # map(lambda x: os.rename(x, f"{x}.org"), fs)
   ! ls
   # print(fs)

.. code:: ipython

   import os
   import glob
   import copy

   fs = os.listdir()
   fs = filter(lambda x: not x.endswith("org"), fs)
   fsc = copy.deepcopy(fs)
   print(list(fsc)[:5])
   #

.. code:: ipython

   map(lambda x: os.rename(x, x+'.org'), fs)
   ! ls | head -n 5

.. code:: ipython

   for f in fs:
       os.rename(f, f"{f}.org")
   ! ls | head -n 5

.. |image0| image:: ./Images/image01287.jpeg

