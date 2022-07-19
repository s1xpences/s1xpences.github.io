---
title: 前端框架之React
categories:
  - 前端学习
tags:
  - React
date: 2022-07-19 09:59:41
index_img: https://img1.baidu.com/it/u=3545225878,2086373205&fm=253&fmt=auto&app=138&f=JPEG?w=450&h=236
---

### react简介
react将数据渲染为HTML视图的js库；vue是毛坯房

原生操作DOM繁琐，直接操作DOM会有大量的回流重绘，占用浏览器性能；没有组件化编码方案，代码复用率低

react声明式编码，不用什么事都亲力亲为，只用告诉你应该是怎么样的，不会告诉你具体应该怎么做

使用虚拟DOM和优秀的diffing算法,减少与真实DOM的交互从而减少回流重绘；案例：原生和框架对于数据增加的渲染过程比较


### react核心模块   
1. babel 
ES6=>ES5 JSX=>JS

2. react-development
核心库

3. react-dom
扩展库帮你操作dom

4. 为什么选择使用jsx语法?
更加简单的创建虚拟DOM;但是babel会将JSX翻译成原始创建虚拟dom(方式二)的写法，即方式一是方式二的语法糖

```javascript
   //方式一
    const VDOM = (//可缩进写法加小括号
      <h1 id="title">
        <span>Hello, React!</span>
      </h1>
    )

   //方式二
   const VDOM = React.createElement('h1',{id:'title'},React.createElement('span',{},'Hello, React!'))
```


### 虚拟dom和真实dom
本质是Object类型的对象(一般对象)

真实dom身上属性多(document.creatElement);虚拟dom(ReactDOM.creatElement)属性少,因为虚拟dom是react内部用，无需那么多属性

虚拟dom最终会被react转换为真实dom呈现在页面中


### JSX语法规则
JSX全称JavaScript XML，XML早期用于存储和传输数据(微信公众号服务器和开发者服务器还是用的xml)，现在使用json

xml语法：
```xml
  <student>
        <name>tomatoes</name>
        <age>23</age>
  </student>
```

jsx规则：    
1. 定义虚拟dom时，不要写引号;  
2. 标签中混入js表达式要用{},且只能是表达式，js代码语句不能写;  
let 变量名 = 表达式/js语句，能接到值的即表达式，函数本身其实也是表达式  

```javascript
  // 表达式      
  a;a+b;demo(1);arr.map();function test() {}; 

  //代码        
  if(){};for(){};switch(){};
```

3. 样式的类名指定不要用class，要用className；因为class为es6中的类定义关键字; 
4. 行内样式要用`style={{key:value}}`的形式，外面的{}表示里面是js表达式，里面的{}表示对象.需要连接符表示的样式名要用小驼峰; 
5. 虚拟dom只有一个根标签  
6. 标签必须闭合 `<input type="text"/>`
7. 标签首字母：
小写字母开头，则必须与html中的标签一致，若html中没有则报错，如：`<good></good>`
大写字母开头，则react渲染相应的组件，若组件没有定义则报错，如：`<Good></Good>`
8. react会自动遍历数组，但是不能遍历对象
9. jsx中的普通函数不要加(),否则在渲染的时候就会调用(和vue不一样)。加了()是将函数的返回值作为事件的回调


### 模块与组件
模块：向外提供特定功能的js程序，一般就是一个js文件

组件：用来实现局部功能效果的代码和资源的集合(html/css/js/img/video/font等)


### 开发者工具
components：观察网页的组件，以及组件的属性

profiler：记录网站的性能，渲染用了多久，哪个组件加载最慢


### 函数式组件
函数命名使用大驼峰;

组件内部的this指向undefined，babel解析时开启了严格模式禁止自定义函数的this指向window;

执行ReactDOM.render后发生了什么？react解析组件标签，找到MyComponent组件;发现组件是函数定义的，随后调用该函数;将返回的虚拟dom转换为真实dom呈现到页面


