## 如何触发一个盒子的bfc
- position:absolute;

- display:inline-block;

- float:left/right;

- overflow:hidden;

## bfc 的作用
- 解决 **margin塌陷**。
解决方法：触发父盒子的bfc
   > 但是需要注意的是，通过bfc 解决margin 塌陷都会有一定的副作用，要根据具体的需求场景来选择哪种方式来触发bfc。

- 解决兄弟元素上下margin之间 **margin合并的问题**
   解决方法：给兄弟元素外面加一层父元素，并触发父元素的bfc，这样两个兄弟盒子之间的上下margin 就会累加，而不是重合。

   > 但是实际上不允许这样解决，因为与解决margin 塌陷不同（只修改css，不必多加层），多加一层父元素，就多了一层结构，实际开发中，不应该添加无意义的结构。所以实际开发中，**margin合并**的问题不用解决，或者说是直接通过简单的数学手段解决就行了。
