---  
layout: post  
title: 原型模式  
categories:  
- 技术  
tags:  
- Software Engineering
---
  
原型模式：当创建给定类的实例过程很昂贵或很复杂时，通过拷贝给定类实例创建新的对象。 

![prototype](/media/pic/prototype.PNG 'prototype')  
 
Prototype 表示声明一个clone自身的接口  
ConcretePrototype 实现一个克隆自身的操作  
Client通过从一个原型实例克隆自身创建一个新的对象

