---
title: Python 基础知识
Category: python  
Tags: [list comprehension, generator, decorator, yield]
layout: post
---



[![image](http://www.python.org/images/python-logo.gif)](http://www.python.org/)


##print, for, while, if

##int
 
what's the max?  
do we need to consider the overflow?

{% highlight python %}
import sys
print sys.maxint
a = sys.maxint * 2
print a
type(a)
{% endhighlight %}


##string:
**There is no char type.**

'a', "b", """"a""""

**concat string:**

{% highlight python %}
'a'*80
'a'+'b'
'%s %s' % ('a', 'b')
{% endhighlight %}


**convert to string**

{% highlight python %}
'a' + 1 #?
'a' + str(1)
str('a')
repr('a')
'a'+repr('a')
{% endhighlight %}


**the methods of str**

{% highlight python %}
dir('a')
{% endhighlight %}


##None, True, False, is

{% highlight python %} 
None, 0, False
type(None)  	#NoneType
type(False)  	#bool
None is 0		#False
True is 1		#False
True is not 1	#False
True == 1		#True
{% endhighlight %} 

## and or

{% highlight python %} 
1 and 2			#2
1 or 2			#1
{% endhighlight %}


##list

{% highlight python %} 
[], list()
a = [1, 'a']
a[0], a[1], a[2]
len(a)
a.count() #??
a.count(1)
a = range(10)
a[1:]  		#[1, 2, 3, 4, 5, 6, 7, 8, 9]
a[4:]  
a[1::2]  	#[1, 3, 5, 7, 9]
a[2:8:3]  	
a[-1]  		#[9]
a[-3:-1]  	#[7, 8]
a[::-1]  	#[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
{% endhighlight %} 

###use list as stack, queue

{% highlight python %} 
dir(list)
a.pop()
a.append(11)
a.pop(0)
{% endhighlight %} 

##tuple:

{% highlight python %} 
1,2
1,
(1,2)
(1)
(1,)
a = 1,2
a[0] = 2
a,b = 1,2
a,b = b,a
()
a, _ = 1, 2
{% endhighlight %} 

##dict
{% highlight python %} 
{}, dict()
d = {1:2,'a':'b'}
d['c']
d.get('c')
{% endhighlight %} 

##useful libs
{% highlight python %}
import collections
{% endhighlight %}

##set
{% highlight python %}
a = set([1,2,1])
{% endhighlight %}

##generator expression && list comprehension
{% highlight python %}
a = (i for i in range(10))
b = [i for i in range(10)]
c = [i for i in range(10) if i % 2]
filter, map, reduce
filter(lambda x: x%2, range(10))
filter(None, range(10))
map(lambda x:x+3, a)
reduce(lambda x,y:x*y, [1,2,3,4,5], 10) #((((((10*1)*2)*3)*4)*5)
{% endhighlight %}

##yield
{% highlight python %}
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n += 1

g = fab(30)

for i in g:
    print i
{% endhighlight %}

##class and function

### classic class and new-style class
{% highlight python %}
class A: pass
class A(object):
    def __init__(self):
        self._name = 'a'
        self.__id = 1

    def func(self):
        pass

    def _func(self):
        pass

    def __func(self):
        pass

if __name__ == '__main__':
    a = A()
    print a._name
    print dir(a)
    print a._A__id

def fun():
    return 1
{% endhighlight %}

###class variable and instance variable
{% highlight python %}
class A(object):
    _name = 'b'

    def __init__(self):
        self._name = 'a'
        self.__id = 1


if __name__ == '__main__':
    a = A()
    print a._name
    print dir(a)
    print a._A__id
    print A._name
    del a._name
    print a._name
{% endhighlight %}

### class method
{% highlight python %}
class A(object):
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return "<%s>: %s" % (self.__class__, self.name)

    @classmethod
    def create(cls, name):
        return cls(name)


class B(A):
    pass
a = A.create('haha')
print a

b = B.create('hehe')
print b
{% endhighlight %}


##exception

###try, raise, except, finnaly
{% highlight python %}
class B:
    pass
class C(B):
    pass
class D(C):
    pass

for c in [B, C, D]:
    try:
        raise c()
    except D:
        print "D"
    except C:
        print "C"
    except B:
        print "B"
    finally:
        print "over"
{% endhighlight %}

##decorators
###bad example
{% highlight python %}
def wrapper(func):
    print '##before %s:' % func.__name__
    return func

@wrapper
def say(something):
    print something


say('yeah')
say('hello, world')
{% endhighlight %}

###without arguments
{% highlight python %}
def wrapper2(func):
    def f(args):
        print '##before %s:' % func.__name__
        result = func(args)
        print '##after %s:' % func.__name__
        return result
    return f


@wrapper2
def say(something):
    print something

say('yeah')
say('hello, world')
{% endhighlight %}

###with arguments
{% highlight python %}
def wrapper3(*arguments):
    def _wrapper3(function):
        def _fun_wrapper(*args, **kwargs):
            print '>>>>>'
            print '###', arguments
            result = function(*args, **kwargs)
            print '<<<<<'
            return result
        return _fun_wrapper
    return _wrapper3


@wrapper3('a', 'b')
def say(name):
    print 'yoyo', name

say('thoughtworks')
{% endhighlight %}

####property
#####Way 1

{% highlight python %} 
class C(object):
    def __init__(self):
        self._x = 'xxx'

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")

c = C()
print c.x
c.x = 3
print c.x
{% endhighlight %} 

#####Way 2

{% highlight python %} 
class TestProp(object):
    @property
    def x(self):
        print 'called getter'
        return self._x
    
    @x.setter
    def x(self, value):
        print 'called setter'
        self._x = value

{% endhighlight %} 


## materials

皮皮书屋

---
**Basic**


* The Python Tutorial  
* python-koans  
* A byte of python  
* Python Tutorial
* Python Cookbook  
* Python Testing Cookbook  
* Core Python Programming

---
**Advanced**

* [decorators](http://wiki.python.org/moin/PythonDecoratorLibrary)
* Python for Data Analysis (scipy, numpy, pandas)
* [Dive Into Python](http://woodpecker.org.cn/diveintopython/toc/index.html)

---
**Exercise**

* python challenge  
* project euler

---
**Web Framework**

* django  
* tornado
* flask
* web.py
* ….











'