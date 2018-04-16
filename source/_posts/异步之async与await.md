---
title: async与await
date: 2017-03-20 21:33:05
tags:
---
async和await是JavaScript的ES7新特性。它可以不用回调函数，像同步代码那样编写异步代码。

## async和await简介
-------------------
async函数定义一个异步函数，该函数返回一个promise。
如果async函数返回的是一个直接量，这个值将被包装成一个理解resolve的promise，等同于return Promise.resolve(value)。
await操作符通常用来等待promise对象,但是根据语法他是可以用来等待各种值的 await只能用在async函数的上下文中。await等到的如果不是promise对象，那await表达式的结果就是await后面的表达式的值
如果他等到的是一个promise对象，他会阻塞后面的代码，直到promise对象resolve，然后得到resolve的值
```javascript
async function getInfo() {
	let username = await 'xiaoming';
	return username;
}
getInfo().catch(function(err) {
	console.log(err);
});
```
## async和await的好处
-------------------
### 去除回调函数
```javascript
function getApi() {
	
}

```
### 简化并发
使用循环逐个执行SQL语句插入数据库太慢，使用并发方式更为简单
#### 简化前
```javascript
let mocks = [{"mock":"aaa","selected":1},{"mock":"asdasd","selected":1},{"mock":"bcd","selected":0}];
for (let item of mocks) {
	let insertMock = `insert into pmock_mocks (mock, selected)
					values (?, ?)`;
	let insertMockParams = [item.mock, item.selected];
	await uniQuery(insertMock, insertMockParams);
}
```
#### 简化后
```javascript
let mocks = [{"mock":"aaa","selected":1},{"mock":"asdasd","selected":1},{"mock":"bcd","selected":0}];
let mockPromises = mocks.map(function(v, k) {
	let insertMock = `insert into pmock_mocks (mock, selected)
					values (?, ?)`;
	let insertMockParams = [v.mock, v.selected];
	return uniQuery(insertMock, insertMockParams);
});
await Promise.all(mockPromises);
```
## reference
-------------------
* [async_function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
* [深入理解ES7的async/await](http://blog.csdn.net/sinat_17775997/article/details/60609498)
* [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
* [Async/Await是这样简化JavaScript代码的](https://blog.fundebug.com/2017/10/16/async-await-simplify-javascript/)
* [AsyncFunction](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction)

