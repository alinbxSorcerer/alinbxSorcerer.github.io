   .. title: 费马小定理
   .. slug:  testing_for_primality
   .. date: 2019-12-18 11:53:29 UTC+08:00
   .. tags: python, math, algorithms, primality
   .. category: programming
   .. link:
   .. description:
   .. type: text

.. contents:: Table of Contents

=====================

This section describes two methods for checking the primality of an integer n , one with order of growth Θ(n**1/2) , and a “probabilistic” algorithm with order of growth Θ (log n) . The exercises at the end of this section suggest programming projects based on these algorithms.

1.Searching for divisors
----------------------

Since ancient times, mathematicians have been fascinated by problems concerning prime numbers, and many people have worked on the problem of determining ways to test if numbers are prime. One way to test if a number is prime is to find the number’s divisors. The following program finds the smallest integral divisor (greater than 1) of a given number n . It does this in a straightforward way, by testing n for divisibility by successive integers starting with 2.

.. code:: ipython

   def smallest_divisor(n):
       return find_divisor(2, n) # 从2开始.

   def find_divisor(test_divisor, n):
       if test_divisor ** 2 > n: return n  # terminating point
       if n % test_divisor == 0: return test_divisor
       else: return find_divisor(test_divisor+1, n) # 这是iteration
   # 需要用tail recursion的时候. 就是iteration.
   print(smallest_divisor(30))

.. code:: ipython

   def smallest_divisor(n):
       for divisor in range(2, n): #incremant
           if divisor ** 2 > n: return n
           if n % divisor == 0: return divisor # 就只有这两种情况

   print(smallest_divisor(76))

因为如果不是质数一定可以分解为两个数字factor.

We can test whether a number is prime as follows: n is prime if and only if n is its own smallest divisor.

The end test for find-divisor is based on the fact that if n is not prime it must have a divisor less than or equal to n(1/2). This means that the algorithm need only test divisors between 1 and n. Consequently, the number of steps required to identify n as prime will have order of growth Θ(n1/2) .

2.The Fermat Test
---------------

The Θ(log n) primality test is based on a result from number theory known as Fermat's Little Theorem.45

Fermat's Little Theorem: If n is a prime number and a is any positive integer less than n, then a raised to the nth power is congruent to a modulo n.

(Two numbers are said to be ``congruent`` modulo n if they both have the same remainder when divided by n. The remainder of a number a when divided by n is also referred to as the remainder of a modulo n, or simply as a modulo n.)

If n is not prime, then, in general, most of the numbers a< n will not satisfy the above relation. This leads to the following algorithm for testing primality: Given a number n, pick a random number a < n and compute the remainder of an modulo n. If the result is not equal to a, then n is certainly not prime. If it is a, then chances are good that n is prime. Now pick another random number a and test it with the same method. If it also satisfies the equation, then we can be even more confident that n is prime. By trying more and more values of a, we can increase our confidence in the result. This algorithm is known as the Fermat test.


To implement the Fermat test, we need a procedure that computes the exponential of a number modulo another number:

.. code:: ipython

   def expmod(base, exp, m):
       # base case
       if exp == 0: return 1
       if evenp(exp):
           return expmod(base, exp/2, m) ** 2 % m
       else:
           return base * expmod(base, exp-1, m) % m

   def evenp(n):
       return n % 2 == 0

   print(expmod(2, 8, 7))

::

   4

This is very similar to the fast-expt procedure of section 1.2.4. It uses successive squaring, so that the number of steps grows logarithmically with the exponent.[fn:1-2-46]

The Fermat test is performed by choosing at random a number a between 1 and n - 1 inclusive and checking whether the remainder modulo n of the nth power of a is equal to a. The random number a is chosen using the procedure random, which we assume is included as a primitive in Scheme. Random returns a nonnegative integer less than its integer input. Hence, to obtain a random number between 1 and n - 1, we call random with an input of n - 1 and add 1 to the result:

.. code:: ipython

   def fermat_test(n):
       def try_it(a):
           return expmod(a, n, n) == a
       try_it(1 + random.randint(n-1)) # 这里的处理很妙

The following procedure runs the test a given number of times, as specified by a parameter. Its value is true if the test succeeds every time, and false otherwise.

.. code:: ipython

   def fast_prime(n, times): #recursive的方法可以协助思考, 思考起点和终点.
       if times == 0: return True
       if fermat_test(n): return fast_prime(n, times -1)
       else: False

.. code:: ipython

   import random

   def fermat_test(n, times): #expmod, 幂模运算.

       # Implementation uses the Fermat Primality Test
       # If number is even, it's a composite number, prime number and composite number.
       # quick return
       if n < 2: return False

       for i in range(times):
           a = random.randint(1, n-1)

           if pow(a, n, n) != a: # test
               return False
       return True

   print(fermat_test(112, 10))


3.Primes Sieve
------------

.. code:: ipython

   def primes(n):
       if n < 2:
           return None
       primes_sieve = [True] * (n + 1)
       primes_sieve[1] = False
       primes_sieve[0] = False

       # sieve
       for i in range(2, int(n ** 0.5) + 1):  # 要包含n這個數字.
           if primes_sieve[i]:
               for j in range(i * i, n + 1, i):  # mark the multiplies, 本數不能劃掉.
                   primes_sieve[j] = False

       return [i for i, p in enumerate(primes_sieve) if p]

   def count_primes(n):
       if n < 2:
           return None
       primes_sieve = [True] * (n + 1)
       primes_sieve[1] = False
       primes_sieve[0] = False

       # sieve
       for i in range(2, int(n ** 0.5) + 1):  # 要包含n這個數字.
           if primes_sieve[i]:
               for j in range(i * i, n + 1, i):  # mark the multiplies, 本數不能劃掉.
                   primes_sieve[j] = False

       return primes_sieve.count(True)

