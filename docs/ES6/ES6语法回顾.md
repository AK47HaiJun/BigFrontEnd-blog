### ES6  语法 回顾

### 基础

#### let  var    的区别

```js
let  与  var  的区别
	
	var : 变量在没有var之前声明之前就可以使用，不过值是 undefind
    
   	let : 变量必须声明之后才可以使用，并且作用域范围在 代码块范围内有效

    // var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
    
ES6 规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。
```

#### 暂时性死区

```js
定义： 在一个代码块范围内，使用let命令声明变量之前，该变量都是不可用的。


eg：
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}



# ES6 规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。
```

#### const 

```js
定义： const声明一个只读的常量。一旦声明，常量的值就不能改变。

const PI = 3.1415;
PI // 3.1415

PI = 3;
// 报错


#注意： 
	1. 一旦定义const 常量 必须初始化赋值
    2.const的作用域与let命令相同：只在声明所在的块级作用域内有效。
    3.const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。
    4. const 变量为 复合类型时，变量指向的内存地址，保存的只是一个指向实际数据的指针，可以为符合类型添加属性或者方法，但是不可以改变指针地址引用
    
    
    const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only


const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```

#### ES6 声明变量的6种方法

```js
ES5 中
	var    function

ES6 中
	let  const    class  import
```

#### 顶层对象属性

```js
1.在浏览器环境指的是window对象，
2.在 Node 指的是global对象。
3.ES5 之中，顶层对象的属性与全局变量是等价的。

window.a = 1;
a // 1

a = 2;
window.a // 2




#注意
4.在ES6中，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined

5. ES6 这样的设计解决了在编译时就可以找到错误
```

### 变量解构赋值

```js
定义： ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined    
z // []

# 如果解构不成功，变量的值就等于undefined
```

#### 数组解构赋值

```js
1. 解构赋值允许指定默认值。
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

```

#### 对象解构赋值

```js
#定义： 对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined


# 注意
1.对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量，然后这个变量就会拥有它的属性和方法
// 例一
let { log, sin, cos } = Math;

// 例二
const { log } = console;
log('hello') // hello



2. 变量名与属性名不一致，必须写成下面这样。（左边和右边不等时）

let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world

# 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
```

#### 解构默认值

```js
#默认值生效的条件是，对象的属性值严格等于undefined。


var {x = 3} = {x: undefined};
x // 3

var {x = 3} = {x: null};
x // null
```

#### 字符串解构赋值

```js
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"


let {length : len} = 'hello';
len // 5
```

#### 解构注意

```js
#解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
```

#### 函数参数解构赋值

```js
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3



# 参数指定默认值
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

#### 解构用途

```js
（1）交换变量的值
（2）从函数返回多个值
（3）函数参数的定义
（4）提取 JSON 数据
（5）函数参数的默认值
（6）遍历 Map 结构
（7）输入模块的指定方法
const { SourceMapConsumer, SourceNode } = require("source-map");
```

### 字符串扩展

#### 模板字符串

```js
定义：模板字符串（template string）是增强版的字符串，用反引号``标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量.
// 模板字符串中使用变量时，   `{$变量名}`

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;

log(`${obj.first}`)
log(`
多行显示模板字符串
${obj.last}
测试
`)
// 多行显示模板字符串
// world
// 测试
```

### 数值扩展

#### Number.isFinite()

```js
Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。

Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
```



#### Number.isNaN()

```js
Number.isNaN()用来检查一个值是否为NaN。


Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
```

#### 小结

```js
#Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。
```

#### Number.parseInt(), Number.parseFloat()

```js
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

####  Number.isInteger()

```js
Number.isInteger()用来判断一个数值是否为整数。


Number.isInteger(25) // true
Number.isInteger(25.1) // false


#如果对数据精度的要求较高，不建议使用Number.isInteger()判断一个数值是否为整数
```

####  指数运算符 **

```js
2 ** 2 // 4

// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2
```

### 函数拓展

#### 与解构赋值默认值结合使用

```js
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined
```

#### rest 参数 ...

