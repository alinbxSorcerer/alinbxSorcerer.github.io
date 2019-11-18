   .. title: Regex Summary
   .. slug: regex-summary
   .. date: 2017-11-28 20:53:29 UTC+08:00
   .. tags: regex, emacs, python, bash
   .. category: programming
   .. link:
   .. description: Summarize regex
   .. type: text

Wildcards
=========

总结基本符号
``shopt -s gotglob.``
``*?[^]{}`` 记住这5种基本符号便可.

-  http://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm

Standard Wildcards (globbing patterns)

? (question mark)

this can represent any single character. If you specified something at the command line like "hd?" GNU/Linux would look for hda, hdb, hdc and every other letter/number between a-z, 0-9.

[ ] (square brackets)

specifies a range. If you did m[a,o,u]m it can become: mam, mum, mom if you did: m[a-d]m it can become anything that starts and ends with m and has any character a to d inbetween. For example, these would work: mam, mbm, mcm, mdm. This kind of wildcard specifies an “or” relationship (you only need one to match).

{ } (curly brackets)

terms are separated by commas and each term must be the name of something or a wildcard. This wildcard will copy anything that matches either wildcard(s), or exact name(s) (an “or” relationship, one or the other).

For example, this would be valid:

cp {**.doc,**.pdf} ~

This will copy anything ending with .doc or .pdf to the users home directory. Note that spaces are not allowed after the commas (or anywhere else).
[^] or [!]

This construct is similar to the [ ] construct, except rather than matching any characters inside the brackets, it'll match any character, as long as it is not listed between the [ and ]. This is a logical NOT. For example rm myfile[!9] will remove all myfiles\* (ie. myfiles1, myfiles2 etc) but won't remove a file with the number 9 anywhere within it's name.

\\ (backslash)

is used as an "escape" character, i.e. to protect a subsequent special character. Thus, "\\” searches for a backslash. Note you may need to use quotation marks and backslash(es).

Regex New Map
=============

#+caption Metacharacters

+-------------+-------------+-------------+-------------+-------------+
| Categories  |             |             |             | Counter     |
+=============+=============+=============+=============+=============+
| dot and     | ^.[^]$ or   |             |             | 5           |
| classes     |             |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| capture and | ()          | \* {}, +, ? | or + ? only | 7           |
| repetation  |             |             | in ERE      |             |
+-------------+-------------+-------------+-------------+-------------+
| back-refere | :raw-latex: |             |             | 1           |
| nce         | `\n`        |             |             |             |
+-------------+-------------+-------------+-------------+-------------+
| Sum:        |             |             |             | 13          |
+-------------+-------------+-------------+-------------+-------------+

这个Meta-class最成功的一点是将 back-reference放在里面.
class, [:space:] [ :raw-latex:`\t`:raw-latex:`\r`:raw-latex:`\n`:raw-latex:`\f`]
[:blank:] [ :raw-latex:`\t`]

#+Intermediate topics

+-----------------------+-----------------------+-----------------------+
| capturing             | a(bc), a(?:bc),       | python的格式          |
|                       | a(?<foo>bc)           |                       |
|                       | (?P<name>) (?P=name)  |                       |
+=======================+=======================+=======================+
| bracket               | [a-z]                 |                       |
+-----------------------+-----------------------+-----------------------+
| greedy and lazy       | <.+?>, <[^<>]+>       |                       |
+-----------------------+-----------------------+-----------------------+

#+Advanced topics

+-----------------------------------+-----------------------------------+
| boundaries                        | :raw-latex:`\babc`:raw-latex:`\b` |
+===================================+===================================+
| back-reference                    | \\1, \\?<foo><foo> k for keyword  |
+-----------------------------------+-----------------------------------+
| look-ahead                        | d(?=r) look r's ahead,            |
|                                   | 从r前面找d, followed              |
+-----------------------------------+-----------------------------------+
| look-behind                       | (?<=r)d 从r的后面找d,             |
|                                   | d小于r的意思, 在其后, proceeded,  |
|                                   | 按照习惯大于等于写在前面          |
+-----------------------------------+-----------------------------------+
| Negotive operator                 | d(?!r) (?<!r)d (大括号代表方向.)  |
+-----------------------------------+-----------------------------------+

(?=r)注意这里的指向与format的指向是相反的.

注意这里的三个flags

#+name old map

