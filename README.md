# JavaScript

## 1、组件封装原则

---

- 单一职责：每个组件应该只负责一个功能或一个部分的逻辑，来提高复用性
- 高内聚低耦合：组件内部的功能应该尽可能紧密相关（高内聚），而组件之间的依赖应该尽可能松散（低耦合），以便于独立开发和维护
- 可配置性：组件可以通过属性（props）或配置选项来调整其行为和外观，而不是硬编码在组件内部
- 可扩展性：设计组件时应考虑到未来可能的扩展需求，避免在需要扩展时进行大量重构
- 易于理解和使用：组件的 API 应该简单明了，使用文档和示例代码应当清晰，方便开发者快速上手

## 2、浅拷贝和深拷贝

---

浅拷贝和深拷贝只针对于复杂数据类型，如数组和对象。对于复杂数据类型不能通过简单的=来进行属性赋值。这样拷贝的是原属性的内存地址。修改其中一个会同步修改另外一个数据，造成数据混乱。

浅拷贝是复制一个对象，属性完全相同，但内存地址是同一个地址；

深拷贝是复制一个对象，属性完全相同，且内存地址也不相同；

对于数组来说，可以使用 Array.slice();Array.concat()来对数组进行分割拼接。这样会返回一个新的数组，新的数组指向新的内存地址。

对于对象来说，可以使用 JSON.parse(JSON.stringfy(obj))进行简单的深拷贝，缺点是会丢失值为 null 或 undefined 的属性值。也可以使用第三方库来实现深拷贝，如 lodash.clonedeep。或通过递归的方式进行深拷贝；

### 2.1、深拷贝实现（递归）

---

```javascript
// 递归实现
function deepclone(obj) {
	// 检查是否是复杂数据类型
	if(obj === null || typeof obj !== 'object') {
		return obj;
	}
	// 创建新的对象
	let copy = Array.isArray(obj) ? [] : {};
	// 对数据进行递归循环
	for(let key in obj) {
		if(obj.hasOwnProperty(key) {
			copy[key] = deepclone(obj[key])
		}
	}

	return copy;
}
// 除此之外，简单的深拷贝还可以使用JSON.parse(JSON.stringify(obj))、lodash的clonedeep或者其他第三方插件库均可以实现深拷贝
```

### 2.2、Object.assign()和拓展运算符的区别

---

Object.assign(target, source)。将 source 中的属性赋值到 target 中去。是浅拷贝。新返回的对象和 target 的内存地址一样；

...拓展运算符为一层的深拷贝。如果第一层的属性是简单数据类型，则两者修改并不会同步。反之是复杂类型则会同步。拓展运算符会生成一个新的内存地址。所以新对象和源对象使用===判断得到的结果是 false；

## 3、JavaScript 有哪些数据类型

---

分为基础数据类型和引用类型：

基础类型常见的有：String，Number，Boolean，null，undefined

引入数据类型常见的有：Array，Object，Function

### 3.1、基础数据类型和引用数据类型的区别

---

基础数据类型储存在栈中，栈中存放的是其对应的值；而引用数据类型存储在堆中，栈中存放的是指向堆内存的地址；

### 3.2、为什么基础存在栈，引用存在堆

---

因为基础数据类型占用空间小，大小固定，是需要频繁调用的数据。而引用类型正好相反，如果将其存储在栈中，会引起性能问题

## 4、判断数据类型的方式

---

typeof、instanceof、object.prototype.toString.call()

```javascript
// instanceof是判断对象是否是某个构造函数的实例，他检查的是原型链。而基础数据类型没有原型链，所以对基础数据类型无效
console.log([] instanceof Array); // true
console.log({} instanceof Object); // true
console.log(function () {} instanceof Function); // true
// 但如果使用new String()这类的包装对象的话，instanceof也是可以检查基础数据类型的类型，但实际开发并不建议这么做，这样会徒增不必要的复杂性，产生潜在的错误
console.log(new String("asd") instanceof String); // true
```

typeof 对 null、array、object 的判断都是 object

### 4.1、判断数组的方式

---

Array.isArray()、instanceof、object.prototype.tostring.call()

## 5、null 和 undefined 的区别

---

null 和 undefined 都是基础数据类型，他们都只有一个值，也就是 null 和 undefined；

