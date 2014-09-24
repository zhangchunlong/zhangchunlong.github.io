---
layout: default  
title: Mock Learning(1)  
category: tool  
comments: true  
---
#Mock类

##1.Mock类的作用
Mock是用来代替“桩”（stubs）和"替身测试"(test double)的一种仿真对象。




##2.Mock类的参数
Mock的构造函数为：Mock(spec=None, side_effect=None, return_value=DEFAULT, wraps=None,name=None, spec_set=None, **kwargs)  
（1）其中spec是一个现有的对象或者是一些字符串作为对Mock对象的说明  
例如： 
<pre>
<code>
class Test(object):
   pass
mock1=Mock(spec=Test)
这里spec是类型对象
</code>
</pre>

（2）spec_set:严格指定spec，当尝试给Mock对象设置值时，若设置的属性不在spec_set中会抛出AttributeError.  
例如（注意与spec的区别）：  
<pre>
<code>
class Test1(object):  
     def test1(self):  
     pass
mock1 = mock.Mock(spec=Test1)
mock1.test2="test2"
print mock1.test2
结果：test2（这里是spec，test2在Test1中没有定义）
mock2 = mock.Mock(spec_set=Test1)
mock2.test2="test2"
print mock2.test2
Traceback (most recent call last):
  File "/home/tralon/PycharmProjects/Test/Tralon_Mock.py", line 43, in <module>
    mock2.test2="test2"
  File "/usr/local/lib/python2.7/dist-packages/mock.py", line 761, in __setattr__
    raise AttributeError("Mock object has no attribute '%s'" % name)
AttributeError: Mock object has no attribute 'test2'
</code>
</pre>

（3）side_effect:当调用Mock对象时，该函数会被调用.通过Mock对象的side_effect可以抛出异常或动态改变返回值，还可以返回多次调用Mock对象的值  
（4）return_value:当Mock对象被调用时的返回值，注意只有当side_effect返回DEFAULT时，调用Mock对象才会使用return_value  
例如：  
<pre>
<code>
def side_effect(*args, **kwargs):
    return  mock.sentinel.DEFAULT
mock3 = mock.Mock(spec=Test1)
mock3.side_effect = side_effect
mock3.return_value = 2
print mock3()
返回：2
但是下面的情况会返回side_effect的值：
mock3 = mock.Mock(spec=Test1)
mock3.side_effect = 1
mock3.return_value = 2
print mock3()
返回：1
</code>
</pre>

（5）wraps：被Mock对象包装的对象，在不显示指定return_value的情况下，若指定了该参数那么当调用Mock时，会把调用传递给被包装的对象。但是当显示指定返回值时，则不会将调用传递给被包装的对象
例如：
<pre>
<code>
class Test1(object):
    def __call__(self):
        print "test1"
mock4 = mock.Mock(wraps=Test1())
mock4()
结果：test1
当是下面代码时：
class Test1(object):
    def __call__(self):
        print "test1"
mock4 = mock.Mock(wraps=Test1())
mock4.return_value="return value"
print mock4()
结果：return value
</code>
</pre>

(6) name:Mock对象的名字
例如：
<pre>
<code>
class Test1(object):
    def __call__(self):
        print "test1"
mock6 = mock.Mock(spec=Test1,name='tralon')
print mock6
结果：Mock name='tralon' spec='Test1' id='140264817772560'
</code>
</pre>
