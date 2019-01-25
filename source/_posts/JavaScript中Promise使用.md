---
layout: default_layout
title: JavaScript中Promise使用
date: 2019-01-24 17:19:36
tags:
---
## 定义

Promise对象用于包装异步函数执行结果，以便用同步的方式处理其结果。

```javascript
var mypromise = new Promise(function(resolve, reject){
 // asynchronous code to run here
 // call resolve() to indicate task successfully completed
 // call reject() to indicate task has failed 
})
```

```javascript
mypromise.then(
    function(successurl){
        document.getElementById('doggyplayground').innerHTML = '<img src="' + successurl + '" />'
    },
    function(errorurl){
        console.log('Error loading ' + errorurl)
    }
)
```

promise 包含3种状态：

**pending**： 初始状态

**fulfilled**:  完成状态

**rejected**: 失败状态

## 链式调用方法
then()返回一个Promise对象 或者构造一个空白的Promise对象（如Promise.resolve()返回一个初始状态是fulfilled的延迟对象）

## 处理延迟对象数组
```javascript
var twodoggypromises = [getImage('dog1.png'), getImage('dog2.png')]
Promise.all(twodoggypromises).then(function(urls){
    console.log(urls) // logs ['dog1.png', 'dog2.png']
}).catch(function(urls){ // if any image fails to load, then() is skipped and catch is called
    console.log(urls) // returns array of images that failed to load
})
```
Promise.all([]) 参数传入Promise对象数组 then中得到结果的数组 
Promise.all() takes an iterable (array or array-like list) of promise objects, and waits until all of those promises have been fulfilled before moving on to any then() method attached to it. The then() method is passed an array of returned values from each promise.

## 解决Promise数组 有一个执行失败 就进入catch问题
**使用map**
```javascript
var doggies = ['dog1.png', 'dog2.png', 'dog3.png', 'dog4.png', 'dog5.png']
var doggypromises = doggies.map(getImage) // call getImage on each array element and return array of promises
var sequence = Promise.resolve()
 
doggypromises.forEach(function(curPromise){ // create sequence of promises to act on each one in succession
    sequence = sequence.then(function(){
        return curPromise
    }).then(function(url){
        var dog = document.createElement('img')
        dog.setAttribute('src', url)
        doggyplayground.appendChild(dog)
    }).catch(function(err){
        console.log(err + ' failed to load!')
    })
})
```