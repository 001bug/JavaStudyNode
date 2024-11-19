## let和const声明变量
### let声明变量
跟var声明变量一样 , 申请内存空间的操作
**var声明的变量和let声明的变量的不同之处**
1.作用域上:
* var声明的变量具有函数作用域 , 变量在整个函数域是都是可见的 . var声明的变量在函数外部 , 他将具有全局作用域

* let:  声明的变量具有块级作用域. 变量仅在包含它的块(如`{}`内)可见 , 不会泄漏到块的外部

2.变量提升
* `var` 声明的变量会被提升到函数或全局作用域的顶部，但变量的赋值不会被提升。这意味着变量在其声明之前是可以访问的，但值为 `undefined`。
```javascript
//使用+,表示字符串的拼接 , 如果x是一个对象,那么只会输出object
console.log("x="+x); //undefined
var x = 5;
//使用`,`可以输出对象的完整信息
console.log("x=",x); //5
```
* 不能在 `let` 声明之前使用该变量。
```javascript
console.log(x);// ReferenceError: x is not defined
let x = 5;
console.log(x);// 5
```
3.重新声明
* `var`在同一作用域内，你可以多次使用 `var` 声明相同名称的变量。这不会导致错误，后来的声明会覆盖先前的声明。
* 在同一作用域内，`let` 不允许重复声明同一个变量。不然会报语法错误
### const声明常量(只读常量)
**概述**
`const`是一种用于声明常量的关键字 , 是一种不可变的**变量**. 其值在声明后不能被重新赋值. 它跟java常量final有非常大的区别 , 那就是 , javascript的常量的内容是可以变的(前提是对象和数组) . 只是变量的引用不能变

