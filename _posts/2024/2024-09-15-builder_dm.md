---  
layout: post  
title: 建造者模式  
categories:  
- 技术  
tags:  
- Software Engineering
---
  
建造者模式：封装一个产品的构造过程，并允许按步骤进行构造。 

![builder](/media/pic/builder.PNG 'builder')  
 
Builder为创建一个Product对象的各个部件指定抽象接口；  
ConcreteBuilder实现Builder接口以构造和装配该产品的各个部件；  
Director构造一个使用Builder接口的对象；  
Product表示被构造的复杂对象，ConcreteBuilder创建该产品的内部表示并定义它的装配过程。  

这里以虚拟机为例说明建造者模式，虚拟机在公有云中一般称为实例。   

	public class Flavor {
    	private Integer ram;
    	private Integer vcpu;
    	private String architecture;

    	public Flavor(Integer vcpu, Integer ram, String architecture) {
    	    this.ram = ram;
    	    this.vcpu = vcpu;
    	    this.architecture = architecture;
    	}
	}

	public class Instance {
    	private Flavor flavor;
    	private String imageId;
    	private List<String> volumes;
    	private List<String> networks;

    	public Flavor getFlavor() {
    	    return flavor;
    	}

    	public void setFlavor(Flavor flavor) {
    	    this.flavor = flavor;
    	}

    	public String getImageId() {
    	    return imageId;
    	}

    	public void setImageId(String imageId) {
    	    this.imageId = imageId;
    	}
	
    	public List<String> getVolumes() {
    	    return volumes;
    	}

    	public void setVolumes(List<String> volumes) {
    	    this.volumes = volumes;
    	}

    	public List<String> getNetworks() {
    	    return networks;
    	}

    	public void setNetworks(List<String> networks) {
    	    this.networks = networks;
    	}
	}


	public interface InstanceBuilder {
    	InstanceBuilder withFlavor(Flavor flavor);
    	InstanceBuilder withImageId(String imageId);
    	InstanceBuilder withVolumes(String ...volumes);
    	InstanceBuilder withNetworks(String ...networks);
    	Instance build();
	}

	public class ComputeOptimizerInstanceBuilder implements InstanceBuilder {
    	private Instance instance = new Instance();
    	@Override
    	public InstanceBuilder withFlavor(Flavor flavor) {
        	instance.setFlavor(flavor);
        	return this;
    	}

    	@Override
    	public InstanceBuilder withImageId(String imageId) {
        	instance.setImageId(imageId);
        	return this;
    	}

    	@Override
    	public InstanceBuilder withVolumes(String... volumes) {
        	instance.setVolumes(List.of(volumes));
        	return this;
    	}

    	@Override
    	public InstanceBuilder withNetworks(String... networks) {
        	instance.setNetworks(List.of(networks));
        	return this;
    	}

    	@Override
    	public Instance build() {
        	return instance;
    	}
	}

	public class InstanceDirector {
    	private InstanceBuilder builder;

    	public InstanceDirector(InstanceBuilder builder) {
    	    this.builder = builder;
    	}

    	public Instance construct() {
    	    builder.withFlavor(new Flavor(2, 4, "x86"))
    	            .withImageId("image-11-22-33-44")
    	            .withNetworks("network-11-22-33-44")
    	            .withVolumes("volume-11-22-33-44");
    	    return  builder.build();
    	}
	}