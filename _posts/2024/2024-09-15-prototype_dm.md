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

常见的浅拷贝实现：  

	public class LineStyle {
    	private String type;
    	private Integer size;

    	public LineStyle(String type, Integer size) {
    	    this.type = type;
    	    this.size = size;
    	}

    	@Override
    	public String toString() {
    	    return "LineStyle{" +
    	            "type='" + type + '\'' +
    	            ", size=" + size +
    	            '}';
    	}
	}

	public abstract class Shape implements Cloneable{
	    private String id;
	    protected String type;
	    protected LineStyle lineStyle;

	    abstract void draw();

	    public String getId() {
	        return id;
	    }

	    public void setId(String id) {
	        this.id = id;
	    }

	    public String getType() {
	        return type;
	    }

	    public Object clone() {
	        Object clone = null;
	        try {
	            clone = super.clone();
	        } catch (CloneNotSupportedException e) {
	            e.printStackTrace();
	        }
	        return clone;
	    }
	}

	public class Circle extends Shape {
    	public Circle() {
    	    type = "Circle";
    	    lineStyle = new LineStyle("straight", 3);
    	}

    	@Override
    	void draw() {
    	    System.out.println("Circle.draw() with " + lineStyle);
    	}
	}

	public class Rectangle extends Shape{
    	public Rectangle() {
    	    type = "Rectangle";
    	    lineStyle = new LineStyle("straight", 2);
    	}

    	@Override
    	void draw() {
    	    System.out.println("Rectangle.draw() with " + lineStyle);
    	}
	}

	public class Square extends Shape{
	    public Square() {
	        type = "Square";
	        lineStyle = new LineStyle("straight", 1);
	    }

	    @Override
	    void draw() {
	        System.out.println("Square.draw() with " + lineStyle);
	    }
	}

	public class ShapeCache {
    	private static Map<String, Shape> shapeMap = new HashMap<>();

    	public static Shape getShape(String shapeId) {
    	    Shape cachedShape = shapeMap.get(shapeId);
    	    return (Shape) cachedShape.clone();
    	}

    	public static void loadCache() { //模仿数据库获取
    	    Circle circle = new Circle();
    	    circle.setId("1");
    	    shapeMap.put(circle.getId(),circle);

    	    Square square = new Square();
    	    square.setId("2");
    	    shapeMap.put(square.getId(),square);

    	    Rectangle rectangle = new Rectangle();
    	    rectangle.setId("3");
    	    shapeMap.put(rectangle.getId(),rectangle);
    	}
	}

	public class PrototypePatternDemo {
    	public static void main(String[] args) {
    	    ShapeCache.loadCache();

    	    Shape clonedShape = ShapeCache.getShape("1");
    	    clonedShape.draw();
	
    	    Shape clonedShape2 = ShapeCache.getShape("2");
    	    clonedShape2.draw();

    	    Shape clonedShape3 = ShapeCache.getShape("3");
    	    clonedShape3.draw();
    	}
	}