**特点**
1.常量声明
* 用 `const` 声明的变量一旦赋值后，其变量中的内容不可被重新赋值。
```javascript
const PI = 3.14;
//PI = 3.12 抛出TypeError错误
```
2.块级作用域
* `const` 声明的变量具有块级作用域，这意味着它们只在其声明的代码块内有效
```javascript
{
	const x = 10;
	console.log(x);//10
}
//console.log(x)//ReferenceError: x is not defined
```
3.不可变性
* `const`本身是不可变的 , 即变量的引用是不能变的 , 但是引用的内容是可以变的(要对象 , 或者数组)
```javascript
const obj = { name: 'Alice' };
obj.name = 'Bob'; // 这是允许的
// obj = { name: 'Charlie' }; // TypeError: Assignment to constant variable.

const arr = [1, 2, 3];
arr.push(4); // 这是允许的
// arr = [1, 2, 3, 4]; // TypeError: Assignment to constant variable.
```
4.必须初始化
```javascript
const a; // SyntaxError: Missing initializer in const declaration
```
## 解构赋值
**概述**: 是一种从数组或者对象中**提取数据并将其赋值**给变量的简洁方式 . 通过解构赋值 , 开发者可以从复杂的数据结构中直接提取值 , 而不是手动的访问每个属性或元素. 解构赋值可以用于数组 , 对像 , 甚至嵌套的结构
传统的方法
```javascript
let person = {
	name:'李华',
	age:12
};
console.log(person.name," ",person.age);
```
可以发现 , 要像java一样 , 用引用的方式 , 访问对象的内部
1.数组解构赋值
* 从数组中按顺序提取值并赋值给变量
```javascript
const arr = [1,2,3];
const [a,b,c] = arr;
console.log(a); //1
console.log(b); //2
console.log(c); //3
```
2.对象解构赋值
* 从对象按属性名(顺序无所谓)提取并赋值给变量
```javascript
//它会去object中找property1和property2的属性值,然后赋值给它
const{property1 , property2} = object;
//1.基本结构赋值
let person ={
	name;"john",
	age:25,
	city:"大大岛"
};
let {name,age}=person;
```
* 给对象里面的变量重命名
```javascript
let person = {
	name:"jhon",
	age:14
};
let{name:fullname,age:years}=person;
```
* 设置默认值
```javascript
const person = {
  name: "John"
};

const { name, age = 30 } = person;

console.log(name); // 输出: John
console.log(age);  // 输出: 30 (age 默认值)
```
* 嵌套解构 , 嵌套指的是对象中包含对象
```javascript
const person = {
  name: "John",
  address: {
    city: "New York",
    zip: "10001"
  }
};

const { address: { city, zip } } = person;

console.log(city); // 输出: New York
console.log(zip);  // 输出: 10001
```
注意 , 这里的`city`和`zip`变成了单独的变量
## 模版字符串
**概述**
在 ES6 中，模板字符串是一种新的字符串字面量表示法，**它用反引号 (`` ` ``) 包裹**，允许在字符串中嵌入变量和表达式。模板字符串为字符串操作提供了更强的灵活性，特别是在处理多行字符串和字符串插值时。
1.字符串插值
* 使用[`${}`]([MyBatis](../../JavaNote/JavaStudyNode/MyBatis/MyBatis.md))插入变量或表达式到字符串中.
```javascript
const name = "Alice";
const age = 25;
const message = `My name is ${name} and I am ${age} years old.`;
console.log(message); // 输出: My name is Alice and I am 25 years old.
```
2.多行字符串: 模版字符串可以直接编写多行字符串 , 而无需使用换行符`\n`.
```javascript
const multiLine = `This is a
multi-line string.`;
console.log(multiLine);
```
3.嵌入表达式:不仅可以插入变量 , 可以在`${}`中插入任何javascript表达式
```javascript
let a=10;
let b=20;
let result = `The sum is =
 ${a+b}.`;
 console.log(result);
```
## 对象相关新特性
### 对象属性简写
如果对象的属性名和变量名相同 , 可以使用属性简写语法. 下面是传统写法和新特性写法的区别
* 传统写法
```javascript
let name='john';
let age='30';
let person={
	name:name,
	age:age
};
```
* ES6写法
```javascript
let name='john';
let age=30;
let person={
	name,
	age
};
```
### 对象方法简写
定义**对象的**方法时, 可以省略`function`关键字.
**传统写法**
```javascript
let person = {
    name: 'John',
    doing: function() {
        console.log('Hello!');
    }
};
```
**ES6简写**
```javascript
let person={
	name:'john',
	doing(){
		console.log('Hello!');
	}
}
```
### 扩展运算符
**扩展运算符(. . .)**
扩展运算符可以用于对象的拷贝和合并

**对象拷贝**
```javascript
const person={name:'john',age:30};
//深拷贝
const copiedPerson={...person};
console.log(copiedPerson);
```
浅拷贝创建一个新对象，这个**新对象对原始对象**中的**引用类型的属性**保持**引用关系**，而不是复制它们的内容。这意味着浅拷贝只复制对象的**第一层属性**，而嵌套的引用类型属性（如数组、对象）依然与原对象共享同一个引用。
深拷贝创建一个新对象，并且递归地复制所有层次的属性，确保新对象与原对象完全独立。深拷贝后的对象与原对象之间没有任何共享引用，所有引用类型的属性都是**独立的副本。**

**对象合并**
对象合并是在JavaScript中将多个对象的属性合并到一个新对象或现有对象中。这个过程通常会将多个对象的键值对合并到一起，如果有重复的键，后面的值会覆盖前面的值。
```javascript
const person={name:'john',age:39};
const job={title:'Developer'};

//合并对象
const mergedObject = {...person, ...job};
console.log(mergedObject);
```
## 箭头函数
**概述**
箭头函数是ES6中引入的一种更加简洁的函数定义方式 , 它不仅语法更短 , 还绑定了`this`, 这与普通的函数不同 .箭头函数常用于简化回调函数的编写和避免`this`绑定问题.
传统函数写法
```javascript
const add = function(a, b) {
	return a + b; 
};
```
基本语法
```javascript
const add =(a,b)=>{
	return a + b;
}
// 如果函数体只有一个表达式(一行语句)，甚至可以省略 `{}` 和 `return` 
const add = (a, b) => a + b;
```
单参数情况下的语法(箭头函数**只有一个参数** , 可以省略() )
```javascript
const square= x =>x*x;
```
无参数时 , 需要加上空括号
```javascript
const greet = () =>console.log('hello');
```
箭头函数多数用于匿函数

**对象解构**
概念:对象解构是ES6中的一种语法 , 可以**从对象中提取属性值**, 并将其赋值给变量. 它允许我们从对象的多个属性中快速提取值, 大大简化了开发
```javascript
const person={
	name: 'john',
	age: 30,
	city:'New York'
}
//对象解构赋值
const{name, age, city}=person;
```
## Promise
**概述**:`Promise`是一种用于处理异步操作的对象 , 它表示一个在某个时间可能会完成(resolve)或者失败(reject)的操作 , 并且允许开发者定义在操作完成或失败后执行的逻辑
**使用这种技术的原因**
1.传统的Ajax异步调用在需要多个操作的时候 , 会导致多个回调函数嵌套 , 导致代码不够直观 , 就是常说的Callback Hell
2.为了解决上述的问题，Promise 对象应运而生，在EMCAScript 2015 当中已经成为标准
3.promise 是异步编程的一种解决方案
4.promise是一个对象
比如如下情况
![](assest/Pasted%20image%2020240910181525.png)
使用jquery原生的ajax会发现明显的嵌套
```javascript
<script type="text/javascript">  
    $.ajax({  
        url:"monster.json",  
        success (resultData) {  
            console.log("第一次ajax请求,拿到monster基本信息=",resultData);  
            if(resultData.id==1){  
                $.ajax({  
                    url:"monster_detail_1.json",  
                    success(resultData) {  
                        console.log("第二次ajax请求=",resultData);  
                    }  
                })  
            }  
        },  
        error(err){  
            console.log("出现异常=",err);  
        }  
    })  
</script>
```

**基本用法**
一个`Promise`**对象**通过传入一个**回调函数创建** , 这个回调函数接受两个参数:`resolve`和`reject`
* `resolve(value)`: 当异步操作成功时调用 , 表示操作成功
* `reject(error)`:当异步操作失败时调用 , 表示操作失败.
```javascript
let myPromise = new Promise(function(resolve, reject) {
    let success = true;  // 模拟一个操作结果

    if (success) {
        resolve("操作成功");
    } else {
        reject("操作失败");
    }
});

myPromise.then(function(result) {
    console.log(result);  // 操作成功时输出: "操作成功"
}).catch(function(error) {
    console.log(error);   // 操作失败时输出: "操作失败"
});
```
处理`Promise`
`Promise`对象提供了`.then()`和`.catch()`方法来处理操作完成或失败的结果:
* `.then()`:接受两个回调函数 , 第一个是操作成功时的回调 , 第二个是操作失败时的回调(可选)
* `.cath()`:专门处理操作失败的回调

**使用Promise解决传统ajax嵌套问题**
```javascript
<script type="text/javascript">  
  
    /**  
     * 这里我们将重复的代码，抽出来,编写一个方法get  
     *  
     * @param url ajax请求的资源  
     * @param data ajax请求携带的数据  
     * @returns {Promise<unknown>}  
     */  
    function get(url, data) {  
        return new Promise((resolve, reject) => {  
            $.ajax({  
                    url: url,  
                    data: data,  
                    success(resultData) {  
                        resolve(resultData);  
                    },  
                    error(err) {  
                        reject(err);  
                    }  
                }  
            )  
        })  
    }  
  
    //需求: 完成  
    //1. 先获取monster.json  
    //2. 获取monster_detail_1.json  
    //2. 获取monster_gf_2.json  
    get("data/monster.json").then((resultData) => {  
            //第1次ajax请求成功后的处理代码  
            console.log("第1次ajax请求返回数据=", resultData);  
            return get(`data/monster_detail_${resultData.id}.json`);  
  
        }).then((resultData) => {  
            //第2次ajax请求成功后的处理代码  
            console.log("第2次ajax请求返回数据=", resultData);  
            //return get(`data/monster_detail_${resultData.id}.json`);  
            return get(`data/monster_gf_${resultData.gfid}.json`);  
         }).then((resultData) => {  
            //第3次ajax请求成功后的处理代码  
            console.log("第3次ajax请求返回数据=", resultData);  
            //继续..  
        }).catch((err) => {  
            console.log("promise请求异常=", err);  
        })  
  
</script>
```
return new Promise这个不能缺少, 这是调用链 , 交给下一个then
`return new Promise` 是在 JavaScript 中创建一个新的 `Promise` 对象，用于处理异步操作。通过 `new Promise`，你可以定义异步任务的成功或失败，之后这个 `Promise` 会被 `resolve` 或 `reject`，来决定接下来的操作。
- **`resolve`**：表示异步操作成功，将结果传递给下一个 `.then()` 方法。
- **`reject`**：表示异步操作失败，将错误传递给 `.catch()` 方法。

**注意事项**
1.如果返回的是Promise对象,可以继续执行.then()
2.`.then((data)=>(){})`的data数据是上一次正确执行后`resolve(data)`返回传入的
3.通过多级`.then()`可以对异步请求分层次请求, 实现代码重排, 代码逻辑更加清晰合理
4.通过多级`.then()`后面的`.catch((err))=>{})`可捕获发生异常, 便于调试
## 模块化编程
#### ES6模块化概述
ES6模块是标准的模块系统. ES6模块具有静态结构, 允许在编译时进行**模块依赖**的解析和优化. 这里我们主要学CommonJS模块化规范 , 每个js文件就是一个模块 , 有自己的作用域. 在文件中定义的变量,函数,类/对象, 都是私有的 , 对其他js文件不可见
**ES6模块的主要特点**
1. 静态导入和导出
2. **严格的作用域隔离**,不用模块化编程,同一文件夹下的js文件中的方法是共用的
3. 按需加载
4. 浏览器原生支持
#### 模块化语法
**1.ES5模块化编程分析(CommonJS)**
commonJS使用`module.exports={} exports={}`导出模块, 使用`let/const名称=require("xx.js")`导入模块
![](assest/Pasted%20image%2020240912151301.png)
math.js(导入)
```javascript
const tmp=require("./math2");  
 const PI=3.14;  
 function add(a,b){  
    return a+b;  
}  
console.log(tmp.number);  
 console.log(tmp.sub(5,1));
