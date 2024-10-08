---  
layout: post  
title: 工厂模式  
categories:  
- 技术  
tags:  
- Software Engineering
---

最近中秋节马上到了，这里以月饼为例进行说明简单工厂、工厂方法、抽象工厂模式。  
简单工厂模式：封装创建对象的逻辑，将创建逻辑放到独立的对象中  

	public class MooncakeStore {
    	Mooncake orderMooncake(String type) {
        	Mooncake mooncake = null;
        	if(type.equals("wuren")) {
            	mooncake = new WurenMooncake();
        	} else if(type.equals("zaoni")) {
            	mooncake = new ZaoniMooncake();
        	} else if(type.equals("dousha")) {
            	mooncake = new DoushaMoonCake();
        	}

        	mooncake.prepare();
        	mooncake.bake();
        	mooncake.box();
        	return mooncake;
    	}
	}  

我们可以通过抽取创建月饼的方法，变成如下方式，将创建月饼放到SimpleMooncakeFactory  

	public class MooncakeStore {
    	private SimpleMooncakeFactory factory;
    	public MooncakeStore(SimpleMooncakeFactory factory) {
        	this.factory = factory;
    	}

    	Mooncake orderMooncake(String type) {
        	Mooncake mooncake = factory.createMooncake(type);

        	mooncake.prepare();
        	mooncake.bake();
        	mooncake.box();
        	return mooncake;
    	}
	}

	public class SimpleMooncakeFactory {
     	public Mooncake createMooncake(String type) {
        	Mooncake mooncake = null;
        	if(type.equals("wuren")) {
            	mooncake = new WurenMooncake();
        	} else if(type.equals("zaoni")) {
            	mooncake = new ZaoniMooncake();
        	} else if(type.equals("dousha")) {
            	mooncake = new DoushaMoonCake();
        	}
        	return mooncake;
    	}
	}

简单工厂适用场景：  
当每个对象的创建逻辑都比较简单，代码中存在if-else分支判断，可以将多个对象的创建逻辑放到一个工厂类中。  

工厂方法模式：定义一个创建对象的接口，但由于子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。通过让子类决定该创建的对象是什么，来达到将对象创建的过程封装的目的。  

![factoryMethod](/media/pic/factoryMethod.PNG 'factoryMethod')  

Product定义工厂方法所创建的对象的接口；  
ConcreteProduct实现Product接口；  
Creator声明工厂方法，该方法返回一个Product类型的对象。  
ConcreteCreator重新定义工厂方法返回一个ConcreteProduct实例

Creator是一个抽象类，它实现所有操作产品的方法，但是不实现工厂方法；Creator所有子类都必须实现这个抽象的factoryMethod方法，ConcreteCrator实现factoryMethod方法制造产品；所有的产品实现Product的接口。 下面继续以月饼为例，比如有广式风格月饼，京式风格月饼。  

	public abstract class MooncakeStore {
    	Mooncake orderMooncake(String type) {
    	    Mooncake mooncake = createMooncake(type);
	
    	    mooncake.prepare();
    	    mooncake.bake();
    	    mooncake.box();
    	    return mooncake;
    	}

    	abstract Mooncake createMooncake(String type);
	}

	class GuangShiMooncakeStore extends MooncakeStore{
    	@Override
    	Mooncake createMooncake(String type) {
        	Mooncake mooncake = null;
        	if(type.equals("wuren")) {
            	mooncake = new GuangshiWurenMooncake();
        	} else if(type.equals("zaoni")) {
            	mooncake = new GuangSZaoniMooncake();
        	} else if(type.equals("dousha")) {
            	mooncake = new GuangshiDoushaMoonCake();
        	}
        	return mooncake;
    	}
	}

	public class JingShiMooncakeStore extends MooncakeStore{
    	@Override
    	Mooncake createMooncake(String type) {
        	Mooncake mooncake = null;
        	if(type.equals("wuren")) {
        	    mooncake = new JingShiWurenMooncake();
        	} else if(type.equals("zaoni")) {
        	    mooncake = new JingShiZaoniMooncake();
        	} else if(type.equals("dousha")) {
        	    mooncake = new JingShiDoushaMoonCake();
        	}
        	return mooncake;
    	}
	}  

