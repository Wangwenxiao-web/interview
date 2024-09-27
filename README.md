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

|      方式      |                                                                          特点                                                                          |                         总结                         |
| :------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------: |
|     cookie     | 用于存储少量数据，通常限制在 4kb；会在每次请求时发送到服务器；可以设置过期时间、域名等信息；通常用于存储用户会话信息，登录状态等需要和服务器同步的数据 |   适用于需要与服务器同步的少量数据，常用于会话管理   |
|  LocalStorage  |         用于存储大量数据，通常限制在 5-10MB，适合长期存储；数据在关闭浏览器后依然存在；存储的数据以键值对形式存在，所有数据都已字符串形式存储          |       适用于存储长期数据，数据持久化，简单易用       |
| SessionStorage |                                         用于存储短期数据，和 localstorage 一样。只不过关闭会话后数据就会被清除                                         |         适用于存储短期数据，只在会话期间有效         |
|   indexedDB    |                                   用于存储大量结构化数据，支持事务、索引等特性，适合存储复杂的数据结构；他是异步操作                                   | 适用于存储和管理大量复杂数据，支持高级查询和业务操作 |

## 19、apply、call、bind 三者的区别

---

三者都是改变 this 指向的方式。

call 第一个参数接收 this 的指向地址，后续可以接收多个参数在函数内调用

apply 和 call 一样，但是接收的参数变成了参数数组；

bind 和 call 的传参方式一样，但是并不会立即调用，而是返回一个新的函数，等待下次调用；且新返回的函数无法再改变 this 指向

## 20、什么是同步、什么是异步

---

同步任务是指代码执行过程中，需要按顺序执行的代码，上一个任务没有完成，下一个任务不会开始。而异步任务则是在代码执行过程中，可以将一个任务放进任务队列中等待，等任务完成后再返回主线程中处理

主要有以下几个区别：

- 执行顺序：同步任务代码依次执行，而异步任务在后台执行，执行完毕后再会主线程处理
- 性能：同步任务会造成主线程的阻塞，造成性能瓶颈。而异步任务能处理多个任务，提高响应速度
- 使用场景：同步任务适用于那些需要依赖于上一个任务的结果的任务；而异步任务需要执行需要等待的任务，例如网络请求、文件读写等

## 21、new 操作符做了什么

---

new 操作符用于创建一个新的对象实例，主要是以下几个步骤

1.  创建一个空对象
2.  设置原型，将对象的原型设置为构造函数的 prototype 属性
3.  链接 this,将 this 指向新创建的对象。执行构造函数
4.  返回新的对象

## 22、JS 的垃圾回收机制

---

定义：

JavaScript 代码运行时，需要分配内存空间来存储变量和值。当变量不再参与运行时，就需要系统回收被占用的内存空间，这就是垃圾回收

回收机制：

- JavaScript 具有自动垃圾回收的机制，会定期对那些不再使用的变量、对象所占用的内存进行释放，原理就是找到不再使用的变量，然后释放掉其占用的内存
- JavaScript 中存在两中变量：全局变量和局部变量。全局变量只有等到页面卸载时才会被回收。而局部变量声明在函数中，当函数执行完毕后这些变量就不再被使用，他们所占用的空间就会被释放
- 但时当局部变量被外部函数使用时，例如闭包。此时局部变量仍然在被使用，所以不会被回收

回收方式：

浏览器的常用垃圾回收方式有两种：**_标记清除_**，**_引用计数_**

- **标记清除：**
- 在标记阶段，浏览器从根对象出发，递归地遍历对象的引用关系。对每个访问到的对象都打上标记，表明该对象是可达的，不是垃圾。在清除阶段，浏览器会遍历整个内存，对没有标记的对象进行回收，释放内存空间
- **引用计数:**
- 每个对象都有一个引用计数器，记录该对象被引用的次数。当引用计数器为 0 时，该对象会被认为是不可达的，就会被回收

V8 垃圾回收策略：

