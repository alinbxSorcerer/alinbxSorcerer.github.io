   .. title: Debugging and Profiling Python Scripts
   .. slug:
   .. date: 2019-12-18 18:15:29 UTC+08:00
   .. tags: python, debug, profile,
   .. category: programming
   .. link:
   .. description:
   .. type: text

.. contents:: Table of Contents

0) Foreword
---------------------

Debugging and profiling play an important role in Python development. The debugger helps programmers to analyze the complete code. The debugger sets the breakpoints whereas the profilers run our code and give us the details of the execution time. The profilers will identify the bottlenecks in your programs. In this chapter, we'll learn about the pdb Python debugger, cProfile module, and timeit module to time the execution of Python code.

In this chapter, you'll learn about the following:

#. Python debugging techniques
#. Error handling (exception handling)
#. Debugger tools
#. Debugging basic program crashes
#. Profiling and timing programs
#. Making programs run faster

1) What is debugging?
---------------------

Debugging is a process that resolves the issues that occur in your code and prevent your software from running properly. In Python, debugging is very easy. The Python debugger sets conditional breakpoints and debugs the source code one line at a time. We'll debug our Python scripts using a pdb module that's present in the Python standard library.

Python debugging techniques
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To better debug a Python program, various techniques are available. We're going to look at four techniques for Python debugging:

#. print() statement: This is the simplest way of knowing what's exactly happening so you can check what has been executed.
#. **logging**: This is like a print statement but with more contextual information so you can understand it fully.
#. \*pdb* debugger: This is a commonly used debugging technique. The advantage of using pdb is that you can use pdb from the command line, within an interpreter, and within a program.
#. IDE debugger: IDE has an integrated debugger. It allows developers to execute their code and then the developer can inspect while the program executes.

2) Error handling (exception handling)
--------------------------------------

In this section, we're going to learn how Python handles exceptions. But first, what is an exception? An exception is an error that occurs during program execution. Whenever any error occurs, Python generates an exception that will be handled using a try…except block. Some exceptions can't be handled by programs so they result in error messages. Now, we are going to see some exception examples.

In your Terminal, start the python3 interactive console and we will see some exception examples:

.. code:: ipython

   Type "help", "copyright", "credits" or "license" for more information.
    >>>
    >>> 50 / 0
    Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
    ZeroDivisionError: division by zero

    >>>
    >>> 6 + abc*5
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'abc' is not defined
    >>>
    >>> 'abc' + 2
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: Can't convert 'int' object to str implicitly
    >>>
    >>> import abcd
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ImportError: No module named 'abcd'
    >>>

These are some examples of exceptions. Now, we will see how we can handle the exceptions.

Whenever errors occur in your Python program, exceptions are raised. We can also forcefully raise an exception using raise keyword.

Now we are going to see a try…except block that handles an exception. In the try block, we will write a code that may generate an exception. In the except block, we will write a solution for that exception.

The syntax for try…except is as follows:

.. code:: ipython

   try:
               statement(s)
   except:
               statement(s)

A try block can have multiple except statements. We can handle specific exceptions also by entering the exception name after the except keyword. The syntax for handling a specific exception is as follows:

.. code:: ipython

   try:
               statement(s)
   except exception_name:
               statement(s)

We are going to create an exception\ :sub:`example`.py script to catch ZeroDivisionError*.\* Write the following code in your script:

.. code:: ipython

   a = 35
   b = 57
   try:
       c = a + b
       print("The value of c is: ", c)
       d = b / 0
       print("The value of d is: ", d)

   except:
       print("Division by zero is not possible")

   print("Out of try...except block")

Run the script as follows and you will get the following output:

3) Debuggers tools
------------------

There are many debugging tools supported in Python:

#. winpdb
#. pydev
#. pydb
#. pdb
#. gdb
#. pyDebug

In this section, we are going to learn about pdb Python debugger. pdb module is a part of Python's standard library and is always available to use.

The pdb debugger
~~~~~~~~~~~~~~~~

The pdb module is used to debug Python programs. Python programs use pdb interactive source code debugger to debug the programs. pdb sets ``breakpoints`` and inspects the stack frames, and lists the source code.

Now we will learn about how we can use the pdb debugger. There are three ways to use this debugger:

#. Within an interpreter
#. From a command line
#. Within a Python script

We are going to create a pdb\ :sub:`example`.py script and add the following content in that script:

.. code:: ipython

   class Student:
       def __init__(self, std):
                   self.count = std

       def print_std(self):
                   for i in range(self.count):
                               print(i)
                   return
   if __name__ == '__main__':
       Student(5).print_std()