工厂方法适用场景： 
当对象的创建逻辑比较复杂，不只是简单的new一些就可以，而是要组合其他类对象，做各种初始化操作的时候，将复杂的创建逻辑拆分到多个工厂类中，让每个工厂类都不至于过于复杂；若将所有的创建逻辑都放到一个工厂类中，会导致这个工厂类变得很复杂。  

抽象工厂模式：提供一个接口用于创建相关或依赖对象的家族，而不需要明确指定具体类。  

![abstractFactory](/media/pic/abstractFactory.PNG 'abstractFactory')  

抽象工厂定义一个接口，所有具体工厂都必须实现此接口，这个接口包含一组方法用来生产产品；具体工厂实现不同的产品家族，客户只需要使用其中一个工厂而不需要实例化具体的产品对象。  
AbstractFactory声明一个创建抽象产品对象的接口；  
ConcreteFactory实现创建具体产品对象的操作；  
AbstractProduct为一类产品对象声明一个接口；  
ConcreteProduct定义一个将被具体工厂创建的产品对象；  
Client 仅使用AbstractFactory和AbstractProduct类声明的接口  

还以月饼为例，做月饼需要原料，比如需要面粉，食用油等，南方和北方使用原料不一样，比如南方喜欢使用米粉，花仔油而北方喜欢使用面粉，花生油。  

	public interface MoonIngredientFactory {
    	public Flour createFlour();
    	public Oil  createOil();
	}

	public class GuangDongMooncakeIngredientFactory implements 	MoonIngredientFactory{
    	@Override
    	public Flour createFlour() {
    	    return new RiceFlour();
    	}

    	@Override
    	public Oil createOil() {
    	    return new RapeseedOil();
    	}
	}

	public class BeijingMooncakeIngredientFactory implements 	MoonIngredientFactory {
    	@Override
    	public Flour createFlour() {
    	    return new WheatFlour();
    	}

    	@Override
    	public Oil createOil() {
    	    return new PeanutOil();
    	}
	}

	public interface Oil {
	}

    public class PeanutOil implements Oil {
	}

	public class RapeseedOil implements Oil {
	}


	public interface Flour {
	}

	public class RiceFlour implements Flour {
	}

	public class WheatFlour implements Flour {
	}

	public class GuangshiMooncakeStore extends MooncakeStore {
    	@Override
    	public Mooncake createMooncake(String type) {
        	MoonIngredientFactory factory = new GuangDongMooncakeIngredientFactory();

        	Mooncake mooncake = null;
        	if(type.equals("wuren")) {
            	mooncake = new WurenMooncake(factory);
        	} else if(type.equals("zaoni")) {
            	mooncake = new ZaoniMooncake(factory);
        	} else if(type.equals("dousha")) {
            	mooncake = new DoushaMooncake(factory);
        	}
        	return mooncake;
    	}
	}

	public abstract class Mooncake {
    	String name;
    	Oil oil;
    	Flour flour;
    	abstract void prepare();

    	public void bake() {
    	}
    	public void box() {
    	}
	}

	public class DoushaMooncake extends Mooncake {
    	private MoonIngredientFactory factory;
		public DoushaMooncake(MoonIngredientFactory factory) {
        	this.factory = factory;
    	}

    	@Override
    	void prepare() {
        	flour = factory.createFlour();
        	oil = factory.createOil();
    	}
	}

	public class WurenMooncake extends Mooncake {
    	private MoonIngredientFactory factory;

    	public WurenMooncake(MoonIngredientFactory factory) {
    	    this.factory = factory;
    	}

    	@Override
    	void prepare() {
    	    flour = factory.createFlour();
    	    oil = factory.createOil();
    	}
	}

	public class ZaoniMooncake extends Mooncake {
    	private MoonIngredientFactory factory;

    	public ZaoniMooncake(MoonIngredientFactory factory) {
    	    this.factory = factory;
    	}
    	@Override
    	void prepare() {
    	    flour = factory.createFlour();
    	    oil = factory.createOil();
    	}
	}

抽象工厂适用场景：  
当强调一系列相关的产品对象的设计以便于进行联合使用