+-----------+-----------+-----------+-----------+-----------+-----------+
| Scope     | Individua | Group     | Repetitio | Flags     |           |
|           | l         |           | n         |           |           |
+===========+===========+===========+===========+===========+===========+
| Basic     | wide-card | ^$        | \* + ?    | re.A      |           |
| Regex     | dot: .    |           |           | ASCII     |           |
|           |           |           |           | only      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | character | :raw-late |           | re.M      |           |
|           | -class:[] | x:`\b`    |           | Multiple- |           |
|           |           |           |           | line      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Negating: |           |           | re.S dot  |           |
|           | ^         |           |           | matches   |           |
|           |           |           |           | all       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Range:    |           |           | re.X      |           |
|           | rang      |           |           | verbose   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| Extensive | \`\, ()   |           | ? +       |           |           |
|           | 不需要escape |        |           |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+

(?P<name>…)

+-----------------------+-----------------------+-----------------------+
| Context of reference  | Ways to reference it  |                       |
| to group “quote”      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ——————————————————-   | —————————————–        |                       |
+-----------------------+-----------------------+-----------------------+
| in the same pattern   | \`(?P=quote)\` as     |                       |
| itself                | shown                 |                       |
+-----------------------+-----------------------+-----------------------+
|                       | \`\1\`                |                       |
+-----------------------+-----------------------+-----------------------+
| when processing match | \`m.group('quote')\`  |                       |
| object **m**          |                       |                       |
+-----------------------+-----------------------+-----------------------+
|                       | \`m.end('quote') \`   |                       |
|                       | (etc.)                |                       |
+-----------------------+-----------------------+-----------------------+
| in a string passed to | \`g<quote>\`          |                       |
| the **repl** argument |                       |                       |
| of \`re.sub()\`       |                       |                       |
+-----------------------+-----------------------+-----------------------+
|                       | \`g<1>\`              |                       |
+-----------------------+-----------------------+-----------------------+
|                       | \\1                   |                       |
+-----------------------+-----------------------+-----------------------+

.. code:: python

   # 需要注意的一点是, findall只返回匹配的group
   {   'methods': {['search', 'match', 'findall', 'finditer'],
                     ['split', 'sub']}, #modify and replcae
       'match_object': ['group','groups', 'groupdict','start', 'end', 'span',]
       }
   # Performing matches
   match(), search(), findall, findinter()
   # module level functions
   match, search, findall, sub,
   #complilation Flags
   DOTALL(S-single), IGNORECASE(I), MULTIPLELINE(M), VERBOSE(X)
   #Modify strings
   re.split

Regex in Python
===============

CLOSED: [2019-06-01 Sat 14:10]

| :LOGBOOK:
| `python's regex summary in stackoverflow <https://stackoverflow.com/a/48207102/7301792>`__
| `what does P mean in (?<name>) <https://stackoverflow.com/a/10060065/7301792>`__
| `regex collections <https://stackoverflow.com/questions/22937618/reference-what-does-this-regex-mean/22944075#22944075>`__
| `python regex how to <https://docs.python.org/3/howto/regex.html#regex-howto>`__

.. code:: python

   Besides the performance.

   Using `compile` helps me to distinguish the concepts of
   1. module(re)*,
   2. regex object
   3. match object   #重要的是我总结的三个概念. 三个层级.
   python的特殊之处是在named group上的纠结,
   When I started learning regex

       #regex object
       regex_object = re.compile(r'[a-zA-Z]+')
       #match object
       match_object = regex_object.search('1.Hello')
       #matching content
       match_object.group()
       output:
       Out[60]: 'Hello'
       V.S.
       re.search(r'[a-zA-Z]+','1.Hello').group()
       Out[61]: 'Hello'

   As a complement, I made an exhaustive cheatsheet of module `re` for
       regex = {
       'brackets':{'single_character': ['[]', '.', {'negate':'^'}], 试图从少到多去解析
                   'capturing_group' : ['()','(?:)', '(?!)' '|', '\\', 'backreferences and named group'],
                   'repetition'      : ['{}', '*?', '+?', '??', 'greedy v.s. lazy ?']},
       'lookaround' :{'lookahead'  : ['(?=...)', '(?!...)'],
                  'lookbehind' : ['(?<=...)','(?<!...)'],
                  'caputuring' : ['(?P<name>...)', '(?P=name)'这是应用, '(?:)'],},
       'escapes':{'anchor'         : ['^', '$', 'b'],
                 'non_printable'   : ['\n', '\t', '\r', '\f', '\v'],
                 'shorthand'       : ['\d', '\w', '\s']},
       'methods': {['search', 'match', 'findall', 'finditer'],
                     ['split', 'sub']}, #modify and replcae
       'match_object': ['group','groups', 'groupdict','start', 'end', 'span',]
       }
   # Performing matches
   match(), search(), findall, findinter()
   # module level functions
   match, search, findall, sub,
   #complilation Flags
   DOTALL(S-single), IGNORECASE(I), MULTIPLELINE(M), VERBOSE(X)
   #Modify strings
   re.split
   #Search and Replace
   >>> p = re.compile('section{ (?P<name> [^}]* ) }', re.VERBOSE)
   >>> p.sub(r'subsection{\1}','section{First}')
   'subsection{First}'
   >>> p.sub(r'subsection{\g<1>}','section{First}')
   'subsection{First}'
   >>> p.sub(r'subsection{\g<name>}','section{First}')
   'subsection{First}' #三种不同的形式.