Using this script as an example to learn Python debugging, we will see how we can start the debugger in detail.

Within an interpreter
~~~~~~~~~~~~~~~~~~~~~

 To start the debugger from the Python interactive console, we are using run() or runeval().

Start your python3 interactive console. Run the following command to start the console:

Import our pdb\ :sub:`example` script name and the pdb module. Now, we are going to use run() and we are passing a string expression as an argument to run() that will be evaluated by the Python interpreter itself:

.. code:: ipython

   >>> import pdb_example
   >>> import pdb
   >>> pdb.run('pdb_example.Student(5).print_std()')
   > <string>(1)<module>()
   (Pdb)

To continue debugging, enter continue after the (Pdb) prompt and press *Enter*. If you want to know the options we can use in this, then after the (Pdb) prompt press the /Tab /key twice.

Now, after entering continue, we will get the output as follows:

.. code:: ipython

       >>> import pdb_example
       >>> import pdb
   In [15]: pdb.run("pdb_example.Student(5).print_std()"
       ...: )
   > <string>(1)<module>()
   (Pdb) continue
   0
   1
   2
   3
   4

.. code:: ipython

   from src import pdb_example
   import pdb
   pdb.run('pdb_example.Student(5).print_std()')

From a command line
~~~~~~~~~~~~~~~~~~~

The simplest and most straightforward way to run a debugger is from a command line. Our program will act as input to the debugger. You can use the debugger from command line as follows:

.. code:: ipython

   $ python3 -m pdb pdb_example.py

When you run the debugger from the command line, source code will be loaded and it will stop the execution on the first line it finds. Enter continue to continue the debugging. Here's the output:

.. code:: ipython

   student@ubuntu:~$ python3 -m pdb pdb_example.py
   > /home/student/pdb_example.py(1)<module>()
   -> class Student:
   (Pdb) continue
   0
   1
   2
   3
   4
   The program finished and will be restarted
   > /home/student/pdb_example.py(1)<module>()
   -> class Student:
   (Pdb)

Within a Python script
~~~~~~~~~~~~~~~~~~~~~~

The previous two techniques will start the debugger at the beginning of a Python program. But this third technique is best for long-running processes. To start the debugger within a script, use ``set_trace()``.

Now, modify your pdb\ :sub:`example`.py file as follows:

.. code:: ipython

   import pdb

   class Student:
       def __init__(self, std):
           self.count = std

       def print_std(self):
           for i in range(self.count):
               pdb.set_trace()
               print(i)
           return


   if __name__ == "__main__":
       Student(5).print_std()

Now, run the program as follows:

.. code:: ipython

   student@ubuntu:~$ python3 pdb_example.py
   > /home/student/pdb_example.py(10)print_std()
   -> print(i)
   (Pdb) continue
   0
   > /home/student/pdb_example.py(9)print_std()
   -> pdb.set_trace()
   (Pdb)

``set_trace()`` is a Python function, therefore you can call it at any point in your program.

So, these are the three ways by which you can start a debugger.

4) Debugging basic program crashes
----------------------------------

In this section, we are going to see the trace module. The trace module helps in tracing the program execution. So, whenever your Python program crashes, we can understand where it crashes. We can use trace module by importing it into your script as well as from the command line.

Now, we will create a script named trace\ :sub:`example`.py and write the following content in the script:

.. code:: ipython

   class Student:
       def __init__(self, std):
           self.count = std

       def print_std(self):
           for i in range(self.count):
               print(i)
           return
   if __name__ == '__main__':
       Student(5).print_std()

The output will be as follows:

.. code:: shell

   python -m trace --trace src/trace_example.py

So, by using trace –trace at the command line, the developer can trace the program line-by-line. So, whenever the  program crashes, the developer will know the instance where it crashes.

5) Profiling and timing programs
--------------------------------

Profiling a Python program means measuring an execution time of a program. It measures the time spent in each function. Python's cProfile module is used for profiling a Python program.

The cProfile module
~~~~~~~~~~~~~~~~~~~

As discussed previously, profiling means measuring the execution time of a program. We are going to use the cProfile Python module for profiling a program.

Now, we will write a cprof\ :sub:`example`.py script and write the following code in it:

.. code:: ipython

   mul_value = 0


   def mul_numbers(num1, num2):
       mul_value = num1 * num2
       print("Local Value: ", mul_value)
       return mul_value


   mul_numbers(58, 77)
   print("Global Value: ", mul_value)

Run the program and you will see the output as follows:

.. code:: shell

   python -m cProfile src/cprof_example.py