```js
rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。


function  getValue(...rest) {
    for(var i = 0; i <rest.length; i ++) {
        console.log(rest[i])
    }
}
let arr = [1,2,3,4,5,6]

getValue(arr)
// [ 1, 2, 3, 4, 5, 6 ]
```

#### 函数 name 属性

```js
函数的name属性，返回该函数的函数名。

#如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名。
var f = function () {};

// ES5
f.name // ""

// ES6
f.name // "f"
```

#### 箭头函数  => 

```
定义： ES6 允许使用“箭头”（=>）定义函数。


var deleItem = (index) => {
    console.log(`索引为${index}`)
}

deleItem(2)
// 索引为2
```

##### 箭头函数 配合参数解构使用

```js
var info = {
    name:"张三",
    id: '002'
}

var  getItem = ({name,id}) => {
    console.log(`查寻name=${name},id=${id}`)
}
getItem(info)
//查寻name=张三,id=002

// 等同于====

function getItems(info){
    console.log(`查寻name=${info.name},id=${info.id}`)
}
```

##### 箭头函数注意

- 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。
- 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
- 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
- 在箭头函数中，this指向是固定的。



### 数组扩展

#### 复制数组

```js
数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]



#扩展运算符提供了复制数组的简便写法。
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

#### 合并数组

```js
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

#### 数组解构结合使用

```js
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

#### **字符串**

```js
扩展运算符可以将字符串转为真正的数组。

