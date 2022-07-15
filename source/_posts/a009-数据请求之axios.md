---
title: 数据请求之axios
categories:
  - 前端学习
tags:
  - axios
date: 2022-07-15 09:26:37
index_img: https://img2.baidu.com/it/u=782602782,576825549&fm=253&fmt=auto&app=138&f=JPEG?w=1000&h=500
---

### 数据请求的特点
**1. 无刷新更新数据**

**2. 异步与服务器通信**

**3. 前端和后端负载平衡**

**4. 基于、支持标准**

**5. 界面与应用分离**

### 什么是axios
axios是基于promise的http客户端，可以在浏览器(发送AJAX)和node(发送http请求)环境中运行
axios的执行结果返回是一个promise对象

### 响应结果结构
```javascript
{config,data,headers,request,status,statusTxt}
```
>config：发送请求的配置对象
>data：响应体的结果，axios自动进行了json解析
>headers：响应头信息
>request：原生的ajax请求对象
>status：响应状态码
>statusTxt：响应状态字符串

### 请求配置对象
```javascript
    {url,method,baseURL,transformRequest,transformResponse,headers,params}
```
>transformRequest：对请求参数进行预处理
>transformResponse：对响应结果进行预处理

### 实例对象
实例对象的功能和axios的功能几近一样，使用时一般不会使用全局的axios
可以应用实例给不同的域名设置默认配置
```javascript
axios.defaults.baseURL
const duanzi = axios.create({baseURL:'http://b.com'})
```
    
### 拦截器
>拦截器类似于promise.prototype.then(成功回调，失败回调)
>请求拦截器config：可以对请求参数进行处理
>响应拦截器response：可以对响应结果进行处理
>请求拦截器使用场景：
    >+   config中的信息不符合服务器的要求，需要更改时
    >+   每次发送网络请求希望界面显示请求图标时(转圈效果)
    >+   某些网络请求(比如登录需要携带token)，必须携带一些特殊信息时

### 网络模块的选择
>原生Ajax
>特点：基于XMLHttpRequert，配置和调用非常混乱
>
>fetch
>特点：低层次的API，类似于原生的XHR，需要进行封装
>
>jquery-Ajax
>特点：Jqury代码1W+行，为了使用$ajax下载jquery大可不必
>
>vue-resource
>特点：支持vue1.x，不支持新版本，官方也不再推荐使用
>
>axios
>特点：本身是一个promise对象

### 跨域设置
接口示例：[coderwhy的接口](http://123.207.32.32:8000/home/multidata)

这里以vue为例：
跨域设置本地服务器需要加上协议，如target:"http://localhost:3000"
axios在解析域名时先在遇到/api前缀在其前加target后将/api转换为pathRewrite中对应的内容

### 补充
>取消请求使用配置对象canceltoken属性
>
>axios发送并发请求axios.all(类似于Promise.all),并可使用axios.spread展开结果的数组
>
>不传method默认get请求，get请求是通过query传递参数的,axios中是params；post是通过requesbody传递参数,axios中是data
>
>axios的封装。不要在每一个组件中单独引用axios，万一axios不再维护需要另一个框架来替换的时候代码量太大

---
中文文档：[中文文档](http://www.axios-js.com/zh-cn/docs/)

参考视频：
[BV1wr4y1K7tq](https://www.bilibili.com/video/BV1wr4y1K7tq?spm_id_from=333.999.0.0)   [BV17j411f74d](https://www.bilibili.com/video/BV17j411f74d?p=141)