.. code:: python

   Local Value:  4466
   Global Value:  0
            6 function calls in 0.000 seconds

      Ordered by: standard name

      ncalls  tottime  percall  cumtime  percall filename:lineno(function)
           1    0.000    0.000    0.000    0.000 cprof_example.py:1(<module>)
           1    0.000    0.000    0.000    0.000 cprof_example.py:4(mul_numbers)
           1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
           2    0.000    0.000    0.000    0.000 {built-in method builtins.print}
           1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}

So, using cProfile, all functions that are called will get printed with the time spent on each function. Now, we will see what these column headings mean:

#. ncalls:* *Number of calls
#. **tottime**:* *Total time spent in the given function
#. percall: Quotient of tottime divided by ncalls
#. cumtime: Cumulative time spent in this and all subfunctions
#. percall: Quotient of cumtime divided by primitive calls
#. filename:lineno(function): Provides the respective data of each function

timeit
~~~~~~

timeit is a Python module used to time small parts of your Python script. You can call timeit from the command line as well as import the timeit module into your script. We are going to write a script to time a piece of code. Create a timeit\ :sub:`example`.py script and write the following content into it:

.. code:: ipython

   import timeit

   prg_setup = "from math import sqrt"
   prg_code = """
   def timeit_example():
               list1 = []
               for x in range(50):
                           list1.append(sqrt(x))
   """
   # timeit statement
   print(timeit.timeit(setup=prg_setup, stmt=prg_code, number=10000))

Using timeit, we can decide what piece of code we want to measure the performance of. So, we can easily define the setup code as well as the code snippet on which we want to perform the test separately. The main code runs 1 million times, which is the default time, whereas the setup code runs only once.

6) Making programs run faster
-----------------------------

There are various ways to make your Python programs run faster, such as the following:

#. Profile your code so you can identify the bottlenecks
#. Use built-in functions and libraries so the interpreter doesn't need to execute loops
#. Avoid using globals as Python is very slow in accessing global variables
#. Use existing packages

Summary
-------

In this chapter, we learned about the importance of debugging and profiling programs. We learned what the different techniques available for debugging are. We learned about the pdb Python debugger and how to handle exceptions. We learned about how to use the cProfile and timeit modules of Python while profiling and timing our scripts. We also learned how to make your scripts run faster.

In the next chapter, we are going to learn about unit testing in Python. We are going to learn about creating and using unit tests.

Questions
~~~~~~~~~

#. To debug a program, which module is used? 
#. Check how to use ipython along with all aliases and magic functions.
#. What is \*Global interpreted lock* (\ **GIL**)?
#. What is the purpose of the PYTHONSTARTUP, PYTHONCASEOK, PYTHONHOME, and PYTHONSTARTUP environment variables?
#. What is the output of the following code? a) [0], b) [1], c) [1, 0], d) [0, 1].

.. code:: ipython

   def foo(k):
       k = [1]
   q = [0]
   foo(q)
   print(q)

#. Which of the following is an invalid variable? a) my\ :sub:`string1` b) 1st\ :sub:`string` c) foo d) \_

Answer:
~~~~~~~

#. To debug the program, the pdb module is used.
#. a) Before running ipython3, install using sudo apt-get install ipython3. b) %lsmagic.
#. A global interpreter lock is a mechanism used in computer language interpreters to synchronize the execution of threads so that only one native thread can execute at a time
#. Following are the answers:

a) PYTHONPATH: It has a role similar to PATH. This variable tells the Python interpreter where to locate the module files imported into a program. It should include the Python source library directory and the directories containing Python source code. PYTHONPATH is sometimes preset by the Python installer.

b) PYTHONSTARTUP: It contains the path of an initialization file containing Python source code. It is executed every time you start the interpreter. It is named as .pythonrc.py in Unix and it contains commands that load utilities or modify PYTHONPATH. c) PYTHONCASEOK: It is used in Windows to instruct Python to find the first case-insensitive match in an import statement. Set this variable to any value to activate it. d) PYTHONHOME: It is an alternative module search path. It is usually embedded in the PYTHONSTARTUP or PYTHONPATH directories to make switching module libraries easy.

#. Answer: [0]. A new list object is created in the function and the reference is lost. This can be checked by comparing the ID of k before and after k = [1].
#. Answer: b. Variable names should not start with a number.

Further reading
~~~~~~~~~~~~~~~

-  How to handle GIL problems in python: https://realpython.com/python-gil/

-  Check how to use pdb module in command line: https://fedoramagazine.org/getting-started-python-debugger/