[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

#### Array.of`方法用于将一组值，转换为数组。

```js
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1

# Array.of总是返回参数值组成的数组。如果没有参数，就返回一个空数组。

```

#### 数组实例的 find() 和 findIndex() 

```js
find()
用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。



[1, 4, -5, 10].find((n) => n < 0)
// -5


findIndex()
数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2	
```

#### 数组实例的 fill()

```js
fill方法使用给定值，填充一个数组。

['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]


# fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']

fill方法从 1 号位开始，向原数组填充 7，到 2 号位之前结束。
```

#### 数组实例的 includes()

```js
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。

[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true



ES5 中 通过 indexOf 实现，
1.它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1
2.它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1
```

#### 数组实例的 flat()

```js
Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

flat()
var  newArr = [1, 2, [3, 4]];
console.log([1, 2, [3, 4]].flat())
console.log('==================')
console.log(newArr)
// [ 1, 2, 3, 4 ]
// ==================
// [ 1, 2, [ 3, 4 ] ]

# 注意：
1 flat()默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将flat()方法的参数写成一个整数，表示想要拉平的层数，默认为1。

[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]

2. 如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。

[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]

3. 如果原数组有空位，flat()方法会跳过空位。
[1, 2, , 4, 5].flat()
// [1, 2, 4, 5]
```

#### 数组中的空位

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

```js
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"



# 注意： 
1.ES6 则是明确将空位转为undefined。
2.Array.from方法会将数组的空位，转为undefined
Array.from(['a',,'b'])
// [ "a", undefined, "b" ]
3.扩展运算符（...）也会将空位转为undefined。
[...['a',,'b']]
// [ "a", undefined, "b" ]
....

```



### 对象拓展

#### 属性的简洁表示法 

```js
ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。


let birth = '2000/01/01';

const Person = {

  name: '张三',

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
```

#### 属性遍历

- **（1）for...in**
- **（2）Object.keys(obj)**
- **（3）Object.getOwnPropertyNames(obj)**
- **（4）Object.getOwnPropertySymbols(obj)**
- **（5）Reflect.ownKeys(obj)**

```js
以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

首先遍历所有数值键，按照数值升序排列。
其次遍历所有字符串键，按照加入时间升序排列。
最后遍历所有 Symbol 键，按照加入时间升序排列
```

### 对象的新增方法

#### Object.is()

```js
ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。

ES6 用来解决这个问题,提出了Object.is，用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。


+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

#### Object.assign()

```js
Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
Object.assign方法的第一个参数是目标对象，后面的参数都是源对象。

var newObj = {}
var newOne = {a:'1'}
var newSecond = {b:'2'}
var newThree = {b:'3'}
newObj = Object.assign(newOne,newSecond,newThree)
console.log(newObj)
// { a: '1', b: '3' }  如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。


#注意：
Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。
```

### Set 和 Map

#### Set

```js
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
Set本身是一个构造函数，用来生成 Set 数据结构。

// 数组去重
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

##### Set 基本用法

```js
#注意
1.Set函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。


// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56
```

##### Set 实例的属性和方法

```js
Set 结构的实例有以下属性:
	1.Set 构造函数
    2.Set.prototype.size  返回Set实例的成员总数。
    
    
Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）

#四个操作方法：
	1.Set.add（）添加某个值，返回 Set 结构本身。
    2. Set.delete() 删除某个值，返回一个布尔值，表示删除是否成功。
    3. Set.has() 返回一个布尔值，表示该值是否为Set的成员。
    4. Set.clear() 清除所有成员，没有返回值。
    
    
    s.add(1).add(2).add(2);
// 注意2被加入了两次
const s = new Set();
s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false



#四个遍历方法
	1.Set.keys()  返回键名的遍历器
	2.Set.values()  返回键值的遍历器
	3.Set.entries()  返回键值对的遍历器
	4.Set.forEach() 使用回调函数遍历每个成员
    
    let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

#entries方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。
```

#### 使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

#### Map

##### Map基本用法

```js
定义： JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。
Object 结构提供了“字符串—值”的对应，使用带来了很大的限制。

Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。


const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false


Map 作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"


# 注意
如果对同一个键多次赋值，后面的值将覆盖前面的值。

const map = new Map();

map
.set(1, 'aaa')
.set(1, 'bbb');

map.get(1) // "bbb"
```

##### Map实例的属性和操作方法

```js
（1）size 属性
size属性返回 Map 结构的成员总数。

const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2



（2）Map.set(key, value)
set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined


（3）Map.get(key)
get方法读取key对应的键值，如果找不到key，返回undefined。
const m = new Map();

const hello = function() {console.log('hello');};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!


（4）Map.prototype.has(key)
has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');


m.has('edition')     // true
m.has('years')       // false


（5）Map.delete(key)
delete方法删除某个键，返回true。如果删除失败，返回false。

const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false


（6）Map.prototype.clear()
clear方法清除所有成员，没有返回值。

let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0



#三个遍历器生成函数和一个遍历方法。
	1.Map.keys()  返回键名的遍历器
	2.Map.values()  返回键值的遍历器
	3.Map.entries()  返回键值对的遍历器
	4.Map.forEach() 使用回调函数遍历每个成员
    
    const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

##### Map 结构转为数组结构，使用扩展运算符（`...`）

```js
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

### Promise

#### 定义

```js
Promise 是异步编程的一种解决方案，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

//Promise对象有以下两个特点。

（1）对象的状态不受外界影响。
Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。


(2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。只有两种情况
 	1. 从pending（进行中）  变为fulfilled（已成功）
 	2.从pending （进行中） 变为rejected （已失败）
 
 
 // Promise 缺点
 	Promise一旦创建就会立刻执行，无法中途取消。
```

#### 基本用法

```js
基本格式： 
var boo = false 
// 创建 Promise, 需要传递一个函数，函数有两个参数： reselve 成功状态， reject 失败状态
//  不同状态执行不同事情
let promise = new Promise( (reselove , reject) => {
    if (boo == false ) {
      return   reject()
    } else {
      return   reselove()
    }
})

promise.then( success => {
    console.log('成功状态')
}, err => {
    console.log('失败状态')
})

//一般来说，调用resolve或reject以后，Promise 的使命就完成了，后继操作应该放到then方法里面，而不应该直接写在resolve或reject的后面。所以，最好在它们前面加上return语句，这样就不会有意外。

----------------------------------------------------------------------------------------------
```

#### 不同状态执行不同业务逻辑 reselve 成功   reject 失败

```js
<script>
        var btn = document.getElementById('btn')
        var boo = false
        var data = {
            name: '张三',
            age: 22
        }
        btn.onclick = () => {
            new Promise((reselve, reject) => {
                // 模拟 通过 boolean 值来判断 该状态为 成功 or  失败
                if (boo == false) {
                    // boo = fasle ，那么它就执行 reject 函数， 参数为要传递的数据
                   return  reject(data)   
                } else {
                    return  reselve(data)
                }
                //  Promise 只要创建成功，那么它就会执行， 状态为初始 状态， 想通过不同状态执行相应业务
                // 就需要通过 then 来执行
                //  第一个 then 为 成功的状态，也就是 reselve 状态
            }).then( success => {
                console.log(`成功：promise 状态为 reselve， 数据为：  ${JSON.stringify(success)} +  `)
                // 为了验证不同状态执行不同事情， 将boo 取反
                boo = false
            }).catch( err => {
                console.log(`失败 ：promise 状态为 reject  `)
                boo = true
            })
        }
    </script>
```

####  Promise 实例 互相依赖,如何执行

```js
// Promise 实例 互相依赖
//  如果Promise2 依赖 Promise1 ， 就得 依据 Promise1 的状态来执行 Promise 的状态
//  如果Promise1 的状态为 reject， 那么 Promise2 的状态则就会执行 reject 的状态


let promise1 = new Promise( (reselve, reject) => {
    reject()
})


let promise2 = new Promise( (reselve, reject) => {
    // console.log(reselve(promise1))
    if (reselve) {
       return  reselve(promise1)
    } else {
       return  reject(promise1)
    }
}).then(success => {
    console.log('成功状态-------promise')
}).catch( err => {
    console.log('失败状态')
})
```



#### Promise.prototype.then()

```js
//Promise 实例具有then方法，它的作用是为 Promise 实例添加状态改变时的回调函数。then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。


//第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

var data = {
    name: '张三',
    age: 22
}
new Promise( (reselve , reject ) => {
    return reselve(data)
}).then(data => {
    console.log(data)
    // { name: '张三', age: 22 }
},err => {
    console.log(err)
})  
    

```

#### Promise.prototype.catch() 

```js
//Promise.prototype.catch方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。

// 何时采用 catch
当 Promise 的 reselve 为 null   或者  undefined 时，采用。



const promise = new Promise(function(resolve, reject) {
  reject(new Error('test'));
});
promise.catch(function(error) {
  console.log(error);
});


#注意
//如果Promise 的状态为 reselved ，然后在 reselve 后面在抛出 错误则是 捕捉不到的，因为 Promise 的状态一旦创建则不能修改


const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });
// ok
```

#### 捕捉错误写法

```javascript
优先采用 catch写法
# 因为 第一种写法可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch），Promise 对象抛出的错误可以传递到外层代码，
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });


不要采用
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });


//一般总是建议，Promise 对象后面要跟catch方法，这样可以处理 Promise 内部发生的错误。catch方法返回的还是一个 Promise 对象，因此后面还可以接着调用then方法。
```

#### Promise.prototype.finally()

```js
//finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

基本格式：
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});

# 注意：
finally方法的回调函数不接受任何参数，finally方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。
```

### async函数

#### 定义

```js
ES2017 标准引入了 async 函数，使得异步操作变得更加方便。它就是 Generator 函数的语法糖。
```

#### 基本用法

```js
async 函数返回一个Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。


async function timeout(ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);




# async 函数有多种使用形式。
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...)

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
```

#### 语法

```js
1.async函数返回一个 Promise 对象。
//async函数内部return语句返回的值，会成为then方法回调函数的参数。

async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"




2. async函数内部抛出错误，会导致返回的 Promise 对象变为reject状态。抛出的错误对象会被catch方法回调函数接收到。

async function f() {
  throw new Error('出错了');
}

f().then(
  v => console.log(v),
  e => console.log(e)
)
// Error: 出错了
```

####  Promise 对象状态变化

```js
async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。


async function getTitle(url) {
  let response = await fetch(url);
  let html = await response.text();
  return html.match(/<title>([\s\S]+)<\/title>/i)[1];
}
getTitle('https://tc39.github.io/ecma262/').then(console.log)
// "ECMAScript 2017 Language Specification"

只有执行完 fetch  response.text   html.match  这三个加await 的函数，才会执行 then后面的 内容
```

### await 命令

```js
await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。


async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123


# 注意
1.任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}


2. 如果想实现 第一个Promise 状态为 reject， 第二个Promise 还可以执行

//第一种 实现方法
try...catch 

async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world



// 第二种实现方法
await后面的 Promise 对象再跟一个catch方法，处理前面可能出现的错误。
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// 出错了
// hello world
```

### Class

#### 定义

```js
在JavaScript中，生成实例对象的传统方法是通过构造函数。

function Point(x, y) {
  this.x = x;
  this.y = y;
}

//Point 的静态方法
Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);


在ES6中 提供了Class（类）这个概念，它只是一个语法糖，它的绝大部分功能，ES5 都可以做到。

基本格式：

class Point {
    //构造函数
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

#### ES6  and  ES5 比较

```js
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```

#### 类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign`方法可以很方便地一次向类添加多个方法。

```js
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});
```

#### constructor 方法

```js
constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

class Point {
}

// 等同于
class Point {
  constructor() {}
}


#注意：
constructor方法默认返回实例对象（即this），完全可以指定返回另外一个对象。
```

#### 取值函数（getter）和存值函数（setter）

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

#### 属性表达式 

```js
    类的属性名可以使用  表达式
    let methodName = 'getArea';

    class Square {
      constructor(length) {
        // ...
      }

      [methodName]() {
        // ...
      }
    }
```

#### Class 表达式

```js

const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};

let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined


# 注意
代码使用表达式定义了一个类。需要注意的是，这个类的名字是Me，但是Me只在 Class 的内部可用，指代当前类。在 Class 外部，这个类只能用MyClass引用。
```

#### 静态方法

```js
#如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。
#如果静态方法包含this关键字，这个this指的是类，而不是实例。


class Foo {
  static classMethod() {
    return 'hello';
  }
  static bar() {
    this.baz();
  }
  static baz() {
    console.log('hello');
  }
  baz() {
    console.log('world');
  }
}

Foo.classMethod() // 'hello'
Foo.bar() // hello

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

#### 父类的静态方法，可以被子类继承。

```js
1. 子类直接 调用父类的静态方法
class Father {
    static say() {
        console.log('会说话 ')
    }
}

class Son extends Father {
    
}
Son.say()
// 会说话




2. 静态方法也是可以通过子类的super对象上调用的。

class Father {
    static say() {
       return  console.log('会说话 ')
    }
}

class Son extends Father {
    static toSay(){
        //通过super调用父类方法
        return super.say()
    }
}
Son.toSay()
// 会说话
```

#### 实例属性

```js
实例属性除了定义在constructor()方法里面的this上面，也可以定义在类的最顶层。

class IncreasingCounter {
  constructor() {
    this._count = 0;
  }
  get value() {
    console.log('Getting the current value!');
    return this._count;
  }
  increment() {
    this._count++;
  }
}


实例属性放在最顶层
class IncreasingCounter {
  _count = 0;
  get value() {
    console.log('Getting the current value!');
    return this._count;
  }
  increment() {
    this._count++;
  }
}

#好处：
这种新写法的好处是，所有实例对象自身的属性都定义在类的头部，看上去比较整齐，一眼就能看出这个类有哪些实例属性。
```



#### 静态属性

```js
静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

class Foo {
}

Foo.prop = 1;
Foo.prop // 1


# ES6 明确规定，Class 内部只有静态方法，没有静态属性。ES6 中提供了一种新的解决办法 在实例属性的前面，加上static关键字。
// 老写法
class Foo {
  // ...
}
Foo.prop = 1;

// 新写法
class Foo {
  static prop = 1;
}
```



### Class 继承

#### 基本使用

```js
#ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

Class 可以通过extends关键字实现继承.子类继承了父类，子类就拥有的父类的方法和属性。
# 子类继承了父类，必须在构造函数中实现 super函数，否则不能创建类的实例

class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); // ReferenceError

