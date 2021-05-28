# TS

## 安装

```bash
npm i -g typescript
```

## 转译

```bash
// 浏览器中只能运行 JS，无法直接运行 TS，因此，需要将 TS 转化为 JS 然后再运行
tsc xxx.ts
node xxx.js 
```

```js
// --watch 表示启用监视模式，只要重新保存了 ts 文件，就会自动调用 tsc 将 ts 转化为 js
tsc --watch index.ts
```

## 简化

```bash
npm i -g ts-node
ts-node xxx.ts
```

## 类型注解

> 类型注解：是一种为变量添加**类型**约束的方式

- **约定类型后，不可以再赋值其他类型的值，系统会报错**

### 基本数据类型

```ts
// 数字
let num: number = 123
// 字符串
let str: string = '123'
// 布尔值
let flag: boolean = true
// null
let n: null = null
// undefined
let u: undefined = undefined
// any
let a: any = 1
a = '1' // ok
```

### 复杂数据类型

```ts
// 数组
// 数组的类型注解由两部分组成：类型+[]。
let arr: string[] = ['1','2','3']
let arr1: number[] = [1, 2, 3]
let arr2: object[] = [ { a: 1 }, { b: 1 } ]

// 元组
// 元组允许数组类型可以不一致，
let x:[number,string]
```

```ts
// 函数
// 参数注解 & 返回值注解
// function fn(i: 参数注解类型): 返回值注解类型 {}
function sing(song: string, num: number): string[] {
  console.log{`唱${num}首${song}`}
  return [song]
}
let a: string[] = sing('五环之歌')
```

```ts
// 对象 -- 接口
// 接口：为对象的类型注解命名，interface 表示接口
interface IUser {
   name: string
   age: number
   sing: () => void
}

let obj: IUser = {
   name: '郭德纲',
   age: 111,
   sing() { console.log(this.name) }
}
```

- 在函数声明形式的事件处理程序中，使用事件对象时，应该指定参数类型，否则，编辑器中访问事件对象的属性时没有任何提示；匿名回调不需要

```ts
btn.addEventListener('click', handleClick)
function handleClick(event: MouseEvent) {
	console.log(event.target)
}
```

- 类型推断（类型注解省略）

常见场景：1 声明变量并初始化时 2 决定函数返回值时

```ts
let age: number = 18 // => let age = 18 
function sum(num1: number, num2: number): number { return num1 + num2 } 
// => function sum(num1: number, num2: number) { return num1 + num2 }
```

### 泛型

> 不同于使用 `any`，它不会丢失信息；不预先指定具体的类型，而在使用的时候再去指定类型

1. 泛型变量

   ```ts
   //泛型变量的使用
   function identity<T>(arg:T):T{
       console.log(typeof arg);
       return arg;
   }
   let output1 = identity<string>('myString');
   let output2 = identity('myString');
   let output3 = identity<number>(100);
   let output4 = identity(200)
   ```

   ```ts
   //使用集合的泛型
   function loggingIdentity<T>(arg:Array<T>):Array<T>{
       console.log(arg.length);
       return arg;
   }
   loggingIdentity([1,2,3])
   ```

2. 定义泛型接口

   ```ts
   //泛型接口
   interface GenericIdentityFn<T> {
       (arg: T): T;
   }
   function identity<T>(arg: T): T {
       return arg;
   }
   let myIdentity: GenericIdentityFn<number> = identity
   ```

3. 定义泛型类

   ```ts
   //泛型类
   class GenericNumber<T>{
       zeroValue:T;
       add:(x:T,y:T)=>T;
   }
   let myGenericNumber=new GenericNumber<number>();
   myGenericNumber.zeroValue=0;
   myGenericNumber.add=function(x,y){return x+y;};
   console.info(myGenericNumber.add(2,5));
   
   let stringNumberic=new GenericNumber<string>();
   stringNumberic.zeroValue='abc';
   stringNumberic.add=function(x,y){return `${x}--${y}`};
   console.info(stringNumberic.add('张三丰','王小明'));
   ```

## 类型断言

- 背景：
  - 调用 querySelector() 通过 id 选择器获取 DOM 元素时，拿到的元素类型都是 Element。
  - 因为无法根据 id 来确定元素的类型，所以该方法就返回了一个**宽泛**的类型：元素（Element）类型
  - Element 类型只包含所有元素共有的属性和方法（比如：id 属性）
  - 导致无法访问特殊元素的属性，比如 img 元素的 src 属性

- 解决：
  - 使用类型断言，来手动指定**更加具体**的类型

- 语法：

  ```javascript
  值 as 更具体的类型
  ```

- 比如：

  ```ts
  // document.querySelector() 方法的返回值类型为： Element 
  // 如果是 h1 标签： 
  let title = document.querySelector('#title') as HTMLHeadingElement 
  // 如果是 img 标签： 
  let image = document.querySelector('#image') as HTMLImageElement
  ```

- 技巧：
  - 通过 console.dir() 打印 DOM 元素，在属性的最后面，即可看到该元素的类型

## 枚举

- 枚举是组织有关联数据的一种方式（比如，男 和 女 就是有关联的数据）
- 使用场景：当变量的值，**只能是几个固定值中的一个**，应该使用枚举来实现
- 注意：JS 中没有枚举，这是 TS 为了弥补 JS 自身不足而新增的

### 语法

```ts
enum 枚举名称 { 成员1, 成员2, ... }
```

示例

```ts
enum Gender { Female, Male }
enum Player { X, O }
```

注意

- 约定枚举名称、成员名称以大写字母开头
- 多个成员之间使用逗号（,）分隔
- 枚举中的成员，根据功能自己指定
- 枚举中的成员不是键值对

### 使用枚举

> 枚举是一种类型，因此，可以其作为变量的类型注解
>
> 枚举是一组有名字的常量（只读）的集合

```ts
enum Gender { Female, Male }
let userGender: Gender = Gender.Male
```

注意：枚举成员是**只读**的，也就是说枚举中的成员可以访问，但是不能赋值

### 枚举类型

#### 数字枚举

数字枚举：枚举成员的值为数字，默认情况下就是数字枚举

```ts
enum Gender { Female, Male } // 0, 1
enum Gender { Female = 100, Male } // 初始化成员的值 100, 101
```

特点：成员的值是**从 0 开始自增的数值**

#### 字符串枚举

字符串枚举：枚举成员的值为字符串

```ts
enum Gender { Female = '女', Male = '男' }
```

特点：没有自增行为，**需要为每一个成员赋值**

