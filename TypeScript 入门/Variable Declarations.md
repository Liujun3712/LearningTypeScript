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
