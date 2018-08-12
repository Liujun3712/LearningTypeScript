#变量声明
`let`和`const`是JavaScript里相对较新的变量声明方式。 像我们之前提到过的，`let`在很多方面与`var`是相似的，但是可以帮助大家避免在JavaScript里常见一些问题。 `const`是对`let`的一个增强，它能阻止对一个变量再次赋值。

因为TypeScript是JavaScript的超集，所以它本身就支持`let`和`const`。 下面我们会详细说明这些新的声明方式以及为什么推荐使用它们来代替`var`。

如果你之前使用JavaScript时没有特别在意，那么这节内容会唤起你的回忆。 如果你已经对`var`声明的怪异之处了如指掌，那么你可以轻松地略过这节。
##`var`声明
一直以来我们都是通过`var`关键字定义JavaScript变量。
```
var a = 10;
```
大家都能理解，这里定义了一个名为`a`值为`10`的变量。   
我们也可以在函数内部定义变量：
```
function f() {
    var message = "Hello, world!";

    return message;
}
```
并且我们也可以在其它函数内部访问相同的变量。
```
function f() {
    var a = 10;
    return function g() {
        var b = a + 1;
        return b;
    }
}

var g = f();
g(); // returns 11;
```
上面的例子里，`g`可以获取到`f`函数里定义的`a`变量。 每当`g`被调用时，它都可以访问到`f`里的`a`变量。 即使当`g`在`f`已经执行完后才被调用，它仍然可以访问及修改`a`。
```
function f() {
    var a = 1;

    a = 2;
    var b = g();
    a = 3;

    return b;

    function g() {
        return a;
    }
}

f(); // returns 2
```
###作用域规则
对于熟悉其它语言的人来说，`var`声明有些奇怪的作用域规则。 看下面的例子：
```
function f(shouldInitialize: boolean) {
    if (shouldInitialize) {
        var x = 10;
    }

    return x;
}

f(true);  // returns '10'
f(false); // returns 'undefined'
```
有些读者可能要多看几遍这个例子。 变量`x`是定义在*`if`语句里面*，但是我们却可以在语句的外面访问它。 这是因为`var`声明可以在包含它的函数，模块，命名空间或全局作用域内部任何位置被访问（我们后面会详细介绍），包含它的代码块对此没有什么影响。 有些人称此为*`var`作用域或函数作用域*。 函数参数也使用函数作用域。   
   
这些作用域规则可能会引发一些错误。 其中之一就是，多次声明同一个变量并不会报错：
```
function sumMatrix(matrix: number[][]) {
    var sum = 0;
    for (var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (var i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }

    return sum;
}
```
这里很容易看出一些问题，里层的`for`循环会覆盖变量`i`，因为所有`i`都引用相同的函数作用域内的变量。 有经验的开发者们很清楚，这些问题可能在代码审查时漏掉，引发无穷的麻烦。   
###捕获变量怪异之处
快速的猜一下下面的代码会返回什么：
```
for (var i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100 * i);
}
```
介绍一下，`setTimeout`会在若干毫秒的延时后执行一个函数（等待其它代码执行完毕）。   
   
好吧，看一下结果：
```
// 以前的编译器会将结果输出成10，但是目前的3.0.1版本会输出成0。
0
0
0
0
0
0
0
0
0
0
```
很多JavaScript程序员对这种行为已经很熟悉了，但如果你很不解，你并不是一个人。 大多数人期望输出结果是这样：
```
0
1
2
3
4
5
6
7
8
9
```
还记得我们上面提到的捕获变量吗？ 我们传给`setTimeout`的每一个函数表达式实际上都引用了相同作用域里的同一个`i`。   
   
让我们花点时间思考一下这是为什么。 setTimeout在若干毫秒后执行一个函数，在`3.0.1`的版本下`i`在*`for`语句*里面定义为0，每次执行for语句的时候，并不会更改`for`循环体内的i的值，意思就是`var`定义域在此示例中不影响`for`的执行体。<font color=red>//可能有错误。</font>
   
一个通常的解决方法是使用[立即执行的函数表达式（IIFE）](https://segmentfault.com/a/1190000003985390)来捕获每次迭代时`i`的值：
```
for (var i = 0; i < 10; i++) {
    // capture the current state of 'i'
    // by invoking a function with its current value
    (function(i) {
        setTimeout(function() { console.log(i); }, 100 * i);
    })(i);
}
```
这种方法就是讲`i`变为`function`的内部变量，讲内部和外部的`i`隔离开来。<font color = red>//仅仅猜测</font>
##`Let`声明

现在你已经知道了`var`存在一些问题，这恰好说明了为什么用`let`语句来声明变量。 除了名字不同外，`let`与`var`的写法一致。
```
let hello = "Hello!";
```
主要的区别不在语法上，而是语义，我们接下来会深入研究。   
###块作用域
当用`let`声明一个变量，它使用的是*词法作用域*或*块作用域*。 不同于使用`var`声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或`for`循环之外是不能访问的。
