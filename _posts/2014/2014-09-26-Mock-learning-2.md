---
layout: post
title: Mock Learning(2)
categories:
- tool
tags:
- Mock
---
#Mock类的方法

##1.assert_called_with(*args,**kwargs)
该方法提供了一种简单的方式来断言Mock对象的调用方式  
例如：  
<pre>
<code>import mock
mock1=mock.Mock()
mock1.method(1,2,3,test='wow')
mock1.method.assert_called_with(1,2,3,test='wow')
这样会执行成功
若最后一句改为：mock1.method.assert_called_with(2,2,3,test='wow')
会出现如下错误：
Traceback (most recent call last):
  File "/home/tralon/PycharmProjects/Test/Tralon_Mock.py", line 5, in <module>
    mock.method.assert_called_with(2, 2, 3, test="wow")
  File "/usr/local/lib/python2.7/dist-packages/mock.py", line 835, in assert_called_with
    raise AssertionError(msg)
AssertionError: Expected call: method(2, 2, 3, test='wow')
Actual call: method(1, 2, 3, test='wow')
</code></pre>

注意：这里是对mock1的属性method调用进行的断言，因为Mock1对象的属性本身的类型也是Mock，当属性被调用时会产生Mock对象,所以这里其属性可以调用Mock类的方法assert_called_with().  
例如：  
<pre>
<code>>>>import mock
>>> mock2 = mock.Mock()
>>> method = mock2.method(1,2)
>>> type(mock2)
class 'mock.Mock'
>>> type(method)
class 'mock.Mock'
</code>
</pre>

##2.assert_called_once_with(*args,**kwargs)
断言Mock对象使用特定参数仅被调用一次  
例如：  
<pre>
<code>mock3 = mock.Mock(return_value=None)
mock3('foo', bar='baz')
mock3.assert_called_once_with('foo', bar='baz')
mock4 = mock.Mock()
mock4('test')
mock4('test')
mock4.assert_called_once_with('test')
结果：
Traceback (most recent call last):
  File "/home/tralon/PycharmProjects/Test/Tralon_Mock.py", line 18, in <module>
    mock4.assert_called_once_with('test')
  File "/usr/local/lib/python2.7/dist-packages/mock.py", line 845, in assert_called_once_with
    raise AssertionError(msg)
AssertionError: Expected to be called once. Called 2 times.
</code></pre>

##3.assert_any_call(*args,**kwargs)
断言Mock对象已经使用特定参数进行调用，注意与assert_called_with和assert_called_once_with的区别，后两者是基于最近的调用.  
例如：  
<pre>
<code>mock5 = mock.Mock(return_value=None)
mock5(1, 2, arg='thing')
mock5('some', 'test')
mock5.assert_any_call(1, 2, arg='thing')
mock5.assert_called_with(1, 2, arg='thing')
结果：
Traceback (most recent call last):
  File "/home/tralon/PycharmProjects/Test/Tralon_Mock.py", line 16, in <module>
    mock5.assert_called_with(1, 2, arg='thing')
  File "/usr/local/lib/python2.7/dist-packages/mock.py", line 835, in assert_called_with
    raise AssertionError(msg)
AssertionError: Expected call: mock(1, 2, arg='thing')
Actual call: mock('some', 'test')
当执行下面代码时：
mock5 = mock.Mock(return_value=None)
mock5(1, 2, arg='thing')
mock5('some', 'test')
mock5.assert_any_call(1, 2, arg='thing')
#mock5.assert_called_with(1, 2, arg='thing')
mock5.assert_called_once_with(1, 2, arg='thing')
结果为：
  File "/home/tralon/PycharmProjects/Test/Tralon_Mock.py", line 17, in <module>
    mock5.assert_called_once_with(1, 2, arg='thing')
  File "/usr/local/lib/python2.7/dist-packages/mock.py", line 845, in assert_called_once_with
    raise AssertionError(msg)
AssertionError: Expected to be called once. Called 2 times.
</code>
</pre>

##4.reset_mock()
重置Mock对象的调用属性，但是不会清除return_value,side_effect以及属性.  
例如：  
<pre><code>>>> mock6 = mock.Mock(return_value=None)
>>> mock6(’hello’)
>>> mock6.called
True
>>> mock6.reset_mock()
>>> mock6.called
False</code></pre>

##5.return_value
设置调用Mock对象的返回值.  
例如：  
<pre>
<code>>>> mock7 = mock.Mock()
>>> mock7.return_value = ’fish’
>>> mock7()
’fish’
默认返回值可以设为Mock对象
>>> mock8 = mock.Mock()
>>> mock8.return_value.attribute = sentinel.Attribute
>>> mock8.return_value()
>>> mock8.return_value.assert_called_with()
返回值可以设在构造器中
>>> mock9 = mock.Mock(return_value=”tralon“)
>>> mock9.return_value
tralon
>>> mock9()
tralon
</code>
</pre>

##6.side_effect
是一个当调用Mock对象时调用的函数或者是抛出的异常.  
例如：  
<pre>
<code>>>> mock10 = mock.Mock()
>>> mock10.side_effect = Exception(’Boom!’)
>>> mock10()
Traceback (most recent call last):
...
Exception: Boom!
</code></pre>

另外，使用side_effect可以返回一列值  
<pre><code>>>>mock11 = mock.Mock()
>>>mock11.side_effect = [3, 2, 1]
>>>mock11(), mock11(), mock11()
(3, 2, 1)
</code></pre>

side_effect所使用的参数与Mock相同，当调用mock对象时会调用该函数，并将其返回值作为mock对象的返回值；只有当side_effect返回为mock.sentinel.DEFAULT时，才会将return_value作为mock对象的返回值.
<pre><code>>>>mock12 = mock.Mock(return_value=3)
>>>def side_effect(*args, **kwargs):
...	 return DEFAULT
...
>>>mock12.side_effect = side_effect
>>>mock12()
3
</code></pre>

side_effect可以在Mock的构造器中使用
<pre><code>
>>>side_effect = lambda value: value + 1
>>>mock13 = mock.Mock(side_effect=side_effect)
>>>mock13(3)
4
>>>mock13(-8)
-7
</code></pre>

##7.\_\_class\_\_
\_\_class\_\_属性是一个对象的类型，对于一个带有spec参数的Mock对象，其\_\_class\_\_属性将要返回其spec参数的类型.  
例如：  
<pre><code>
>>> mock14 = mock.Mock(spec=3)
>>> isinstance(mock14, int)
True
>>> mock15 = mock.Mock()
>>> isinstance(mock15, mock.Mock)
True
</code></pre>

##8.其他方法
Mock类还有很多其他的方法，如assert_has_calls,mock_add_spec,attach_mock等方法，感兴趣的可以去[这里]（www.voidspace.org.uk/python/mock/）.





