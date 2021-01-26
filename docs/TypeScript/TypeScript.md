[![reRoNR.png](https://s3.ax1x.com/2020/12/13/reRoNR.png)](https://imgchr.com/i/reRoNR)

### 入手导图

[![reWaP1.png](https://s3.ax1x.com/2020/12/13/reWaP1.png)](https://imgchr.com/i/reWaP1)



## 	TypeScript 

## 一，安装环境

```js
#npm install -g typescript
```

### 1.1 VSCode 配置自动编译文件

```js
#1. 在目录下  
	tsc --init   自动生成 tsconfig.json 
	tsconfig.json 下 outdir 是输出的路径
#2.  任务--- 运行任务   监视 tsconfig.json

```

## 二，基本语法

#### 2.1 数组

##### 定义使用

```typescript
// 第一种定义方法   let 数组名：类型[] = []
var arr:number[] = [1,2,3];
console.log(arr);
// 第二种定义方法    let 数组名：Array[类型] = []
var newArr:Array<number> = [1,2,3];
console.log(newArr)
```

#### 2.2 元组

>它表示 已经 元素的个数和元素类型的数组，各个元素类型可以不一样。

##### 访问元组长度 和 元素

```javascript
var strArr:[number,string,boolean] = [22,'测试',false]
console.log(strArr.length)
console.log(strArr[0])
#它只能按类型的优先顺序输入内容，否则报错
```



#### 2.3 枚举 enum

> `enum`类型是对JavaScript标准数据类型的一个补充。
>
> - 如果没有给枚举指定索引的话，默认为 0 ， 通过  `枚举对象[索引]`  可以获取值
> - 如果指定了枚举索引为字符串的话，通过  `枚举.属性`  获取的 它的值

```typescript
enum Sex {Man,Woman}

let m:Sex = Sex.Man;
console.log(m) //0


let w: string = Sex[1]
console.log(w) //Woman


enum Animal {Dog = 3, Cat, Tiger};

console.log(Animal[5]) //Tiger


enum info {student = '学生', teacher = '教师',  parent = '家长' };

console.log(info.teacher)  //教师
```

#### 2.4 任意类型 any

> any 为 任意类型，一般在获取dom 使用

```typescript
// 任意类型
const newArrs:any = ['测试不同数据 ',222,false]
console.log(newArrs)
# 输出结果为[ '测试不同数据 ', 222, false ]
# 使用场景： 当你不知道类型 或 一个对象 或数据 需要多个类型时，使用any
```

#### *2.5 undefined 类型*

```typescript
let num:number | undefined ;
console.log(num) // 输出 undefined， 但是不会报错
let newNum:number | undefined = 33;
console.log(newNum)  // 输出 33 
```

#### 2.6 never 类型

> never 代表不存在的值类型，常用作为 抛出异常或者 无限循环的函数返回类型

```js
# 应用场景 
	#1. 抛错误
    const errDate = (message:string): never => {
        throw new Error(message)
    }
    #2.  死循环
    const date_for = (): never => {
        while(true) {}
    }
    
# never 类型是任意类型的子类型，没有类型是never 的子类型
别的类型不能赋值给never类型， 而 never 类型可以赋值给任意类型
```



#### 2.7 void 类型

> void  为 函数没有类型，一般用在没有返回值的函数

```typescript
# 如果方法类型为number， 则必须返回内容， 内容且必须为数字
function add():number{
    return 2323;
}

# 如果方法类型为void，不需要返回内容
function getAdd():void{
    console.log('测试')
}

# 如果方法类型为any，则可以返回任意类型
function getAny():any{
    return 999 + 'Hello TypeScript'
}

console.log(getAny())//999 'Hello TypeScript'
```

## 三，类型断言

> 什么是类型断言？
>
> - 有时候你在定义一个变量时，起初是不知道是什么类型，但在使用过程中知道是什么类型，这时就会用到类型断言了。

##### 3.1第一种写法 尖括号

```javascript
const str = '测试'

const resLength : number = (<string>str).length
```

##### 3.2第二种写法  as

```javascript
const str = '测试'

const resLength : number = (str as string).length
```

## 四，接口

> TypeScript的核心原则之一是对值所具有的*结构*进行类型检查。
>
> 验证类型时，顺序不影响验证。
>
> 简单的来说，它是类型约束的定义，当你使用这个定义接口时，它会一一匹对接口中定义的类型。
>
> 只要不满足接口中的任何一个属性，都不会通过的。



##### 4.1 接口可选属性

> 有时候，接口属性不是必须全部需要的，满足某些条件才会需要，这时，就可以采用可选属性
>
>  格式 ： `属性  ?:   类型`

```javascript
interface  Login{
    userName: string,
    password: string,
    auth ?: string
}



function getLogin(obj: Login) {
    if(obj.auth == '管理员') {
        console.log('可以查看所有菜单')
    } else {
        console.log('您的权限比较低,目前不能查看')
    }
}


getLogin({
    userName:'zhangsanfeng',
    password: '12121121sd',
    auth: '管理员'
})    //可以查看所有菜单


getLogin({
    userName:'zhangsanfeng',
    password: '12121121sd'
})    //您的权限比较低,目前不能查看



```



##### 4.2 接口 只读属性

> 只读属性： 意味着给属性赋值了后，不可改变。
>
> 格式： `readonly   属性 :  类型 `

```javascript
interface Menus {
    readonly title?:string,
    icon?:string,
    readonly path?:string,
    readonly Layout?:string
}


function getMenuInfo(data:Menus){
    console.log(data)
    data.icon = '修改图标'   // 可以修改
    // data.path = '/home'    报错，禁止修改，接口属性为只读
    console.log(data)
}


getMenuInfo({
    title: '主页',
    icon:'homes',
    path:'/home',
    Layout: 'Layput'
})

```



##### 4.3 接口函数类型

> 用来约束函数传递参数类型
>
> - 函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。
> - 格式： `(参数1： 类型，参数2：类型) ： 返回值类型 `

````javascript
// 获取用户信息
interface UserInfo {
    (name: string,adress: string,phone: number) : string
}

let getUsefInfo:UserInfo = function(name,adress,phone){
    return `${name}住在${adress},手机号为${phone}`
}

console.log(getUsefInfo('张锋','天津南开区xx小区',188888888))
````

##### 4.4 接口可索引类型

> 在定义一个数组时，可以定义一个 索引类型接口，这样就约束了它必须传递哪些类型的值。
>
> 访问: `通过  变量[索引]`

```javascript
interface Code{
    [index : number] : string
}

let errCode : Code = ['200 成功','301 重定向', '400 客户端错误', '500  服务端出错']

console.log(errCode[3])  //500  服务端出错
```



##### 4.5 类型接口实现

> 接口描述了类的公共部分，而不是公共和私有两部分。 它不会帮你检查类是否具有某些私有成员。

```javascript
interface Animals {
    eye: number,
    leg: number,  
}


class Dog  implements Animals {
    eye: number;
    leg: number;
    kind: string
    constructor(eye: number, leg: number, kind: string) {
        this.eye = eye
        this.leg = leg
        this.kind = kind
    }
    getDogInfo(){
        console.log(`品种为${this.kind},有${this.eye}只眼，${this.leg}条腿`)
    }
}

let hashiqi = new Dog(2,4,'哈士奇');
hashiqi.getDogInfo() //品种为哈士奇,有2只眼，4条腿
```

##### 4.6 接口继承(多合一)

> 接口之间可以互相继承，这样可以更灵活地将接口分割到可重用的模块里。

````javascript
interface Shape1 {
    data: string
}

interface Shape2  extends Shape1{
    code: number
    // Shape2 具有 Shape1 的特征
}


class Message implements Shape2 {
    code: number;
    data: string;
    constructor(code : number,data: string) {
        this.code = code;
        this.data = data
    }
    getCodeInfo(){
        console.log(`状态码为${this.code},返回信息为${this.data}`)
    }
}


let err = new Message(500,'服务端错误')
err.getCodeInfo()  //状态码为500,返回信息为服务端错误
````



##### 4.7 接口继承类

> 当接口继承了一个类，那么接口也会拥有类的属性和方法。
>
> 当别的类  实现这个 接口时，会同时实现 接口的属性和方法， 继承类的属性和方法

````javascript
class Log {
     time: string = '2020-11-2';
     getLog(){
         console.log('测试')
     }
}

interface  Shape3  extends Log{
    message : string
}



class ErrLog implements Shape3 {
    message: string ;
    time: string;
    constructor(message: string, time: string) {
        this.message = message;
        this.time = time
    }
    getLog(): void {
        console.log("Method not implemented.");
    }
}

let errs = new ErrLog('测试','2020-11-2')
errs.getLog()  //Method not implemented.

````

## 五，泛型

> 接触过JAVA 的同学，应该对这个不陌生，非常熟了。
>
> 作为前端的我们，可能第一 次听这个概念。  通过 字面意思可以看出，它指代的类型比较广泛。
>
> - 作用：  : 避免重复代码，代码冗余
>
> 但是它和 any  类型 还是有区别的。
>
> - any  类型：  如果一个函数类型为any，那么它的参数可以是任意类型，一般传入的类型与返回的类型应该是相同的。如果传入了一个 string 类型的参数，那么我们也不知道它返回啥类型。
> - 泛型 ： 它可以使  `返回类型  和 传入类型` 保持一致，这样我们可以清楚的知道函数返回的类型为什么类型。
>
> 





##### 5.1 泛型接口

> 泛型接口可以这样理解： 
>
> 当你需要给接口指定类型时，但目前不知道属性类型为什么时，就可以采用泛型接口
>
> 你可以给接口指定参数为多个泛型类型，也可以单个；当使用时，明确参数类型即可。

[![reJsYR.png](https://s3.ax1x.com/2020/12/13/reJsYR.png)](https://imgchr.com/i/reJsYR)

```javascript
 interface User <T,S,Y> {
     name: T;
     hobby: S;
     age: Y;
 }

 class People implements User<String,String,Number> {
     name: String;
     hobby: String;
     age: Number;
     constructor(name:string,hobby:string,age:number){
         this.name = name;
         this.hobby = hobby;
         this.age = age; 
     }
    getInfo(){
        console.log(this.name+"------------------"+this.hobby)
        console.log(`${this.name}的年龄为${this.age}`)
    }  
 }
 let xiaoZhou =  new People('小周','敲代码',22)
 xiaoZhou.getInfo() 
 //小周------------------敲代码
//  小周的年龄为22

```



##### 5.2 泛型函数

> 定义泛型函数，可以让 传入参数类型参数 和 返回值类型保持一致。
>
> 泛型 标志一般用字母大写，T 可以随意更换
>
> ```
> 格式 ：  函数名<T> (参数1：T) : 返回值类型 T
> ```

```typescript
function genericity<T> (data: T) : T {
    console.log(data)
    return data
}

genericity("测试")
genericity(666)
genericity(['前端','后端','云端'])


```



##### 5.3 泛型类

> 1. 什么是泛型类
>
> 它规定了类中属性和方法的 类型，而且必须和类型定义的类型保持一致。
>
> 2. 泛型类的作用
>
> 可以帮助我们确认类的所有属性都在使用相同的类型
>
> 3. 使用格式
>
> ```javascript
> class 类名<T> {
>     name!: T;
>     hobby!: T;
> }
> 
> # 这样这个类的所有类型为 number
> let 实例 =  new 类名<number>();
> 
> ```

```typescript
class GenericityA<X>{
    sex!: X;
    age!: X;
}


let gen = new GenericityA<number>();

// gen.sex = '测试'   报错
gen.age = 3
console.log(gen.age)
```



##### 5.4 泛型约束

> 1. 接口约束
>
> - 通过定义接口， 泛型函数继承接口，则参数必须实现接口中的属性，这样就达到了泛型函数的约束
>
> 2. 类约束
>
> - 通过给类的泛型指定为另一个类，这样就规定了类泛型的类型都为另一个类

```javascript
# 第一种
// 定义接口
 interface DataInfo{
     title: string,
     price: number
 }


//  泛型函数 继承接口，进行对参数类型约束, 如果传入的参数中，没有包含接口属性，则编译不通过
 function getDataInfos< T extends DataInfo> (obj: T) : T {
     return obj
 }

 let book = {
     title: '前端进阶',
     price: 50,  
     author: '小新'
 }

 console.log(getDataInfos(book)) //{ title: '前端进阶', price: 50, author: '小新' }
```



```javascript
# 第二种
//  通过类来约束
 class Login{
    username: string;
    password: string;
    constructor(username: string,password:string){
        this.username = username
        this.password = password
    }
 }

class Mysql<T>{
    login<T>(info:T):T{
        return info
    }
}

let  x = new Login('admin','12345');
let  mysql = new Mysql<Login>();
console.log(mysql.login(x)) //Login { username: 'admin', password: '12345' }
```











<hr/>

## 六，类 Class

> 说到类，做后端的朋友应该都了解，前端 在ES6 中，才出现了 类 Class 这个关键词。
>
> Class 有哪些特征
>
> - 属性
> - 构造器
> - 方法
> - 继承  `extends`
> - 属性 / 方法 修饰符
> - 静态属性
> - 抽象类
> - 存取器 `getters/setters`



#### 6.1 修饰符

#####  `public` 共有的

> 当属性 / 方法 修饰符为 public 时， 如果前面没有，默认会加上，我们可以自由的访问程序里定义的成员。

```javascript
class Fruit {
    public name: string;
    price: number;
    // 以上为等价
    constructor(name: string, price: number) {
        this.name = name;
        this.price = price
    }
    getFruitInfo(){
        console.log(`您要购买的水果为${name},价格为${this.price}`)
    }
}
```

##### `private` 私有的

> 当成员被标记成 `private`时，它就不能在声明它的类的外部访问。

```javascript
class Fruit {
    public name: string;
    private price: number;

    // 以上为等价
    constructor(name: string, price: number) {
        this.name = name;
        this.price = price
    }
    getFruitInfo(){
        console.log(`您要购买的水果为${name},价格为${this.price}`)
    }
}

const apple = new Fruit('苹果',22)
// console.log(apple.price)   报错， 实例不可以访问私有属性

```

##### `protected `受保护的

> `protected`修饰符与 `private`修饰符的行为很相似，但有一点不同， `protected`成员在派生类中仍然可以访问,不可以通过实例来访问受保护的属性。

```javascript
class A {
    protected name : string;
    protected age : number;
    constructor(name: string , age: number) {
        this.name = name;
        this.age = age
    }
    getA(){
        console.log('A')
    }
}

class B extends A {
    protected job : string;
    constructor(name: string, job: string,age: number) {
        super(name,age)
        this.job = job
    }
    getB(){
        console.log(`B 姓名为${this.name} && 年龄为${this.age} && 职业为${this.job},`)
    }
}

let b = new B('小飞','前端工程师',22)
b.getA()  //A
b.getB()   //B 姓名为小飞 && 年龄为22 && 职业为前端工程师,
// console.log(b.name)  报错，访问不了，protected成员只能在派生类中可以访问，不能通过实例来访问。
```

#### 6.2 静态属性

> 类的静态成员(属性 和 方法) 只能通过 类来可以访问。
>
> 定义： static  属性   /   static 方法

````javascript
class Food {
    public name: string;
    private price: number;
    static adress: string = '四川';
    // 以上为等价
    constructor(name: string, price: number) {
        this.name = name;
        this.price = price
    }
    getFruitInfo(){
        console.log(`您要购买的东西为${name},价格为${this.price}`)
    }
}

const spicy = new Food('辣条',3)

console.log(Food.adress)  //四川
// console.log(spicy.adress)  报错  类的实例对象不可以访问 类的静态属性。 只可以通过类.属性来访问


````

#### 6.3 继承 `extend`

> 继承的本意很好理解，当子类继承了父类，那么子类就拥有了父类的特征(属性) 和 行为(方法)，

`````javascript
class T {
    name:string;
    constructor(name:string){
        this.name = name
    }
    getNames(){
        console.log('继承类T')
    }
}

class S extends T {
    constructor(name:string){
        // 派生类拥有T属性和方法
        super(name)
    }
    getName(){
        console.log(this.name)
    }
}

let  ss = new S('测试继承')
ss.getName()  
ss.getNames()
// 测试继承
// 继承类T
`````



#### 6.4 抽象类

> - 抽象类可以包含成员的实现细节。 `abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。
>
> - 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 

```javascript
abstract class E{
    abstract name: string;
    abstract speak():void;
    abstract play():void;
}


class F implements E {
    name: string;
    constructor(name:string){
        this.name = name
    }
    //  派生类 F 必须实现 抽象类E 的方法和属性 
    speak(): void {
        console.log('具有聊天功能')
    }
    play(): void {
        console.log('具有娱乐功能')
    }
    get(){
        console.log(this.name)
    }
}


let f = new F('测试');
f.play() //具有娱乐功能
f.get() // 测试
f.speak()  //具有聊天功能
```

## 七，TS 中的函数

> 函数类型包括 `参数类型  和  返回值类型`

##### 7.1 函数添加返回值类型

> 每个参数添加类型之后再为函数本身添加返回值类型.
>
> TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。
>
> 下面会介绍在TS 中，两种写函数的格式

````javascript

//  第一种写法
let getInterFaceInfo : (obj:object) => void = function(obj){
    console.log(obj)
}

let infos: object = {
    code: 200,
    message: '发送成功'
}
getInterFaceInfo(infos)


//  第二种写法
function getCode(code: number, message:string) : void {
    console.log(`code为${code}，message为${message}`)
}

getCode(200,'接受成功')	
````



##### 7.2 函数可选参数  /  默认参数

> JavaScript里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是undefined。
>
> 在TypeScript里我们可以在参数名旁使用 `?`实现可选参数的功能。 
>
> - **可选参数必须放在必须参数后面。**	
>
>   `````
>   格式 ： 函数名（变量名？：类型）:类型 {}  
>   `````
>
> - 默认参数，在传递参数时，指定默认值
>
>   ```javas
>   格式 ： 函数名（变量名 ：类型 = "xx"）:类型 {}  
>   ```
>
>   

````javascript
//  可选参数
function getNetWork(ip:string,domain:string,agreement?:string){
    console.log(`ip地址为:${ip},域名为${domain},协议为${agreement}`)
}

getNetWork('127.0.0.1','www.xiaomi.com')  //ip地址为:127.0.0.1,域名为www.xiaomi.com,协议为undefined

// 默认参数
function getNetWorks(ip:string,domain:string,agreement:string = 'http'){
    console.log(`ip地址为:${ip},域名为${domain},协议为${agreement}`)
}
getNetWorks('127.0.0.1','www.xiaomi.com') //ip地址为:127.0.0.1,域名为www.xiaomi.com,协议为http    
````



##### 7.3 函数剩余参数

>  有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。
>
> 在JavaScript里，你可以使用 `arguments`来访问所有传入的参数。
>
> 在TypeScript 中，可以把所有参数集中在一个变量中，前面加上`...` 表示 剩余参数。
>
> 注意
>
> - 直接通过变量访问
>
> - 也可以通过索引访问
>
> - 只能定义一个剩余参数，且位置在 默认参数和可选参数后面
>
>   

```javascript
function getNumberInfo(num:number,...peopleArray: string []) {
    console.log(`人员个数为${num},成员为${peopleArray}`)  // 也可以通过索引来获取元素
    console.log(`人员个数为${num},成员为${peopleArray[1]}`) 
}

getNumberInfo(4,'小明','小李','小红','小张')  
//人员个数为4,成员为小明,小李,小红,小张
//人员个数为4,成员为小李
```

<hr/>

## 八，枚举

> 枚举可以清晰地表达一组对应关系。
>
> TypeScript支持数字的和基于字符串的枚举。

##### 8.1 数字枚举

> 默认枚举的顺序以 0 开头，然后自动递增。
>
> 枚举顺序也可以指定 值， 指定后，它前面第一个还是以0 递增
>
> 访问
>
> - 通过 ` 枚举名.属性`  访问到的是 `序号`
> - 通过 `枚举名[序号]`  访问到的是 `属性名`

```javascript
enum Sex {
    x,
    man = 4,
    woman 
}

console.log(Sex.x)   //0
console.log(`小红的性别为${Sex[5]}`) //小红的性别为woman
console.log(`后端接受小红的性别ID ${Sex.woman}`) //后端接受小红的性别ID 5
```

##### 8.2 字符串枚举

````javascript
enum Job {
    frontEnd = '前端',
    backEnd = '后端'
}

console.log(Job) //{ frontEnd: '前端', backEnd: '后端' }	
````

## 九，高级类型

##### 9.1 交叉类型

>  它指 可以将多个类型合并为一个类型。标识符为 ` &` , 当指定一个变量类型为 交叉类型时，那么它拥有交叉类型的所有属性，也就是并集。

```javascript
interface DonInterface {
    run():void;
}
interface CatInterface {
    jump():void;
}
//这里的pet将两个类型合并,所以pet必须保护两个类型所定义的方法
let pet : DonInterface & CatInterface = {
    run:function(){},
    jump:function(){}
}

```



##### 9.2 联合类型	

> - 联合类型表示一个值可以是几种类型之一。
>
> -  用竖线（ `|`）分隔每个类型。	
> - 一个值是联合类型，我们只能访问此联合类型的所有类型里共有的成员。		

``````javascript

 function getMenus(info: string | number) {
     console.log(info)
 }


getMenus("测试")
getMenus(2)
// getMenus(false)  报错
``````

## 十，模块

> 模块： 定义的变量，函数，类等等，只能在自身的作用域里使用。 如果想在外部访问使用，那么必须使用`export` 将其导出即可。
>
> 使用模块： 通过 `import`  将模块内容导入即可使用。
>
> - 模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。
> - 模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。

#### 10.导出

##### 10.1 导出声明

> 任何声明（比如变量，函数，类，类型别名或接口）都能够通过添加`export`关键字来导出。
>
> `导出可以对任何声明 进行重命名，防止命名冲突， 通过  as 来修改`

`````javascript
# 模块A 文件

// 导出接口
 export interface A {
     getList() : void
 }

//  导出变量
export const  GET_METHOD =  "get"
 

//  导出类
export class S implements A {
    getList(): void {
        console.log("导出类")
    }
    
}

function getQueryData():void {
    console.log("获取分页数据")
}


// 导出模块 变量重命名
export { getQueryData as getQuery}



# 文件B
import {getQuery,S,A} from './模块A';

// 使用模块中的函数
getQuery()

// 实例模块中类的对象
const a = new S();
a.getList()  // 输出导出类

// 实现模块中的 A 接口
class Y implements A {
    getList(): void {
        throw new Error('Method not implemented.');
    }
    
}

`````

##### 10.2 组合模块使用

> 通常一个大的模块是多个子模块组成的。那么我们可以通过 在大的模块中导入多个子模块。
>
> 格式： `  export  *   from "模块" ` 
>
> 使用组合模块： `  import  *  as  重命名变量   from   ‘组合模块路径’   `

```javascript
# 模块C
    //  导出变量
    export const  GET_METHOD =  "get"
```

````javascript
# 模块B

export const str: string = "B模块"

export  function getContent():void{
    console.log("我是模块B的内容")
}
````

````javascript
#组合模块


 const  res : object =  {
    code: 200,
    message: "请求成功"
 }


export function getRes(): void {
     console.log(res)
 }

 
 # 导出子模块
 export * from "./modulesC"
 export * from "./moduleB"
````

##### 10.3 使用组合模块

````javascript
import * as T from "./modulesA";


// C 模块中的
console.log(T.GET_METHOD)


// B 模块中的内容
console.log(T.str)  //B模块
T.getContent() //我是模块B的内容


// A 模块中的内容
T.getRes() //{ code: 200, message: '请求成功' } 
````

##### 10.4 默认导出

> 每个模块都可以有一个`default`导出。 默认导出使用 `default`关键字标记；并且一个模块只能够有一个`default`导出。

````javascript
#模块

export interface K {
    name:string;
    birth:string;
}


export default class Student implements K {
    name: string;
    birth: string;
    constructor(name:string,birth:string){
        this.name = name;
        this.birth = birth;
    } 
    getStudentInfo(){
        console.log(this.name+this.birth)
    }
}
````



````javascript
#文件A
import D,{K} from './modulesD'


//  使用默认导出
 const d = new D('小明','1998')
 d.getStudentInfo()


// 参数类型为接口K 
 function getMessage(obj: K): void {
    console.log(obj)
 }
 let obj = {
     name:"小红",
     birth: "1998"
 }
 getMessage(obj);
````

​		



#### 	10.5 `export =  和 import =  require()`

> CommonJS和AMD的环境里都有一个`exports`变量，这个变量包含了一个模块的所有导出内容。
>
> CommonJS和AMD的`exports`都可以被赋值为一个`对象` 
>
> `exports   和  export default ` 用途一样，但是 `export default `   语法不能兼容CommonJS和AMD的`exports`。
>
> 在TypeScript 中，为了达到这样效果，可以这样写:
>
>  导出： `export = `   等于 `exports` 
>
> 导入： `import module = require("module")`	

````javascript
# 模块
// 相当于默认导出
export = class Mongodbs{
    host:string;
    user:string;
    password:string;
    port:number;
    databaseName:string;
    constructor(host:string,user:string,password:string,port:number,databaseName:string) {
        this.host = host;
        this.user = user;
        this.password = password;
        this.port = port;
        this.databaseName = databaseName
    }
    query(table:string){
        console.log(`select * from ${table}`)
    }
}

````

````javascript
#使用模块

import MogoDb = require("./modulesE")  

const mogodb = new MogoDb('1270.0.1','admin','123456',3006,'TypeScript')

mogodb.query('Vue') //select * from Vue

````

## 十一， 命名空间

> 1. 定义
>
> -  “内部模块”称为“命名空间”
>
> -  “外部模块”称为“模块”
>
> 2. 作用
>
> - 减少命名冲突，将代码组织到一个空间内，便于访问。	
>
> 3. 使用格式
>
> - 通过 `  namespace  空间名   { } `  ，内部通过  `  export ` 导出来使用内部成员

````javascript
namespace XiaoXin {
    export interface  GetData{
        name: string;
        price: number;
        getInfo(obj:object):any;
    }
    export interface  GetMessage {
        code: number;
        message: string;
    }

    export class Book implements  GetData{
        name: string;
        price: number;
        constructor(name:string,price:number){
            this.name = name;
            this.price = price
        }
        getInfo(obj: object) {
            throw new Error("Method not implemented.");
        }
        buyBook(obj: GetMessage) {
            console.log(obj)
        }
    }           
}


const fontEnd = new  XiaoXin.Book("前端开发手册",99)

var obj = {
    code: 200,
    message:"购买成功"
}

fontEnd.buyBook(obj)  //{ code: 200, message: '购买成功' }


function test(obj:XiaoXin.GetMessage){
    console.log(obj)
}

test(obj)  //{ code: 200, message: '购买成功' }	
````



#### 11.1 拆分命名空间

> 当应用变得越来越大时，我们需要将代码分离到不同的文件中以便于维护。
>
> 我们可以将命名空间文件拆分成多个文件，但是它们的命名空间名还是使用的同一个，各个文件相互依赖使用。但是必须文件最开头引入 命名空间文件。
>
> 格式:  `   ///   <reference path="MS1.ts"/>    `

````javascript
# 根命名空间
namespace School {
    export const schoolName =  "清华大学"
}
````



````javascript
# 子命名空间1
/// <reference path="MS1.ts" />

namespace School{
    export class Teacher {
        faculty:string;
        name:string;
        age:number;
        constructor(faculty:string,name:string,age:number){
            this.faculty = faculty;
            this.name = name;
            this.age = age
        }
        getInfo(){
            console.log(`${this.name}为${this.faculty},年龄为${this.age}`)
        }
        getSchool(schoole:string){
            console.log(`${this.name}老师就职于${schoole}`)
        }
    }
}
````



``````javascript
#  子命名空间2
///  <reference path="MS1.ts" />

 namespace School{
     export class Student{
         name:string;
         age:number;
         hobby:string;
         constructor(name:string,age:number,hobby:string) {
             this.name = name;
             this.age = age;
             this.hobby = hobby;
         }
         getInfo(){
             console.log(`${this.name}是一个学生，年龄为${this.age},爱好是${this.hobby}`)
         }
     }
 }

``````





````javascript
# 使用合并的命名空间


导入命名空间
/// <reference path="MS1.ts" />
/// <reference path="MS2.ts" />
/// <reference path="MS4.ts" />



let teacher = new School.Teacher('计算机教授','张博士',34);
teacher.getInfo() //张博士为计算机教授,年龄为34
teacher.getSchool(School.schoolName)  //张博士老师就职于清华大学


let students = new School.Student('张三',17,'玩LOL');
students.getInfo()  //张三是一个学生，年龄为17,爱好是玩LOL
````



​			

##### 编译命名空间文件

`````javascript
第一种方法： 会编译为 一个js文件
tsc --outFile sample.js Test.ts


第二种方法：  会编译为多个js文件，然后通过 <script> src 引入js文件即可
tsc --outFile sample.js Validation.ts LettersOnlyValidator.ts ZipCodeValidator.ts Test.ts

`````

## 十二，装饰器

> 装饰器是一种特殊类型的声明，它能够附加到类声明、方法、访问符、属性、类方法的参数上，以达到扩展类的行为。
>
> 自从 ES2015 引入 `class`，当我们需要在多个不同的类之间共享或者扩展一些方法或行为的时候，代码会变得错综复杂，极其不优雅，这也是装饰器被提出的一个很重要的原因。

##### 12.1 修饰器分类

> - 类装饰器
> - 属性装饰器
> - 方法装饰器
> - 参数装饰器
>
> ````javascript
> 修饰器写法： 1. 普通修饰器  （不传参数）
> 		   2.  装饰器工厂 (传参数)
> ````

##### 12.2 类装饰器

> 类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。
>
> 使用场景：应用于类构造函数，可以用来监视，修改或替换类定义。

````javascript
const extension = (constructor: Function):any => {
    constructor.prototype.coreHour = '10:00-15:00'
  
    constructor.prototype.meeting = () => {
      console.log('重载：Daily meeting!');
    }

  }

@extension
class Employee {
  public name!: string
  public department!: string


  constructor(name: string, department: string) {
    this.name = name
    this.department = department
  }

  meeting() {
    console.log('Every Monday!')
  }

}

let e: any = new Employee('装饰器', '测试')
console.log(e)  //Employee { name: '装饰器', department: '测试' }
 console.log(e.coreHour) // 10:00-15:00
 e.meeting()             // 重载：Daily meeting!
  
````





##### 12.3 类属性装饰器

> 作用于类属性的装饰器表达式会在运行时当作函数被调用，传入下列3个参数 `target`、`name`、`descriptor`：
>
> - `target`: 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象
> - `name`: 成员的名字
> - `descriptor`: 成员的属性描述符
>
> #### 执行顺序： 当调用有装饰器的函数时，会先执行装饰器，后再执行函数。

> 通过修饰器完成一个属性只读功能，其实就是修改数据描述符中的 `writable` 的值 ：

```javascript
function readonly(value: boolean){
    return function(target:any,name:string,descriptor:PropertyDescriptor) {
        descriptor.writable = value
    }
}


class Student{
    name:string;
    school:string = '社会大学'
    constructor(name:string) {
        this.name = name
    }
    @readonly(false)
    getDataInfo(){
        console.log(`${this.name}毕业于${this.school}`)
    }
}

let sss = new Student('小李子')
// 报错,  只能读,不能修改
// sss.getDataInfo = () => {
//     console.log("测试修改")
// }

sss.getDataInfo()
```





## 十三，TS + Vue 环境搭建

#### 13.1 升级Vue-cli

```js
1. 先 卸载旧的版本
npm uninstall -g @vue/cli 
#or
yarn global remove @vue/cli

2.安装最新版本
npm install -g @vue/cli
# OR
yarn global add @vue/cli

```

#### 13.2 创建项目

```js
vue create  projectname   项目名必须小写
```

#### 13.3 关闭变量未声明报错

```js
打开 package.json ，修改rules  内容，改为如下：
    "rules": {
      "no-unused-vars": 0
    }

这样就关闭了
```





## 一起成长 



[![re5ZQ0.png](https://s3.ax1x.com/2020/12/13/re5ZQ0.png)](https://imgchr.com/i/re5ZQ0)