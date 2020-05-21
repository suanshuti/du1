## FC
Formatting Context
```
Formatting context 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 Formatting context 有 Block fomatting context (简称BFC)和 Inline formatting context (简称IFC)。
```
## 如何触发一个盒子的bfc
- 根元素
- 浮动元素
- 定位元素
- overflow 不等于visible的块盒 
- display的值是inline-block、table-cell、flex、table-caption或者inline-flex


## bfc 的作用
1. 在元素的内部，创造一块独立的渲染区域
2. 规定了该区域中，常规流块盒的部局
3. 不同的BFC区域，进行渲染时互不干扰。
4. 创建了BFC的元素，隔绝了它内部和外部的联系，内部的渲染不会影响到外部，反之也如此。

## bfc 具体规则：
- 内部的Box会在垂直方向，一个接一个地放置。
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
- 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
1. 创建了BFC的元素，它的自动高度需要计算其“内部”的浮动元素
2. 创建了BFC的元素，它的边框盒不会与“外部”浮动元素重叠
3. 创建了BFC的元素，不会和它的子元素进行外边距合并


  
## 人为触发bfc 的使用场景

- 解决 **margin塌陷**。
解决方法：触发父盒子的bfc
   > 但是需要注意的是，通过bfc 解决margin 塌陷都会有一定的副作用，要根据具体的需求场景来选择哪种方式来触发bfc。

- 解决兄弟元素上下margin之间 **margin合并的问题**
   解决方法：给兄弟元素外面加一层父元素，并触发父元素的bfc，这样两个兄弟盒子之间的上下margin 就会累加，而不是重合。

   > 但是实际上不允许这样解决，因为与解决margin 塌陷不同（只修改css，不必多加层），多加一层父元素，就多了一层结构，实际开发中，不应该添加无意义的结构。所以实际开发中，**margin合并**的问题不用解决，或者说是直接通过简单的数学手段解决就行了。
