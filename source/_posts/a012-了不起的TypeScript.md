---
title: 了不起的TypeScript
categories:
  - 前端学习
tags:
  - TypeScript
date: 2022-07-18 14:17:57
index_img: https://img0.baidu.com/it/u=4167016203,3166254551&fm=253&fmt=auto&app=138&f=JPEG?w=499&h=311
---

### 在webpack中使用ts
1. 安装依赖
```
npm init -y
npm i -D webpack webpack-cli typescript ts-loader webpack-dev-server html-webpack-plugin cross-env clean-webpack-plugin
```

2. 配置package.json
```json
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.js",
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  },
```

3. 配置webpack.config.js
```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

const isProd = process.env.NODE_ENV === 'production' // 是否生产环境

function resolve(dir) {
    return path.resolve(__dirname, '..', dir)
}

module.exports = {
    mode: isProd ? 'production' : 'development',
    entry: {
        app: './src/main.ts'
    },

    output: {
        path: resolve('dist'),
        filename: '[name].[contenthash:8].js'
    },

    module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: 'ts-loader',
                include: [resolve('src')]
            }
        ]
    },

    plugins: [
        new CleanWebpackPlugin({//清除dist里面的旧文件
        }),

        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ],

    //设置哪些可以作为引用的模块
    resolve: {
        extensions: ['.ts', '.tsx', '.js']
    },

    devtool: isProd ? 'cheap-module-source-map' : 'cheap-module-eval-source-map',

    devServer: {//可以不设置
        host: 'localhost', // 主机名
        stats: 'errors-only', // 打包日志输出输出错误信息
        port: 8088,
        open: true
    },
}
```

4. 配置tsconfig.json，文件生成方式tsc --init
```json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */
    "experimentalDecorators": true,           //开启装饰器
    "emitDecoratorMetadata": true, 
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    "strict": false,                           /* 所有严格检查的总开关 */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 
    "forceConsistentCasingInFileNames": true  /* Disallow inconsistently-cased references to the same file. */
  }
}

```


### 类型注解 
为变量添加类型约束。约定了什么类型，就只能给变量赋值该类型的值，否则就会报错

### 常用基础类型
js已有类型(类型都是小写的)
>原始类型：number/string/boolean/null/undefined/symbol
>对象类型：object(数组、对象、函数)  

ts新增类型
>联合类型、自定义类型(类型别名)、接口、元组、字面量类型、枚举、void、any等  

#### 联合类型(|)
由两个或多个其他类型组成的类型，表示可以是这些类型中的任意一种


#### 类型别名
为任意类型起别名  

使用场景：当同一类型且类型还比较复杂，被多次使用时，可以通过类型别名，简化该类型的使用 

使用type创建类型别名后，直接使用该类型别名作为变量的类型注解即可  

命名规范可以是任意合法的变量名称，如：CustomArray

类型别名不能extends


#### 函数类型
实际指的是函数参数和返回值的类型  

两种方式：单独指定参数和返回值类型、同时指定参数和返回值类型(只使适用于函数表达式)  

void：如果函数没有返回值，则返回类型为void  

可选参数：函数参数可传可不传，在参数后面加?，可选参数只能出现在参数列表的最后。如：数组slice方法 

有默认参数的也只能出现在参数列表的最后


#### 对象类型
就是在描述对象的结构，有什么类型的属性和方法  

在一行代码中指定对象多个属性类型时，用分号隔开；如果一行代码只指定一个属性类型，可以去掉分号通过换行分隔多个属性类型  

可选属性：对象的属性和方法，也是可选的。如：axios，发送get请求时，method可省  
    

#### 接口
接口能作用于引用类型数据：对象，数组，函数

当一个对象类型被多次使用时，一般会使用接口来描述对象的类型，达到复用的目的 

使用interface关键字声明，接口名称可以是任意合法的变量名称，如：IPerson  


#### 接口和类型别名的异同 
相同点：
>都可以给对象指定类型  

不同点：
>接口只能为对象指定类型  
>类型别名实际上可以为任意类型指定别名  
>同一作用域下的多个同名接口会自动合并，而类型别名则会报错  


#### 接口继承
如果两个接口之间有相同的属性或方法，可以将公共的属性或方法抽离到一个接口中，通过继承实现复用  

使用extends关键字实现接口继承  


