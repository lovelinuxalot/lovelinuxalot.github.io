---
layout: post
title:  "Python Doctest!"
date:   2020-07-18 16:43:32 +0200
categories: Python Testing
---
Python doctest is a useful tool for testing functions during development of the program. It is a unique Python test 
framework, that turns documented Python statements into test cases. Doctest can be written in two places:
    1. Directly in the docstrings of a function inside a module
    2. In Separate text files. This is good for better organization of doctest cases
    
The doctest module searches for examples (lines starting with “>>>”) and runs them as if they were interactive sessions 
entered into a Python shell. The subsequent lines, until the next “>>>” or a blank line, contain the expected output. 
A doctest will fail if the actual output does not match the expected output verbatim (e.g., string equality).

The doctest module is installed out of the box. For reports additional package, xmlrunner and unittest-xml-reporting 
should be installed with pip

A sample function for multiplying two numbers:
{% highlight python %}
def multiply(a,b):
    return a*b
{% endhighlight %}

With doctest, the code will look like:
{% highlight python %}
def multiply(a,b):
    """
    >>> multiply(4,3)
    12
    >>> multiply('a', 3)
    'aaa'
    """
    return a*b
{% endhighlight %}

There is also a possibility to add the code as it is in the file and separate the test code on a different text file 
under doctest directory. Another function similarly will look like this:
{% highlight python %}
def add(a,b):
    return a+b
{% endhighlight %}

Create a new directory {% highlight python %}mkdir doctests{% endhighlight %} and add a file {% highlight python %}touch add.txt{% endhighlight %}
with the following content
{% highlight python %}
>>> import test
>>> test.add(4,3)
7
>>> test.multiply(9,3)
12
{% endhighlight %}

With the code and tests in place, we can test it. If there is no error, there will be no output
{% highlight python %}
python -m doctest multiply.py
{% endhighlight %}

TO view the outputs in details, run the command
{% highlight python %}
$ python -m doctest -v multiply.py
Trying:
    multiply(4,3)
Expecting:
    12
ok
Trying:
    multiply('a', 3)
Expecting:
    'aaa'
ok
1 items had no tests:
    multiply
1 items passed all tests:
   2 tests in multiply.multiply
2 tests in 2 items.
2 passed and 0 failed.
Test passed.

{% endhighlight %}

{% highlight python %}
$ python -m doctest -v doctests/add.txt
Trying:
    import add
Expecting nothing
ok
Trying:
    add.add(4,3)
Expecting:
    7
ok
Trying:
    add.add(9, 3)
Expecting:
    12
ok
1 items passed all tests:
   3 tests in add.txt
3 tests in 1 items.
3 passed and 0 failed.
Test passed.
{% endhighlight %}

Now the project structure looks like this
{% highlight python %}

 - my-test-program
 |--- add.py
 |--- multiply.py
 |--- doctests
     |--- add.txt
     
{% endhighlight %}

I tried to play around with xmlrunner with doctests to generate JUnit Report. So the test.py looks like this:
{% highlight python %}
import xmlrunner
import doctest
import multiply

if __name__ == "__main__":
    suite = doctest.DocTestSuite(multiply)
    xmlrunner.XMLTestRunner().run(suite)
{% endhighlight %}

Running this script will generate a JUnit XML Report

