## this 的知识点：
1. 预编译过程的this -> window
```javascript
function test(c) {
    var a = 123;
    function b() { };
    console.log(this);
}

test(1);    // 打印的结果，即this 指向window
new test(1); // 打印的结果，即this 指向自己（test构造出的对象）
```
对应AO
```javascript
AO{
    arguments : [1],
    this : window,
    c : 1,
    a : undefined,
    b : function(){}
}
```
代码执行打印信息：
![this_001.png](this_001.png)

**注意**：test(1) 和 new test() 执行打印出的结果（this）不一样！为什么会这样？
**因为**：
- test(1)：如果是一般的调用执行，AO就是如上面所示的，指向window。
- new test()：可是如果是用new 调用执行的话，函数就会变成构造函数，系统会（隐式地）把this 替换掉，作以下修改：

```javascript
function test(c){
    // var this = Object.create(test.prototype);
    var a = 123;
    function b(){}
}
```
> 疑问：var this = Object.create(test.prototype); 这句代码，是在new 的时候先由系统加上去再进行预编译，还是在预编译阶段再加上去的。

2. 全局作用域里的this -> window

![this_002.png](this_002.png)

3. call/apply 可以改变函数运行时this 指向
4. obj.cunc(); func() 里面的this指向obj

## 案例分析
### eg1.
```javascript
var name = '222';
var a = {
    name: '111',
    say: function () {
        console.log(this.name);

    }
}

var fun = a.say;
fun();  // 222
a.say(); // 111

var b = {
    name: '333',
    say: function (fun) {
        // this -> b

        // 注意这里，这里虽然this指向b，但是没有this.fun()，而是空执行。
        // 所以，这里的调用者，就相当于fun 函数经过预编译后的结果——指向window，
        // 这句的执行（空执行）其实相当于是window.fun()：
        fun();
    }
}

b.say(a.say);   // 222
b.say = a.say;
b.say();    // 333
```

这个案例中的最难点在于b.say(a.say) ，理解的关键点在于
首先：fun 是b.say() 的形参，而a.say 是作为实参传进来的：
```javascript
say: function (fun) {
    fun();
}
```
所以，传进来之后，b.say() 的方法体，相当于是：

```javascript
say: function (fun) {
    function fun () {
        console.log(this.name);

    }
    fun();
}
```
最关键的是：虽然是在b.say() 中调用执行的，但是 fun() 并不等于 this.fun()。而是等于window.fun();
> 没有明确写调用者的方法调用，就是空执行，执行环境就是预编译的环境，就是window。


### eg2.
eg2.1
```javascript
var foo = 123;
function print() {
    this.foo=234;
    console.log(foo);
}
print();    // 234
console.log(this.foo);  // 234
console.log(window.foo);    // 234
console.log(this === window);   // true
```
print()是空执行，所以：
- print(); 相当于window.print();
- this.foo = 234，相当于window.foo = 234。
- console.log(foo) ，是空执行，所以是预编译的状态。传入的实参foo指向window.foo。

eg2.2
```javascript
var foo = 123;
function print() {
    this.foo=234;
    console.log(foo);
}
new print();    // 123
```
- new print() 会改变this指向 Object.create(print.prototype) 创建出的对象。所以this.foo 不再是预编译时的window.foo。window.foo 也就不会被修改为234。
- console.log(foo) ，是空执行，所以是预编译的状态。传入的实参是foo，而不是this.foo，所以foo指向window.foo。
- 从上面两句可以得出： console.log(foo) 传入的实参是预编译时的window.foo 值123。

