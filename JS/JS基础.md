# 作用域与闭包
## let
1. 块级作用域
2. 不会变量提升

## const
1. 块级作用域
2. 不会变量提升
3. 常量

## 提升
1. 提升以作用域为单位
2. var会提升
3. 函数优先提升

## 函数作用域
1. 从内作用域查询到外作用域，外层同名变量会被遮蔽
2. 函数作用域可用于隐藏变量和函数，以及避免冲突
3. 立即执行函数不会污染外围作用域

## 闭包
闭包就是函数能够记住并访问它的词法作用域，即使当这个函数在它的词法作用域之外执行时

### 闭包形成的条件
1. 函数嵌套
2. 内部函数引用外部函数的局部变量

### 闭包两种情况
1. 函数作为返回值
2. 函数作为参数传递

## 作用域与执行上下文
作用域是一个“地盘”，其中变量没有值，要通过作用域对于的执行上下文环境来获取变量的值，同一个作用域下不同的调用会产生不同的执行上下文环境，继而产生不同的变量的值

作用域中变量的值是在执行过程中产生的、确定的，而作用域却是在函数创建时就确定了

# this与对象原型
## 四种改变this方法
1. new后，this指向新构造对象
2. call，apply和bind明确绑定
3. obj.foo()，this指向obj隐含绑定
4. foo()，Call-site默认绑定，严格模式undefined，否则就是全局对象

## 深浅拷贝
### 浅拷贝
1. Object.assign()：方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
2. Array.prototype.concat()：方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat
3. Array.prototype.slice()：方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice

### 深拷贝
1. JSON.parse(JSON.stringify(obj))：方法用来解析JSON字符串，构造由字符串描述的JavaScript值或对象。https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse
2. 递归方法
3. 调用函数库

## 原型链
1. prototype函数独有，总是被_proto_所指
2. _proto_指向该对象的构造函数的原型
3. constructor和prototype相反
4. Function构造一切
5. _proto_终点：object.prototype的_proto_指向null

# 类型
## 六种基本类型
1. null
2. uundefined
3. boolean
4. number
5. string
6. symbol

## 装箱
``` javascript
var a = "abc";

a.length; // 3
a.toUpperCase(); // "ABC"
```
“一般来说，基本上没有理由直接使用对象形式。让封箱在需要的地方隐含地发生会更好。换句话说，永远也不要做 new String("abc")、new Number(42) 这样的事情 —— 应当总是偏向于使用基本类型字面量 "abc" 和 42。”

## 拆箱
``` javascript
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true


var a = new String( "abc" );
var b = a + ""; // `b` 拥有开箱后的基本类型值"abc"

typeof a; // "object"
typeof b; // "string"
```

## 类型判断
### typeof
``` javascript
typeof undefined     === "undefined"; // true
typeof true          === "boolean";   // true
typeof 42            === "number";    // true
typeof "42"          === "string";    // true
typeof { life: 42 }  === "object";    // true
typeof null === "object"; // true


// 在 ES6 中被加入的！
typeof Symbol()      === "symbol";    // true
```
typeof无法区分object和null，而复杂数据类型里，除了函数返回了"function"其他均返回“object”
``` javascript
typeof({a:1}) // "object" 普通对象直接返回“object”
typeof [1,3] // 数组返回"object"
typeof(new Date) // 内置对象 "object"

typeof function(){} // "function"
```

## object.prototype.toString.call
基本数据类型都能返回相应的类型。
``` javascript
Object.prototype.toString.call(999) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(Symbol()) // "[object Symbol]"
Object.prototype.toString.call(42n) // "[object BigInt]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(true) // "[object Boolean]
```

复杂数据类型也能返回相应的类型
``` javascript
Object.prototype.toString.call({a:1}) // "[object Object]"
Object.prototype.toString.call([1,2]) // "[object Array]"
Object.prototype.toString.call(new Date) // "[object Date]"
Object.prototype.toString.call(function(){}) // "[object Function]"
```

## obj instanceof Object
可以左边放你要判断的内容，右边放类型来进行JS类型判断，只能用来判断复杂数据类型,因为instanceof 是用于检测构造函数（右边）的 prototype 属性是否出现在某个实例对象（左边）的原型链上。在不同window或者iframe间，不能使用instanceof。
``` javascript
[1,2] instanceof Array  // true
(function(){}) instanceof Function // true
({a:1}) instanceof Object // true
(new Date) instanceof Date // true
```

## Array常用方法
1. Array.length

    length 是Array的实例属性。返回或设置一个数组中的元素个数。

2. Array.prototype.concat()

    concat() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

3. Array.prototype.slice()
 
    slice() 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。

4. Array.prototype.splice()
   
    splice() 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。


## String常用方法
1. String.prototype.charAt()

    charAt() 方法从一个字符串中返回指定的字符。

2. String.prototype.charCodeAt()

    charCodeAt() 方法返回 0 到 65535 之间的整数，表示给定索引处的 UTF-16 代码单元

3. String.prototype.concat()
   
    concat() 方法将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回。

4. String.prototype.slice()
   
    slice() 方法提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串。

5. String.prototype.split()

    split() 方法使用指定的分隔符字符串将一个String对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。 