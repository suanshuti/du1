## 作用
二者作用完全一样，就是改变this 指向

## 二者区别
二者区别只有一点：传参列表不同，call 传参列表要跟函数的形参列表个数相同，apply 只能传进一个数组，把所有对应参数依次放到这个数组里。

## 应用场景：
直接看例子
```javascript
function Wheel(wheelSize, style) {
    this.wheelSize = wheelSize;
    this.style = style;
}

function Sit(c, sitColor) {
    this.c = c;
    this.sitColor = sitColor;
}

function Model(height, width, len) {
    this.height = height;
    this.width = width;
    this.len = len;
}

function Car(wheelSize, style, c, sitColor, height, width, len) {
    Wheel.apply(this, [wheelSize,style]);
    Sit.call(this,c,sitColor);
    Model.call(this,height,width,len);
}

var car = new Car(100,'花里胡哨的','真皮','red',1800,1900,4900);
console.log(car);

```

