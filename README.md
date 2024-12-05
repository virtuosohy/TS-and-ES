## 基础类型

- 布尔值（boolean）：用于表示`true`或者是`false`

```typescript
let isActive: boolean = true;
```



- 数字（number）

```typescript
let age：number =10； 
```



- 字符串（string）

```typescript
let name:string = 'aa'
```



- 数组（Array）

```
let numbers:number[] = [1,2,3];
//或者是
let strings:Array<string> = ['a','b']
```



- 元组（Tuple）：元祖是固定长度的数组，可以包含不同的类型

```typescript
let person:[string,number] = ['jack' , 30]
```



- 枚举（Enum）：用于表示一组命名常数

```typescript
enum Color {
    Red = 1,
    Green,
    Blue
}
let color :Color = Color.Green
```



- 任意类型（any）：允许任何类型的值，类似JavaScript的动态类型

```typescript
let data:any = 11;
```



- `null`和`undefined`



## 接口（Interface）

### 定义基本接口

```typescript
interface Person {
 name: string;
 readonly age: number;   //只读属性
 sex?:string;    //可选属性 
}
let user: Person = {
name: "Alice",
age: 30
};
```



### 函数类型:

可以通过接口来定义函数的类型。

```TypeScript
interface Greet {
(name: string): string;
 }
 const greet: Greet = (name) => `Hello, ${name}};
```




### 接口扩展

接口支持继承,可以扩展其他接口。

```TypeScript
interface Animal {
    name: string;
    }
interface Dog extends Animal {
    breed: string;
    }
 let dog: Dog = { name: "Buddy", breed: "Golden Retriever" };
```





## 泛型（Generics）

### 泛型函数

使用泛型来使函数支持不同的输入输出

```typescript
function identity<T>(arg: T): T {
    return arg;
}
let result = identity(42); //T 被推断为 number
let result2=identity("Hello"); //T被推断为string
```



### 泛型接口

```typescript
interface IdentityFn<T> {
    (arg: T): T ;
}
const identity: IdentityFn<number> = (arg) => arg;
```



## `extends` 关键字

`extends` 在TypeScript中有多个用法,主要有以下几种:
1.用于继承类;
2.用于泛型约束;
3.用于条件类型的类型推断。



### 用于继承类

在类之间使用`extends`关键字可以继承一个类的所有属性和方法。

```typescript
class Animal {
   name: string;
  constructor(name: string) {
    this.name = name;
}
speak() {
   console.log(`${this.name} makes a sound`)
 }
}

class Dog extends Animal {
 constructor(name: string) {
   super(name);//调用父类的构造函数
}
speak() {
   console.log(`${this.name} barks`);
  } 
}
const dog = new Dog("Buddy");

dog.speak(); //输出 "Buddy barks"
```





## Typescript编译方案与打包工具选择

TypeScript作为一种静态类型语言,它的编译方式和工具链配置尤为重要。





### `tsc`TypeScript Compiler):传统的TypeScript编译工具

`tsc`是TypeScript官方提供的编译工具,用于将TypeScript代码编译为JavaScript

它是TypeScript项目中最基础的工具,提供了非常多的自定义选项,允许我们根据需求配置不同的编译行为。



#### tsconfig.json配置详解

tsc的构建输出由tsconfig.json 文件配置。

`tsconfig.json`是TypeScript编译器的配置文件,所有的TypeScript编译选项都可以在这个文件中设置。以下是一些常常用的配置项及其含义:

```json
{
"compilerOptions": {

"target": "es5",               //设置编译后的Javascript版本

"module": "esnext",          //设置模块系统(例如:ES6模块,CommonJS)
"lib": ["dom", "es6"],       // 指定编译时使用的库(默认包括ES5)

"sourceMap": true,         //生成.map文件,用于调试

"strict": true,          //启用所有严格类型检查选项
    
"esModuleInterop": true,   //使默认导入工作正常(兼容CommonJS)
    
"skipLibCheck": true,     //跳过库文件的类型检查,加快编译速度

"forceConsistentCasingInFileNames":true  //确保文件名的大小写一致 
                  }
}
```



项目结构配置

```json
{

"include": ["src///*"),    //指定要编译的文件夹或文件
"exclude":["node_modules"],   //排除不需要编译的文件夹

 }

```



`target`:设置编译后生成的JavaScript的目标版本(如es5、es6
等)。在支持现代JavaScript的项目中,通常选择 esnext,这样编译后的代码就会尽可能
符合当前的标准。

`module`:用于指定模块系统(如CommonJS, ESNext)。如果你在Node.js环
境中开发,一般使用CommonJS,如果你在前端使用模块化,建议使用
ESNext 。

`lib`:配置TypeScript编译时可用的库文件。例如,es6表示支持ES6的标准
库,dom表示浏览器相关的API库。

`strict`:开启TypeScript的严格模式。它包含了对类型检查的严格要求,例如
noImplicitAny、strictNullChecks等,强烈建议在项目中启用。

`sourceMap`:配置是否生成源映射文件(.map 文件),便于调试时追踪TypeScript代码。

`esModuleInterop`:启用后,允许从CommonJS模块默认导入。它通常在与老旧JavaScript代码兼容时非常有用。



### tsc的优缺点

优点:
广泛支持:tsc是官方提供的TypeScript编译工具,支持所有TypeScript特性。
配置灵活:可以通过 tsconfig.json文件高度定制。

缺点:
构建速度较慢:相比于现代的构建工具,tsc在大型项目中的构建速度较慢。
缺乏增量构建支持:每次编译都会重新编译整个项目,缺乏快速增量构建的支持。



## Vite:现代前端开发的构建工具

Vite是一个现代化的前端构建工具,它以极快的启动速度和优化的构建流程著称。
Vite内部使用了Rollup作为打包工具,同时它还提供了对TypeScriipt的良好支持,因
此在很多项目中,尤其是前端项目中,Vite已成为首选的构建工具。



### 为什么选择Vite?

`快速启动`:Vite通过利用浏览器的原生ES模块支持,只在需要要时动态构建代码。
这比传统的打包工具(如Webpack)要快得多。

`热更新(HMR)`:Vite提供了非常快的模块热更新(Hot Module Replacement),
提高了开发时的效率。

`TypeScript集成`:Vite内部已经集成了TypeScript支持,无需额外配置。只需在
项目中安装TypeScript,Vite会自动识别并处理TypeScript文件

`现代化配置`:使用Vite时,你只需配置非常少的内容,大多数功能默认开箱即用。
例如,Vite会自动使用tsconfig.json中的TypeScript配置。



Vite配置文件实例：

```javascript
// vite.config.ts
import { defineConfig } from 'vite';

