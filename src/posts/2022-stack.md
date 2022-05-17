---
title: The Technologies I am Learning in 2022
date: 01/09/2022
description: The world of programming is growing so quick so here is my short list of tech that I want to learn in the new year
---

``` js
let func = () => {
  console.log(1)
}
setTimeout(() => {
  func = () => {
    console.log(2)
  }
}, 0)

setTimeout(func, 100)
```
**输出结果：** 1
**分析过程：**
	首先明确的是这段代码不存在 函数声明 ，`func`是 函数表达式 。
	第1--->3行：使用let声明变量`func`，指向一个函数。
	第4 --->8行：遇到setTimeout，setTimeout的`回调函数参数`会在主代码执行完成后，加入到`task queue。`
	第10行：又遇到了setTimeout，参数是func变量，100ms后func被加入到task queue。
	主代码执行结束。
	第一个setTimeout的回调函数被加入到了`task queue`。
	此时100ms还没到哦，所以取出`task queue`的回调函数执行掉，执行完后，`func变量`指向了一个新的箭头函数
	....100ms间隔
	100ms时间到了，将func加入到`task queue`。
	此时没有其他的任务，取出func执行，此处有陷阱！大多人会觉得**输出2**，因为func在之前被重新指向了另一个箭头函数(代码里输出2)，但其实不是，setTimeout并不会感知到func变量的变化，它接收的其实是变量地址，只不过是通过func这个载体来传达的变量地址，后续func怎么变，都与setTimeout无关了，这就是JS的晦涩之处吧。
**变体：**
	变体1：
		``` js
					  const obj = {
					   func: () => {
					    console.log(1)
					    }
					  };
					  setTimeout(() => {
					    obj.func = () => {
					      console.log(2)
					    }
					  }, 0)
					  
					  setTimeout(obj.func, 100)
		```
		依旧会输出1。
	变体2：
		``` js
					  let func = () => {
					    console.log(1)
					  }
					  setTimeout(() => {
					    func = () => {
					      console.log(2)
					    }
					  }, 0)
					  
					  setTimeout(() => {
					    func();
					  }, 100)
		```
		这次会输出2！这个例子中func延迟执行，不再是setTimeout的参数了，具有延迟动态性。
