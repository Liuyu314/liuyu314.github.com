---
layout: post
title: "对象和数据结构"
description: ""
category: "java"
tags: [Java]
---
{% include JB/setup %}

偷个懒，这篇博文直接取自《代码整洁之道》第六章的标题，事实上，本文也是对这一章做了一个总结，或者算是篇笔记吧。

这一章讲的东西虽然不多，但会让人晕头转向。事实上，只要你注意看本章的开篇段落，就会发现后面讲了半天都在讲什么。比如我们命名变量时，将其限定为private，这种做法的意义在于使得其他人不能访问这些变量。我们将这些变量隐藏起来，作为这个类的“私有财产”，有时（应该是多数时候），我们甚至不希望外界知道这些变量。

于是，作者提出了这样的问题，为什么还是有那么多程序员给对象自动添加赋值器和取值器？所谓赋值器和取值器，类似类中的方法，比如我们有个私有变量info，我们又写了一些方法如setInfo（赋值）或getInfo（取值）。

好吧，你可能觉得这没什么，也许你经常还会这么做。但事实上，这样的方法在不知不觉中透露了类中抽象数据（变量），这与我们的本意是不符的。来看两个例子：

示例1：

{% highlight Java %}
public interface Vehicle {
	double getFuelTankCapcityInGallons();
	double getGallonsOfGasoline();
}
{% endhighlight %}

示例2：

{% highlight Java %}
public interface Vehicle {
	double getPercentFuelRemainning();
}
{% endhighlight %}

初看只是两个接口而已，但通过这些接口的命名，我们发现第一个例子能够清楚知道（至少是猜测）出这些都是变量的存取器（甚至是这个类都拥有哪些变量），而后者只是一个百分比的抽象，我们很难得知其中的数据形态。显然，后者更为合适，它隐藏了数据的细节。

实际上，这里要回到我们的标题，对象和数据。我们希望对象不要暴露其中的数据，而数据结构（相对于对象）本身一定会暴露其中数据，且没有提供什么有意义的函数（方法）。有点绕，要记得对象和数据结构是不同的两类事物。下面两个例子很好展示了这一点：

过程式形状代码（请注意数据结构）：

{% highlight Java %}
public class Square {
	public Point topLeft;
	public double side;
}

public class Rectangle {
	public Point topLeft;
	public double height;
	public double width;
}

public class Circle {
	public Point center;
	public double radius;
}

public class Geometry {
	public final double PI = 3.141592653589793;

	public double area(Object shape) throws NoSuchShapeException {
		if (shape instanceof Shape) {
			Square s = (Square)shape;
			return s.side * s.side;
		} else if (shape instanceof Rectangle) {
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		} else if (shape instanceof Circle) {
			Circle r = (Circle)shape;
			return PI * c.radius * c.radius;
		}
		throw new NoSuchShapeException();
	}
}
{% endhighlight %}

多态式形状代码（请注意对象）：

{% highlight Java %}
public class Square implements Shape {
	private Point topLeft;
	private double side;

	public double area() {
		return side * side;
	}
}

public class Rectangle implements Shape {
	private Point topLeft;
	private double height;
	private double width;

	public double area() {
		return height * width;
	}
}

public class Circle implements Shape {
	private Point center;
	private double radius;
	public final double PI = 3.141592653589793;

	public double area() {
		return PI * radius * radius;
	}
}
{% endhighlight %}

第一个例子更像C语言代码，几个形状的数据结构（没错，我们所说的数据结构就是类似这里的代码，变量都是public，且没有其他方法）类似C中的结构体。而在Geometry类中，更是过程式编程思想。而在第二个例子中，显然是面向对象的思想，每个类中都定义了私有变量和area方法。显然更适合多态实现。

对于第一个例子，优点在于，如果我们想要添加一个求周长的方法，只需要添加这个方法，而不需要修改形状类（即数据结构），但是当我们添加一个新的形状时，我们不得不对每一个方法都进行修改。

对于第二个例子，优点在于，如果我们想要添加一个新的形状，直接添加即可，不需要关心处理多态那部分的代码。但是，如果我们要添加一个求周长的方法，就需要对所有的类都进行修改。

两种做法各有利弊，如果我们要使用对象的方法（第二例），就要防止暴露其中的数据。即本文开篇所说的问题。

也许你听说过得墨忒耳律（The Law of Demeter）—— 模块不应了解它所操作对象的内部情形。作者举了一个成为“火车失事”的例子：

    final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();

因为它违反了得墨忒耳律，调用了getOptions返回值的getScrachDir函数，又调用getScrachDir返回值getAbsolutePath方法。（有没有发现Android里经常是这样的做的。。。）

最好做如下的切分：

    Options opts = ctxt.getOptions();
    File scratchDir = opts.getScratchDir();
    final String outputDir = scratchDir.getAbsolutePath()；

即使这样做，也并不能保证不违反得墨忒耳律，如果ctxt，Options和ScratchDir是对象还是数据结构（注意上面过程式代码和多态式代码的例子），如果是数据结构且没有其他行为，那就不违反（数据结构的数据都是暴露的）。如果是对象，显然这些方法暴露了太多信息。不相信，那我们来看看这段代码都会暴露什么信息：Options是类（对象），则getScratchDir取得了其中的某个叫做ScratchDir的类（File型），而这个类最终取得AbsolutePath（通过getAbsolutePath方法）。它至少透露了Options类中包含一个返回File对象的方法，和它的一个私有变量（String型的代表绝对路径的私有变量）。

所以，对于这些包含对象的编程，要始终注意隐藏数据。那么到底为什么要隐藏这些数据？因为暴露了抽象接口，用户便无需了解数据的实现就能操作数据本体。这使得本来私有的数据被公之于众，使得我们的本意不能够实现。

最后介绍一种精炼的数据结构，只有公共变量，没有函数的类。其被称为**数据传送对象**，或DTO（Data Transfer Objects）：
{% highlight Java %}
public class Address {
	private String street;
	private String city;

	public Address (String street, String city) {
		this.street = street;
		this.city = city;
	}

	public String getStreet() {
		return street;
	}

	public String getCity() {
		return city;
	}
}
{% endhighlight %}
