---  
layout: post  
title: 单例模式  
categories:  
- 技术  
tags:  
- Software Engineering
---

单例模式：确保一个类只有一个实例，并提供一个全局访问点。

![singleton](/media/pic/singleton.png 'singleton')  
常见实现方式：  
	
	public class Singleton {
    	private static Singleton uniqueInstance;

    	private Singleton() {
    	}

    	public static Singleton getInstance() {
    	    if(uniqueInstance == null) {
            	uniqueInstance = new Singleton();
        	}
        	return uniqueInstance;
    	}
	}

这种方式可以在需要使用的时候才创建，但是在多线程场景下，可以产生多个实例

通过在getInstance方法上添加synchronized关键字，会迫使每个线程进入这个方法之前，要先等其他线程离开该方法，这种方式会导致每次调用都需要获取锁。

	public class Singleton {
    	private static Singleton uniqueInstance;

    	private Singleton() {
    	}

    	public static synchronized Singleton getInstance() {
        	if(uniqueInstance == null) {
            	uniqueInstance = new Singleton();
        	}
        	return uniqueInstance;
    	}
	}

多线程还可以通过如下的方式创建，JVM在任何线程访问uniqueInstance静态变量之前，一定会先创建实例， 这种方式会导致即使不使用实例也会创建

	public class Singleton {
    	private static Singleton uniqueInstance = new Singleton();

    	private Singleton() {
    	}

    	public static Singleton getInstance() {
        	return uniqueInstance;
    	}
	}

还有一种双重检查加锁的方式，在getInstance方法减少使用同步

	public class Singleton {
    	private volatile static Singleton uniqueInstance;

    	private Singleton() {
    	}

    	public static Singleton getInstance() {
    	    if(uniqueInstance == null) {
    	        synchronized (Singleton.class) {
    	            if (uniqueInstance == null) {
    	                uniqueInstance = new Singleton();
    	            }
    	        }
    	    }
    	    return uniqueInstance;
    	}
	}

注意这里使用的volatile关键字，它的主要作用是用来保证可见性和禁止指令重排序  
保证可见性：当一个共享变量被volatile修饰后，它会保证修改的值会立即被更新到主存，当有其他线程需要读取这个值的时候，它会从主存中读取新的值，而不是使用线程自己的缓存。

禁止指令重排序：在并发编程中，为了提高性能，编译器和处理器常常会对指令进行重排序，但是有些指令之间是有依赖关系的，这样的话重排序可能会影响程序的执行结果，volatile修饰的变量禁止了这种重排序优化。