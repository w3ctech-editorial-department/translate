# 勿让你的调用栈原地爆炸

[原文链接](http://sking7.github.io/articles/661487318.html)

对于编程中的一些类型判定问题（尤其是数学问题）最优雅的解决方案是使用递归，因为递归可以直接把这个问题转变成一个有关于数学定义的问题。不幸的是，在 JavaScript 中使用递归，会让你遇到一些麻烦事。

假定我们想定义一个递归，该递归函数用于告诉我们一个数字是否是偶数。（是的，确定一个数字是否是偶数，这里有很多方法，我们只是拿递归来探索一下）我们可能会这样写代码就像下面这样：

```javascript

function isEvenNaive (num) {
    if (num === 0) {
        return true;
    }

    if (num === 1) {
        return false;
    }

    return isEvenNaive(Math.abs(num) - 2);
}

isEvenNaive(10);
// => true

isEvenNaive(9);
// => false
```

这代码看起来非常合理。但是如果我们把一个大数，比如 9999 传给上述递归，会发生什么情况呢？

```javascript
isEvenNaive(99999);
// => InternalError: too much recursion
```

具体出现哪种错误，这各不相同，取决于 JavaScript 引擎，不过像上面这样的大数字，最终会让你达到引擎调用堆栈的深度限制，并且会伴随有[堆栈溢出](http://en.wikipedia.org/wiki/Stack_overflow)。

[调用栈](http://en.wikipedia.org/wiki/Call_stack) 本质上是一个保存在内存中的列表，该列表包括了所有那些未被执行完的函数调用、与函数相关的变量以及与函数相关的信息。每次使用大于 1 的值来调用 isEvenNaive，它都会自己调用自己，在这过程中，不但会把信息添加到堆栈，而且还会消耗更多资源。

只有当我们最后遇到递归的终止条件 0 或者 1，递归才不会继续下去，接着是清空调用栈以及释放资源。没有递归深度的限制（或者[尾递归](http://stackoverflow.com/questions/310974/what-is-tail-call-optimization)），一个深度递归可能会耗完电脑上的所有资源。

但是递归还是很用的。有没有办法可以让我们既可以写递归又不会出现栈溢出？事实证明，还真有。

```javascript
function isEvenInner (num) {
    if (num === 0) {
        return true;
    }

    if (num === 1) {
        return false;
    }

    return function() {
        return isEvenInner(Math.abs(num) - 2);
    };
}

isEvenInner(8);
// => function() {
//        return isEvenInner(Math.abs(num) - 2);
//    };

isEvenInner(8)()()()();
// => true
```

首先你会注意到， `isEvenInner` 函数这次没有直接调用自己，而是返回一个匿名函数。这就意味着每次调用 `isEvenInner` 函数，该函数都会被立即 resolved 掉，**并且不会增加堆栈的大小**。这也就意味着我们需要一种方法，来自动调用递归过程中所返回的全部匿名函数。这也是 `trampoline` 出现的由来。

```javascript
function trampoline (func, arg) {
    var value = func(arg);

    while(typeof value === "function") {
        value = value();
    }

    return value;
}

trampoline(isEvenInner, 99999);
// => false

trampoline(isEvenInner, 99998);
// => true
```

`trampoline` 函数能够有效地将递归算法转换成用 `while` 循环来执行。只要 `isEvenInner` 函数一直返回函数，`trampoline` 函数就能一直执行这些返回函数。当我们最后终于返回一个非函数的值，`trampoline` 函数会将结果返回出去。

现在我们可以避免调用栈原地爆炸，但是这样调用 `trampoline(isEvenInner, 3)` 还不够优雅。那让我们用 bind 来加点[”料“](http://designpepper.com/blog/drips/partial-application-with-function-bind)。

```javascript
var isEven = trampoline.bind(null, isEvenInner);

isEven(99999);
// => false
```

需要着重注意的是，虽说所阐述的原理具有普适性，但是 `trampoline` 函数使用起来，有一定的局限性。

* 假定你只会传一个参数给递归函数。
* 假定函数最后的返回值是一个非函数类型的值。
* 在一些老的 JavaScript 引擎里面，`typeof function` 不可靠。

有关更健壮的实现，可以参考 [underscore-contrib](http://documentcloud.github.io/underscore-contrib/#trampoline)。

我希望上面讲的这些可以帮助你更好地理解 JavaScript 中递归的工作原理。

有些人提到，他们对深入的主题感兴趣，所以我正在努力增加这样的内容。如果你有推荐的话题，只需要回复这封邮件。我喜欢你们能够给出你们的反馈。