undefined 可以理解为缺省的意思，意思是这里应该有一个值，但是目前还没赋值，所以是 undefined。例如一个变量被声明了，但是没有赋值，此时他就是 undefined；而 null 则是一个没有任何属性和方法的空对象

## 6、为什么 0.1+0.2 != 0.3

---

这是由于浮点数的表示和运算的精度问题。将 0.1 和 0.2 转换成二进制时会变成无限循环的数，此时计算就会导致精度的缺失。然而该问题不是 js 独有的，所有使用 IEEE 754 浮点数标准的编程语言都有这种问题。像是 Java、C、python 等。解决办法也很简单，使用 tofixed 将值四舍五入到一个合理的小数位数，或是将其全部转成整数运算，运算完后再转回浮点数，也可以利用第三方库来解决浮点数精度计算问题。

## 7、箭头函数和普通函数的区别

---

- 声明方式：箭头函数的声明方式更加简洁。而普通函数需要 function 关键字来声明；普通函数既可以生命具名函数也可以声明匿名函数，而箭头函数只能声明匿名函数，但是可以通过表达式的方式让箭头函数具名
- this 指向：普通函数的 this 指向调用该函数的对象，而箭头函数没有自己的 this 指向，他的 this 指向定义时上层作用域的对象。普通函数可以通过 apply.bind.call 的方式进行改变，箭头函数的 this 指向永远不会发生改变
- 参数绑定：箭头函数没有自己的 arguments 对象，他会使用上层作用域中的 arguments，但可以使用剩余参数的形式即(...args)的方式来代替。而普通函数有自己 arguments 对象
- 构造函数：普通函数可以使用 new 的方式来调用，而箭头函数不行。因为箭头函数没有原型 prototype，而 new 的过程是创建一个空对象，绑定到原型，链接 this 执行构造函数，返回新对象。因为没有原型，所以无法当成构造函数

## 8、拓展运算符的作用及常见场景

---

拓展运算符是 ES6 中非常常用的一个特性，它由三个点“...”表示。比较常用的情况有复制、拼接对象或数组。在函数中，将入参转成参数序列。比如 function A(x,y)，可以通过 A\[...\[1,2]]的方式使用，同样也可以将参数序列转换成数组。也可以将字符串转成字符串数组。最最常用的还是在对象和数组的解构赋值中取出部分元素和属性。

## 9、解构

---

解构主要分为对数组的解构和对对象的解构。对数组的解构主要就是取出匹配位置的元素，而对对象解构则是取出匹配属性名的值。简化了之前在数组和对象中取值的繁杂步骤，简化代码，使代码变得简洁易懂。

## 10、use strict 是什么？

---

use strict 是 js 的一种严格模式，它旨在消除 js 编程中的一些不合理之处，提高编译效率。使用之后常见的变化有：不允许再使用 with 语句；无法使用未声明的变量；this 不会指向全局对象；对象或函数不允许有重名的属性和变量等

## 11、Event Loop 事件循环机制

---

因为 js 是单线程，只有上一个任务完成才会执行下一个。对于异步操作而言，就可能导致整个进程阻塞。所以 js 通过事件循环机制来解决这一问题。其主要流程是：

自上而下执行代码，同步代码直接在主线程上执行，形成执行栈。如果遇到异步任务则将其挂起添加至任务队列中，宏任务加入宏任务队列，微任务加入微任务队列。同步代码执行完毕后，判断任务队列中是否有任务，先执行微任务，再执行宏任务。执行完毕之后再去看宏任务有没有产生新的微任务，以此循环。直到任务队列清空

> 参考链接：<https://article.juejin.cn/post/7231986666511237175#heading-0>

## 12、闭包

---

- 定义：闭包是指有权访问另一个函数作用域里变量的函数。最常见的创建方式就是在一个函数里创建另一个函数，创建的函数可以访问当前函数的局部变量。
- 本质：上级作用域内变量的生命周期，因为下级作用域的使用而没有被释放。就导致上级作用域里的变量要等到下级作用域中执行完才可以释放
- 用途：
- 1、创建私有变量，完成数据的隐藏和封装，避免全局变量的污染
- 2、保持对某一个变量的引用，使其在函数执行完毕后依然可以访问
- 3、用于事件处理，确保事件处理器其能访问到定义时的环境变量
- 缺点：
- 1、因为函数执行上下文执行完未被释放，所以导致内存消耗很大
- 2、不当使用还会导致内存泄漏问题