### 类式组件
类基础：  
>类中的constructor在new 实例时调用; 
> 
>只使用父类的属性则constructor可以省略，相当于自动帮你调用了，实例化时同样需要传入参数；若需添加子类属性则需要constructor且super()必须写且参数个数等于父类属性个数加子类属性个数;  
>
>类中所定义的方法，都是放在了类的原型对象上，供实例去使用;  
>
>子类prototype上的_proto_指向父类的prototype; 
> 
>**super用来调用父类的构造器必须放在当前构造器的最前面（对于this操作的最前面）,就像调用函数类似** 
> 
>super除了调用父类构造器，还能调用父类上的实例方法，如super.say();
>  
>js中构造器的主要作用是用来接收参数，调用this，可以写变量声明，如let a=1; 
> 
>若子类上有和父类相同的属性则实例中子类属性会覆盖父类实例;  
>
>当时子类和父类相同的方法是不会覆盖的，都各自在自己的原型对象上，只是调用时原型链有就近原则;  
>
>类里面的内容：构造器、构造器外面的赋值语句的属性和方法，如a=1、方法(写在原型身上)、静态static(写在类身上);

类式组件继承自React.Component,暂时不用考虑构造器,但是必须要有有返回值的render方法

执行ReactDOM.render后发生了什么？
>react解析组件标签，找到MyComponent组件;发现组件是类定义的，随后new出该类的实例并通过该实例调用(所以render中的this没有问题)到原型上的render方法;将返回的虚拟dom转换为真实dom呈现到页面

### 理解state
复杂组件(类式组件)和简单组件(函数式组件)的判断标准，即有无state；函数式组件连this都没有，但是可以使用hooks

state存在于组件实例对象上


### 组件三大核心属性
仅限类式组件

#### state  
>state最开始定义在React.Component类上值为null，通过继承到了类组件上;类组件通过重新定义从而覆盖了父类上的state，并应用到了每一个组件实例上。 
>
>为什么会this丢失？ 因为this.changeWheather并不是直接调用，而是给了onClick作为回调点击之后才调用。

![this丢失](/img/sample/a013/this丢失.png)

>注意状态不可直接更改，要借助一个内置的API(setState存在于React.Component的原型上)，setState更新状态是一个合并的动作不是替换;  
>
>**类里面可以直接写赋值语句。所有实例都有的属性且值一样，可以不用写在constructor里面；如果是new实例传入的则必须通过constructor接收;**  
>
>简写：state直接写赋值语句，实例方法不用写在原型上而是直接赋值语句+箭头函数;

#### props
>将实例化组件时的组件属性传递到类的props中；疑问：为什么不写super(props)也会值也会传递到React.Component的this.props上？(相当于react帮你自动调用了super(props));
>  
>es6扩展运算符(浅拷贝)，可以展开数组(...arr)不能直接展开对象(...obj)，但是能够以字面量形式复制对象({...obj});  
>
>props是只读的;  
>
>构造器写不写没有任何影响，但是如果写的话就要传props否则会出现在构造器中访问this.props==undefined；所以说类中的构造器基本都省略。构造器中扩展state则必须传props;
>  
>函数式组件没有state、refs，但是可以有props

#### refs
>refs拿到的是真实dom;
>  
>不要过度使用ref;
>  
>不推荐使用字符串形式的ref，存在一些效率的问题;  
>
>回调函数形式的ref，是往实例自身上挂真实dom;
>  
>内联函数形式`ref={c=>{this.xx = c}}`，react会在render时自动调用函数，并且将当前节点作为实参传入; 
>
>**内联函数在页面首次render时会调用一次，但是state改变时render再次调用且将之前的内联函数清除创建一个新的函数实例，所以每次state更新回调两次内联函数,且第一次打印的参数为null，第二次才是当前的真实dom;**
>
>class的绑定函数形式的ref，第一次render时调用绑定函数，之后改变state触发render时，组件知道函数为自身函数并且之前调用过，不会创建一个新的函数实例且不再调用函数；
>
>内联函数的问题无关紧要，真实开发常用；  
>
>createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是专人专用,用一个创建一个,虽然繁琐但是强烈推荐  
>
>使用： 字符串形式：`this.refs.xxx` 回调函数形式：`this.xxx` createRef形式：`this.xxx.current`


### 事件处理
通过onXxx属性指定事件处理函数(注意大小写)

