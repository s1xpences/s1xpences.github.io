---
title: 异步编程之Promise
categories:
  - 前端学习
tags:
  - Promise
date: 2022-07-12 09:11:32
index_img: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png
---

官方文档：​[mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
参考视频：[BV1GA411x7z1](https://www.bilibili.com/video/BV1GA411x7z1?spm_id_from=333.999.0.0)  [BV17j411f74d](https://www.bilibili.com/video/BV17j411f74d?spm_id_from=333.999.0.0)

---

### 理解和使用
+ promise是js中异步编程的新的解决方案，旧方案是单纯使用回调函数
  ```javascript
  //回调地狱
  var fs = require('fs');

  fs.readFile('./views/index.html',  (err, data) => {
    if (err) {
        throw err
    }
    fs.readFile('./views/main.html', (err, data) => {
        if (err) {
            throw err
        }
        fs.readFile('./views/update.html', (err, data) => {
            if (err) {
                throw err
            }
            console.log(data.toString());
        })   
        console.log(data.toString());
    })
    console.log(data.toString());
  })
  ```

+ 异步编程例子：
  ```javascript
  //fs 文件操作
  require('fs').readFile('./index.html',(err,data)=>{})

  //数据库操作
  let db = require('mysql').createConnection({
    host:'localhost',
    port:'3306', // 可选,默认3306
    user:'root',
    password:'xxxxxx',
    database:'mydbNew'
  })
  let strSql7 = 'insert into studetNew (id,name,password) value (?,?,?)'
  db.query(strSql7,[123,'s1xpences','123123'],(err,results) =>{
    if(err){
        console.log(err)
    }else{
        console.log('插入数据操作成功')
    }
  }

  //AJAX
   $.get('/server.json',(data)=>{})

  //定时器
  setTimeOut(()=>{},1000)
  ```


### promise里面包裹一个异步操作
+   异步操作成功调resolve，将promise对象的状态设置为成功；失败调reject，将promise对象的状态设置为失败

+   then指定成功使的回调，catch指定失败时的回调

+   获取成功和失败的结果值，通过resolve和reject传递，通过then的value,和catch的reason接受


### promise对象的状态
+   就是实例对象中的一个属性 PromiseState，有三种状态
    >pendding   未决定的，初始化的默认值
    >resolved / fullfilled  成功
    >rejected  失败

+   状态变换只有两种可能且只能改变一次 pending ==> resolved, pending ==> rejected


### promise对象的值
*   实例对象的一个属性 PromiseResult，保存的是异步任务 成功/失败 的结果，只能通过reslove和reject对结果进行修改

*   调resolve和reject即修改状态又设置结果值


### promise的基本流程
![基本流程](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png)


### 深入
1. executer函数：执行器(reslove,reject)=>{}
    >即promise实例的参数，在promise内部立即同步执行，异步操作在执行器内部执行

2. Promise.prototype.then,指定成功的回调，返回一个新的promise对象

3. promise.prototype.catch，指定失败的回调

4. Promise.resolve(参数)
    >传入参数为非promise类型的对象，则返回成功的promise对象，值为参数
    >传入参数为promise对象，则参数决定resolve的结果，成功失败由内部promise决定，值也是

5. Promise.reject(参数)
    >返回一个失败的promise对象
    >不论传入的参数是何种类型，返回结果都是一个失败的promise对象，值为传入的参数
    >即使参数为一个成功的promise对象，返回的状态仍未失败，值为传入的promise对象
    >报错：当前有一个失败的promise而且没有对应的回调对其做处理

6. Promise.all()
    >参数是promise对象组成的数组
    >返回一个新的promise，所有成功则成功，一个失败即失败
    >成功则值为每一个promise值组成的数组，失败则值为其中第一个失败的promise对象的值

7. Promise.race()
    >参数是promise对象组成的数组
    >返回一个新的promise，第一个完成的promise状态即为最终状态，第一个的值则为最终值


### 问题
1.  改变promise对象的状态？
    >reslove、reject、throw(异步任务里面不能抛错)

2.  改变状态和指定的回调函数谁先执行？
    >如果executer函数中是同步任务，则先改变状态后执行回调
    >若executer函数中是异步任务，则先执行回调后改变状态
    >回调执行的时候既能拿到数据

3.  promise.then()返回的新的promise的结果状态由什么决定？
    >then里面的回调是异步执行的
    >由then()指定的回调函数的返回(return)结果决定
    >+  throw value，则状态rejected,值value
    >+  return value，则状态fullfilled,值value
    >+  return new Promise((reslove,reject)=>{})，状态和值都由里面promise决定

4.  链式调用？
    >通过then的链式调用串联多个 同步/异步 任务

5.  异常穿透？
    >当使用promise的then链式调用时，可以在最后指定失败的回调
    >前面任何操作出了异常，都会传到最后失败的回调中处理

6.  中断promise链？
    >当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数
    >方法：在调用函数中返回一个pending状态的promise对象,return new Promise(()=>{})


### async函数
1.  函数执行返回值为promise对象
    >promise对象的结果由async函数执行的返回值决定(规则同then())

2.  await表达式
    >await右侧的表达式一般为promise对象，但也可以是其他值
    >如果表达式是promise对象，await返回的是promise成功的结果值
    >如果表达式是其他值，直接将此值作为await的返回值
    >await必须写在async函数中，async函数中可以没有await
    >如果await的promise失败了，就会抛出异常，需要通过try...catch捕获处理，结合async和await使用
    >错误不用每一层都判断，只用在最外面加try...catch即可

3. 使用
  ```javascript
  try{
      //有可能出现错误的代码写在这里
  }catch(e){
      //出错后的处理,e为错误对象
  }
  async function main() {
      let p = new Promise((reslove,reject)=>{
          reject('error')
      })
      try{
          let res = await p
      }catch(e) {
          console.log(e) //拿到失败的结果值
      }
  }
  main()
  ```


### 代码演示
```javascript
    //回调函数
    fs.readFile('index1.html',(err,data1)=>{
        if(err) throw err
        fs.readFile('index2.html',(err,data2)=>{
            if(err) throw err
            fs.readFile('index3.html',(err,data3)=>{
                if(err) throw err
                console.log(data1+data2+data3)
            })
        })
    })

    //Promise形式
    new Promise((reslove,reject)=>{
        fs.readFile('index1.html',(err,data1)=>{
            reslove(data1)
        })
    }).then((value)=>{
        return new Promise((resolve,reject)=>{
            fs.readFile('index2.html',(err,data2)=>{
                reslove(value + data2)
            })
        })
    }).then((value)=>{
         return new Promise((resolve,reject)=>{
            fs.readFile('index3.html',(err,data3)=>{
                console.log(value + data3)
            })
        })
    }).catch((reason)=>{
        console.log(reason)
    })

    //async形式
    let p1 =  new Promise((reslove,reject)=>{
        fs.readFile('index1.html',(err,data1)=>{
            reslove(data1)
        })
    })
    let p2 =  new Promise((reslove,reject)=>{
        fs.readFile('index2.html',(err,data2)=>{
            reslove(data2)
        })
    })
    let p3 =  new Promise((reslove,reject)=>{
        fs.readFile('index1.html',(err,data3)=>{
            reslove(data3)
        })
    })
    async function main() {
        try{
            let data1 = await p1
            let data2 = await p2
            let data1 = await p3
            console.log(data1+data2+data3)
        }catch(err) {
            console.log(err)
        }
    } 
    main()
```