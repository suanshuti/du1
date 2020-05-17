## 异步加载的三种方案

1. defer 异步加载：
    - 要等到dom文档全部解析完才会被执行，
    - 可以将js代码写到script标签内部
    - 只有IE能用。

2. async 异步加载：
    - 加载完就执行。
    - async只能加载外部脚本。
    - 不能把js代码写在script标签里。

3. 创建script，插入到dom中，加载完毕后callback。