react使用的是自定义(合成)事件，而不是原生dom事件，为了更好的兼容性

react中的事件是通过事件委托方式处理的(委托给组件最外层的元素`<div></div>`，冒泡)，为了高效

通过event.target得到的是触发事件的dom元素对象(事件源)，可以避免过度使用ref


### 收集表单数据
非受控组件  
>表单提交默认会刷新页面使用e.preventDefault()可阻止提交，一般使用ajax页面无刷新获取数据;  
>点击按钮后取节点的value值(现用现取);

受控组件  
>随着输入维护状态(类似vue的双向数据绑定)，推荐使用，基本没用ref


### 高阶函数和函数柯里化
一个函数接受的参数是一个函数，或者调用的返回值是一个函数。那么该函数称之为高阶函数

```javascript  
  // 常见的高阶函数
  new Promise((reslove,reject) => {})

  setTimeout(()=>{}, 1000)

  arr.map((item,index) => {})

  const saveData = (type) => { return (event) => { this.setState({ [type]: event.target.value })}}
``` 

函数柯里化，通过函数调用继续返回函数的方式。实现多次接收参数最后统一处理的函数编码形式。

```javascript
  function sum(a) {  
      return(b)=> {  
          return(c) =>{  
            return a + b +c  
          }    
      }  
  }  
  sum(1)(2)(3)
``` 

不用柯里化的方式是函数里面返回函数，回调地狱是函数里面调函数


### 组件的生命周期
#### 旧生命周期 

![生命周期(旧)](/img/sample/a013/生命周期(旧).png) 

常用：  
>componentDidMount               一般做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息  
>componentWillUnmount            一般做一些收尾的事，例如：关闭定时器、取消订阅消息  
>render                          必须使用  

#### 新生命周期
![生命周期(新)](/img/sample/a013/生命周期(新).png) 

新旧生命周期对比：   
>旧废弃三个(componentWillMount、componentWillUpdate、componentWillRecieveProps) 
>新增加两个(getDerivedStateFromProps、getSnapshotBeforeUpdate)，不常用


### DOM的diffing算法
diff的最小单位是节点

#### 虚拟DOM中的key的作用：  
当状态数据发生变化时，react会根据新的数据生成一堆新的虚拟dom，随后进行新虚拟dom和旧虚拟dom的diff比较 

旧虚拟dom中找到与新虚拟dom中相同的key：
>若虚拟dom中的内容没变，直接使用之前的真实dom  
>若虚拟dom中内容变了，则生成新的真实dom，随后替换掉页面中之前的真实dom 

旧虚拟dom中未找到与新虚拟dom中相同的key：根据数据创建新的真实dom，随后渲染到页面

#### 用index作为key可能会引发的问题：  
对数据进行逆序添加、逆序删除等破坏顺序操作，会产生没有必要的真实dom更新。界面效果没有问题，但效率低。

![造成不必要的真实dom更新](/img/sample/a013/index作为key造成不必要的真实dom更新.png)

如果结构中还包含输入类的dom(表单元素类)，会产生错误dom更新。界面有问题 

![index作为key结构中包含输入类dom](/img/sample/a013/index作为key结构中包含输入类dom.png)

如果不存在上述情况，使用index作为key是没有问题的


### create-react-app
#### xxx脚手架
>用来帮助程序员快速创建一个基于xxx库的模板项目
>包含了所有需要的配置(语法检查、jsx编译、devserver...)
>下载好了所有相关依赖
>可以直接运行一个简单效果

react提供了一个用于创建react项目的脚手架库：create-react-app；整体架构为react+webpack+es6+eslint

使用脚手架开发项目的特点：模块化、组件化、工程化

创建项目并启动：`npm i -g create-react-app => create-react-app xxx => cd xxx => npm start`    


#### 项目结构
public 静态资源文件(页面、样式、公共图片)  
>   --robots.txt    爬虫规则文件

src  源文件
>   --App.js        App组件，首字母大写的都是组件  
>   --App.test.js   测试App的，几乎不用  
>   --index.css     公用样式  
>   --index.js      入口文件，和vue中的main.js类似  
>   --logon.svg     dome页面中一直旋转的图  
>   --reportWebVitals.js   记录页面性能  
>   --setupTest.js          组件测试：性能整体测试，模块的单元测试

