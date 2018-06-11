---
title: es6箭头函数和普通函数的区别
---

### 1、 写法：
		箭头函数：
		var a = >{
				console.log(b)
			}
		
		普通函数：
			function a(){
				console.log(b)
			}
		
### 2. 箭头函数不能作为构造函数

		var a =() =>{
			console.log(b);
		 }
		 var b = new a(); //TypeError  a is not a constructor

### 3.箭头函数不绑定arguments,取而代之用rest参数...解决。

		function A(a){
			console.log(arguments)
		}
		
		var B = (b) =>{
			console.log(arguments);  
		}
		
		var C = (...c) =>{
			console.log(c)
		}
		
		A(1);
		B(2);  //arguments is not defined
		C(3);

### 4.箭头函数会捕获其所在上下文的this值，作为自己的this值。

	var Obj = {
		a: 10,
		b: function(){
			console.log(this.a);
		},
		c: function(){
			return ()=>{
				console.log(this.a);
			}
		}
		
	}
	Obj.b();
	Obj.c()();

### 5.使用call() 和 apply()调用；

	通过call()和apply()方法调用一个函数时。只是传入了参数而已，对this并没有什么影响。
	var Obj = {
		a: 10,
		b: function(n){
			var f = (v) => v+this.a
			return f(n)
		},
		c: function(n){
			var f = (v) => v+this.a
			var m = {a:20};
			return f.call(m,n)
		}
	}
	
	console.log(Obj.b(1)); //11
	console.log(Obj.c(1)); //11
	
### 6.箭头函数没有原型属性。

	var a =() =>{
		return 1;
	}
	
	function b(){
		return 2;
	}
	
	console.log(a.prototype); //undefined
	console.log(b.prototype); // object{...}
	
### 7.箭头函数不能换行

	var a = () =>
	{
	}

## 函数的this指向问题：

	箭头函数的this永远指向其上下文的 this，任何方法都改变不了其指向，如call(), bind(), apply()
	普通函数的this指向调用它的那个对象
	
	