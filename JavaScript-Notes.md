# JavaScript-Notes

---

### **克隆数组**

经常用到克隆一个一模一样的数组，但是不能直接 arr1 = arr2，因为 arr1 只是指向同一份数据的另一个指针而已，这种情况下，修改arr2，arr1也会发生变化，而我们需要的是完全的克隆。

ES5 中可以这样写： 

```
arr2 = arr1.concat()
// 或
arr2 = arr1.splice()
```
根据 concat 和 splice 返回原数组的克隆的特性

ES6 中出现了扩展运算符就可以更加简便地实现了： 
```
arr2 = [...arr1]
```

### **合并数组**

将一个数组的所有元素合并到另一个数组中去，最简单的做法就是遍历数组，然后把每个元素 push 进另一个数组中，很显然，这种做法非常不优雅，看看更优雅的：

ES5 的做法：
```
arr1.concat(arr2)
```
concat 本身的作用就是合并两个数组，这样做非常自然。这种做法不会改变 arr1 的内容，它会返回一个新的数组

ES6 的做法，扩展运算符再一波刷新了它的存在感：
```
arr1.push(...arr2)
```
这种做法会改变 arr1 的内容，就再次省略了赋值的操作，优雅。合并多个数组的时候，只需往 push 里添加参数就可以了

### **NodeJS 和 Javascript 的 this**

初用 NodeJS 的时候第一时间就发现了 NodeJS 中的 this 怪怪的，下面请看案发现场：

```
var a = 'a';
var fn = function () {
	console.log(this.a);
};

fn();
```

这个简单的例子，在浏览器中执行大家都知道肯定是打印 a 出来。然而这个理所当然在 NodeJS 中并不管用，NodeJS 直接扔了个 undefined 回来，说这个玩意我不认识。都是兄弟，怎么弟弟就脸盲了呢？咱们去下一个案发现场：

```
var a = 'a'
var fn = function () {
	this.a = 'b';
};

fn()
console.log(a);
```

在函数中修改了全局变量 a 的值，至少在浏览器上运行是这么理解的，打印出来的结果是理所当然的 b。我们倔强的 NodeJS 说什么也不变，就是 a。那么在 NodeJS 中这个函数里的 this.a 去哪了呢？打印一下这个就知道了：

```
console.log(global.a)
```

有没有一丝明白，我们再来理一下。函数中的 this 指向的是调用改函数的对象，所以在全局中执行的函数 fn this 指向的是全局对象 window ，这是在浏览器中的情况。NodeJS 没有 window，全局对象是 global，所以函数中定义的 this 的内容全部都存储在 global 中，而在顶级作用域中所有的内容都是属于 module.exports 老大的，所以顶级作用域定义的 this.a ，可以通过这样来获取：

```
console.log(module.exports.a)
```




