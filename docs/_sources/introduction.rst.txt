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
  help(math.sqrt)
  print int(a)

The most common data structure in Python is the list (similar to the C++
``std::vector``, but without type constraints)::

  x = [1, 2]
  a, b = x
  assert a == 1
  assert b == 2
  x.append(3)
  print [1, 2, 3]
  assert len(x) == 3

Another common data structure is the tuple (an immutable list)::

  x = (1, 2)
  assert x[1] == 2

Lists, tuples and strings are sequences. More information about sequences can be
found in `Sequence Types
<http://docs.python.org/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange>`_.

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

Python has a :func:`bool` type with two values: :data:`True` and :data:`False`.
You can also use any sequence as a boolean value in ``if`` or ``while``
conditions (empty sequences are :data:`False`, non-empty sequences are
:data:`True`).

To iterate over a sequence you can use the ``for`` statement::

  l = [1, 3, 5]
  for x in l:
      print x

If you just want to iterate over a range of integers, you can create with the
:func:`range` function::

  assert range(3) == [0, 1, 2]
  assert range(1, 3) == [1, 2]

  for i in range(len(l)):
      print "l[%d] = %d" % (i, l[i])

This could be also written as::

  for idx, val in enumerate(l):
    print "l[%d] = %d" % (idx, val)

To define a function use ``def``::

  def even(x):
      return x % 2 == 0

If you don't explicitly ``return`` from a function, the caller will receive the
special value :data:`None`.

You can have a variable number of arguments::

  def csv(*args):
      return ', '.join(args)

  print csv('a', 'b', 'c')

You can also call a function with arguments from a list object::

  print csv(*['a', 'b', 'c'])

Exercise
~~~~~~~~

Define a function which returns :data:`True` if a number is prime and
:data:`False` otherwise.

.. function:: isprime(n)

**Unit test**::

  def test_primes():
      assert not isprime(1)
      assert isprime(2)
      assert isprime(3)
      assert not isprime(4)
      assert isprime(97)


Functional Style
----------------

If you want to write code in a functional style, you have access to the usual
tools:

- :func:`map`
- :func:`filter`
- :func:`reduce`

However Python also provides list comprehensions to replace :func:`map` and
:func:`filter` with a syntax closer to the mathematical set notation::

  f1 = filter(lambda x: x % 2 == 0, range(10))
  f2 = [x for x in range(10) if x % 2 == 0]
  assert f1 == f2

  mf1 = map(lambda x: x*x, filter(lambda x: x % 2 == 0, range(10)))
  mf2 = [x*x for x in range(10) if x % 2 == 0]
  assert mf1 == mf2

List comprehensions can have multiple ``for`` and ``if`` clauses::

  print [(x, y) for x in range(3) for y in range(3)]

Exercise
~~~~~~~~
Define a function which returns the prime numbers smaller than *n*.

.. function:: primesuntil(n)

**Unit test**::

  def test_primesuntil():
      assert primesuntil(10) == [2, 3, 5, 7]


Something Useful
----------------

The Python equivalent of C/C++ ``NULL`` is :data:`None`. You should always use
the ``is`` (or ``is not``) operator to compare things with :data:`None`.  ``is``
tests for object identity (think of it as pointer comparison).

You can get a sub-sequence from a sequence using the slice notation::

  a = range(10)
  assert a[1:3] == range(1, 3)

You can omit one or both ends of the slice range, which means the beginning or
the end of the sequence is used::

  assert a[:3] == range(3)
  assert a[3:] == range(3, 10)
  assert a[:] == a

Note that the return value of a slice is a shallow copy::

  assert a[:] is not a

Negative indexes count backwards from the end of the sequence::

  assert a[-1] == a[9]
  assert a[-3:] == range(7, 10)


Exercise
~~~~~~~~

Implement binary search using a recursive function. The function definition
should start with::

  def bsearch(l, x):

where *x* is the element being searched and *l* is the list. The return value
must satisfy ``l[bsearch(l, x)] == x`` or be :data:`None` if *x* is not found in *l*.

You can use slicing to avoid defining a helper function.

**Unit test**::

  def test_bsearch():
      assert bsearch([], 0) is None
      assert bsearch([0], 0) == 0
      assert bsearch([0], 1) is None
      for i in range(11):
          assert bsearch(range(11), i) == i


.. _mappings:

Mappings
--------

Another important built-in data type is :class:`dict`, similar to the C++
``std::map`` or the Java ``HashMap``::

  emptydict = {}
  specs = {'color': 'black', 'size': 3.5}

If the keys are strings that conform to Python identifier naming rules, you can
use the :class:`dict` constructor to avoid quoting them::

  specs = dict(color='black', size=3.5)

::

  assert len(specs) == 2
  assert specs['color'] == 'black'

  print specs['price']
  # raise a KeyError exception if the key is not found
  print spec.get('price')
  # return None if key is not found

  assert 'color' in specs
  assert 'price' not in specs
  del specs['color']
  assert 'color' not in specs
  specs['color'] = 'green'
  print specs.keys()
  print specs.values()

  print list(specs.iteritems())
  for k, v in specs.iteritems():
      print k, '=', v

You can call a function by specifying the name of some parameters together with
their value (keyword arguments)::

  def print_report(rows, columns, color='red'):
      print "A %s report with %d rows and %d columns" % (color, rows, columns)

  print_report(1, 1)
  print_report(1, 1, 'black')
  print_report(color='blue', columns=10, rows=2)
      
A function can receive variable keyword arguments (*kwargs* is a
:class:`dict`)::

  def map2str(**kwargs):
      tmp = ["%s=%s" % (str(k), str(v)) for k, v in kwargs.iteritems()]
      return ', '.join(tmp)

  print map2str(size=10, color='black')

  def apply_function(f, *args, **kwargs):
      return f(*args, **kwargs)

Exercise
~~~~~~~~

Write a word counter function.

.. function:: word_count(text, case_sensitive=True)

The return value should be a :class:`dict` which maps (whitespace-separated)
words to the number of occurrences in *text*.

Hint: read the documentation for `String Methods
<http://docs.python.org/library/stdtypes.html#string-methods>`_ and
:class:`dict`.

**Unit test**::

  def test_word_count():
      assert word_count("a b c") == dict(a=1, b=1, c=1)
      assert word_count("a b a") == dict(a=2, b=1)
      assert word_count("a b A") == dict(a=1, b=1, A=1)
      assert word_count("a b A", False) == dict(a=2, b=1)
