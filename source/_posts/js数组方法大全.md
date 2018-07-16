# js数组方法大全

标签（空格分隔）： js array

---
###  背景
    js数组也是我们平常操作最多的，本篇整理了js数组的所能想到的方法，作为参考用。 其实也是面试时经常会问道的一部分。

###  1、数组创建
JavaScript 数组创建有两种方法：
第一种是使用Array构造函数

    var array = new Array();
    
第二种方法是使用数组字面量创建数组

    var array = [];
    var array = [10];

如何确定是不是数组呢！ Array.isArray()

### 2、数组的方法
    
> join()
> push() 和 pop() 
> shift()和unshift()
> sort() 和 reverse()
> concat()
> splice()和slice()
> indexOf 和 lastIndexOf
> foreach, map, filter, every ,some,reduce, reduceRight

下面详细讲下每一个的作用：
#### 1）  join()
> 将数组的元素转变成一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符，该方法只接收一个参数：即分隔符

    var arr = [1,2,3];
    arr.join(,); //1，2，3
    
通过join()可以实现重复字符串。

    function repeatString(str,n){
        return new Array(n+1).joint(str)
    }
    console.log(repeatString("abc", 3)); // abcabcabc
#### 2) push()和pop(): 

> push可以添加想要数量的参数，把他放到数组的末尾，返回数组的长度。
> pop数组末尾减少最后一项，返回数组的长度
  push和pop正好对应，一个操作数组开始，一个操作末尾。

#### 3）shift() 和unshift()

> shift():删除原数组的第一项并返回该项的值。
> unshift()：将参数添加到原数组的第一项并返回数组的长度。

这组方法跟push和pop方法类似，都是操作数组的开始和末尾。
#### 4）sort()

> 按升序排列数组项，即最小的值位于最前面，最大的值排在最后面。
在排序时，sort方法会调用数组方法的tostring方法，然后比较得到的字符串确定如何排序，即使数组中的每一项都是数值，sort方法比较的也是字符串。

    var arr1 = ["a", "d", "c", "b"];
    console.log(arr1.sort()); // ["a", "b", "c", "d"]
    arr2 = [13, 24, 51, 3];
    console.log(arr2.sort()); // [13, 24, 3, 51]
    console.log(arr2); // [13, 24, 3, 51](元数组被改变)
    
 解决上述问题:
 sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值  的前面。
 比较函数接收两个参数，
 如果第一个参数应该位于第二个之前则返回一个负数，
 如果两个参数相等则返回0，
 如果第一个参数应该位于第二个之后则返回一个正数。
 以下就是一个简单的比较函数：
 
    function compare(v1,v2){
        if(v1<v2){
            return -1;
        } else if(v1> v2){
            return 1;
        } else {
            return 0;
        }
    }
    var arr1 = [13, 24, 3, 51];
    console.log(arr1.sort(compare));
如果想要降序的排序结果，只需要在函数内交换一下判断条件。
reverse(): 
> 反转数组项的顺序。

    var arr = [13, 24, 51, 3];
    console.log(arr.reverse()); //[3, 51, 24, 13]
    console.log(arr); //[3, 51, 24, 13](原数组改变)

#### 5)concat()

> 将参数添加到原数组中，这个方法会先创建当前数组一个副本，然后接收到的数组添加到这个数组的末尾，最后返回新创建的数组。在没有给 concat()方法传递参数的情况下，它只是复制当前数组并返回副本。

    var arr = [1,3,5,7];
    var arrCopy = arr.concat(9,[11,13]);
    console.log(arrCopy); //[1, 3, 5, 7, 9, 11, 13]
    console.log(arr); // [1, 3, 5, 7](原数组未被修改)
    如果传入的是二维数组
    var arrCopy2 = arr.concat([9,[11,13]]);
    console.log(arrCopy2); //[1, 3, 5, 7, 9, Array[2]]
    console.log(arrCopy2[5]); //[11, 13]
    
上述代码中，arrCopy2数组的第五项是一个包含两项的数组，也就是说concat方法只能将传入数组中的每一项添加到数组中，如果传入数组中有些项是数组，那么也会把这一数组项当作一项添加到arrCopy2中。
#### 6) slice()：
> 返回从原数组中指定开始下标到结束下标之间的项组成的新数组。

slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。
在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。
如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

    var arr = [1,3,5,7,9,11];
    var arrCopy = arr.slice(1);
    var arrCopy2 = arr.slice(1,4);
    var arrCopy3 = arr.slice(1,-2);
    var arrCopy4 = arr.slice(-4,-1);
    console.log(arr); //[1, 3, 5, 7, 9, 11](原数组没变)
    console.log(arrCopy); //[3, 5, 7, 9, 11]
    console.log(arrCopy2); //[3, 5, 7]
    console.log(arrCopy3); //[3, 5, 7]
    console.log(arrCopy4); //[5, 7, 9]
splice()：很强大的数组方法，它有很多种用法，可以实现删除、插入和替换
删除：可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数。
例如， splice(0,2)会删除数组中的前两项。
插入：可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、 0（要删除的项数）和要插入的项。
例如，splice(2,0,4,6)会从当前数组的位置 2 开始插入4和6。
替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,4,6)会删除当前数组位置 2 的项，然后再从位置 2 开始插入4和6
splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项，如果没有删除任何项，则返回一个空数组

    var arr = [1,3,5,7,9,11];
    var arrRemoved = arr.splice(0,2);
    console.log(arr); //[5, 7, 9, 11]
    console.log(arrRemoved); //[1, 3]
    var arrRemoved2 = arr.splice(2,0,4,6);
    console.log(arr); // [5, 7, 4, 6, 9, 11]
    console.log(arrRemoved2); // []
    var arrRemoved3 = arr.splice(1,1,2,4);
    console.log(arr); // [5, 2, 4, 4, 6, 9, 11]
    console.log(arrRemoved3); //[7]

#### 7) indexOf()和lastIndexOf()

> indexOf()：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的开头（位置 0）开始向后查找。 
> lastIndexOf：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的末尾开始向前查找。

这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回1。在比较第一个参数与数组中的每一项时，会使用全等操作符。

    var arr = [1,3,5,7,7,5,3,1];
    console.log(arr.indexOf(5)); //2
    console.log(arr.lastIndexOf(5)); //5
    console.log(arr.indexOf(5,2)); //2
    console.log(arr.lastIndexOf(5,4)); //2
    console.log(arr.indexOf("5")); //-1

#### 8) foreach()
> 对数组进行遍历循环，对数组中的每一项运行给定函数。这个方法没有返回值。参数都是function类型。
    
    var arr = [1, 2, 3, 4, 5];
    arr.forEach(function(x, index, a){
    console.log(x + '|' + index + '|' + (a === arr));
    });
    // 输出为：
    // 1|0|true
    // 2|1|true
    // 3|2|true
    // 4|3|true
    // 5|4|true
    
#### 9) map() 
> 指映射,对数组的每一项运行给定函数，返回每次函数调用的结果组成的数组

    var arr = [1, 2, 3, 4, 5];
    var arr2 = arr.map(function(item){
    return item*item;
    });
    console.log(arr2); //[1, 4, 9, 16, 25]

#### 10) filter()
>过滤功能，数组中的每一项运行给定函数，返回满足过滤条件组成的数组

    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    var arr2 = arr.filter(function(x, index) {
    return index % 3 === 0 || x >= 8;
    }); 
    console.log(arr2); //[1, 4, 7, 8, 9, 10]
    
#### 11) every()
> 判断数组中每一项是否满足条件，只有所有项都满足条件才会返回true。

    var arr = [1, 2, 3, 4, 5];
    var arr2 = arr.every(function(x) {
    return x < 10;
    }); 
    console.log(arr2); //true
    var arr3 = arr.every(function(x) {
    return x < 3;
    }); 
    console.log(arr3); // false
    
#### 12) some()
> 判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回true。

    var arr = [1, 2, 3, 4, 5];
    var arr2 = arr.some(function(x) {
    return x < 3;
    }); 
    console.log(arr2); //true
    var arr3 = arr.some(function(x) {
    return x < 1;
    }); 
    console.log(arr3); // false

#### 13) reduce()和 reduceRight()
> 两个方法都会实现迭代数组的所有项，然后构建一个最终返回的值。
reduce()方法从数组的第一项开始，逐个遍历到最后。 reduceRight()则从数组的最后一项开始，向前遍历到第一项。

这两个方法都接收两个参数：
一个在每一项上调用的函数和（可选的）作为归并基础的初始值。
传给 reduce()和 reduceRight()的函数
接收 4 个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

下面代码用reduce()实现数组求和，数组一开始加了一个初始值10。

    var values = [1,2,3,4,5];
    var sum = values.reduceRight(function(prev, cur, index, array){
        return prev + cur;
    },10);
    console.log(sum); //25