```javascript
// innerFn就是闭包
function outerFn() {
  let count = 0;

  function innerFn() {
    console.log(count);
  }

  return innerFn;
}

const fn = outerFn();
fn();
// 创建私有变量 count变量只能通过add、remove函数访问
function counter() {
  let count = 0;

  return {
    add() {
      count++;
      console.log(count);
    },
    remove() {
      count--;
      console.log(count);
    },
  };
}
const count = counter();
count.add();
count.remove();
// 保持对某个变量的引用
function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
const newCount = createCounter();
console.log(newCount()); // 1
console.log(newCount()); // 2
console.log(newCount()); // 3
// 事件处理
function bindFnOnBtn() {
  const btn = document.getElementById("btn");
  let count = 0;

  btn.addEventListener("click", function () {
    count++;
    console.log(`第${count}次点击按钮`);
  });
}
bindFnOnBtn();
```

## 13、作用域和作用域链

---

- 作用域：
- 作用域就是程序中变量和函数可以被访问的区域。主要分为三种，全局作用域、函数作用域、块级作用域；全局作用域则是不在任何函数和大括号内的，而函数作用域则是在函数内部。块级作用域是 ES6 中引入 let const 声明后才有的，它是 let 和 const 被大括号包起来的区域
- 作用域链：
- 在嵌套函数中，内部函数可以访问外部函数的作用域以及更外层的作用域，直到全局作用域。这个层级关系就是作用域链。作用域链保证了函数和变量的有序访问

## 14、原型和原型链

---

- 原型：
- 原型本质上就是一个对象。当创建一个 constructor 构造函数时，构造函数内部会默认带上一个 prototype 属性，该属性所指向的对象，就是该构造函数的原型对象。存放着所有实例共享的属性和方法。
- 原型链：
- 当访问对象的某个属性时，会先从该对象自身上查找，如果查找不到就会去对象的原型查找，然后原型的原型，一直到链路的顶端也就是 Object.prototype，值为 null。这个链路关系就是原型链。

## 15、执行上下文

---

执行上下文时 js 代码执行的环境。当 js 代码执行时都会创建一个执行上下文来管理代码的执行。执行上下文主要分为三类，分别是全局执行上下文，函数执行上下文，eval 函数执行上下文。但 eval 函数执行上下文并不常用。

- 全局上下文在 js 代码首次执行时创建，他只有一个。在浏览器中绑定至 window 对象
- 函数执行上下文在函数被调用时创建，并在函数执行完毕后销毁

执行上下文分为三个阶段：创建、执行、销毁

## 16、Promise

---

Promise 是处理异步操作的一种机制，他表示一个暂未完成但将来可能完成的操作。它改善了异步编程的困境，避免了回调地狱。

它主要有三种状态：

- pending 请求中
- resolved 已完成
- rejected 已拒绝

当把一件事交给 promise 时，它的状态就是 pending，任务完成状态就是 resolve，任务失败则就是 rejected

Promise 的特点：

- 对象的状态不受外界的影响。只有异步操作的结果才能确定 promise 的状态
- 状态一旦发生改变就无法再更改。

Promise 的缺点：

- 无法取消 promise，promise 一旦创建就会立即执行
- 如果不使用.catch 的话，内部抛出的错误并不会反应到外部

Promise 常用的方法：

- then：接受两个参数，第一个参数接受成功的回调函数，第二个参数接收失败的回调函数。可以链式调用
- catch：相当于 then 的第二个参数，用于指定发生失败时的回调函数
- all：可以用来并行任务。只有当全部的 promise 全部完成后，promise 的状态才会变成已完成。如果有失败的，会返回最先失败的那个错误
- race：顾名思义就是赛跑的意思。他会返回最先发生状态改变的 promise，如果最新发生改变的 promise 变成 resolved 那么就是 resolve，反正就是 rejected
- finally：无论如何最终都会执行的可以放在 finally 中

## 17、防抖和节流

---

两者都是前端控制函数调用频率的技术，用来减少不必要的函数调用

### 17.1 防抖

---