#### todolist案例
1. vscode代码提示
快捷生成组件结构，先进入文件，生成类式组件rcc,生成函数式组件rfc(类似于.vue)，需要安装插件；
jsx中html标签提示，在setting.json中加入

```json
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    }
```

2. 可以在子组件中更改父组件中的状态，即将父组件更改自己状态的函数当做props传给子组件；    
类似于vue中子组件调用this.$emit('xxx','参数'),或者直接 :fn="fn",子组件this.fn('参数')

3. 状态在哪里，操作状态的方法就在哪里

4. checkbox 上使用checked属性，则必须使用onChange事件;defaultChecked只在第一次能用，后面改变勾选和不勾选状态和它无关    
defaultValue和value的区别同上

5. 动态初始化列表，如何确定将数据放在哪个组件的state中？  
某个组件使用：放在其自身的state中  
某些组件使用：放在他们的共同组件state中(状态提升)

6. 父子组件通信：  
父组件给子组件传递数据，通过props  
子组件给父组件传递数据，通过props(函数)，子组件通过调用父组件传过来的函数向父组件传参

#### ajax
跨域代理

![跨域代理原理](/img/sample/a013/跨域代理原理.png)

方式一
>package.json中配置 (这种方式只能配一个代理)  
>"proxy": "http://localhost:5000"
>axios中url写localhost:3000，会自动转发给5000  
>如果请求为3000(public目录下)有的则返回，没有的则找5000,5000也没有则404

方式二
>在src目录下建立setupProxy.js文件(使用commonjs语法)  
>可以配置多个代理  

方式三
>后端解决(cors 一劳永逸)

#### 消息订阅-发布机制
兄弟(任意)组件通信(传递数据),PubSubJs(类似vue中的eventBus,($on|subscribe)、($emit|publish)、($off|unsubscribe))

#### fetch
axios、jquery都是对原生xhr(window自带api)的封装
fetch是window自带的api，也是promise风格;老版本浏览器可能不支持
xhr和fetch都是用来发送ajax请求
参考博客：https://segmentfault.com/a/1190000003810652

#### route
1. 前后端路由
>单页面应用(SPA)，**点击页面中的链接不会刷新页面或打开新的页面，只会做页面的局部刷新**，单页面多组件；
>一个路由就是一个映射关系(key|path,value|component(前端路由)或function(后端路由))；
>后端路由：route.get(path,function(req,res){})；
>
>route的底层是通过操作BOM上的history实现的
>方式一：直接使用H5退出的history身上的api  
>方式二：hash值(类似于锚点跳转，不会刷新页面，并且有跳转记录)，兼容性好，#号后面的东西不会带给服务器

![后端路由](/img/sample/a013/后端路由.png)

![前端路由](/img/sample/a013/前端路由原理.png) 

2. react-router-dom  
>路由工作过程：点击导航链接引起路径变化，路径变化被路由器检测到进行匹配组件从而展示
>
>封装NavLink，标签内容会以children传到子组件的this.props中，同时子组件可以设置children改变自己的内容  
>需要选中样式时使用NavLink,之后封装MyNavLink；不需要时直接使用Link
>
>使用Routes包裹后相同的路径只匹配一次，后面的不会再匹配，效果同之前的Switch(提高路由匹配效率，单一匹配)

3. 解决样式丢失问题
>当路由出现多级时，如：/a/home，之后点击刷新会出现样式丢失，因为此时请求文件路径localhost:3000/a/css/bootstrap.css  
>
>请求文件地址localhost:3000/css/bootstrap.css,其实是请求public下的/css/bootstrap.css。  
>
>在不设置跨域的情况下，请求文件在public没有则默认返回public/index.html 
> 
>方式一：直接/css/bootstrap.css不要./  
>方式二：%PUBLIC_URL%//css/bootstrap.css(此方法只适用与react脚手架)  
>方式三：仍然使用./css/bootstrap.css，index.js中使用HashRouter  