# 因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。

基本格式：
class Animal {

}
class Dog extends Animal {
    constructor(){
        super()
    }
}
```

#### 需要注意

```js
//在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。这是因为子类实例的构建，基于父类实例，只有super方法才能调用父类实例。

class Animal {
    types = '种类'
    run(){
        console.log('会奔跑')
    }
}

class Dog extends Animal {
    constructor(age,name){
        //this.types   报错
        // super 调用父类属性和方法
        super()
        this.age = age
        this.types
        this.name = name
    }
}
var dog = new Dog(2,'二哈')
console.log(dog.types)
console.log(dog.age)
console.log(dog.name)
dog.run()
// 种类
// 2
// 二哈
// 会奔跑

```

#### Object.getPrototypeOf()

```js
#Object.getPrototypeOf方法可以用来从子类上获取父类。

Object.getPrototypeOf(ColorPoint) === Point
// true
```

#### super

```js
# super这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。
#super虽然代表了父类的构造函数，但是返回的是子类的实例，即super内部的this指的是子类的实例，因此super()在这里相当于父类.prototype.constructor.call(子类)。

1. 第一种情况，super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。
class A {}

class B extends A {
  constructor() {
    super();
  }
}
2.第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2

# 在子类的静态方法中通过super调用父类的方法时，方法内部的this指向当前的子类，而不是子类的实例。

