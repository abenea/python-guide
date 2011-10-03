Introduction
============

Goals
-----

This guide will explain some basic python concepts through a series of
exercises. However, it's far from complete and cannot teach you Python. You
should read the `The Python Tutorial`_ and use the `Library Reference`_ whenever
necessary.

.. _The Python Tutorial: http://docs.python.org/tutorial/
.. _Library Reference: http://docs.python.org/library/index.html


Humble Beginnings
-----------------

You don't need type declarations. To create a variable simply assign to it::

  width = 10

To use functions from modules use the ``import`` keyword::

  import math
  a = math.sqrt(7)
  print int(a)

In contrast with C and Java, Python uses indentation to delimit blocks::

  x = 1
  if x == 0:
      print 'zero'
  elif x % 2:
      print 'odd'
  else:
      print 'even'

Indented blocks are always preceded by a statement which ends in a colon
(``:``). You should always use 4 spaces for indentation (`PEP8`_).

.. _PEP8: http://www.python.org/dev/peps/pep-0008/

The most common data structure in Python is the list (similar to the C++
``std::dequeue``)::

  x = [1, 2]
  x.append(3)
  print [1, 2, 3]

Another common data structure is the tuple (an immutable list)::

  x = (1, 2)
  assert x[1] == 1

To iterate over a sequence you can use the ``for`` statement::

  l = [1, 3, 5]
  for x in l:
      print x

If you just want to iterate over a range of integers, you can create with the
range function::

  for i in range(len(l)):
      print "l[%d] = " % (i, l[i])

This could be also written as::

  for idx, val in enumerate(l):
    print "l[%d] = %d" % (idx, val)


Exercise
~~~~~~~~

Define a function which returns *True* if a number is prime and
*False* otherwise.

The function definition should start with::

  def isprime(n):

**Unit test**::

  def test_primes():
      assert not isprime(1)
      assert isprime(2)
      assert isprime(3)
      assert not isprime(4)
      assert isprime(97)


Comprehensions
--------------

**Exercise**: Define a function which returns the prime numbers smaller than *n*.

The function definition should start with::

  def primesuntil(n):

**Unit test**::

  def test_primesuntil():
      assert primesuntil(10) == [2, 3, 5, 7]



Something Useful
----------------

**Exercise**: Implement binary search using a recursive function. The function
definition should start with::

  def bsearch(l, x):

where *x* is the element being searched and *l* is the list. The return value
must satisfy ``l[bsearch(l, x)] == x`` or be *None* if *x* is not found in *l*.

**Unit test**::

  def test_bsearch():
      assert bsearch([], 0) is None
      assert bsearch([0], 0) == 0
      assert bsearch([0], 1) is None
      for i in range(11):
          assert bsearch(range(11), i) == i