#### 元组(Tuple)
元组类型是另一种类型的数组，它确切的知道包含多少个元素，以及特定索引对应的类型


#### 类型推断
某些没有明确指明类型的地方，ts的类型推断机制会帮助提供类型  

常见场景：声明变量并立即初始化时；决定函数返回值时的两种情况，类型注解可以不写 

如果声明变量但是没有立即初始化值，此时必须手动添加类型注解  

能省略类型注解的地方就省略，充分利用ts的类型推断能力，提高开发效率  


#### 类型断言
某些情况下你会比ts更加明确一个值的类型，此时可以使用类型断言来制定更加具体的类型 

getElementById返回值是HTMLElement，该类型只包括常见标签公共的属性和方法，无法访问a标签的href，此时需要类型断言指定更加具体的类型  

推断HTML元素的类型，浏览器开发者工具选中元素，控制台输入console.dir($0)，展开翻到最下面就能看到  

非空断言


#### 字面量类型  
str2是一个常量，它的值不能变化，所以它的类型是'hello' 

上面的'hello'就是一个字面量类型，某个特定的字符串也可以作为ts中的类型  

除字符串外，任意的js字面量(比如：对象、数字等)都可以作为类型使用  

字面量类型常和联合类型一起使用，用来表示一组明确的可选值列表

相当于string类型，字面量类型更加精确、严谨


#### 枚举
枚举类型定义一组命名常量。它描述一个值，该值可以是这些命名常量中的一个  

枚举的功能类似于字面类型+联合类型组合的功能，也可以表示一组明确的可选值

使用enum关键字定义枚举，枚举中的值以大写字母开头，多个值通过逗号分隔  

访问枚举成员 Direction.Up  

枚举成员是有值的，默认为从0开始自增的数值  

枚举成员的值为数字的枚举，称为**数字枚举**。当然也可以给枚举中的成员初始化值  

**字符串枚举**没有自增长的行为，每个成员必须有初始值  

枚举不仅用作类型，还提供值。其他类型在编译成js代码时会被移除，但是枚举类型会被编译成js代码 Direction['Up'] = 0  

一般情况下，推荐使用字面量类型+联合类型的方式，因为相比枚举更加直观简洁高效  


#### any类型
不推荐使用any，这样会失去ts类型保护机制(不会在写代码的时候提示，而是在运行后才报错)

隐式具有any类型的情况：
>声明变量不提供类型也不提供默认值  
>函数参数不加类型  

除非临时使用any来避免书写一个当前并不知道很长很复杂的类型


#### typeof运算符
可以在类型上下文(:后面)中引用变量或属性的类型(类型查询)

根据已有变量的值，获取该值的类型，来简化类型书写

只能用来查询变量或对象属性的类型，无法查询其他形式的类型(如，函数调用的类型)


### 高级类型
>class类型、类型兼容性、交叉类型、泛型和keyof、索引签名类型和索引查询类型、映射类型

#### class类 
面向对象：封装(成员和其可见性)，继承，多态

ts中的类，不仅提供了类的语法功能，也作为一种类型存在  

属性初始化后，如：age:number，constructor中才可以通过this.age访问实例成员(constructor用来实例属性初始化) 

需要为构造函数参数指定类型注解，否则会被隐式推断为any；构造函数不需要返回值类型  

方法不需要初始化，方法的类型注解(参数和返回值)与函数用法相同  

类的两种继承方式：(1)extends子类继承父类   (2)implements实现接口  
>类实现接口意味着，类中必须提供接口中指定的所有方法和属性  

类成员可见性：可以使用ts来控制class的属性或方法对于class外的代码是否可见  
>public：表示公开的，公有成员可以被任何地方访问；class成员默认为public，可以直接省略
>  
>protected：表示受保护的，仅对其声明所在类和子类中可见，**实例中不可见** 
> 
>private：表示私有的，只在当前类中可见，**实例和子类中均不可见**  
>
>readonly：表示只读，用来防止在构造函数之外对属性进行赋值(属性初始化时可以设置默认值)；只能修饰属性不能修饰方法  
>+   使用readonly时属性后面的类型注解不加，则属性的类型为=后面的值(字面量类型)  
>+   接口或{}表示的对象类型，也可以使用readonly  