export default defineConfig({
    plugins: [],
    server: {
        port: 3000,  //设置开发服务器的端口
        open: true   //启动时自动打开浏览器
        },
  build: {
   target:   'esnext',   // 设置编译目标为最新的ES版本
   outDir:    'dist',    //设置输出目录  
},
resolve: {
   alias: {
     '@': '/src',     //// 设置路径别名
     },
  },
});
```





# ESLint

## ESLint 的核心原理

ESLint基于 AST （抽象语法树）分析代码 ，将其转化为结构化的数据，通过分析树节点来识别不符合规则的部分，



## ESLint规则约定

- ### 规则的三种级别

`off`:关闭规则,不进行检查。
`warn`:警告级别,违反规则时会发出警告,但不阻止代码运行다.
`error`:错误级别,违反规则时会报错,常用于团队必需遵循的规则。



- ### 核心规则讲解

`语法检查`:确保代码符合语法规则。例如,禁止使用未定义的变量或重复声明
变量。
`风格一致性`:统一代码风格,例如强制使用分号、规范空格、缩进等,保持代
码结构一致。
`最佳实践:`如禁止使用eval函数、避免原型污染等,以冰减少代码中潜在的安
全隐患。

- 常用规则的意义

  例如 no-unused-vars(未使用变量)可以帮助减少冗余代码、eqeqeq(强制使用全等)可减少隐式类型转换导致的Bug。项目中可根据需求进行细化配置。



ESLint与Prettier的集成
在开发中,`ESLint`主要负责语法和代码逻辑检查,而`Prettier`专注于代码格式化。将两者结合可使代码格式和风格一致,减少团队冲突。



## JS项目的ESLint配置

### 基础配置

在JS项目中,基础配置主要用于检查常见的代码问题。通过ESLint,我们能识别出如未定义变量、未使用变量等问题,这有助于提前规避错误,提升代码质量。在eslint.config.js中,基础配置如下:

```typescript
export default {
rules: {
"no-console": "error",
"no-unused-vars": "error",
"no-sparse-arrays": "error",
"no-undef": "error",
"no-unreachable": "error",
"no-dupe-keys": "error",
},
 };
```

### 核心规则介绍

为了确保代码质量和一致性,我们配置了几个重要的规则:
`"no-console":"error"`:禁止使用 console,避免在生产代码中输出调试信息。

`"no-unused-vars"`:"error":禁止未使用的变量,确保代码中所有声明的变量都有实际用途。

`"no-sparse-arrays"`:"error":避免稀疏数组,防止不必必要的空元素带来的潜在问题。

`"no-undef":"error"`:禁止使用未定义的变量,确保所有变量都是明确定义的。

`"no-unreachable"`:"error":避免无法到达的代码,提升代码可读性。

`"no-dupe-keys"`:"error":禁止对象字面量中的重复键值对,避免逻辑错误。



### 规则集简化



 ```TypeScript
import js from "@eslint/js";

 export default [js.configs.recommended];

 ```



## TS项目的ESLint配置

在将JS项目迁移到TS项目时,发现一些JS项目的ESLint规则无法完全适应TypeScript的类型系统。
因此,引入了@typescript-eslint/parser,以便解析TypeScript语法,同时在原有JS配置基础上扩展了规
则。

### TS项目配置

在`eslint.config.js`中,配置如下:

```typescript
import js from "@eslint/js";
import tsParser from "@typescript-eslint/parser";

export default [
{
ignores: ["eslint.config.js"],
files: ["src/**/*.ts"],
rules: {
"no-console": "error",
"no-unused-vars": "error",
"no-sparse-arrays": "error",
"no-undef": "error",
"no-unreachable": "error",
"no-dupe-keys": "error",
},
languageOptions: {
parser: tsParser,
},
},
];
```







## Vue项目的ESLint配置

在迁移到Vue项目时,由于Vue文件的.vue后缀格式特殊,普通的JS和TS配置无法完全适应。

因此,引入了 vue-eslint-parser以解析 .vue文件,并结合@typescript-eslint/parser处理其中的TypeScript代码。

### Vue项目配置

在eslint.config.js中,配置如下:

```typescript
import js from "@eslint/js";
import tsParser from "@typescript-eslint/parser";
import vueEslintParser from "vue-eslint-parser";

export default [
ignores: ["eslint.config.js"],
files: ["src/**/*.vue"],
rules: {
"no-console": "error",
"no-unused-vars": "error",
"no-sparse-arrays": "error",
"no-undef": "error",
"no-unreachable": "error",
"no-dupe-keys": "error"
},
languageOptions: {
parser: vueEslintParser,
parserOptions: {
extraFileExtensions: [".vue"],
ecmaFeatures: {
jsx: true,
},
parser: tsParser,
sourceType: "module",
},
},
},
];

```

