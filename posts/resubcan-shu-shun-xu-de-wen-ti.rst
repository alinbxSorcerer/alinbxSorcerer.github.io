.. title: Re.sub参数顺序的问题
.. slug: resubcan-shu-shun-xu-de-wen-ti
.. date: 2019-05-08 17:17:55 UTC+08:00
.. tags: python, regex
.. category: programming
.. link: 
.. description: 
.. type: text


提出问题
========

在写 ``re.sub`` 或者 ``re.subn`` 的时候, 常常会不太确定引用参数的顺序,
需要中断的时间查看提示或者help文档. 比如 ``input_string`` 'trade war'
修改为 ``trade negotiation``

.. code:: python

   In [17]: re.sub("war", "negotiation", "trade war")
   Out[17]: 'trade negotiation'
   sub(pattern, repl, string, count=0, flags=0)
   def sub(pattern, repl, string, count=0, flags=0):
       return _compile(pattern, flags).sub(repl, string, count)#首先处理regex-pattern

分析问题
========

与 ``string.methods`` 比较. ``pattern``
是与source(input\ :sub:`string`)的匹配的内容,
repl是修改后的内容(destination), 这里顺序与=str.replace=是一致.

::

   replace(self, old, new, count=-1, /)

old 来自source, new是输出到destination结果中.

.. code:: python

   In [16]: "trade war".replace("war", "negotiation")
   Out[16]: 'trade negotiation'

``sed`` 也遵循同样的模式.

.. code:: bash

   s/regexp/replacement/
          Attempt  to  match  regexp  against  the  pattern  space.   If successful, replace that portion matched with
          replacement.  The replacement may contain the special character & to refer to that portion  of  the  pattern
          space  which  matched,  and  the  special  escapes \1 through \9 to refer to the corresponding matching sub-
          expressions in the regexp.

regex-pattern匹配 source 数据中的内容,
replacement则是替换后输出到destination结果中.

.. code:: shell

   $ echo 'trade war' | sed "s/war/negotiation/g"
   trade negotiation

其他的Text Processing

.. code:: bash

   $ echo "trade-war" | tr "-" "\n"
   trade
   war
      tr [OPTION]... SET1 [SET2]

SET1 is from the source, SET2 is the result of the destination after
been processed.

总结这种模式和思维惯例:

::

   function source destination

Text Processing如此,

File Handling的utilities遵循同样的模式.

.. code:: bash

   mv [OPTION]... SOURCE... DIRECTORY
   cp [OPTION]... [-T] SOURCE DEST
   ln [OPTION]... Source... DIRECTORY
   rsync [OPTION...] SRC... [DEST]
   scp  SRC... DEST
   dd if=/dev/{{source_drive}} of=/dev/{{dest_drive}}

例外的情况是tar. =tar=是将目标放在前面.

再回头看 ``re.sub``

.. code:: python

   re.sub(pattern, repl, string)
   #扩展后
   re.sub(pattern_from_source, replacement_to_result, source_data)

三个参数中 ``pattern_from_source``, ``replacement_to_result`` ,
``source_data``
的最后一个是=source\ :sub:`data`\ ``, 将source放置在最后. =grep`` 与
=sed=都遵循同样的模式

.. code:: bash

   sed 's/{{regex}}/{{replace}}/' {{filename}}
   grep [OPTIONS] -e PATTERN ... [FILE...] #grep regex source

例外的情况是=find=

::

   find [-H] [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]
   find [Option] source pattern

总结:
=====

Data Stream Processing和File Handling遵循 ``subroutine src dst``
模式.两个例外的情况是=tar and find=

这个问题之所以值得探讨,是因为涉及底层的方法论和工作模式.