#### 类型兼容性
两种类型系统：Structural Type System(结构化类型系统)    Nominal Type System(标明类型系统，如：C# java)

ts采用的是结构化类型系统，也叫作duck typing，类型检查关注的是值的形状。也就是说如果两个对象具有相同的形状，则认为它们属于同一类型  

对象：对于对象类型来说，Apple的成员至少与Food的相同(成员名字也要相同，成员参数名字可以不同)，则Food兼容Apple，即**成员多的Apple的实例可以赋值给成员少的Food类**
>Apple的实例确实符合Food中的类型  

接口：接口之间的兼容性类似于上面，且class与interface之间也可以兼容 

函数：函数兼容性需要考虑，参数个数、参数类型、返回值类型  
>参数个数，**参数少的可以赋值给多的**，这就是forEach(()=>{})的原理，js中省略函数用不到的参数是很常见的  
>参数类型，
>+   相同位置的参数类型要相同(原始类型number、string等)或兼容(对象类型class、interface)；  
>+   如果参数有对象，把对象拆开，把每个属性看成一个个参数，参数少的可以赋值给参数多的  
>返回值类型，原始类型类型相同即可；对象类型，此时成员多的可以赋值给成员少的

#### 交叉类型(&) 
功能类似于接口继承，用于组合多个类型为一个类型(常用于对象类型)  

**交叉类型和接口继承的对比**  
>相同点：都可以实现对象类型的组合
>不同点：同名属性合并时，接口中同名属性的类型需保持一致否则报错，而交叉类型会直接合并


#### 泛型  
定义传入参数的类型

泛型在保证类型安全(不使用any)的同时，可以让函数等与多种类型一起工作，从而实现复用，常用于：函数、接口、class 

<>里面为类型变量，可以是任意合法的变量名称，具体什么类型由用户调用时指定 

在调用**泛型函数**时，可以省略<类型>来简化调用，ts内部会采用类型参数推断机制；当编译器无法推断或推断不准确时，就需要显式传入类型参数 

泛型约束：泛型可以表示任何类型，无法保证一定存在length属性，如：number类型就没有length，此时就要为泛型添加约束来收缩范围  
>方式一：指定更加具体的类型；  
>方式二：添加约束(extends接口)，创建描述约束的接口，通过extends使用接口为泛型添加约束，该约束表示传入的类型必须包含接口中的成员 

泛型的类型变量可以有多个，并且类型变量之间还可以约束

keyof关键字接收一个对象类型，生成其键名的联合类型，如：'name'|'age'  

**泛型接口**，使用泛型接口时，需要显式指定具体的类型，接口中的所有成员都能使用类型变量  

数组是通过泛型接口实现的，当我们在使用数组时，ts会根据数组的不同类型，来自动将类型变量设置为相应的类型  

鼠标返在forEach上通过ctrl+鼠标左键(mac:option+鼠标左键)可以查看数组通过interface Array实现 

**泛型类**，类似于泛型接口，如果class在不用明确指定类型的情况下能够推断出需要的类型，则可以省略类型参数 

**泛型工具类型**：ts内置了一些常用的工具类型，来简化ts中的常见操作，都是基于泛型实现的，且是内置的，可以在代码中直接使用  
>(1)Partial<Type>：构造一个新类型，将Type的所有属性设置为可选  
>(2)Readonly<Type>：构造一个新类型，将Type的所有属性都设置为readonly  
>(3)Pick<Type,Keys>：从type中选择一组属性来构造新类型，Type表示选谁的属性，Keys表示选择哪几个属性，Keys只能是Type中存在的属性
>(4)Record<Keys,Type>：构造一个对象类型，属性为Keys，属性类型为Type


#### 索引签名类型
当无法确定对象中有哪些属性，此时就需要用到索引签名类型  

js中对象的键是string类型的  

key只是一个占位符，可以换成任意合法的变量名称  

js数组是一类特殊的对象，数组的键是number类型的  


#### 映射类型  
基于旧类型创建新类型(对象类型)，减少重复、提升开发效率；映射对象的键  

映射类型是基于索引签名类型的，语法类似，也使用了[] 

映射类型只能在类型别名中使用，不能在接口中使用  

映射类型除了根据联合类型创建外，还能根据对象类型创建(keyof)  

泛型工具类型，如：Partial<Type>，都是基于映射类型实现的；鼠标指定Partial，ctrl+鼠标左键


#### 索引查询类型
T[P]语法，在ts中叫做索引查询类型，用来查询属性的类型

[]中的属性必须存在于被查询类型中，否则会报错

同时查询多个索引类型


### <>的使用场景
数组声明 
```typescript
const arr:Array<string>
```

类型断言 
```typescript
<string>str
```

泛型
```typescript
getName<string>('akman')
```


### extends使用场景
class可以继承 只能继承一个类

接口可以继承  可以同时继承过个接口，用逗号隔开

泛型中参数类型也可以继承extends


### 类型声明文件
用来为已存在的js库提供类型信息

鼠标放在axios上，ctrl+鼠标左键，跳入index.d.ts文件即类型声明文件

#### ts中的两种文件类型
1. .ts文件(implementtation)
>既包含类型信息又可执行代码
>可以被编译为.js文件，然后执行代码
>用途：编写程序代码的地方
```typescript
type Obj = { age: number }
function add(a: number, b: number): number {
    return a + b
}
```

2. .d.ts文件(declaration)
>只包含类型信息的类型声明文件
>不会生成.js文件
>用途：为js提供类型信息
```typescript
type Obj = { age: number }
interface Iperson {
    age: number
}
```

####    使用类型声明文件的两种方式(别人的)
1. 使用已有的类型声明文件
>内置类型声明文件
>内置api都提供了类型声明文件，ctrl+鼠标左键跳到对应的类型声明文件下

2. 第三方库的类型声明文件
>库自带的类型声明文件，如：axios中index.d.ts
>
>由DefinitelyTyped提供，github仓库，用来提供高质量ts类型声明，这些包的名称为@types/
>引入第三方库，如果下面有...则没有类型声明文件，需要手动下载，如：npm i -D @types/lodash；
>会下载到node_modules/@types/下，之后自动加载下载好的类型声明文件，ctrl+鼠标左键跳到对应的类型声明文件下
>包查询地址：https://www.typescriptlang.org/dt/search?search=

#### 创建自己的类型声明文件(自己的)
场景：项目内共享类型；为已有的js文件提供类型声明
1. 项目内共享类型
>如果多个ts文件都用到了同一个类型，此时可以创建.d.ts提供该类型，实现类型共享
>创建需要共享的类型，使用export导出，在需要共享类型的ts文件中使用import导入即可，.d.ts后缀可省

2. 为已有的js文件提供类型声明
>将js项目迁移到ts项目时，为了让已有的.js文件有类型声明
>成为库作者，创建库为他人使用，如axios中的index.d.ts
>在ts文件中使用js文件中的功能时，ts会自动加载.js同名的.d.ts文件，以提供类型声明
>declare关键字，用于类型声明，为.js文件中已存在的变量声明，而不是创建一个新的变量
>对于type、interface只能在ts中使用的，可以省略declare关键字
>对于let、function在js、ts中都能用的，应该使用declare关键字，明确指定此处用于类型声明
```typescript
    // utils.d.ts
    declare let count:number
    interface IPoint {x:number;y:number}
```

### 补充
#### 装饰器
```typescript
    /* 
        类装饰器
    */
    const moveDecorator: ClassDecorator = (target: Function) => {
        // console.log(target); //Tank的构造函数
        target.prototype.getPosition = (): { x: number, y: number } => {
            return { x: 100, y: 200 }
        }
    }

    const MusicDecorator: ClassDecorator = (target: Function) => {
        // console.log(target); //Tank的构造函数
        target.prototype.payMusic = (): void => {
            console.log('爱情来得太快就像龙卷风');
        }
    }

    const SayDecorator: ClassDecorator = (target: Function) => {
        target.prototype.message = (msg: string): void => {
            console.log(msg);
        }
    }
    //把装饰器用到任何类上，该类都会具有该种功能
    @moveDecorator //相当于moveDecorator(Tank)
    @MusicDecorator
    @SayDecorator
    class Tank {
        [x: string]: any
        say() {
            this.message('hello this is tank')
        }
    }
    const t1 = new Tank()

    // console.log(t1.getPosition()); //这里可以使用类型断言，也可以在类中声明成员
    // t1.payMusic()
    // t1.say()

    @SayDecorator
    class Fighter {
        [x: string]: any
        say() {
            this.message('hi this is fighter')
        }
    }
    // new Fighter().say()


    /* 
        装饰器工厂
    */
    const jumpDecoratorFactory = (type: string): ClassDecorator => {
        switch (type) {
            case 'tigger':
                return (target: Function) => {
                    target.prototype.jump = (time: number): void => {
                        console.log('跳' + time + '次');
                    }
                }
            case 'lion':
                return (target: Function) => {
                    target.prototype.roar = (time: number): void => {
                        console.log('吼' + time + '次');
                    }
                }
        }

    }
    @jumpDecoratorFactory('tigger')
    class Tigger {
        [k: string]: any
    }
    // new Tigger().jump(10)

    @jumpDecoratorFactory('lion')
    class Lion {
        [k: string]: any
    }
    // new Lion().roar(100)


    /* 
        方法装饰器
    */
    function enumerable(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        //target        普通方法是原型对象，静态方法是构造函数
        //propertyKey   方法名
        //descriptor    方法描述
        // descriptor.value()
        // descriptor.writable = false //方法是否可以重新声明

        descriptor.value = () => {
            console.log('this is enumerable');

        }
    }
    function hightlightDecorator(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        const keyword = descriptor.value()
        descriptor.value = () => {
            return `<h2 style="color:red">${keyword}<h2>`
        }
    }
    function sleepDecoratorFactory(delay: number) {
        return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
            descriptor.value = () => {
                setTimeout(() => {
                    console.log(`延迟${delay}ms执行`);
                }, delay)
            }
        }
    }
    const ErrorDecorator: (target: any, propertyKey: string, descriptor: PropertyDescriptor) => void = (target, propertyKey, descriptor) => {
        const method = descriptor.value
        descriptor.value = () => {
            try {
                method()
            } catch (error) {
                console.log('%c出错了', 'color:green;font-size:30px');
            }
        }
    }
    class Greeter {
        //普通方法
        @enumerable
        greet() {
            console.log('this is Greeter');
        }
        @enumerable
        static smile() {
            console.log('this is smile');
        }

        @hightlightDecorator
        hightlight() {
            return '高亮内容'
        }
        @sleepDecoratorFactory(3000)
        sleep() { }
        @ErrorDecorator
        error() {
            throw new Error("");
        }
    }

    // new Greeter().greet()
    // Greeter.smile()
    document.querySelector('.heightlight').innerHTML = new Greeter().hightlight()
    // new Greeter().sleep()
    // new Greeter().error()


    /* 
        属性装饰器
    */

    function propDecorator(target: Object, propertyKey: string | symbol) {
        let value: string
        Object.defineProperty(target, propertyKey, {
            get() {
                return value.toLowerCase()
            },
            set(v) {
                value = v
            }
        })
    }

    function colorDecorator(target: Object, propertyKey: string | symbol) {
        function color16() {//十六进制颜色随机
            var r = Math.floor(Math.random() * 256);
            var g = Math.floor(Math.random() * 256);
            var b = Math.floor(Math.random() * 256);
            var color = '#' + r.toString(16) + g.toString(16) + b.toString(16);
            return color;
        }
        Object.defineProperty(target, propertyKey, {
            get() {
                return color16()
            }
        })

    }

    class Person {
        @propDecorator
        names?: string

        @colorDecorator
        color?: string

        constructor(names?: string, color?: string) {
            this.names = names
            this.color = color
        }

        draw() {
            document.querySelector('.radomcolor').innerHTML = `<h3 style="color:${this.color}">radomcolor<h3>`
        }
    }
    const p1 = new Person('HhHA')
    // console.log(p1.names);
    let timer = null
    clearInterval(timer)
    timer = setInterval(() => {
        p1.draw()
    }, 5000)


    /* 
        参数装饰器
    */
    function paramDecorator(target: Object, propertyKey: string | symbol, parameterIndex: number) {
        let requiredParams: number[] = []
        requiredParams.push(parameterIndex)
        Reflect.defineMetadata('required', requiredParams, target, propertyKey)
    }
    function validateDecorator(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
        const method = descriptor.value
        descriptor.value = function () {
            let requiredParams: number[] = Reflect.getMetadata('required', target, propertyKey) || []
            let arg = arguments
            requiredParams.forEach(index => {
                if (index > arg.length || arg[index] === undefined) {
                    throw new Error("请传递必要参数");
                }
                return method.apply(this, arg)
            })
        }
    }
    class Animal {
        @validateDecorator
        show(@paramDecorator id: number, @paramDecorator content: string) {
            console.log('请求参数完整', id, content);
        }
    }
    new Animal().show(10010, 'hello')
```

#### 存取器
```typescript
    enum Gender { male = "MALE", female = "FEMALE", unknown = "UNKNOWN" }

    class Person {
        private _gender: Gender = Gender.male //私有属性名字需要下划线

        //内置
        get gender() {
            return this._gender;
        }
        set gender(gender: Gender) {
            this._gender = gender
        }

        //自定义
        public getGender() {
            return this._gender
        }

        public setGender(gender: Gender) {
            this._gender = gender
        }


        public walk() {
            this.left()
            this.forward()
            this.right()
        }

        private left() {
            console.log('迈左腿');
        }
        private right() {
            console.log('迈右腿');
        }
        private forward() {
            console.log('前进一步');
        }
    }

    const p1 = new Person()
    // p1.walk()
    // p1._gender() //报错
    // console.log(p1);

    //内置使用
    console.log(p1.gender);
    p1.gender = Gender.unknown
    console.log(p1.gender);

    //自定义使用
    console.log(p1.getGender());
    p1.setGender(Gender.female)
    console.log(p1.getGender());
```

#### 抽象类
```typescript
    /* 
        抽象类是一种特殊的类，接口是一种特殊的抽象类
        如果一个类中有一个方法是抽象方法，那么这个类也是抽象类，需要abstract声明
        抽象方法没有方法体
        抽象类不能被实例化，自己不实现交给子类实现
    */

    abstract class Person {
        name: string = 'akman'
        abstract age: number

        run() {
            return '$$$$$'
        }

        abstract say(): string;
    }
    // const p1 = new Person() //报错

    //抽象类必须有子类，并且子类需要将抽象类上的抽象成员重写
    class Woman extends Person {
        age: number = 18
        say(): string {
            return '%%%%'
        }
    }
    const w1 = new Woman()
    // console.log(w1);


    /* 
        接口的所有成员都是抽象成员，自带abstract声明，并且都是public
    */
    interface IPerson {
        paly: () => void
    }

    class Man extends Person implements IPerson {
        name: string    //Person成员
        age: number     //Person成员
        run: () => string = () => { //Person成员
            return ''
        }
        say(): string { //Person成员
            return ''
        }
        paly: () => void    //IPerson成员
        jump: () => void    //自定义成员
    }
```

#### 多态
```typescript
    /*
        多态：由继承产生了相关不同的类，对同一个方法可以有不同的响应 
        多态并不是新语法，而是新概念
        将子类对象(new KeyBoard())赋值给父类(USB)的引用,然后调用父类中的成员
    */
    interface USB {
        start(): void
        run(): void
        end(): void
    }

    class KeyBoard implements USB {
        start(): void {
            console.log('KeyBoard-start');
        }
        run(): void {
            console.log('KeyBoard-run');
        }
        end(): void {
            console.log('KeyBoard-end');
        }
    }

    class Microphone implements USB {
        start(): void {
            console.log('Microphone-start');
        }
        run(): void {
            console.log('Microphone-run');
        }
        end(): void {
            console.log('Microphone-end');
        }
    }

    function insert(usb: USB) {
        usb.start()
        usb.run()
        usb.end()
    }

    insert(new KeyBoard())
    insert(new Microphone())
```

#### 命名空间
```typescript
export namespace first { //命名空间也可导出在其他文件中使用，导入方式为es6
    document.querySelector('h2').innerHTML = '命名空间'
    export function getPosition(x: number, y: number) {
        return { x, y }
    }
    // console.log(getPosition(1, 2));

    export namespace firstOne {
        export const data = 'hello'
    }
    // console.log(firstOne.data);
}

namespace second {
    function getPosition(x: string, y: string) {
        return { x, y }
    }
    // console.log(getPosition('10px', '20px'));
}

// console.log(first.getPosition(10, 100));
// console.log(second.getPosition(10, 100)); //没有导出报错
// console.log(first.firstOne.data);
```

---
中文文档：[typescript中文文档](https://www.tslang.cn/docs/home.html)


参考视频：[BV14Z4y1u7pi](https://www.bilibili.com/video/BV14Z4y1u7pi?spm_id_from=333.999.0.0)  [BV1c34y1v7gi](https://www.bilibili.com/video/BV1c34y1v7gi/?spm_id_from=333.788)  [BV1WR4y1J7Lc](https://www.bilibili.com/video/BV1WR4y1J7Lc?spm_id_from=333.999.0.0)