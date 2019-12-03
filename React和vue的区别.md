**React和Vue的异同:**

1.监听数据变化的实现原理不同：

vue采取的是数据劫持(Object.defineProperty,Proxy)和发布者/订阅模式能精确知道数据的变化,并且数据是可变的

React则采用的是比较引用模式进行的(PureComponent/shouldComponentUpdate),数据是不可变(Immutable.js)的

2.数据的可变性:

vue采用的是可变数据,就是直接改变传进来的数据,

React则采用的是不可变数据,就是不改变传进来的数据,这是符合函数式编程的思想,它是生成一个新的数据,通过对比新旧数据来进行更新的,其中immutable.js就是进行不可变数据的一个工具库

3.数据流的方向:

vue是进行双向绑定的

React则是单向数据流,但是可以利用函数的方法对数据进行回传

4.jsx和html模板：

vue推荐使用html模板

React则使用jsx