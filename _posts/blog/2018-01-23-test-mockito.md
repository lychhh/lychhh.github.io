---
layout:     post
title: mockito测试
category: blog
description: 先学习了解下
---

maven项目添加依赖引入，Mockito需要Junit配合使用


* Mock 对象的创建
mock(Class classToMock);
mock(Class classToMock, String name)
mock(Class classToMock, Answer defaultAnswer)
mock(Class classToMock, MockSettings mockSettings)
mock(Class classToMock, ReturnValues returnValues)
可以对类和接口进行mock对象的创建，创建时可以为mock对象命名。对mock对象命名的好处是调试的时候容易辨认mock对象。


* when(mock.someMethod()).thenReturn(value) 来设定 Mock 对象某个方法调用时的返回值

* when(mock.someMethod()).thenThrow(new RuntimeException) 的方式来设定当调用某个方法时抛出的异常

* doThrow(new RuntimeException()).when(mockedList).clear();
//将会 抛出 RuntimeException:
mockedList.clear();
这个实例表示当执行到mockedList.clear()时，将会抛出RuntimeException。其他的doXXX执行与它类似。
例如 ： doReturn()|doThrow()| doAnswer()|doNothing()|doCallRealMethod() 系列方法。

* Spy函数：
你可以为真实对象创建一个监控（spy）对象，当你使用这个spy对象时，真实的对象也会被调用，除非它的函数被打桩。你应该尽量少的使用spy对象，使用时也需要小心，例如spy对象可以用来处理遗留代码，Spy示例如下：
List list = new LinkedList();
//监控一个真实对象
List spy = spy(list);
//你可以为某些函数打桩
when(spy.size()).thenReturn(100);
//使用这个将调用真实对象的函数
spy.add("one");




附录：参考文档一览
 
[Mockito官网](http://site.mockito.org/)
[5分钟了解Mockito](http://liuzhijun.iteye.com/blog/1512780)
[Mockito简单介绍及示例](http://blog.csdn.net/huoshuxiao/article/details/6107835)
[Mockito浅谈](http://www.jianshu.com/p/77db26b4fb54)
[单元测试利器-Mockito 中文文档](http://blog.csdn.net/bboyfeiyu/article/details/52127551)
[Mockito使用指南](http://blog.csdn.net/shensky711/article/details/52771493)
[JUnit+Mockito 单元测试（二）](http://blog.csdn.net/zhangxin09/article/details/42422643)


