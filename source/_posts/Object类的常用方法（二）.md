---
title: Object类的常用方法 （二）
---
### 背景：



> 续上文Object类的常用方法（一）

> 上一篇文章介绍了Object.create...等
##### 5、 Object.entries:	

> Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 
> for-in 循环也枚举原型链中的属性）。

Object.entries()返回一个数组，其元素是与直接在object上找到的可枚举属性键值对相对应的数组。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

参数： Object.entries(obj), 其中 obj 可以返回其可枚举属性的键值对的对象。

返回值： 给定对象自身可枚举属性的键值对数组。

举个栗子：

	const obj = { foo: 'bar', baz: 42 };
	console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
	
	// array like object
	const obj = { 0: 'a', 1: 'b', 2: 'c' };
	console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]
	
	// array like object with random key ordering
	const anObj = { 100: 'a', 2: 'b', 7: 'c' };
	console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
	
	// getFoo is property which isn't enumerable
	const myObj = Object.create({}, { getFoo: { value() { return this.foo; } } });
	myObj.foo = 'bar';
	console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]
	
	// non-object argument will be coerced to an object
	console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]
	
	// iterate through key-value gracefully
	const obj = { a: 5, b: 7, c: 9 };
	for (const [key, value] of Object.entries(obj)) {
	  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
	}
	
	// Or, using array extras
	Object.entries(obj).forEach(([key, value]) => {
	console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
	});


##### 6、 Object.create:	
		
> 方法会使用指定的原型对象及其属性去创建一个新的对象
> 
> Object.create(proto[, propertiesObject])

> 其中参数proto为一个对象，作为新创建对象的原型
> 
> propertiesObject: 对象，可选参数，为新创建的对象指定属性对象。

注意，使用Object.create()方法创建对象时，如果不是继承一个原有的对象，而是创建一个全新的对象，就要把proto设置为null。

来看一个简单的例子：

	// 基类
	function Site() {
	  this.name = 'Site';
	  this.domain = 'domain';
	}

	Site.prototype.create = function(name, domain) {
	  this.name = name;
	  this.domain = domain;
	};
	
	// 子类
	function Itbilu() {
	  Site.call(this); //调用基类的构造函数
	}
	
	// 继承父类
	Itbilu.prototype = Object.create(Site.prototype);
	
	// 创建类实例
	var itbilu = new Itbilu();
	
	itbilu instanceof Site;  // true
	tbilu instanceof Itbilu;  // true
	
	itbilu.create('IT笔录', 'itbilu.com');
	itbilu.name;    // 'IT笔录'
	itbilu.domain;  // 'itbilu.com'

使用 Object.create 的 propertyObject参数

	var o;
	
	// 创建一个原型为null的空对象
	o = Object.create(null);
	
	
	o = {};
	// 以字面量方式创建的空对象就相当于:
	o = Object.create(Object.prototype);
	
	
	o = Object.create(Object.prototype, {
	  // foo会成为所创建对象的数据属性
	  foo: { 
	    writable:true,
	    configurable:true,
	    value: "hello" 
	  },
	  // bar会成为所创建对象的访问器属性
	  bar: {
	    configurable: false,
	    get: function() { return 10 },
	    set: function(value) {
	      console.log("Setting `o.bar` to", value);
	    }
	  }
	});
	
##### 7、Object.defineProperty
	
	语法： Object.defineProperty(obj, prop, descriptor)
	参数： 
		obj：必需。目标对象 
		prop：必需。需定义或修改的属性的名字
		descriptor：必需。目标属性所拥有的特性
	返回值： 传入函数的对象。即第一个参数obj

针对属性，我们可以给这个属性设置一些特性，比如是否只读不可以写；是否可以被for..in或Object.keys()遍历。

给对象的属性添加特性描述，目前提供两种形式：数据描述和存取器描述。

数据描述：
当修改或者定义对象的某个属性的时候，给这个属性添加一些特性：
	
	var obj = {
	    test:"hello"
	}
	//对象已有的属性添加特性描述
	Object.defineProperty(obj,"test",{
	    configurable:true | false,
	    enumerable:true | false,
	    value:任意类型的值,
	    writable:true | false
	});
	//对象新添加的属性的特性描述
	Object.defineProperty(obj,"newKey",{
	    configurable:true | false,
	    enumerable:true | false,
	    value:任意类型的值,
	    writable:true | false
	});

数据描述中的属性都是可选的。看一下每个设置的作用。