防抖是 n 秒后执行该事件，如果在 n 秒内重新触发，则重新计时。适用于需要频繁触发，但是只需要最后一次才调用的需求。例如控制用户的输入

```javascript
function debounce(fn, delay) {
  let timer = null;

  return function () {
    if (timer) {
      cleartimeout(timer);
    }
    timer = settimeout(() => {
      fn.apply(this, arguments);
    }, delay);
  };
}
```

### 17.2 节流

---

节流则是 n 秒内只触发一次，如果 n 秒内多次触发，只有一次生效。适用于那种频繁调用的需求，减少调用频率。例如页面滚动

```javascript
function throttle(fn, delay) {
  let timer = null;

  return function () {
    const that = this;
    const args = arguments;
    if (!timer) {
      timer = settimeout(() => {
        fn.apply(that, args);
        timer = null;
      }, delay);
    }
  };
}
```

## 18、JavaScript 本地存储的方式

---

## 19、apply、call、bind 三者的区别

---

## 20、什么是同步、什么是异步

---

同步任务是指代码执行过程中，需要按顺序执行的代码，上一个任务没有完成，下一个任务不会开始。而异步任务则是在代码执行过程中，可以将一个任务放进任务队列中等待，等任务完成后再返回主线程中处理

主要有以下几个区别：

- 执行顺序：同步任务代码依次执行，而异步任务在后台执行，执行完毕后再会主线程处理
- 性能：同步任务会造成主线程的阻塞，造成性能瓶颈。而异步任务能处理多个任务，提高响应速度
- 使用场景：同步任务适用于那些需要依赖于上一个任务的结果的任务；而异步任务需要执行需要等待的任务，例如网络请求、文件读写等

## 21、new 操作符做了什么

---

## 22、JS 的垃圾回收机制

---

## 23、JS 的数据结构

---

# Vue

## 1、Vue 的生命周期

---

```javascript
// vue2
beforecreated // 创建前：当前阶段data、method、computed以及watch上的数据和方法都不能被访问
created // 创建后：可以使用数据、更改数据。但在这里更改数据不会触发updated函数
beforemounted // 挂载前：当前阶段虚拟dom已经创建完成，即将开始渲染
mounted // 挂载后：真实DOM已经挂载完毕，数据完成双向数据绑定
beforeUpdated // 更新前：响应式数据更新时调用，此时响应式数据更新了，但是对应的真实DOM还没有被渲染
updated // 更新后：真实DOM更新完毕。应避免在此钩子函数内修改数据，否则会导致无限循环
beforeDestory // 销毁前：实例销毁前调用，此时可以进行清除定时器等操作
destroyed // 销毁后：实例完全销毁
// vue3
1、在vue3中，beforecreated和created被合并成了setup，并且这两个钩子函数的功能也被setup所完全继承
2、beforedestory和destroyed更名为beforeunmount和unmounted，且不可再使用


// 关于this问题
1、在vue3中如果使用选项式api，则this用法和vue2基本相同。仍然可以在生命周期钩子函数、计算属性、方法等处使用this来访问组件实例的属性和方法
2、但是在组合式api中，this无法再访问组件实例，而是改用setup的方式来定义组件的属性和方法。
// example
import { ref } from 'vue';

export default {
  setup() {
    const message = ref('Hello, Vue 3!');

    const greet = () => {
      console.log(message.value);
    };

    return {
      message,
      greet
    };
  }
}
避免使用this的好处：避免了使用this带来的上下文切换问题，尤其是在异步操作中；逻辑更分离，更易于理解和维护代码；
3、但是在某些时刻，仍需要访问组件实例，此时可以使用getCurrentInstance函数
// example
import { getCurrentInstance } from 'vue';

export default {
  setup() {
    const _this = getCurrentInstance();
    console.log(_this);

    return {};
  }
}
```

## 2、SSR

---

SSR 即服务端渲染，也就是 HTML 页面直接在服务端进行生成，然后将完整的 HTML 发送给客户端浏览器

优点：

- 更快的首屏加载速度
- SEO 友好

缺点：

- 服务器的负载增加，服务器要处理更多的工作

适用场景：

- 内容密集型网站：如博客、新闻网站等
- 需要良好 SEO 的网站：如企业官网等

## 3、Vue 的基本原理

---