```
math2.js(导出)
```javascript
const number=10;  
function sub(a,b){  
    return a-b;  
}  
module.exports={  
    number:number,  
    sub:sub()  
};
```
重点关键字是`require`和`exports`
要看运行效果 , 需要Node环境 , node环境我们后面搭建, 只要use.js , 可以解析sum和sub说明是正确的

**2.ES6模块语法**
导出模块`export`关键字. 导入模块`import`关键字
使用`export{名称/对象/函数/变量/常量}` , `export定义=` , `export default{}`导出模块
使用`import{} from "xx.js"/import 名称 form "xx.js"`导入模块
math2.js(导出)
```javascript
export const number=10;
export function sub(a,b){
	return a-b;
};
//第二种导出方式,也是比较常用的导入方式
export default{
	const number=10;
	function sub(a,b){
		return a-b;
	}
}
//批量导出
const number=10;
function sub(a,b){
	return a-b;
}
```
math.js(导入)
```javascript
import{number,sub}from './math2.js';  
console.log(number);  
console.log(sub(5,1));
//第二种导入方式
import tmp from './math2.js';
console.log(tmp.number);
console.log(tmp.sub(5,1));
```
![](assest/Pasted%20image%2020240912155702.png)
总结:export不仅可以导出对象 , 一切JS变量都可以导出. 比如: 基本类型变量 , 函数 , 数组 , 对象
没有导出的不能使用. ES6导出的方式很多 , 不同的导出方式对导入方式也有一定影响,常用的是第二种导入方式 , 因为这种方式可以解决包的冲突问题