value： 属性对应的值,可以使任意类型的值，默认为undefined

	var obj = {};
	//第一种情况，不设置value属性。
	Object.defineProperty(obj,"newKey",{})
	console.log(obj.newKey);   //undefined	
	//第二种 设置value属性

	Object.defineProperty(obj,"newKey",{
		value： 'hello'
	})
	console.log(obj.newKey);   //hello	

writable 
属性的值是否可以被重写。设置为true可以被重写，设置为false,不可被重写。 默认为false

	var obj = {}
	//第一种情况：writable设置为false，不能重写。
	Object.defineProperty(obj,"newKey",{
	    value:"hello",
	    writable:false
	});
	//更改newKey的值
	obj.newKey = "change value";
	console.log( obj.newKey );  //hello

	//第二种情况：writable设置为true，可以重写
	Object.defineProperty(obj,"newKey",{
	    value:"hello",
	    writable:true
	});
	//更改newKey的值
	obj.newKey = "change value";
	console.log( obj.newKey );  //change value


enumerable

此属性是否可以被枚举（使用for...in或Object.keys()）。设置为true可以被枚举；设置为false，不能被枚举。默认为false。

	var obj = {}
	//第一种情况：enumerable设置为false，不能被枚举。
	Object.defineProperty(obj,"newKey",{
	    value:"hello",
	    writable:false,
	    enumerable:false
	});
	
	//枚举对象的属性
	for( var attr in obj ){
	    console.log( attr );  
	}
	//第二种情况：enumerable设置为true，可以被枚举。
	Object.defineProperty(obj,"newKey",{
	    value:"hello",
	    writable:false,
	    enumerable:true
	});
	
	//枚举对象的属性
	for( var attr in obj ){
	    console.log( attr );  //newKey
	}

configurable

是否可以删除目标属性或是否可以再次修改属性的特性（writable, configurable, enumerable）。设置为true可以被删除或可以重新设置特性；设置为false，不能被可以被删除或不可以重新设置特性。默认为false。

这个属性起到两个作用：

1. 目标属性是否可以使用delete删除

2. 目标属性是否可以再次设置特性

		//-----------------测试目标属性是否能被删除------------------------
		var obj = {}
		//第一种情况：configurable设置为false，不能被删除。
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:false,
		    enumerable:false,
		    configurable:false
		});
		//删除属性
		delete obj.newKey;
		console.log( obj.newKey ); //hello
		
		//第二种情况：configurable设置为true，可以被删除。
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:false,
		    enumerable:false,
		    configurable:true
		});
		//删除属性
		delete obj.newKey;
		console.log( obj.newKey ); //undefined
		
		//-----------------测试是否可以再次修改特性------------------------
		var obj = {}
		//第一种情况：configurable设置为false，不能再次修改特性。
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:false,
		    enumerable:false,
		    configurable:false
		});
		
		//重新修改特性
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:true,
		    enumerable:true,
		    configurable:true
		});
		console.log( obj.newKey ); //报错：Uncaught TypeError: Cannot redefine property: newKey
		
		//第二种情况：configurable设置为true，可以再次修改特性。
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:false,
		    enumerable:false,
		    configurable:true
		});
		
		//重新修改特性
		Object.defineProperty(obj,"newKey",{
		    value:"hello",
		    writable:true,
		    enumerable:true,
		    configurable:true
		});
		console.log( obj.newKey ); //hello

除了给新特性的属性设置特性，也可以给已有的属性设置特性。

	//定义对象的时候添加的属性，是可删除、可重写、可枚举的。
	var obj = {
	    test:"hello"
	}
	//改写值
	obj.test = 'change value';
	
	console.log( obj.test ); //'change value'
	
	Object.defineProperty(obj,"test",{
	    writable:false
	})
	
	
	//再次改写值
	obj.test = 'change value again';
	
	console.log( obj.test ); //依然是：'change value'

提示： 一旦使用Object.defineProperty给对象添加属性，那么如果不设置属性的特性，那么configrable,enumerable,writable这些值都是默认的false。

	var obj = {};

	定义新属性后，这个属性的特性中，都是默认为false。这就导致了newKey这个是不能重写，不能枚举，不能再次设置特性。
	//
	Object.defineProperty(obj,'newKey',{
	
	});
	
	//设置值
	obj.newKey = 'hello';
	console.log(obj.newKey);  //undefined
	
	//枚举
	for( var attr in obj ){
	    console.log(attr);
	}

特性总结：

> value: 设置属性的值
> 
>writable: 值是否可以重写。true | false
>
>enumerable: 目标属性是否可以被枚举。true | false
>
>configurable: 目标属性是否可以被删除或是否可以再次修改特性 true | false
##### 8、Object.hasOwnProperty