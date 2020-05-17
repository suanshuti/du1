### arguments.callee 是什么？
是指向当前实参所属函数的引用。说白了，就是 **函数自己**。
eg.1
```javascript
function test() {
    console.log(arguments.callee == test); 
}
test();  // true
```

eg.2
#### 应用场景
```javascript
var num = (function (n) {
    if (n == 1) {
        return 1;
    }
    return n * arguments.callee(n - 1);
}(100));
console.log(num);
```
##@ caller
```javascript
function test() {
    demo();
}

function demo() {
    console.log(demo.caller);
}

test(); 
// 打印的是其执行时所在的环境
// ƒ test() {
//     demo();
// }
```

### 注意：ES5 标准模式中，callee和caller 都不允许被使用。
