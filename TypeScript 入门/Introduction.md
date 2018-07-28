#Introduction

为了让程序有价值，我们需要能够处理最简单的数据单元：数字，字符串，结构体，布尔值等。 TypeScript支持与JavaScript几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用。
   
##布尔值   

最基本的数据类型就是简单的true/false值，在JavaScript和TypeScript里叫做boolean（其它语言中也一样）。   
```
let isDone: boolean = false;

document.body.innerHTML = isDone;
```   
反馈结果为`false`。

##数值   
和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是number。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。   
```
let decLiteral: number = 6;                     //十进制
let hexLiteral: number = 0xf00d;                //十六进制
let binaryLiteral: number = 0b1010;             //二进制
let octalLiteral: number = 0o744;               //八进制
```

##字符串
JavaScript程序的另一项基本操作是处理网页或服务器端的文本数据。 像其它语言里一样，我们使用`string`表示文本数据类型。 和JavaScript一样，可以使用双引号（`"`）或单引号（`'`）表示字符串。
```
let name: string = "bob";
name = "smith";
```
你还可以使用*模版字符串*，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（<code>`</code>），并且以<code>${ expr }</code>这种形式嵌入表达式
```
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.<br>

I'll be ${ age + 1 } years old next month.`;
```
使用`document.body.innerHTML = sentence;`打印出来，输出结果为
```
Hello, my name is Gene.
I'll be 38 years old next month.
```
这与下面定义sentence的方式效果相同：
```
let sentence: string = "Hello, my name is " + name + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

##数组
TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上`[]`，表示由此类型元素组成的一个数组：   
```
let list: number[] = [1, 2, 3];
```
第二种方式是使用数组泛型，Array<元素类型>：
```
let list: Array<number> = [1, 2, 3];
```

##元祖 Tuple
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为`string`和`number`类型的元组。