V8 是一种执行 JavaScript 的开源引擎，主要用于浏览器和 node.js。它采用了更加复杂的垃圾回收机制，包括分代回收、增量回收、并行和并发回收等

> 参考链接：<https://juejin.cn/post/7274146202496090170>

## 23、地址栏输入 URL 敲下回车后发生了什么

1.  解析 URL：分析所需要使用的传输协议和请求的资源路径，将非法字符转义
2.  缓存判断：浏览器检查本地缓存，看是否有未失效的缓存，有就直接使用，没有就重新请求
3.  DNS 解析：解析 URL 域名来获取其对应的 IP 地址
4.  TCP 三次握手：客户端发送 SYN 包到服务器，服务器返回 SYN-ACK 包到客户端，客户端发送 ACK 包到服务器，连接建立（浏览器向服务器发送建立连接的请求，服务器接收后向客户端发送同意建立连接的响应，浏览器接收后发送确认收到响应的请求，建立连接）
5.  SSL/TLS 握手：如果是 https 的话还会进行 tls 握手来建立一个安全连接
6.  HTTP 请求：
7.  服务器处理请求：
8.  服务器返回响应：返回一个 html 页面，包含 css 和 js。
9.  页面渲染：先对 HTML 文件构建 DOM 树，根据解析的 CSS 文件构建 CSSOM 树，然后解析并执行 js 代码；之后浏览器会根据 DOM 树和 CSSOM 树构建渲染树；然后计算每个元素的大小和位置，计算完毕后将元素绘制在屏幕上
10. TCP 四次挥手断开连接：浏览器向服务器发送想断开连接的请求；服务器向浏览器发送收到请求的响应；再向浏览器发送断开连接的请求；最后浏览器断开连接并向服务器发送一个反馈请求；服务器收到后断开连接

## 24、重绘和重排的区别

重绘是指元素的外观发生了改变但并没有影响到布局时，浏览器会重新绘制元素。常见的情况有：改变背景色、文字颜色等

重排是指元素的几何属性发生改变，例如宽高、边距、位置等，浏览器需要重新计算元素的布局，重新构建渲染树。

## 25、http 和 https 的区别

| 特性   | HTTP                         | HTTPS              |
| :----- | :--------------------------- | :----------------- |
| 安全性 | 不安全，以纯文本形式传输数据 | 安全，数据加密传输 |
| 端口号 | 80                           | 443                |
| 证书   | 不需要                       | 需要 SSL/TLS 证书  |

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

当 vue 实例被创建的时候，vue 会遍历 data 中的属性，在 vue2 中使用 object.defineProperty、在 vue3 中使用 proxy 进行数据劫持，将其转换成 getter 和 setter，并在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都对应着一个 watcher 实例，它会在组件渲染的过程中把属性记录为依赖，当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而使关联的组件更新

## 4、双向数据绑定的基本原理

Vue 中的双向绑定是通过数据劫持结合发布-订阅者模式来实现的。核心模块有三个，分别是：Observer 监听器、watcher 订阅者、compile 编译器；

首先 observer 会对数据进行响应化处理，也就是通过数据劫持的方式为所有属性添加 getter 和 setter。同时 compile 编译器对模板进行编译，并找到其中动态绑定的数据并初始化视图。同时也会绑定个更新函数到 watcher 实例，当数据发生改变时 watcher 就会调用这个更新函数来更新视图。但是 data 中的属性在一个组件里可能有多处调用，所有每个属性都对应着一个“管家”（dep）(属性订阅器，一个容器，用来存放所有订阅)。所以当 data 中的某个属性发生改变时，会首先找到其对应的 dep，然后 dep 通知所有的 watcher 实例进行更新。

## 5、使用 Object.defineProperty 来进行数据劫持有什么缺点

在对一些属性进行操作时，使用这种方式无法进行拦截。比如通过数据下标的方式修改数组内容，或是给对象新增一个属性，都无法触发组件的重新渲染。

## 6、MVVM