class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3

静态方法B.m里面，super.print指向父类的静态方法。这个方法里面的this指向的是B，而不是B的实例。
```



### Module 的语法

#### 定义

```js
Module将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。
在 ES6 之前，模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。

// ES6模块
import { stat, exists, readFile } from 'fs';

加载fs 模块得三个方法，其他方法不加载， 实现了模块的静态加载
```

#### export 命令

```js
模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。

一个模块内部的成员想要被外部访问，就需要使用 export 导出

var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };

//注意
1.export输出的变量就是本来的名字，但是可以使用as关键字重命名。

function v1() { ... }commit
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
               
在导入的文件内，可以直接使用as 后的变量名 使用
```

#### export 导出 格式

```js
//变量

var name  = '张三'
var age = 22

export { name , age }


//函数
1. export function  getAge() {}

2. function getAge() {}     
导出 export {getAge}

// 类
var obj  = new Object()

export { obj}


//注意
export命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，

function foo() {
  export default 'bar' // SyntaxError
}
foo()
```



### import 命令

#### 定义

```js
使用export命令定义了模块的对外接口以后，其他 JS 文件就可以通过import命令加载这个模块。

/ main.js
import { firstName, lastName, year } from './profile.js';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}



# 注意
1. import命令输入的变量都是只读的，因为它的本质是输入接口。不允许在加载模块的脚本里面，改写接口。