4. 模糊匹配
v6以上版本默认为精准匹配  
模糊匹配navlink上的to，按/从前往后匹配，第一个有则展示相应的route

5. 重定向
路由和组件的匹配是从上往下匹配的  
重定向一般写在路由注册的最下方，当所有路由都无法匹配时，跳转到redirect指定的路由

6. 路由传参
>嵌套路由的匹配是从最外层开始的，当点击home/news时匹配`<Route path="/home">`完美匹配，加载路由对应的组件。然后匹配当前组件中对应的路由/home/news  
>react-router-dom V6- 直接在路由组件的this.props上取              
>[中文文档](https://react-router.docschina.org/web/guides/philosophy) 
>
>react-router-dom V6 在**函数组件**中                              
>获取params可以使用useParams；  
>获取search可以使用useSearchParams或useLocation；(使用useLocation需要将字符串转义为对象)  
>获取state可以使用useLocation  
>该方法虽然浏览器地址没有记录(不同的state，history栈中只记录一个)，但是在使用BrowserRouter刷新传递的参数不会丢失  
>**HashRouter刷新传递的参数会丢失**  
>[官方文档](https://reactrouter.com/docs/en/v6/api)

7. 编程式路由导航  
>react-router-dom V6- this.props.history.push()  
>只有路由组件中才有history，普通组件中没有，因为路由组件中自动传了路由相关的props；  
>普通组件中使用export default withRouter(组件名) 
>
>react-router-dom V6  使用useNavigate  
>hook必须在函数组件内部调用，且在最外层
>
>react-router-dom 没有像vue-router中的集中管理文件router/index.js

#### react UI组件库  
国外：material-ui  
国内：ant-design


### redux
redux是专门用于状态管理的js库(不是react插件库)  
集中管理react中多个组件共享的状态，可用于状态共享和组件通信

#### redux工作原理
1. action      
包含两个属性(就是一个简单的js对象)  
type：标识属性，值为字符串，唯一必要属性(初始化时为@@REDUX/INIT_a.3.b.4)  
data：数据属性，值为任意类型，可选属性(初始化时没有该属性) 

2. reducers    
用于加工状态、初始化状态。加工时，根据旧的state和action，产生新的state的纯函数   
初始化时previousState为undefined  

3. store       
将state、action、reduce联系在一起的对象

![原理图](/img/sample/a013/redux原理图.png)

4. redux只负责管理状态，状态改变驱动页面展示，需要自己写

5. 异步action  
就是action的值为函数，store会帮忙调用传过去的函数；  
异步action中一般都会调用同步action；  
想要对状态进行操作，但是具体数据靠异步任务返回；  
异步action不是必须要用的，完全可以自己等待异步任务的结果后再去分发同步action 

6. 同步action  
action的值为Object的一般对象

#### react-redux      
UI组件：不使用任何redux的api，只负责页面的呈现、交互  
容器组件：负责和redux通信，将结果交给UI组件  
mapDispatchToProps也可以是一个对象  
react-redux不需要监听状态更新驱动页面展示  
Provider不需要在每个容器组件上通过props传递store，自动给所有容器组件传递store；这样传UI组件的props中没有store  
数据共享，需要引入combineReducers；redux管理的状态合并为一个对象

![react-redux模型图](/img/sample/a013/react-redux模型图.png)

#### 纯函数  
只要是同样的输入，必定得到同样的输出  
不得改写参数数据  所以personReducer中不能preState.push(data)  
不会产生任何副作用，例如不会调用网络请求(不确定什么时候返回结果)  
不能调用Date.now()或Math.random()等不纯的方法(同样的输入，没有同样的输出)  
redux中的reducer必须是一个纯函数

#### 配置
redux开发者工具 npm i redux-devtools-extension，store中配置composeWithDevTools
项目打包运行全局安装npm i serve -g，npm run build之后serve build将build作为根目录快速启动一台服务器，可以看到react图标是蓝色而不是红色


### 扩展
#### setState   
回调函数在状态更新完毕，页面也更新后(render调用后)才被调用  
对象式setState是函数式setState的语法糖  
如果新状态不依赖于原状态——建议使用对象式(不管原来和是多少,点击按钮最终变成99)  
如果新状态以来于原状态——建议使用函数式  
使用没那么绝对，都可以用

#### lazyload  
不使用懒加载，查看开发者工具network，将disable cache勾上，点击切换路由组件没有网络请求，说明网页加载时将所有组件都加载了  
使用懒加载后，路由组件随用随请求，且只请求一次，后面切换到相同的路由不会重复请求

#### Hooks  
Hook是react 16.8.0版本增加的新特性/新语法，可以让你在函数式组件中使用state以及其他的react特性  
函数组件相当于类式组件的render执行 1+n次  
1. stateHook：  
>可以让函数组件用state状态  
>const[xxx,setxxx] = React.useState(初始值)，setxxx用法同类式组件的setState都有两种用法 

2. EffectHook：  
>可以让你再函数式组件中执行副作用操作(用于模拟类组件中的生命周期)  
>可以把EffectHook看做componentDidMount、componentDidUpdate、componentWillUnmount的组合 

3. RefHook

#### Fragment
避免页面结构层级过深，占位标签

#### Context
常用于祖孙组件之间的通信  
在应用开发中一般不用context，一般都用它封装react插件，如：react-redux 的 `<Provider store={store}>`

#### 组件优化  
component的两个问题：  
>只要执行setState()即使不改变状态，组件也会重新render()  
>当前组件重新render()，其子组件也会重新render()，导致效率低

解决：  
方式一：shouldComponentUpdate  
>在shouldComponentUpdate中比较新旧state或props，如果有变化返回true，没有则返回false

方式二：PureComponent(常用)  
>PureComponent重写了shouldComponentUpdate，只有state或props有变化时才返回true  
>只是进行了state和props的浅比较，如果只是数据对象内部数据变了，数据引用地址没变，返回false；正常更新state也是同理  
>即：this.setState(新对象)，如果新对象为空返回false，如果新对象是this.state的地址应用同样返回false
>所以说不要直接修改state数据，而是要产生新数据
```javascript
//如：
state = {name:'rose'}
    const obj = this.state
    obj.name = 'jack'
    this.setState(obj)

//又如：
state={arr:[1,2]}
      const {arr} = this.state
      arr.unshift(3)
      this.setState({arr})
```

childrenProps：son向grandson通过props传递自身state不方便      `<Son><GrandSon/></Son>`              `this.props.children`  
renderProps：类似于vue中的插槽                                `<Son render={(xxx)=><GrandSon xxx={xxx}/>}/>`     `this.props.render(this.state.xxx)`

错误边界  
>将错误控制在一定范围内，子组件出错不会影响外层的组件，使用getDerivedStateFromError写在子组件的父组件里  
>配合componentDidCatch统计错误，反馈给后台  
>只适用于生产环境，开发环境还是会报错(20220210两处都能用)  
>只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误

组件通信  
组件之间相互传递东西(状态、修改状态的方法)  
>父子组件：props  
>兄弟组件：消息订阅-发布、集中管理(redux)  
>祖孙组件：消息订阅-发布、集中管理(redux)、conText(开发用的较少，主要用来封装插件)


### router-v6
#### Routes与Route
`<Routes>`必须要包裹`<Route>`使用
`<Route>`相当于一个if语句，如果其路径与当前url匹配，则呈现其对应的组件
当url发生变化时，`<Routes>`会查看子`<Route>`找到最佳匹配并呈现组件
`<Route>`也可以嵌套使用，且可配合`useRoutes()`配置路由表，但需要通过`<Outlet>`来渲染其子路由

#### Navgate
只要<Navgate>被渲染，就会修改url，切换视图

#### Outlet
指定路由组件的展示位置

#### 路由传参
params参数，可通过 useParams,useMatch 两种方式获取
search参数，可通过 useSearchParams,useLocation 两种方式获取

#### 编程式路由
使用 useNavigate hook

#### useInRouterContext()
如果组件在`<Router>`(BrowserRouter、HashRouter)的上下文中呈现，则useInRouterContext钩子返回true，否则返回false

#### useNavigationType()
返回当前的导航类型，用户是如何来到当前页面的
返回值，POP(刷新)、PUSH、REPLACE

#### useOutlet()
呈现当前组件中要渲染的嵌套路由
如果嵌套路由没有挂载，则为null；如果已经挂载，则为路由对象

#### useResolvedPath()
给定一个url，解析其中的path、search、hash值


### React中使用ts
创建支持ts的react项目
`npx create-react-app 项目名称 --template=typescript`
`yarn create react-app 项目名称 --template=typescript`

在已有的项目中使用ts
[参考文档](https://create-react-app.bootcss.com/docs/adding-typescript)

#### 文件变化说明
tsconfig.json，指定ts的编译选项，如：编译时是否移除注释；也可以手动生成tsc --init
react组件的文件扩展名变为.tsx
src目录下新增react-app-env.d.ts，react项目默认的类型声明文件

```typescript
  /// <reference types="react-scripts" />
```

>三斜线指令，指定依赖的其他类型声明文件，types表示依赖的类型声明文件包的名称，告诉ts帮我加载react-scripts这个包提供的类型声明文件
>react-scripts中又依赖了其他的类型声明文件(react、react-dom、node)，声明了图片、样式等模块的类型
>react项目中会自动加载src下的.d.ts文件，通过修改tsconfig.json中的include配置；react项目中会自动处理.tsx文件

#### tsconfig.json
[配置参考文档](https://www.typescriptlang.org/tsconfig)
可通过鼠标移入的方式查看解释说明

```json
      "compilerOptions": {                              //编译选项
        "target": "es5",                                //生成代码的语言版本
        "lib": [                                        //指定要包含在编译中的library
        "dom",
        "dom.iterable",
        "esnext"
        ],
        "allowJs": true,                                //允许ts编译器编译js文件
        "skipLibCheck": true,                           //跳过类型声明文件的所写类型的检查，可能会有类型准确性的问题
        "esModuleInterop": true,                        //es模块互操作，屏蔽EsModule和commonJS之间的差异
        "allowSyntheticDefaultImports": true,           //允许import a form 'b，即使模块中没有显示指定export导出
        "strict": true,                                 //开启严格模式
        "forceConsistentCasingInFileNames": true,       //对文件名称强制区分大小写
        "noFallthroughCasesInSwitch": true,             //为switch语句启动错误报告
        "module": "esnext",                             //生成js代码的模块化标准
        "moduleResolution": "node",                     //模块查找策略
        "resolveJsonModule": true,                      //允许导入.json的模块
        "isolatedModules": true,                        //是否将没有import|export的文件视为全局脚本文件
        "noEmit": true,                                 //编译时不生成任何文件，只进行类型检查；
                                                        //开启时使用babel进行类型转换，通过babel-loader处理ts文件
        "jsx": "react-jsx"                              //指定将jsx编译成什么形式
    },
    "include": [                                        //指定允许ts处理的目录
        "src"
    ]
```

可通过命令行为文件指定编译配置，tsc ./index.ts --target es6，这种方式将忽略tsconfig.json文件
直接使用tsc命令，会将当前目录下的ts文件按照tsconfig.json转换成js文件
推荐使用tscofig.json配置文件

#### react中的常用类型
在不使用ts时，可以使用prop-types，为react组件提供类型检查
ts项目中，推荐使用ts实现组件类型校验，代替prop-types
react项目通过@types/react、@types/react-dom类型声明包，来提供类型。CRA已帮我们安装(react-app-env.d.ts)，直接使用即可

#### 函数组件
组件类型、属性类型、属性默认值(解构时提供默认值)、事件和事件对象类型
如何知道e的类型`onChange={e=>{}}`，将鼠标放在e上，利用ts的类型推断查看事件对象类型

#### 类式组件
组件类型、属性类型、属性默认值(解构时提供默认值)、状态类型、事件

---
参考文档：
[react文档](https://react.docschina.org/docs/getting-started.html) 
[react-router](https://reactrouter.com/docs/en/v6)
[react-redux](https://react-redux.js.org/)
[redux](http://cn.redux.js.org/introduction/getting-started)

参考视频：[BV1wy4y1D7JT](https://www.bilibili.com/video/BV1wy4y1D7JT?spm_id_from=333.999.0.0)

