---  
layout: post  
title: 设计原则  
categories:  
- 技术  
tags:  
- Software Engineering
---

**SOLID原则**  
S：Single Responsibility Principle，单一职责原则，缩写SRP,其含义为一个类或者一个模块只负责完成一个职责；也可以这么解释，就一个模块或者类而言，应该就有一个引起它变化的原因。  

O：Open Closed Principle，开闭原则，缩写ISP，其含义为软件实体(类、模块、函数等等）应该是可以扩展的，但是不可修改的。遵循开闭原则设计出的模块具有两个主要的特征:  
1. 对于扩展是开放的  
   模块的行为是可以扩展的，当应用的需求改变时，我们可以对模块进行扩展，使其具有满足那些改变的新行为。  
2. 对更改是封闭的  
   对模块行为进行扩展时，不必改动模块的源代码或者二进制代码。  

L：Liskov Substitution Principle, 其含义是子类型必须能够替换掉它们的基类型。最早由Barbara Liskov提出：若对每个类型S的对象s1，都存在一个类型T的对象t1，使得在所有针对T编写的程序P中，用s1代替t1后，程序P行为功能不变，则S是T的子类型。  

I：Interface Segreagtion Principle，接口隔离原则，缩写ISP，其含义是不应该强迫客户依赖于它们不用的方法。如果一个客户程序依赖于一个含有它不使用的方法的类，但是其他客户程序却要使用该方法，那么当其他客户要求这个类改变的时候，就会影响到这个客户程序。  

D：Dependency Inversion Principle，依赖反转原则，缩写DIP，其含义是高层模块不应该依赖于底层模块，二者都应该依赖于抽象；抽象不应该依赖于细节，细节应该依赖于抽象。该原则是框架设计的核心原则。  


**KISS原则**  
KISS原则一般代表"Keep It Simple，Stupid"，表示尽量保持简单，鼓励去除不必要的复杂性。
解决如何做的问题（保持简单）

**YAGNI原则**  
YAGNI全程是"You Ain't Gonna Need It", 表示你不需要它，说明不要过度设计。
解决要不要做的问题(当前不需要的就不要做）

**DRY原则**  
DRY全程为"Don't Repeat Yourself", 表示不用重复自己，不要写重复的代码。  
需要注意区分三种重复，实现逻辑重复、功能语义重复、代码执行重复；实现逻辑重复但是功能语义不重复，并不违反DRY原则。实现逻辑不重复但功能语义重复，违反DRY原则。代码执行重复违反DRY原则。  


SRP（单一职责原则）与ISP（接口隔离原则）的区别：  
1. 单一职责原则针对的是模块、类、接口的设计。接口隔离原则侧重于接口的设计。  
2. 接口隔离原则提供了一种判断接口的职责是否单一的标准：通过调用者如何使用接口来间接判断，若调用者仅使用部分接口或者接口的部分功能，那么接口的设计就是不够职责单一。 

**LOD法则**  
LOD全程是"Law of Demeter", 又叫最小知识原则(The Least Knowledge Principle), 含义是每个模块只应该了解那些与它关系密切的模块的有限知识 或者说 每个模块只和自己的朋友说话，不和陌生人说话。核心思想是一个对象应该对其他对象有尽可能少的了解，以实现高内聚、低耦合的的代码。




 