import {a} from './xxx.js'

a = {}; // Syntax Error : 'a' is read-only;	


2.如果多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。
```

#### 模块的整体加载 

> 除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。
>
> 模块整体加载所在的那个对象，它是可以静态分析的，所以不允许运行时改变。



```js
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

```js
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
```

### export default 命令  指定默认输出

> 使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。
> 而使用 export default  命令， 用户不需要内部 的变量名 或者 函数名，可以直接导入使用
>
> 1. 使用 import 命令。 变量名 或者 函数 需要 放在  { }  里
>
>    import { name ,  age }  from './test.js'
>
> 2. 使用 export default 命令， 用户可以直接使用一个自定义的变量名直接使用该文件的中提供的方法或者变量/
>
>    import  age as getAge  from './test.js'
>
>    使用时，直接调用 getAge（） ， 上面代码 将 age 重命名为 getAge

##### 注意

```js
1.export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1;

// 正确
export default 42;

// 报错
export 42;

2.export default也可以用来输出类。

// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();

```

### export 与 import 的复合写法 

> ​	如果在一个模块之中，先输入后输出同一个模块，`import`语句可以与`export`语句写在一起。

```js
export { foo, bar } from 'my_module';

// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```

### 跨模块常量,一个模块中的变量 / 方法 多个模块使用

```js
可以这样写

// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;

// test1.js 模块
import * as constants from './constants';
console.log(constants.A); // 1
console.log(constants.B); // 3

// test2.js 模块
import {A, B} from './constants';
console.log(A); // 1
console.log(B); // 3
```

