---
title: 浅谈typeof 和instanceof的区别
---

####  先介绍下js的基本类型和引用类型
基本类型： Null,Undefined,Boolean ,Number和String

引用类型：  Object、Array、RegExp、Date、Function
		

#### typeof和instanceof
  
相同点： 都是用来判断一个变量是什么类型的。

区别：
typeof是一元运算放在一个运算数之前可以是任意类型。

typeof 一般只能返回以下几种类型：Number,String,Boolean,Function, Object,Null,Undefined. Symbol 

	我们可以使用typeof 来判断一个变量是否存在，如if(typeof a !== 'undefined'){alert("a")}; 
	最好不要使用if(a) 如果a不存在会报错。
	对于array和null等特殊对象使用typeof 都会返回object。

判断一个变量的类型常常会用typeof，在使用typeof运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象它都会返回object。

	let s = new String('abc');
	typeof s === 'object'// true
	s instanceof String // true

此时就需要用到instanceof 来判断。

### typeof的原理：
我们可以想想js在底层是怎么存储数据的类型信息呢。js的变量的底层实现。

其实：js在底层存储变量的时候。会在变量的机器码的低位1-3位存储其类型信息。

	000 对象
	010 浮点数
	100 字符串
	110 布尔
	1 整数

但是对与null和undefined来说，这两个值的存储信息是有点特殊的。
	
	null 所有机器码均为0
	undefined: 用-2^30 整数来表示。


所以判断 typeof null的时候就出现问题了。因为null的机器码，因此就被当作了对象来对待。

如果用instanceof 来判断的话。
  
	null instanceof null   //TypeError: Right-hand side of 'instanceof' is not an object

还有一个判断类型的方法

	Object.prototype.toString.call(1) //"[object, Number]"
	其他类型以此类推



### instanceof的原理 

下面介绍 instanceof  
instanceof 主要是判断一个实例是否属于某种类型。

1.

	let person  = function(){}
	let nichol = new person();
	nichol instanceof person  //true
2.
	
	let person = function(){}
	let person_per = function(){}
	person_per.prototype = new peroson()

	let nichol = new  person_per();
	nichol instanceof person; //true
	nichol instanceof person_per ;  //true 

这是 instanceof 的用法，但是 instanceof 的原理是什么呢？根据 ECMAScript 语言规范，我梳理了一下大概的思路，然后整理了一段代码如下

	function new_instance_of(leftVaule, rightVaule) {
		let rightProto = rightVaule.prototype; // 取右表达式的 prototype 值
		leftVaule = leftVaule.__proto__; // 取左表达式的__proto__值
		while (true) {
			if (leftVaule === null) {
				return false;
			}
			if (leftVaule === rightProto) {
				return true;
			}
			leftVaule = leftVaule.__proto__
		}
	}

instanceof的实现原理大致就是只要右侧变量的prototype在左侧变量的原型链上即可。

因此 instanceof在查找的过程中会遍历左侧的原型链，直到找到右侧变量的prototype,如果查找失败就会返回false,告诉我们左侧变量并非是右侧变量的实例、

举个栗子：
	
	function foo(){

	}
	function instanceof Object //true
	function instanceof function //true
	Object instanceof Object //true
	foo instanceof Object //true
	foo instanceof foo //false
	foo instanceof function //true

下面我们来一一解释其结果为什么这样：
这就涉及到javascript的继承原理
关于原型继承的原理，我简单用一张图来表示
![](https://i.imgur.com/X44Z17j.png)

每个javascript对象都有一个隐式的_proto_ 原型属性。而显示属性是prototype, 只有object.prototype._proto_属性在未修改的情况下为null值。

根据图上的原理，我们来梳理上面提到的几个有趣的 instanceof 使用的例子。

1. object instanceof object 

由图可知，Object的prototype属性是Object.prototype,而由于Object本身是一个函数，由function所创建。

所有Object._proto_的值是Function.prototype,而Function.prototype的_proto_属性是Object.prorotype，

所有可能判断出 object instanceof object 的结果是true.

	leftValue = Object._proto_ = Function.prototype;
	
	rightValue = Object.prototype
	
	//第一次判断
	
	leftValue != rightValue
	
	leftValue = Function.prototype._proto_ = Object._prototype
	
	//第二次判断
	
	leftValue = rightValue

Function instanceof Function和 Function instanceof Object的运行过程与Object instanceof Object类似。故不再详说

2. Foo instanceof Foo
3. 
Foo函数的prototype 属性是Foo.prototype, 而foo的_proto_属性是Function.prototype,

由图可知，Foo的原型链上并没有Foo.prototype,因此Foo instanceof Foo也就返回false;

我们用代码简单的表示一下：

	leftValue = Foo, rightValue = Foo;
	leftValue = Foo._proto = function.prototype;
	rightValue = Foo.prototype;
	//第一次判断
	leftValue != rightValue;
	leftValue = Function.prototype._proto_ = Object.prototype
	//第二次判断
	leftValue != rightValue;
	leftValue = Object.prototype　＝null
	//第三次判断
	leftValue = null;

Foo instanceof Object

	leftValue = Foo, rightValue = Object
	leftValue = Foo.__proto__ = Function.prototype
	rightValue = Object.prototype
	// 第一次判断
	leftValue != rightValue
	leftValue = Function.prototype.__proto__ = Object.prototype
	// 第二次判断
	leftValue === rightValue
	// 返回 true

Foo instanceof Function


	leftValue = Foo, rightValue = Function
	leftValue = Foo.__proto__ = Function.prototype
	rightValue = Function.prototype
	// 第一次判断
	leftValue === rightValue
	// 返回 true


总结来说：

我们使用typeof　来判断基本类型是OK的。不过需要注意当用typeof 来判断null类型的问题。如果想要判断一个对象的具体类型可以用instanceof 。但是instanceof　可能也判断不准确，例如一个数组，可以被instanceof　判断为Object。

所以我们想要比较准确的判断一个数的类型时，可以用Object.prototype.toString.tocall 方法。