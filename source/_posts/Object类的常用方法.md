---
title: Object类的常用方法 （一）
---
### 背景：



> js中对Object的一些操作

> 对象是值： 成对的名称（字符串）和值（任何值），其中名称通过冒号与值分隔；	

一 、我们创建对象的方法：

1. 字面量形式：

	var obj = {
		a:1，
		b:2
	}; 

2. 对象实例

	var obj = new Object();
3. 构造函数形式

		function employee(name,job,born) {
		    this.name=name;
		    this.job=job;
		    this.born=born;
		}

		var bill=new employee("Bill Gates","Engineer",1985);
 
		// employee {name: "Bill Gates", job: "Engineer", born: 1985} 

	
4. es6 class模式

	//定义类

		class point = {
			constructor(x,y){
				this.x = x;
				this.y = y;	
			};
	
			//定义方法
			tostring(){
				return '('+this.x+','+this.y+')';
			}
	
			multiplication(){
				return this.x*this.y
			}
	
			test(){
				return '1234567890'
			}
		}

		console.log(point.name)
		//Point （name属性总是返回紧跟在class关键字后面的类名。）
        
        // 实例化并调用（实例化和调用方法同es5相同）
        var x = new Point(5, 6)
        console.log(x.toString(), '相乘结果：' + x.multiplication(), x.test())
        // (5, 6) 相乘结果：30 1231354654
       
        // 快速添加方法到原型上 （如下图，直接可添加到原型）
        Object.assign(Point.prototype, {
            dosomething() { },
            toValue() { }
        });
        console.log(Point.prototype)
	
二、Object的常用方法

### 1、 Object.assign：  浅拷贝复制

> Object.assign()方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象，它将返回目标对象。 Object.assign(target, ...sources)

参数:

target:目标对象;sources:源对象;返回值:目标对象

1. 复制一个对象，并不会影响原对象

		var obj = {a:1};
		var copy = Object.assign({},obj);
		console.log(copy) // {a:1}

2. 合并对象
		
		//object.assign(obj, obj2)  obj2是源对象，obj 是目标对象，返回目标对象
	 
		var obj = { a: 1 };
		var obj2={b:2};
		
		console.log(Object.assign(obj,obj2)===obj);  //true，返回目标对象
		console.log(obj);       //{a:1,b:2} obj的值已被更改

3. 如果目标对象和源对象有相同的属性，则会被覆盖，后面的对象会覆盖掉前面的属性
	
	 
		var obj = { a: 1 };
		var obj1 = {a:2,b:5};
		var obj2={b:2};
		
		Object.assign(obj,obj1,obj2);  //{a:2,b:2}

4. 当assign只有一个对象时，则直接返回这个对象，不做任何操作

5. Object.assign是浅拷贝，copy的是一个对象的引用

		var obj1 = { a: 0 , b: { c: 0}};
		var obj2 = Object.assign({}, obj1);
		obj1.b.c=5;
	
		console.log(obj2)       //{a:0,b:{c:5}};

	当我们在改变obj1的值时，并没有想改变obj2，但obj2的值也发生了改变，这违背了我们的想法

6.  深拷贝
	
		var obj = { a: 0 , b: { c: 0}};
		var obj2 = JSON.parse(JSON.stringify(obj));
		obj.a = 4;
		obj.b.c = 4;
		console.log(obj2)   //{ a: 0 , b: { c: 0}}
		

### 2、 Object.is()

判断两个值是否是同一个值 

参数:  Object.is(value1,value2)
	
value1:需要比较的第一个值。 value2:需要比较的第二个值。 返回值:表示两个参数是否相同的Boolean


		Object.is('haorooms', 'haorooms');     // true
		Object.is(window, window);   // true
		
		Object.is('foo', 'bar');     // false
		Object.is([], []);           // false
		
		var test = { a: 1 };
		Object.is(test, test);       // true
		
		Object.is(null, null);       // true
		
		// 特例
		Object.is(0, -0);            // false
		Object.is(-0, -0);           // true
		Object.is(NaN, 0/0);         // true	

### 3、 Object.keys()

> 这个方法会返回一个由给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）。

	1）数组对象
		var arr=[1,2,3];            
		Object.keys(arr)         //  ["0", "1", "2”]

	2） object对象
		var obj = { foo: "bar", baz: 42 };
		Object.keys(obj)        //  ["foo", "baz”]

	(3)类数组，对象

		var obj = { 0 : "a", 1 : "b", 2 : "c”};
		Object.keys(obj)       // ["0", "1", "2"] 

### 4、 Object.values()


> 方法返回一个给定对象自己的所有可枚举属性值的数组，值的顺序与使用for..in循环相同，返回的对象的value值，与Object.key()相反

	var obj = {a:1,b:2,c:3};
	console.log(Object.values(obj))   //  [1, 2, 3]

	var obj ={0:'a',1:'b',2:'c'};
	console.log(Object.values(obj)).       //  a,b,c

	var obj={100:'a',10:'b',1:'1'};
	console.log(Object.values(obj)).   // ["1", "b", "a"]

### 待更新：
### 5、 Object.entries:	

### 6、 Object.create:	
		
### 7、 Object.defineProperties

### 8、Object.defineProperty

### 9、Object.hasOwnProperty


其他不常用方法：

	1、Object.freeze() 方法可以冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象。
	
	2、Object.isFrozen() 方法判断一个对象是否被冻结（frozen）。
	
	3、Object.isExtensible() 方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。
	
	4、Object.isSealed() 方法判断一个对象是否是密封的（sealed）。
	
	5、Object.seal() 方法可以让一个对象密封，并返回被密封后的对象。密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象。