MVVM 分为三个部分，model（数据层）view（视图层）viewmodel（视图模型层）。model 层用来存放所有的数据和业务逻辑，view 层则用来展示 model 层中的数据，viewmodel 层则是两者之间的桥梁，负责监听 model 层的数据改变并更新 view 层，处理用户的交互操作。

### 6.1 优点

1.  分离了视图层和模型层，降低了代码耦合，提高了视图和逻辑的复用性
2.  自己自动更新 DOM，不再需要开发者自己手动去频繁操作 DOM

### 6.2 缺点

1.  BUG 难以调试，尤其是数据流和更新逻辑复杂时，难以追踪数据的变化来源
2.  对于大型的图形程序而言，视图的状态将会很多，viewmodel 层的构建和维护成本都会偏高

## 7、vue 父子组件的生命周期顺序

### 7.1 加载

1.  父组件 beforecreate
2.  父组件 created
3.  父组件 beforemount
4.  子组件 beforecreate
5.  子组件 created
6.  子组件 beforemount
7.  子组件 mounted
8.  父组件 mounted

### 7.2 更新

1.  父组件 beforeupdate
2.  子组件 beforeupdate
3.  子组件 updated
4.  子组件 updated

### 7.3 销毁

1.  父组件 beforedestroy
2.  子组件 beforedestroy
3.  子组件 destroyed
4.  父组件 destroyed

## 8、computed、watcher 的区别

### 8.1、computed

1.  支持缓存，只有依赖的数据发生了变化，才会重新进行计算
2.  不支持异步，当其中有异步操作时无法监听数据的变化
3.  如果一个属性是由其他属性计算而来，这个属性的值依赖于其他属性，一般会使用 computed

### 8.2、watcher

1.  不支持缓存，数据变化时就会执行相应操作
2.  支持异步监听，接收两个参数，第一个时新值，第二个是旧值
3.  函数有两个参数，immediate：组件加载立即触发；deep：深度监听，发现数据内部变化。在引用数据类型中使用，可以发现数组中的对象发生变化。但无法监听到数组和对象内部的变化。

## 9、插槽（slot）

插槽可以简单理解为在组件模板中已经占好了位置，使用该组件标签时，组件标签里的内容会自动填充到 slot 中去，从而实现更灵活和动态的组件布局。常见的应用常见就是布局组件。插槽主要分为三种：默认插槽、具名插槽、作用域插槽

```html
<!-- 默认插槽 -->
// slot标签内的内容即默认内容，如果父组件没有传入内容，则展示该部分
<button type="submit">
  <slot> Submit </slot>
</button>
<!-- 具名插槽 -->
// 具名插槽允许在一个组件内使用多个插槽，通过name属性来进行区分 //
当一个组件同时接收默认插槽和具名插槽时，所有位于顶级的非
<template>
  节点都被隐式地视为默认插槽的内容 // 子组件
  <template>
    <div>
      <header>
        <slot name="header"></slot>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>
  </template>
  // 父组件
  <my-component>
    <template v-slot:header>
      <h1>Header Content</h1>
    </template>

    <!-- 隐式的默认插槽 -->
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>

    <template #footer>
      <p>Footer Content</p>
    </template>
  </my-component>
  <!-- 作用域插槽 -->
  //
  作用域插槽允许父组件访问子组件传递的数据，在slot标签上绑定数据，父组件使用v-slot来访问插槽上的值
  // 子组件
  <template>
    <div>
      <slot :text="'text'"></slot>
    </div>
  </template>

  //父组件
  <my-component v-slot:default="slotProps">
    <p>{{ slotProps.text }}</p>
  </my-component></template
>
```

## 10、v-if、v-show 的区别

v-if 是动态的向 DOM 树中添加和删除 DOM，有个局部的编译和卸载的过程

v-show 则是简单的控制 css 中的 display 属性来控制 dom 的显隐，更适合频繁切换的场景

## 11、data 为什么是个函数而不是对象

因为一个组件可能在多处被引用。如果是对象的形式的话，修改一处的值那么所有该组件的值都会改变，造成数据混乱。所以将其写成一个方法返回的对象，避免了所有组件实例共享一个 data 对象。