.. code:: python

   import re
   p = re.compile('section{ (?P<name> [^}]* ) }', re.VERBOSE)
   res = p.search('section{First}')
   res2 = p.findall('section{First}')
   res3 = p.finditer('section{First}')
   print(res)
   print(res2)
   print(list(res3))
   text = "He was carefully disguised but captured quickly by police."
   for m in re.finditer(r"\w+ly", text):
       print('%02d-%02d: %s' % (m.start(), m.end(), m.group(0)))
   # 这里关键的一点, 在sub中用的是Search

Grep examples
=============

Remove the blank lines
----------------------

.. code:: zsh

   Remove the blank lines.
   $ grep . 2daygeek.txt
   or
   $ grep -Ev "^$" 2daygeek.txt
   or
   $ grep -v -e '^$' 2daygeek.txt
   2daygeek.com is a best Linux blog to learn Linux.
   It's FIVE years old blog.
   This website is maintained by Magesh M, it's licensed under CC BY-NC 4.0.
   He got two GIRL babes.
   Her names are Tanisha & Renusha.

Remove lines containing logging
-------------------------------

[bash - Remove lines which contain 'logging' - Ask Ubuntu](\ https://askubuntu.com/questions/1127589/remove-lines-which-contain-logging?noredirect=1#comment1866586_1127589)

::

   grep -v logging twoSum.py > logging-new
   sed "/^\t$name/d" in-file
   sed -i '/adf\.ly/d' inputfile

Sed
===

.. _remove-the-blank-lines-1:

Remove the blank lines
----------------------

$ sed '/^$/d' 2daygeek.txt

Emacs Regular Expression Syntax
===============================

| Here is the syntax used by Emacs for regular expressions. Any character matches itself, except for the list below.
| The following characters are special : . \* + ? ^ $ \\ [
| Between brackets [], the following are special : ] - ^
| Many characters are special when they follow a backslash – see below.

.. code:: elisp

   .        any character (but newline)
   *        previous character or group, repeated 0 or more time
   +        previous character or group, repeated 1 or more time
   ?        previous character or group, repeated 0 or 1 time
   ^        start of line
   $        end of line
   [...]    any character between brackets
   [^..]    any character not in the brackets
   [a-z]    any character between a and z
   \        prevents interpretation of following special char
   \|       or
   \w       word constituent
   \b       word boundary
   \sc      character with c syntax (e.g. \s- for whitespace char)
   \( \)    start/end of group
   \&lt; \&gt;    start/end of word (faulty rendering: backslash + less-than and backslash + greater-than)
   \_< \_>  start/end of symbol
   \` \'    start/end of buffer/string
   \1       string matched by the first group
   \n       string matched by the nth group
   \{3\}    previous character or group, repeated 3 times
   \{3,\}   previous character or group, repeated 3 or more times
   \{3,6\}  previous character or group, repeated 3 to 6 times
   \=       match succeeds if it is located at point

| \*?, +?, and ?? are non-greedy versions of \*, +, and ? – see NonGreedyRegexp. Also, W, :raw-latex:`\B`, and ⪼ match any character that does not match :raw-latex:`\w`, :raw-latex:`\b`, and ≻.
| Characters are organized by category. Use C-u C-x = to display the category of the character under the cursor.

.. code:: elisp

   \ca      ascii character
   \Ca      non-ascii character (newline included)
   \cl      latin character
   \cg      greek character

Here are some syntax classes, also known as character classes, that can be used between brackets, e.g. [[:upper:]\|[:digit:]\.].

.. code:: elisp

   [:digit:]  a digit, same as [0-9]
   [:alpha:]  a letter (an alphabetic character)
   [:alnum:]  a letter or a digit (an alphanumeric character)
   [:upper:]  a letter in uppercase
   [:lower:]  a letter in lowercase
   [:graph:]  a visible character
   [:print:]  a visible character plus the space character
   [:space:]  a whitespace character, as defined by the syntax table, but typically [ \t\r\n\v\f], which includes the newline character
   [:blank:]  a space or tab character
   [:xdigit:] an hexadecimal digit
   [:cntrl:]  a control character
   [:ascii:]  an ascii character

Syntax classes:

.. code:: elisp

   \s-   whitespace character        \s/   character quote character
   \sw   word constituent            \s$   paired delimiter
   \s_   symbol constituent          \s'   expression prefix
   \s.   punctuation character       \s<   comment starter
   \s(   open delimiter character    \s>   comment ender
   \s)   close delimiter character   \s!   generic comment delimiter
   \s"   string quote character      \s|   generic string delimiter
   \s\   escape character

Publish
=======

.. code:: ipython

   ! ls ~/Public/nikola_post/posts
   ! pwd
   ! pandoc --wrap=preserve regex-offprint.org  -o ~/Public/nikola_post/posts/Regex-Summary.rst

.. code:: commonlisp

   (print (replace-regexp "war" "negotiation" "trade war")))

::

   nil