## 12、keep-alive 组件

keep-alive 是 vue 内置的一个组件，用户缓存组件实例，减少了频繁切换场景下组件重新渲染带来的性能消耗。keep-alive 组件常用的参数有 include，exclude，max。只有在 include 中的组件才会被缓存，exclude 中的组件不会被缓存，max 值用来限制最大缓存的组件数量。最久没有被访问的实例会被销毁。

keep-alive 有两个钩子函数：activated，deactivated。分别在组件激活时、组件停用时调用

## 13、mixin

主要用途：

代码复用：当多个组件中有相同的逻辑和代码时。可以将其提取到一个 mixin 中，减少重复代码，提高代码的可维护性

逻辑分离：将复杂的逻辑分离到多个 mixin 中，使组件的代码更加简洁和清晰

## 14、nexttick

vue 更新 DOM 的操作是异步的，当数据变化时会产生一个异步更新队列，当更新队列结束后才会统一更新视图。这就导致修改数据后获取 DOM 的值仍然是旧值。在修改数据后使用该方法就可以获取更新后的 DOM。使用 promise.then 和 settimeout 也可以实现同样的效果。其本质就是事件循环机制的一种应用

### 14.1 为什么有异步更新队列机制

如果视图更新是同步的话，就可以导致以下情况：多次对一个或多个属性赋值，则会频繁触发 DOM 渲染，带来的性能消耗是很大的。同样，因为有虚拟 DOM 的存在，每次数据更新后都会去计算需要更新的节点位置，需要的计算量也很大。所以引入异步更新队列机制原因就是减少一些无用功

## 15、组件之间通信

1.  props/emits：父组件可以使用 props 向子组件传递数据。子组件使用 emits 向父组件传递事件
2.  ref/\$refs：在子组件上设置 ref 属性，ref 会指向子组件的实例。此时父组件可以通过 refs 访问子组件中的所有数据和方法
3.  provide/inject：适用于组件层级很深的情况。provide 钩子用来发送数据和方法，inject 钩子用来接收数据和方法
4.  eventBus：事件总线适用于兄弟组件之间传递数据，其本质就是一个 Vue 实例。
5.  vuex：对于一些更加复杂的组件通信可以使用 vuex 来进行状态管理。

```javascript
// eventBus.js
import Vue from "vue";
export const EventBus = new Vue();

// 发送事件
EventBus.$emit("event", params);
// 接收事件
EventBus.$on("event", (params) => {});
```

## 16、vuex

Vuex 是一个为 vue 设计的状态管理模式和库，它通过集中式存储来管理应用所有组件的状态，使得组件之间的状态共享和管理更加简单高效。vuex 的核心分为四个模块：state、actions、mutations、modules

state：用来存放所有的共享变量

actions：用来存放修改 state 的方法，actions 是在 mutations 的基础上进行，可以进行异步操作

mutations：用来存放修改 state 的方法，不支持异步

module：可以将 store 分割成模块，每个模块拥有自己单独的 state、mutations、actions，对于较大的项目可以方便的管理状态

### 16.1、pinia 和 vuex 的区别

pinia 和 vuex 一样，都是一个全局状态管理库。但是他们也有一些细微上的差别

pinia 的核心部分只有三个：state、getter、action。他们其实就对应着 vue 里的 data、computed、method。和 vuex 相比 api 更加简洁易懂。vuex 在大型项目里使用 module 来分割模块，而 pinia 更为简单，之间新定义一个仓库（store）即可（defineStore）。并且 pinia 在 vue3 中有着更好的 ts 支持。

## 17、vue 路由

### 17.1、vue-router 的懒加载如何实现

### 17.2、route 和 router 的区别

### 17.3、如何定义动态路由

### 17.4、params 和 query 的区别

### 17.5、路由守卫有哪些

## 18、相较于 vue2，vue3 更新了哪些内容

## 19、什么是虚拟 DOM?DIFF 算法又是什么？

## 20、vue 单页应用和多页应用的区别
